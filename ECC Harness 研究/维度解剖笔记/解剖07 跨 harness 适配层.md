# ECC 跨 harness 适配层 解剖笔记

> 研究对象：`/Users/aa00158/harness-research/ECC`（只读浅克隆，HEAD = `e4e4163`）
> 维度：跨 harness 适配层（多平台投射机制）
> 术语约定：**harness** = 跑 AI agent 的宿主程序/客户端（Claude Code、Cursor、Codex CLI、OpenClaw 等）。同一套 skill/agent/command 资产要在不同 harness 里生效，得按各家的目录约定和文件格式落地——这就是"适配层"。

---

## 0. 一句话结论

ECC 的跨 harness 适配是**「一份内容 + 每平台一个几十行的落点翻译器」**：内容只在仓库根写一遍（`skills/` `rules/` `commands/` `agents/` `hooks/`），`manifests/install-modules.json` 里每个模块挂一张"允许哪些平台"的白名单，`scripts/lib/install-targets/*.js` 里每个平台一个 adapter 负责"这些文件在我这儿该放哪、该改成什么名"，最后 `fs.copyFileSync` 逐文件拷过去（**全是拷贝，零软链**）。

能力损失全部体现在那张白名单上：**14 个安装目标里只有 5 个（claude / claude-project / cursor / opencode / codebuddy）能装 hooks**，其余 9 个平台的 hook 被降级成"写在规则文档里的政策文字"。你在用的 **OpenClaw 是最浅的一档**——45 个 skill（Claude 是 280）、无 hooks、无专属内容（`.openclaw/` 里只有一个 21 行 README）、不在官方合规矩阵里，而且 ECC 官方文档把 OpenClaw 定位成"要迁移**出来**的源系统"和一篇安全批判长文的对象。

## 1. 平台目录全景：仓库根到底有多少个 harness 目录

### 1.1 实测文件数（`find <dir> -type f | wc -l`，HEAD=e4e4163）

| 根目录 | 文件数 | 性质（一句话） |
|---|---:|---|
| `.agents/` | 89 | 跨平台通用规范目录（AGENTS.md 生态），只放 skills + 一个 marketplace.json |
| `.claude/` | 14 | ECC 仓库**自己**给 Claude Code 用的项目级配置（不是分发物） |
| `.claude-plugin/` | 4 | Claude Code 插件清单（marketplace.json / plugin.json） |
| `.codebuddy/` | 6 | 只有 install/uninstall 脚本 + README，**无内容资产** |
| `.codex/` | 5 | config.toml + AGENTS.md + 3 个 agent 的 .toml |
| `.codex-plugin/` | 2 | Codex 插件清单 plugin.json + README |
| `.cursor/` | 69 | hooks.json + **17 个 hook JS**（含 1 个 adapter.js）+ **39 个 `.mdc` rules** + 11 个 skill 目录 |
| `.gemini/` | 1 | 只有 GEMINI.md |
| `.hermes/` | 1 | 只有 README.md（占位） |
| `.kimi/` | 1 | 只有 README.md（占位） |
| `.kiro/` | 153 | install.sh + settings + 大量 agents（.md + .json 双份） |
| `.openclaw/` | 1 | **只有 README.md（占位）** |
| `.opencode/` | 81 | 完整 TS 包：package.json / index.ts / tools / plugins / commands |
| `.qwen/` | 1 | 只有 QWEN.md |
| `.trae/` | 4 | install/uninstall 脚本 + 双语 README，**无内容资产** |
| `.zed/` | 1 | 只有 settings.json |

对照：仓库根的"资产源"目录规模是 `skills/` 452 个文件、`rules/` 122、`commands/` 94、`agents/` 67、`hooks/` 4。也就是说**内容只有一份，放在仓库根**，各 `.<platform>/` 目录里放的是"这个平台需要的特殊形态"（清单、hook 适配层、配置文件），不是内容副本。

证据：`find` 计数见上；根资产目录列表见 `CLAUDE.md:31-42`（Architecture 一节）。

### 1.2 三类平台目录，别混

读代码时会发现这 16 个目录不是同一种东西，混着看会得出错误结论：

1. **安装目标模板（会被 installer 拷到用户 home/项目里）**：`.cursor/`、`.opencode/`、`.codex/`、`.zed/`、`.gemini/`、`.qwen/`、`.hermes/`、`.openclaw/`、`.kimi/`、`.codebuddy/`
2. **插件清单（给平台的插件市场读的）**：`.claude-plugin/`、`.codex-plugin/`
3. **仓库自用配置（ECC 开发自己用，不分发）**：`.claude/`、`.github/`、`.vscode/`

`.kiro/`、`.trae/`、`.codebuddy/` 又是第四类：**自带独立 install.sh**，不走主 installer 的 adapter 体系（`.kiro/install.sh`、`.trae/install.sh`、`.codebuddy/install.sh`、`.codebuddy/install.js`）。

---

## 2. 投射机制本体：installer 怎么把同一套资产铺到不同平台

### 2.1 三层结构：manifest（内容清单）→ adapter（落点翻译）→ executor（真写盘）

ECC 的跨平台适配不是"每个平台一份内容"，而是**一份内容 + 每个平台一个"落点翻译器"**。整条链是：

```
manifests/install-modules.json   ← 内容清单：模块 = 一组源路径 + 支持哪些 target
        ↓  scripts/lib/install-manifests.js（解析、按 target 过滤模块）
scripts/lib/install-targets/*.js  ← adapter：把源路径翻译成"这个平台该放哪儿"
        ↓  scripts/lib/install-executor.js（把目录展开成逐文件操作）
scripts/lib/install/apply.js      ← executor：fs.copyFileSync 真写盘
```

- 入口：`install.sh:32` 只做一件事——`exec node scripts/install-apply.js "$@"`。真正逻辑全在 Node 里，install.sh 只是 shim（`install.sh:2` 自称 "Legacy shell entrypoint"）。
- 支持的 target 清单硬编码在 `scripts/lib/install-manifests.js:7`：
  `['claude', 'claude-project', 'cursor', 'antigravity', 'codex', 'gemini', 'opencode', 'codebuddy', 'joycode', 'qwen', 'zed', 'hermes', 'openclaw', 'kimi']` —— 14 个。
- adapter 注册表在 `scripts/lib/install-targets/registry.js:16-31`，同样 14 个（`claude-home` / `claude-project` 各算一个）。有测试守着两边一致：`tests/lib/install-targets.test.js:852-861`。

🔑 注意：`.kiro/`、`.trae/`、`.codebuddy/`（的 shell 脚本部分）不在这条链里。`.kiro/install.sh`、`.trae/install.sh` 是各自独立的安装脚本，而 `codebuddy` 同时既有 adapter（`scripts/lib/install-targets/codebuddy-project.js`）又有自己的 `.codebuddy/install.sh`。`.kiro`、`.trae` 完全不在 `SUPPORTED_INSTALL_TARGETS` 里。

### 2.2 "模块"是什么：一个模块 = 一组源路径 + 一张 target 白名单

`manifests/install-modules.json` 里每个模块长这样（模块 = 一组功能相关的文件的打包单位）。用 `node scripts/install-plan.js --list-modules` 能看到 25 个非 skill 模块 + 一大堆 `skill-<name>` 合成模块。举两个真实的：

```
- hooks-runtime [hooks]
  targets=claude, claude-project, cursor, opencode, codebuddy   ← 只有这 5 个平台
  Runtime hook configs and hook script helpers.

- rules-core [rules]
  targets=claude, claude-project, cursor, antigravity, codebuddy, joycode, qwen, zed, hermes, openclaw, kimi
                                                        ↑ 注意没有 codex、没有 gemini、没有 opencode
```

（输出来自 `node scripts/install-plan.js --list-modules`；定义在 `manifests/install-modules.json`）

**能力损失就发生在这张 targets 白名单上**：一个平台不在某模块的 targets 里，那个模块就整块被 skip。运行 `--profile full --target <t>` 会打出 `Skipped for target <t>` 段落（`scripts/install-plan.js:171-177`）。

### 2.3 adapter 干的三件事

每个 adapter 由 `createInstallTargetAdapter(config)` 造出来（`scripts/lib/install-targets/helpers.js:255-351`），配置只有 4-5 个字段：

```js
// scripts/lib/install-targets/openclaw-home.js:3-10  —— 整个文件就 10 行
createInstallTargetAdapter({
  id: 'openclaw-home',
  target: 'openclaw',
  kind: 'home',                       // home = 装到 ~/ ；project = 装到当前项目根
  rootSegments: ['.openclaw'],        // → ~/.openclaw
  installStatePathSegments: ['ecc-install-state.json'],
  nativeRootRelativePath: '.openclaw', // 仓库里 .openclaw/ 的内容 → 直接铺到目标根
});
```

adapter 负责：
1. **算根目录**：`resolveRoot()`（helpers.js:264-267）。`kind:'home'` → `~/.openclaw`；`kind:'project'` → `<cwd>/.cursor`。
2. **翻译落点**：`resolveDestinationPath()`（helpers.js:272-284）。默认是"原样保留相对路径"，即 `skills/foo` → `<root>/skills/foo`。
3. **过滤外平台路径**：`isForeignPlatformPath()`（helpers.js:27-37 + 常量表 5-18）。这一步很关键——模块的 paths 里可能同时列了 `.cursor`、`.opencode`、`.openclaw` 等多个平台目录，adapter 只保留属于自己的那个，其余直接丢弃。所以给 openclaw 装的时候，`.cursor/` 和 `.opencode/` 不会被误拷过去。

### 2.4 五种落点策略（strategy）

在 plan 输出里每条操作末尾方括号里的就是策略：

| strategy | 含义 | 出处 |
|---|---|---|
| `preserve-relative-path` | 原样保留目录结构（默认） | helpers.js:295 |
| `sync-root-children` | 源目录的**子项**直接铺到目标根（不多套一层同名目录） | helpers.js:288-293 |
| `flatten-copy` | 把 `rules/python/hooks.md` 压成 `python-hooks.md` 平铺 | helpers.js:168-175 |
| `merge-json` | 读目标已有 JSON，深合并后写回（不覆盖用户配置） | cursor-project.js:38-54 + apply.js:191-208 |
| （remap） | 换个落点，如 claude 把 `rules/` 塞进 `rules/ecc/` | claude-home.js:12-46 |

**flatten-copy 存在的原因**：Cursor / Zed / Antigravity / CodeBuddy / JoyCode 的 rules 目录是**平的**，不支持子目录。ECC 仓库里 `rules/` 是按语言分子目录的（`rules/python/`、`rules/golang/`…），所以必须压平并加前缀防重名。看 `helpers.js:216`：`` `${namespace}-${relativeFile.replace(/\//g,'-')}` ``。

### 2.5 平台专属改名：Cursor 的 .md → .mdc

Cursor 的 rules 文件必须是 `.mdc` 扩展名，所以 cursor adapter 挂了一个改名函数：

```js
// scripts/lib/install-targets/cursor-project.js:13-21
function toCursorRuleFileName(fileName, sourceRelativeFile) {
  if (path.basename(sourceRelativeFile).toLowerCase() === 'readme.md') return null;  // README 不装
  return fileName.endsWith('.md') ? `${fileName.slice(0, -3)}.mdc` : fileName;
}
```

agent 文件也有专门的改名器：`scripts/lib/cursor-agent-names.js`（被 cursor-project.js:4 引用）。

还有一条 Cursor 专属的**否决规则**——`AGENTS.md` 不装进 `.cursor/`：

```js
// scripts/lib/install-targets/cursor-project.js:138-142
if (sourceRelativePath === 'AGENTS.md') {
  // Cursor treats nested AGENTS.md files as directory context; do not
  // install ECC's root project identity into a host project's .cursor/.
  return [];
}
```

这是一个真实的"平台语义差异导致必须特判"的例子：同一个文件在 Claude Code 里是身份说明，在 Cursor 里会被当成"这个目录的上下文"，装进去反而污染宿主项目。

### 2.6 Antigravity：唯一一个做"概念改名"的 adapter

大多数 adapter 只改路径不改概念，但 antigravity 把 ECC 的概念映射到了它自己的词汇表：

```js
// scripts/lib/install-targets/antigravity-project.js:23, 59-79
rootSegments: ['.agent'],           // 注意根目录叫 .agent 不是 .antigravity
commands → <root>/workflows         // ECC 的"命令"= Antigravity 的"工作流"
agents   → <root>/skills            // ECC 的"agent" = Antigravity 的"skill"
rules    → <root>/rules（压平）
```

而且它有白名单，只接受 5 类源路径（`antigravity-project.js:10`）：`rules / commands / agents / skills / .agents / AGENTS.md`，其他一律不装。

### 2.7 Claude 的 rules 命名空间

Claude 是唯一把 rules 装进子命名空间的：

```js
// scripts/lib/install-targets/claude-home.js:10, 16-18
const CLAUDE_ECC_NAMESPACE = 'ecc';
if (normalizedSourcePath === 'rules') return path.join(targetRoot, 'rules', CLAUDE_ECC_NAMESPACE);
```

→ `~/.claude/rules/ecc/...`。目的是不跟用户自己的 `~/.claude/rules/` 打架。因为落点变了，markdown 里的相对链接会断，所以 apply 阶段专门做了链接重写（`scripts/lib/install/apply.js:218-233`，实现在 `scripts/lib/install/link-rewrite.js`）。

### 2.8 实测：同一条 `--profile full` 命令在各 target 上的规模差异

命令：`node scripts/install-plan.js --profile full --target <t> --json`（只读，不写盘）

| target | 选中模块 | 跳过模块 | 落点根 | 文件操作数 |
|---|---:|---:|---|---:|
| claude | 25 | 0 | `~/.claude` | 301 |
| claude-project | 25 | 0 | `<项目>/.claude` | 301 |
| cursor | 24 | 1 | `<项目>/.cursor` | 482 |
| codebuddy | 24 | 1 | `<项目>/.codebuddy` | 415 |
| zed | 23 | 2 | `<项目>/.zed` | 413 |
| joycode | 23 | 2 | `<项目>/.joycode` | 412 |
| qwen | 23 | 2 | `~/.qwen` | 292 |
| antigravity | 22 | 3 | `<项目>/.agent` | 397 |
| codex | 19 | 6 | `~/.codex` | 215 |
| **openclaw** | **7** | **18** | `~/.openclaw` | **56** |
| **hermes** | **7** | **18** | `~/.hermes` | **56** |
| **kimi** | **7** | **18** | `<项目>/.kimi` | **56** |
| gemini | 3 | 22 | `<项目>/.gemini` | 6 |
| opencode | — | — | `~/.opencode` | 报错（需先 build，见 §8） |

🔑 这张表是理解"能力损失"的核心：openclaw / hermes / kimi 三家共享同一档（7 模块 56 文件），gemini 是最低档（3 模块 6 文件）。cursor 的文件数比 claude 还多，是因为 rules 压平后一个文件变一个操作，而 claude 是整目录保留。

⚠️ 注意 `scripts/install-plan.js:153` 自己打的免责声明：
> "target filtering and operation output currently reflect scaffold-level adapter planning, not a byte-for-byte mirror of legacy install.sh copy paths."
所以上表是"计划层"的数字，真实 `install-apply.js` 跑出来的文件数可能略有出入（apply 阶段还会加 hooks 解析、MCP 合并等操作）。置信度：模块选中/跳过数**高**（同一份 manifest），操作数**中 ~60%**。

---

## 3. 逐平台能力矩阵

数据来源：`manifests/install-modules.json` 里每个模块的 `targets` 字段 + `node scripts/install-plan.js --profile full --target <t> --json` 的实际落点。全部实测，非 README 转述。

### 3.1 五类能力的"通行证"（模块 targets 白名单）

| 模块（能力） | 源路径 | 允许的 target |
|---|---|---|
| `rules-core` | `rules/` | claude, claude-project, cursor, antigravity, codebuddy, joycode, qwen, zed, hermes, openclaw, kimi （**11 个；缺 codex / gemini / opencode**） |
| `agents-core` | `.agents/`, `agents/`, `AGENTS.md` | 上面 11 个里去掉 gemini，加上 codex（**12 个；缺 gemini / opencode**） |
| `commands-core` | `commands/`, `scripts/harness-audit.js`, `scripts/skills-health.js` | claude, claude-project, cursor, antigravity, opencode, codebuddy, joycode, qwen, zed, hermes, openclaw, kimi（**12 个；缺 codex / gemini**） |
| `hooks-runtime` | `hooks/`, `scripts/hooks/`, `scripts/lib/` | **只有 5 个**：claude, claude-project, cursor, opencode, codebuddy |
| `platform-configs` | 各 `.<platform>/` + `mcp-configs/` + 2 个脚本 | **全部 14 个** |
| `workflow-quality`（43 个核心 skill） | `skills/<43 个>` | 除 gemini 外全部 13 个 |
| `skill-unified-memory` / `ito-compute` | 单个 skill | **全部 14 个** |
| `framework-language` / `database` / `security` / … （大类 skill 包） | `skills/…` | claude, claude-project, cursor, antigravity, codex, opencode, codebuddy, joycode, qwen, zed（**10 个；缺 gemini / hermes / openclaw / kimi**） |
| `orchestration` | tmux/worktree 脚本 + dmux skill | **只有 4 个**：claude, claude-project, codex, opencode |
| `docs-*`（9 种语言译文） | `docs/<locale>/` | **只有 2 个**：claude, claude-project |

证据：以上全部读自 `manifests/install-modules.json`（用 `node -e` 直接解析出来的 targets 数组）；`docs-*` 与 `--locale` 的限制另见 `scripts/lib/install/request.js:103-105`——`--locale` 只允许配 claude / claude-project。

### 3.2 主矩阵：每个平台支持哪几项 + 装到哪

`✅` = 装；`—` = 该平台被模块 targets 排除；括号里是落点（`~` = 用户 home，`<proj>` = 当前项目根）。

| 平台 target | 落点根 | skills | agents | commands | hooks | rules | MCP | 备注 |
|---|---|---|---|---|---|---|---|---|
| `claude` | `~/.claude` | ✅ 280 个目录 → `skills/` | ✅ → `agents/` | ✅ → `commands/` | ✅ → `hooks/` | ✅ → `rules/**ecc**/`（加命名空间） | ✅ `mcp-configs/` | 唯一全量；另有 `.claude-plugin/` 铺到根 |
| `claude-project` | `<proj>/.claude` | ✅ 280 | ✅ | ✅ | ✅ | ✅ `rules/ecc/` | ✅ | 与上面同构，只是装项目里 |
| `cursor` | `<proj>/.cursor` | ✅ 279 | ✅ **67 个文件平铺**改名 | ✅ | ✅ **10 项**（自带 18 个 hook JS） | ✅ **82 个 `.mdc` 平铺** | ✅ 合并进 `.cursor/mcp.json` | `AGENTS.md` 被显式拒装 |
| `codebuddy` | `<proj>/.codebuddy` | ✅ 279 | ✅ → `agents/` | ✅ | ✅ 2 项 | ✅ **122 个平铺 .md** | ✅ | 唯一"非 Claude/Cursor/OpenCode 但有 hooks"的平台 |
| `zed` | `<proj>/.zed` | ✅ 279 | ✅ | ✅ | **—** | ✅ 122 平铺 | ✅ | `.zed/settings.json` 铺到根 |
| `joycode` | `<proj>/.joycode` | ✅ 279 | ✅ | ✅ | **—** | ✅ 122 平铺 | ✅ | 仓库里**没有** `.joycode/` 目录（纯 adapter，无模板） |
| `qwen` | `~/.qwen` | ✅ 279 | ✅ | ✅ | **—** | ✅ 整目录（不平铺） | ✅ | `.qwen/QWEN.md` 铺到根 |
| `antigravity` | `<proj>/**.agent**` | ✅ 271 | ✅ 但改名 → `skills/` | ✅ 但改名 → `workflows/` | **—** | ✅ 122 平铺 | **—** | 概念改名；只收 5 类源路径 |
| `codex` | `~/.codex` | ✅ 206 | ✅ | **—** | **—** | **—** | ✅ | 靠 `AGENTS.md` 做规则载体 |
| `hermes` | `~/.hermes` | ⚠️ **45** | ✅ | ✅ | **—** | ✅ 整目录 | ✅ | 仓库里只有 README.md |
| **`openclaw`** | `~/.openclaw` | ⚠️ **45** | ✅ | ✅ | **—** | ✅ 整目录 | ✅ | 仓库里只有 README.md（详见 §9） |
| `kimi` | `<proj>/.kimi` | ⚠️ **45** | ✅ | ✅ | **—** | ✅ 整目录 | ✅ | 仓库里只有 README.md |
| `gemini` | `<proj>/.gemini` | ⚠️ **2** | **—** | **—** | **—** | **—** | ✅ | 最低档：只有 unified-memory + ito-compute |
| `opencode` | `~/.opencode` | ✅（走单独构建） | **—** | ✅ | ✅（TS 插件） | **—** | ✅ | 必须先 `build:opencode`，见 §8 |

数字来源：上面每格的计数是 `--profile full` 下 plan 的去重落点数（skills 那列是 skill 目录数；rules 那列 122/82 是平铺后的文件数，1 表示整目录一次性拷）。

⚠️ 一个反直觉点：**`joycode` 在 adapter 和 manifest 里都是一等公民（23 个模块、412 个操作），但仓库根本没有 `.joycode/` 目录**。而 `.kiro/`（153 文件）、`.trae/`（4 文件）有目录却**不在** `SUPPORTED_INSTALL_TARGETS` 里（`scripts/lib/install-manifests.js:7`）。目录存在 ≠ 被主 installer 支持，反之亦然。

### 3.3 各平台"专属模板目录"里到底放了什么

| 目录 | 内容实质 | 是模板还是占位 |
|---|---|---|
| `.cursor/` | `hooks.json` + 18 个 hook JS（`adapter.js` 是事件翻译层）+ 46 个 `.mdc` rules | **真模板**，有可执行代码 |
| `.opencode/` | 完整 TypeScript 包：`package.json` / `index.ts` / 9 个 tool / `plugins/ecc-hooks.ts` / 20+ commands | **真模板**，需编译 |
| `.codex/` | `config.toml` + `AGENTS.md` + 3 个 agent `.toml` | 真模板（小） |
| `.zed/` | 只有 `settings.json` | 配置片段 |
| `.gemini/` | 只有 `GEMINI.md` | 指令文件 |
| `.qwen/` | 只有 `QWEN.md` | 指令文件 |
| `.kimi/` | 只有 `README.md` | **占位** |
| `.hermes/` | 只有 `README.md` | **占位** |
| `.openclaw/` | 只有 `README.md` | **占位**（§9 详述） |
| `.kiro/` | 自带 `install.sh` + 30 多个 agent 的 `.md`+`.json` 双份 | 独立生态，不走主 installer |
| `.trae/` | `install.sh` / `uninstall.sh` / 双语 README | 独立脚本，无内容 |
| `.codebuddy/` | `install.sh` / `install.js` / `uninstall.*` / 双语 README | 独立脚本 + 主 installer adapter 双轨 |

---

## 4. 能力损失点：谁不支持 hooks / skills / commands

### 4.1 hooks 是最大的断层：14 个 target 里只有 5 个有

**hooks** = harness 在特定时刻（工具执行前后、会话开始结束、提交 prompt 前）自动跑的一段代码，是 ECC 用来做强制约束（拦命令、自动格式化、审计日志）的手段。

支持名单（`manifests/install-modules.json` 的 `hooks-runtime.targets`）：
```
claude, claude-project, cursor, opencode, codebuddy
```
**不支持的 9 个**：codex, gemini, zed, qwen, joycode, antigravity, hermes, **openclaw**, kimi。

ECC 官方对这件事的定性写在合规记录里，说得很直白：
> Codex：`unsupported_surfaces: ['Native hook enforcement and Claude slash-command semantics are not equivalent']`，`risk_notes: ['Treat hooks as policy text unless a native Codex hook surface exists.']`
> —— `scripts/lib/harness-adapter-compliance.js:85, 91`

也就是说，对不支持 hooks 的平台，ECC 的策略是**把 hook 降级成"写在规则文档里的政策文字"**，靠模型自觉遵守，没有运行时强制。这是本质性的能力损失，不是配置差异。

### 4.2 支持 hooks 的三家，实现方式完全不同

| 平台 | 事件模型 | ECC 怎么接 |
|---|---|---|
| Claude Code | `hooks/hooks.json` 里按 matcher 声明 | 安装时把 `${PLUGIN_ROOT}` 占位符替换成真实路径再写出（`scripts/lib/install/apply.js:120-145`）。这一步**只对 claude/claude-project 生效**（apply.js:121 显式判断） |
| Cursor | `.cursor/hooks.json`，15 种事件（sessionStart / beforeShellExecution / beforeSubmitPrompt / beforeTabFileRead / preCompact …） | **16 个 hook JS + 1 个事件翻译层** `.cursor/hooks/adapter.js`：把 Cursor 的 stdin JSON 转成 Claude Code 的 hook 输入格式，再 `execFileSync` 调用共享的 `scripts/hooks/*.js`（adapter.js:1-6, 28-61）。共享脚本池在 `scripts/hooks/`，50 个文件 |
| OpenCode | 插件系统，20+ 事件 | `.opencode/plugins/ecc-hooks.ts` 里写死映射：`PreToolUse → tool.execute.before`、`PostToolUse → tool.execute.after`、`Stop → session.idle`、`SessionStart → session.created`、`SessionEnd → session.deleted`（ecc-hooks.ts:8-14） |

🔑 **这就是 ECC 跨 harness 的核心设计原则**，也写在合规记录里：
> "Keep hook logic in shared scripts and adapt only event shape at the edge."
> —— `scripts/lib/harness-adapter-compliance.js:116`

翻成人话：业务逻辑（比如"检测到 `git commit --no-verify` 就拦下来"）只写一份放在 `scripts/hooks/`，每个平台只写一个几十行的"事件格式转换器"。`.cursor/hooks/adapter.js` 的 `transformToClaude()`（adapter.js:28-47）就是那个转换器——把 Cursor 给的 `{command, path, output}` 拼成 Claude 格式的 `{tool_input:{command,file_path}, tool_output:{output}}`。

另外 adapter.js 还实现了统一的 hook 开关（adapter.js:63-79）：环境变量 `ECC_HOOK_PROFILE`（minimal/standard/strict）和 `ECC_DISABLED_HOOKS`（逗号分隔的黑名单）。同一套开关语义在 Cursor 和 OpenCode 两边都有实现。

### 4.3 其余能力损失一览

| 损失项 | 谁没有 | 证据 |
|---|---|---|
| **rules**（always-follow 规则文件） | codex, gemini, opencode | `rules-core.targets` |
| **commands**（slash 命令） | codex, gemini | `commands-core.targets` |
| **agents**（子 agent 定义） | gemini, opencode | `agents-core.targets` |
| **大类 skill 包**（framework-language / database / security / research-apis / devops / ML / swift / 供应链 / 文档处理 等 15 个包） | gemini, **hermes, openclaw, kimi** | 各模块 targets 均为 10 个平台的固定名单 |
| **orchestration**（tmux/worktree 并行编排） | 除 claude / claude-project / codex / opencode 外全部 | `orchestration.targets` |
| **多语言文档** | 除 claude / claude-project 外全部 | `docs-*.targets`；`scripts/lib/install/request.js:103-105` 抛错 |
| **MCP 配置** | antigravity | 落点数据里没有 `mcp-configs` |

### 4.4 用"合规状态"看的另一种分层

`scripts/lib/harness-adapter-compliance.js:9-14` 定义了 4 个状态，这是 ECC 自己给平台打的分级：

| 状态 | 官方定义（原文直译） | 谁在这档 |
|---|---|---|
| **Native** | ECC 能直接安装或验证这个界面 | Claude Code、Terminal-only |
| **Adapter-backed** | 有一层薄适配器/插件/包，但各家 parity 不同 | OpenCode、Cursor、Zed、dmux |
| **Instruction-backed** | ECC 能给出指导和文件，但**这个 harness 没有暴露 ECC 需要的运行时 hook/session 界面来做强制** | Codex、Gemini |
| **Reference-only** | 作为设计压力有用，但 ECC 还没给它做安装器或适配器 | Orca、Superset、Ghast |

⚠️ 注意这份合规记录只覆盖 **11 条**（`ADAPTER_RECORDS`，compliance.js:42-308），而 installer 支持 14 个 target。**openclaw、hermes、kimi、qwen、joycode、antigravity、codebuddy 都不在合规记录里**——它们有 adapter 但没有被正式定级和验证。这是我认为最值得注意的一个缺口（置信度 高，直接数记录条目即可复核：`node scripts/harness-adapter-compliance.js --format json | grep '"id"'`）。

---

## 5. `.agents/` 是什么：AGENTS.md 标准与 ECC 的用法

### 5.1 结论先说：`.agents/` 不是"跨平台通用规范"，它是 OpenAI Codex 的 skill 发现目录

这里有个极易混淆的点，必须分清两个东西：

| 名字 | 是什么 | 在 ECC 里 |
|---|---|---|
| **`AGENTS.md`**（一个文件） | 业界的跨工具开放约定（agents.md）：项目根放一个 markdown，各家 agent 工具都读它。README 自己也这么说："**AGENTS.md at root is the universal cross-tool file (read by Claude Code, Cursor, Codex, and OpenCode; GitHub Copilot uses `.github/copilot-instructions.md` instead)**"（README.md:1430） | 根 `AGENTS.md`（8.6 KB）+ `.codex/AGENTS.md`（Codex 专属补充）共 2 份，README.md:1550 |
| **`.agents/`**（一个目录） | **Codex 的 skill 自动发现目录**，跟 agents.md 标准没关系 | 39 个 skill 目录，每个 = `SKILL.md` + `agents/openai.yaml` |

证据链（三处互相印证）：
- `.codex/AGENTS.md:18-21`："Skills are auto-loaded from `.agents/skills/`. Each skill contains: `SKILL.md` — Detailed instructions and workflow; `agents/openai.yaml` — **Codex interface metadata**"
- `README.md:1551`：`| Skills | 32 | `.agents/skills/`: SKILL.md + agents/openai.yaml per skill |`；`README.md:1556`："Skills at `.agents/skills/` are **auto-loaded by Codex**"
- 校验测试文件名就叫 `tests/ci/codex-skill-surface.test.js`，头两行注释：`Validate the Codex-facing .agents/skills surface.`（该文件 :1-4, :11）

（README.md:1551 写 32 个，实测 39 个目录——文档滞后。复核命令：`ls /Users/aa00158/harness-research/ECC/.agents/skills | wc -l`）

### 5.2 `agents/openai.yaml` 长什么样

```yaml
# .agents/skills/security-review/agents/openai.yaml —— 全文 7 行
interface:
  display_name: "Security Review"
  short_description: "Security checklist and vulnerability review"
  brand_color: "#EF4444"
  default_prompt: "Use $security-review to review sensitive code with the security checklist."
policy:
  allow_implicit_invocation: true
```

这是 Codex 的界面元数据（显示名、卡片颜色、默认提示语、是否允许隐式调用）。ECC 有 CI 强校验（`tests/ci/codex-skill-surface.test.js:94-116`）：
- 每个 skill 目录必须有 `agents/openai.yaml`
- `short_description` 必须 **25-64 字符**
- `default_prompt` 必须包含 `$<skill 名>`
- `SKILL.md` 的 frontmatter 只允许 5 个 key：`allowed-tools / description / license / metadata / name`（test:12-18），且 `name` 必须等于目录名

### 5.3 一个真实问题：`.agents/skills/` 是 `skills/` 的**手工副本**，已经漂移

我直接 diff 了同名 skill：

```
diff .agents/skills/security-review/SKILL.md skills/security-review/SKILL.md
```
结果有实质差异（不只是格式）：
- `skills/` 版本多了 `metadata: origin: ECC` frontmatter
- `skills/` 版本修了一处 bug：`error.errors` → `error.issues`
- `skills/` 版本的 CSP 示例更严格（加了 `base-uri 'self'` / `object-src 'none'` / `frame-ancestors 'none'`，并删掉了 `unsafe-eval`/`unsafe-inline`），还多了一段"Start strict and loosen only with a documented removal plan"的说明

也就是说 `.agents/` 版本是**旧的、安全性更差的**那份。这跟 `docs/architecture/cross-harness.md:149` 自己立的规矩直接冲突：
> "If a change requires editing three harness copies of the same workflow, the shared source is in the wrong place."

同样的问题也存在于 `.cursor/skills/`（11 个目录，`docs/architecture/cross-harness.md:99-101` 称 Codex 和 Cursor 收到的是 "behavior-identical packaging copies"——实测不 identical）。

🔑 这是我在这个维度找到的**最实质的架构缺陷**：三份 skill 副本（`skills/` 452 文件 / `.agents/skills/` 89 文件 / `.cursor/skills/` 11 目录）靠人工同步，没有生成脚本。置信度 高——diff 结果可直接复现。

### 5.4 `.agents/` 会被拷到几乎所有平台（可能是无意的）

`agents-core` 模块的 paths 是 `['.agents', 'agents', 'AGENTS.md']`（`manifests/install-modules.json`），targets 有 12 个。而 `isForeignPlatformPath()` 的所有者表（`scripts/lib/install-targets/helpers.js:5-18`）里**没有 `.agents` 这一项**——所以它不会被当成"别家的平台目录"过滤掉。

实测结果：给 openclaw / kimi / hermes / qwen / zed / cursor / claude 装的时候，都会多出一条操作：
```
agents-core | .agents -> ~/.openclaw/.agents        [preserve-relative-path]
agents-core | .agents -> ~/.claude/.agents          [preserve-relative-path]
agents-core | .agents -> <proj>/.cursor/.agents     [preserve-relative-path]
```
（来自 `node scripts/install-plan.js --profile full --target <t> --json`）

一个 Codex 专属的目录被复制进了 11 个非 Codex 平台的根目录里。可能是刻意的"通用副本"策略，也可能是漏了过滤。**未验证**是否有意为之——仓库里没找到说明。要判定的话可以看 `.agents` 是否被有意排除在 `PLATFORM_SOURCE_PATH_OWNERS` 之外（helpers.js:5-18 里 12 个平台目录都列了，唯独没 `.agents`）。

### 5.5 `.agents/plugins/marketplace.json`

`.agents/` 下还有一个 4 行的 marketplace 清单，声明一个名为 `ecc` 的本地插件（`.agents/plugins/marketplace.json`），`source.path: "./plugins/ecc"`——但仓库里 `.agents/plugins/ecc` 目录**不存在**（`find .agents -type f` 只列出 marketplace.json 一个文件）。这是个悬空引用。对应 README 里说的 "experimental ECC marketplace"（README.md:1406）。

---

## 6. 同步机制：拷贝 vs 软链 vs 生成

### 6.1 结论：全是**拷贝**，零软链

主 installer 的写盘只有一行：

```js
// scripts/lib/install/apply.js:235
fs.copyFileSync(operation.sourcePath, operation.destinationPath);
```

验证：
- 全仓库（排除 node_modules/.git）`find . -type l` → **0 个软链**
- `scripts/`、`.cursor/`、`.opencode/`、`install.sh`、`install.ps1` 里 grep `symlinkSync|ln -s` → **0 个命中**

所以"改仓库里的 skill，装出去的副本自动更新"这件事在 ECC 里**不成立**——必须重跑 installer。这也是 `scripts/repair.js` 和 `npx ecc doctor` 存在的原因：靠 `ecc-install-state.json` 记录装了哪些文件，事后比对漂移。

### 6.2 但拷贝之外有 4 种"非纯拷贝"路径

| 机制 | 触发条件 | 代码位置 |
|---|---|---|
| **JSON 深合并** | 目标是 MCP 配置或 Cursor 的 `hooks.json` | `apply.js:191-208`（`deepMergeJson(现有内容, 新载荷)`）。目的：不覆盖用户已有配置 |
| **MCP 服务器过滤** | 环境变量 `ECC_DISABLED_MCPS` 有值 | `apply.js:171, 197-216`——安装时就把黑名单里的 MCP server 剔除再写 |
| **Markdown 链接重写** | 落点路径变了（如 claude 的 `rules/ecc/`）且文件是 `.md` | `apply.js:218-233` + `scripts/lib/install/link-rewrite.js`。只改指向"被安装文件"的相对链接，其余原样 |
| **hooks.json 占位符解析** | 只有 claude / claude-project | `apply.js:120-145`——把 `${PLUGIN_ROOT}` 替换成真实安装根后**生成**一份新 hooks.json，而不是拷贝原文件 |

### 6.3 还有第三条路：Codex 的 sync 脚本是**生成**而非拷贝

`scripts/sync-ecc-to-codex.sh`（README.md:207 推荐给 Codex 用户的"the reliable ECC setup"）不走 adapter 体系，它做的是转换生成：

- 从 `commands/*.md` **生成** Codex 的 prompt 文件（`sync-ecc-to-codex.sh:141-148, 303-322`）
- 额外**生成** 5 个"rules pack" prompt（common / typescript / python / golang / swift，:425-497）和 3 个 tool prompt（run-tests / check-coverage / security-audit，:351-399）
- `AGENTS.md` 用 **marker 标记合并**进用户已有的 `~/.codex/AGENTS.md`，保留用户内容（:6）
- MCP 服务器"add-only"合并进 `config.toml`（:11，用 Node TOML parser）
- 装全局 git hook（pre-commit / pre-push）作为 Codex 没有原生 hook 的替代（:9）—— 这是"hook 能力损失的补偿手段"的具体实现
- 每次先做**带时间戳的备份**到 `~/.codex/backups/ecc-<时间戳>/`（:49-50）

🔑 所以 Codex 有**两条并行的安装路径**：`install.sh --target codex`（走 adapter，只铺 skills/agents/AGENTS.md）和 `sync-ecc-to-codex.sh`（走生成，产出 prompts + git hooks + config 合并）。README.md:207 明确说后者才是"reliable"的那条。

### 6.4 第四条路：三个平台的独立 install.sh

`.kiro/install.sh`、`.trae/install.sh`、`.codebuddy/install.sh`(+`.codebuddy/install.js`) 各自独立。以 `.kiro/install.sh` 为例（:19-26）：脚本住在 `.kiro/` 里，把自己所在目录当源，拷到 `<target>/.kiro/`，支持 `./install.sh ~` 装到 `~/.kiro/`。完全不碰 manifest、adapter、install-state。

### 6.5 一个防覆盖的细节：dedupeCopyFileOperations

同一个落点被多条操作写时（比如通用 `commands/foo.md` 和 OpenCode 覆盖版 `.opencode/commands/foo.md` 都要写到同一处），ECC 只保留**最后一条**：

```js
// scripts/lib/install-executor.js:696-719（注释原文）
// only the last one actually determines the installed content. Recording the
// shadowed earlier writes in install-state makes `doctor` report perpetual
// drift and drives `repair` to clobber the override with the generic source (issue #2414).
```

这是一个真实 bug（issue #2414）的修复：如果把被遮蔽的写入也记进 install-state，`doctor` 会永远报漂移，`repair` 会拿通用版覆盖掉平台覆盖版。**平台覆盖的优先级靠"操作顺序"实现，不靠显式优先级字段**——这是理解 ECC 覆盖语义的关键。

---

## 7. 合规校验器：harness-adapter-compliance.js / harness-audit.js

这两个脚本名字像，但干的事完全不同，别搞混。

### 7.1 `harness-adapter-compliance.js` = 文档不许骗人的守门员

它**不检测任何 harness 的实际能力**。它做的是"数据是唯一真相源，文档必须从数据生成"：

1. 平台支持信息硬编码在 `scripts/lib/harness-adapter-compliance.js:42-308` 的 `ADAPTER_RECORDS` 数组里（11 条，每条 11 个必填字段：`id / harness / state / supported_assets / unsupported_surfaces / install_or_onramp / verification_commands / risk_notes / last_verified_at / owner / source_docs`，见 :16-28）。
2. `validateAdapterRecords()`（:352-411）逐条校验：id 必须是小写 slug 且唯一、state 必须是 4 个合法值之一、6 个数组字段必须非空、`last_verified_at` 必须是 `YYYY-MM-DD`。
3. `validateDocumentation()`（:425-440）读 `docs/architecture/harness-adapter-compliance.md`，抠出 `<!-- harness-adapter-compliance:matrix-start -->` 和 `:matrix-end` 之间的内容，跟 `renderMarkdownTable()` 现场渲染的表格逐字符比对——**不一致就报错**。

实测：
```
$ node scripts/harness-adapter-compliance.js --format text
Harness Adapter Compliance: PASS
Adapters: 11
```

🔑 这是一个值得学的机制设计：**把"平台支持矩阵"这种最容易腐烂的文档，变成从代码生成 + CI 强制比对**。CI 命令是 `npm run harness:adapters -- --check`（`docs/architecture/harness-adapter-compliance.md:33`）。

⚠️ 但它有个盲区：`ADAPTER_RECORDS` 是**手写**的，跟 `SUPPORTED_INSTALL_TARGETS`（14 个）和 `registry.js` 的 adapter 列表（14 个）没有交叉校验。所以 openclaw / hermes / kimi / qwen / joycode / antigravity / codebuddy 这 7 个 target 有 installer 支持却没有合规记录，校验器也不会报错。相反，`terminal-only`、`dmux`、`orca`、`superset`、`ghast` 这 5 条记录里没有一个是 install target（dmux 是 session 适配器，后三个是 "Reference-only" 的竞品参考）。

### 7.2 `harness-audit.js` = 给一个仓库的"agent 工程成熟度"打分

跟跨平台无关，是个 rubric 评分器。12 个类别（`scripts/harness-audit.js:7-20`）：Tool Coverage / Context Efficiency / Quality Gates / Memory Persistence / Eval Coverage / Security Guardrails / Cost Efficiency + 5 个部署商集成（GitHub / Vercel / Netlify / Cloudflare / Fly）。

关键设计：**双模式自动识别**（`harness-audit.js:216-232`）——
- `repo` 模式：`package.json` 的 name 是 `everything-claude-code`，或同时存在 `scripts/harness-audit.js` + `.claude-plugin/plugin.json` + `agents/` + `skills/`
- `consumer` 模式：其他情况，改用一套 `consumer-*` 检查项（:829-933，如 `consumer-plugin-install` / `consumer-hook-guardrails` / `consumer-secret-hygiene`）

实测在 ECC 仓库自己跑：`target_mode: "repo"`, `overall_score: 80 / 80`（满分，因为规则就是照着自己写的）。部署商集成是**条件计分**——只有检测到 `vercel.json` / `netlify.toml` / `wrangler.toml` / `fly.toml` 才纳入（:24-58）。

`rubric_version: '2026-05-19'`（:22）——有版本号，说明评分标准本身是要演进的。

这个脚本会被装到 12 个平台（`commands-core` 模块 paths 里带 `scripts/harness-audit.js`），是所有平台合规记录里出现频率最高的 `verification_commands`。

---

## 8. build-opencode.js：为什么 opencode 需要单独构建

### 8.1 OpenCode 是唯一"要编译"的平台

`scripts/build-opencode.js` 只有 26 行，做三件事（:11, 16, 23）：
1. 删掉 `.opencode/dist`
2. 找到 root 依赖里的 TypeScript 编译器（`require.resolve('typescript/bin/tsc')`）
3. 用 `.opencode/tsconfig.json` 跑 `tsc -p`

原因：其他平台的资产都是 markdown / JSON，OpenCode 的插件是 **TypeScript 源码**（`.opencode/plugins/ecc-hooks.ts`、9 个 `.opencode/tools/*.ts`），必须编译成 JS 才能被 OpenCode 加载。

### 8.2 installer 会因此**硬拦**未构建的安装

opencode 是唯一一个自定义了 `validate()` 的 adapter（`scripts/lib/install-targets/opencode-home.js:36-80, 89`）。它检查三个产物：

```js
// opencode-home.js:11-15
{ '.opencode/dist/index.js',  type: 'file'      },
{ '.opencode/dist/plugins',   type: 'directory' },
{ '.opencode/dist/tools',     type: 'directory' },
```

缺任何一个就抛 `opencode-plugin-not-built` 错误。我实测触发了：
```
$ node scripts/install-plan.js --profile full --target opencode --json
Error: OpenCode install requires the compiled plugin payload under .opencode/dist,
but the following artefact(s) were missing or had the wrong type:
.opencode/dist/index.js, .opencode/dist/plugins, .opencode/dist/tools.
Run node scripts/build-opencode.js (or: npm run build:opencode) from the repo root
before re-running the installer.
```

代码里还有一段值得注意的严谨：`isExpectedType()`（:23-34）只把 `ENOENT`/`ENOTDIR` 当"文件不存在"，`EACCES`/`EIO` 这类真实系统错误会往上抛，不会被误报成"没构建"（注释在 :18-21）。

### 8.3 OpenCode 是一个能独立发布的 npm 包

`.opencode/package.json` 声明的包名是 **`ecc-universal`** v2.1.0（:2-3），`main: dist/index.js`，peerDependency 是 `@opencode-ai/plugin >= 1.0.0`（:59-61）。用户可以 `npm install ecc-universal` 然后在自己的 `opencode.json` 里写 `"plugin": ["ecc-universal"]`（`.opencode/index.ts:11-21`）。

⚠️ 注意 `ecc-universal` 这个包名在别处也出现——`skill-unified-memory` 和 `ito-compute` 两个模块的描述都写着 "requires the separately installed **ecc-universal CLI runtime**"（`node scripts/install-plan.js --list-modules`）。同名但可能不是同一个东西（一个是 OpenCode 插件包，一个被描述成 CLI runtime）。**未验证**是否是同一个 npm 包。

### 8.4 OpenCode 的"复用"策略跟别家不一样

看 `.opencode/opencode.json`：
- `"skills": { "paths": ["../skills"] }`（:25-29）—— **直接指向仓库根的 `skills/`**，不复制。这是唯一一个用"引用"而非"拷贝"来共享 skill 的平台（但只在 repo-local 使用时成立；装到 `~/.opencode` 后这个相对路径的含义会变）。
- `"instructions"` 数组（:6-21）把 11 个 SKILL.md 当成**常驻指令**直接加载，这跟 Claude Code 的"按需触发 skill"是不同的加载模型。
- `"agent"` 段（:30+）把 ECC 的 agent 表达成 OpenCode 的 primary/subagent 模型，还带 `tools` 白名单和 `prompt: "{file:prompts/agents/planner.txt}"` 引用。

这说明 OpenCode 的适配深度是最高的（有自己的 tool 实现、agent 定义、命令 35 个），但也意味着**它是漂移风险最大的一份**——agent 定义在 `agents/` 和 `.opencode/opencode.json` 里各有一套。

---

## 9. OpenClaw 支持深度专章

（你本地在用 OpenClaw，所以这节写详细一点。）

### 9.1 一句话结论

**OpenClaw 在 ECC 里是"三等公民"：有 installer target，但没有专属内容、没有 hooks、没有合规记录、拿不到大部分 skill 包，而且 ECC 官方文档里把 OpenClaw 定位成"要迁移出来的源系统"和"安全反面教材"。**

### 9.2 ECC 里所有跟 OpenClaw 有关的文件（全量清单，7 处）

| # | 路径 | 内容 | 行数/规模 |
|---|---|---|---|
| 1 | `.openclaw/README.md` | 目录里**唯一**的文件 | 21 行 |
| 2 | `scripts/lib/install-targets/openclaw-home.js` | adapter 定义 | **10 行**（模板化，无任何 OpenClaw 专属逻辑） |
| 3 | `manifests/install-modules.json` | 在 7 个模块的 targets 里出现 | — |
| 4 | `scripts/lib/install-manifests.js:7` | 在 `SUPPORTED_INSTALL_TARGETS` 里 | 一个字符串 |
| 5 | `skills/openclaw-persona-forge/` | 一个社区贡献的中文 skill（给 OpenClaw 龙虾 agent 造人设） | 9 个文件（含 `gacha.py` 抽卡引擎） |
| 6 | `docs/zh-CN/the-openclaw-guide.md` + `docs/ja-JP/the-openclaw-guide.md` | 一篇**批判 OpenClaw 安全性**的长文（各 471 行） | 942 行，**英文原版在仓库里不存在** |
| 7 | `docs/HERMES-OPENCLAW-MIGRATION.md` | 迁移指南：怎么**从** OpenClaw 迁**到** ECC | 240 行 |

另外 `package.json:53` 把 `.openclaw/` 列进 npm 包的 `files`，`package.json:391` 列了 `skills/openclaw-persona-forge/`。

### 9.3 `.openclaw/` 目录：纯占位，而且 README 内容跟代码对不上

整个目录只有一个 21 行的 README（`.openclaw/README.md`）。它声称：

```markdown
## What is installed
- `rules/ecc/` — shared coding rules and guidelines     ← ❌ 实际是 rules/（没有 ecc 子层）
- `skills/ecc/` — reusable skills                        ← ❌ 实际是 skills/<名>（没有 ecc 子层）
- `commands/` — slash commands                           ← ✅
- `AGENTS.md` — agent instructions                       ← ✅
```

实测（`node scripts/install-plan.js --profile full --target openclaw`）：
```
rules-core    | rules       -> ~/.openclaw/rules          [preserve-relative-path]
commands-core | commands    -> ~/.openclaw/commands       [preserve-relative-path]
agents-core   | agents      -> ~/.openclaw/agents         [preserve-relative-path]
agents-core   | AGENTS.md   -> ~/.openclaw/AGENTS.md      [preserve-relative-path]
agents-core   | .agents     -> ~/.openclaw/.agents        [preserve-relative-path]
platform-configs | .openclaw -> ~/.openclaw               [sync-root-children]
...45 条 skills/<名> -> ~/.openclaw/skills/<名>
```

`rules/ecc/` 那个 `ecc` 命名空间**只有 Claude 有**（`scripts/lib/install-targets/claude-home.js:10, 16-18`），openclaw 用的是默认 adapter，没有 remap 逻辑。README 大概是从 claude 那份抄的没改。

还有一条：README 说 "Use `npx ecc doctor --target openclaw` to check install health"——这条我**未验证**（要跑 `npx` 属于安装动作，本次只读研究不执行）。要验证的话看 `scripts/doctor.js` 是否接受 `--target openclaw`。

`sync-root-children` 那条操作的实际效果：把仓库 `.openclaw/` 的**子项**铺到 `~/.openclaw/` 根——因为里面只有一个 README.md，所以就是把这个 README 拷过去。**ECC 给 OpenClaw 的"专属内容"就是这一个 README。**

### 9.4 能力清单：openclaw 拿得到什么 / 拿不到什么

装了 `--profile full --target openclaw` 之后（7 个模块 / 56 个文件操作）：

**拿得到：**
- `rules/` 全套（122 个文件，整目录拷，不压平）
- `agents/` 全套 + 根 `AGENTS.md` + Codex 的 `.agents/`
- `commands/` 全套（94 个文件）+ 2 个脚本（`harness-audit.js`、`skills-health.js`）
- **45 个 skill**：`workflow-quality` 那 43 个（tdd-workflow / verification-loop / eval-harness / continuous-learning / strategic-compact / plan-canvas / code-tour / repo-scan …）+ `unified-memory` + `ito-compute`
- `mcp-configs/` + `auto-update.js` + `setup-package-manager.js`

**拿不到（被模块 targets 排除）：**
| 缺什么 | 影响 |
|---|---|
| **hooks-runtime** | 没有任何运行时强制。ECC 在 Claude/Cursor/OpenCode 上靠 hook 拦 `git commit --no-verify`、自动格式化、检测 prompt 里的密钥——这些在 OpenClaw 上全都没有 |
| `framework-language`（框架/语言 skill 包） | React/Next/Vue/Swift/Go 等语言专项 skill 全没有 |
| `database` / `security` / `research-apis` / `business-content` / `devops-infra` / `machine-learning` / `agentic-patterns` / `optimization-workflows` / `swift-apple` / `supply-chain-domain` / `document-processing` / `media-generation` / `social-distribution` / `operator-workflows` / `prediction-market-skills` | **15 个 skill 大包全部拿不到**——这就是为什么 openclaw 只有 45 个 skill 而 claude 有 280 个 |
| `orchestration` | tmux/worktree 多 agent 并行编排没有 |
| `docs-*` 多语言文档 | 没有 |

🔑 **数字对比**：Claude 280 个 skill，Cursor 279，Codex 206，**OpenClaw 45**。约等于 Claude 的 16%。

### 9.5 为什么是这三家（openclaw / hermes / kimi）同一档

这三个 target 在 manifest 里的待遇**完全一样**（都是 7 模块 56 操作）。它们的共同点：
- adapter 都是 10 行模板文件，零自定义逻辑
- 平台目录里都只有一个 README.md
- 都被 15 个 skill 大包的 targets 排除

对比：qwen、zed、joycode 同样没有专属内容或只有一个配置文件，却拿到了 23 个模块 / 279 个 skill。所以**这不是"平台能力不够"，而是 manifest 里的白名单没写 openclaw**。要给 OpenClaw 补齐能力，只需要在 `manifests/install-modules.json` 的那 15 个模块的 `targets` 数组里各加一个 `"openclaw"`——不需要写任何代码。（置信度 中 ~65%：机制上确实只差白名单，但没验证这些 skill 装到 OpenClaw 后是否真能被发现和加载。）

### 9.6 ECC 官方对 OpenClaw 的定位：源系统 + 安全反面教材

这部分是理解"为什么 OpenClaw 支持这么浅"的关键——**不是没做完，是刻意的**。

**(a) 定位为"要迁移出来的源系统"**

`docs/HERMES-OPENCLAW-MIGRATION.md:24` 原话：
> "Treat Hermes and OpenClaw as **source systems, not as the final runtime**."

`:26-36`：
> "ECC is the durable public system: skills / agents / commands / hooks / install surfaces / session adapters / ECC 2.0 control-plane work.
> Hermes and OpenClaw are useful **inputs** because they contain repeated operator workflows that can be distilled into ECC-native surfaces."

整篇文档是一张"翻译表"（:55-166），教你把 OpenClaw 的 scheduler / gateway / memory / skill / tool 五层分别改写成 ECC 的对应物。这跟"支持 OpenClaw 作为运行平台"是相反方向的工作。

**(b) 有一篇专门批判 OpenClaw 安全性的长文**

`docs/zh-CN/the-openclaw-guide.md`（471 行）标题就是《**OpenClaw 的隐藏危险**》，副标题"来自智能体前沿的安全教训"。开篇（:8-30）作者自述用了 OpenClaw 一周，第三天在一个社区 ClawdHub skill 里发现了埋在注释块下面 12 行的隐藏系统指令（prompt injection），并写道：

> "OpenClaw 有很多攻击面。很多频道。很多集成点。**很多社区贡献的技能没有审查流程。**"

⚠️ **文档漂移**：这篇文章的**英文原版在仓库里已经不存在**了。根目录只有 `the-shortform-guide.md` / `the-longform-guide.md` / `the-security-guide.md` 三篇，但 `docs/zh-CN/` 和 `docs/ja-JP/` 各有四篇（多出 `the-openclaw-guide.md`）。译文里明确写着"这是《ECC 指南系列》的第 3 部分"，而英文的第 3 部分现在是 `the-security-guide.md`（一篇更宽泛的 agent 安全文，只在 8 处提到 OpenClaw）。推测英文原版被替换/下架了，译文没跟着删。**未验证**下架原因（浅克隆没有历史，`git log` 只有 1 个 commit）。

**(c) 唯一的 OpenClaw 专属 skill 是社区贡献的娱乐向 skill**

`skills/openclaw-persona-forge/` —— 给 OpenClaw 的"龙虾 agent"生成人设（SOUL.md / 名字 / 头像提示词），带一个 `gacha.py` 抽卡引擎。frontmatter 里 `metadata: origin: community`（SKILL.md:4-5），说明是社区投稿而非 ECC 官方维护。它跟 OpenClaw 的**工程能力适配无关**。

### 9.7 openclaw 不在合规矩阵里

`scripts/lib/harness-adapter-compliance.js` 的 11 条 `ADAPTER_RECORDS` 里**没有 openclaw**（也没有 hermes / kimi / qwen / joycode / antigravity / codebuddy）。这意味着：
- 没有官方的 `state` 定级（Native / Adapter-backed / Instruction-backed / Reference-only）
- 没有 `last_verified_at`（其他 11 家都有 2026-05-12 或 2026-05-17）
- 没有 `verification_commands`、没有 `risk_notes`

复核命令：`node /Users/aa00158/harness-research/ECC/scripts/harness-adapter-compliance.js --format markdown | grep -i openclaw`（应该无输出）。

### 9.8 如果你想在 OpenClaw 上用 ECC，实际能拿到什么

按你（用户）的本地情况——`~/.openclaw/workspace/skills/` 是 skill drop-in 目录、纯 Anthropic 格式无 metadata.json——我的判断：

1. **走 installer 意义不大**。`--target openclaw` 装到的是 `~/.openclaw/skills/`（`rootSegments: ['.openclaw']` + kind `home`，openclaw-home.js:5-7），跟你实际的 `~/.openclaw/workspace/skills/` 不是同一个路径。**未验证** OpenClaw 是否也扫 `~/.openclaw/skills/`——这个只有你能确认。
2. **更实际的做法是直接拿 `skills/` 目录里的东西**。ECC 的 skill 就是标准 `SKILL.md` + frontmatter（`docs/architecture/cross-harness.md:36` 说 "SKILL.md is the most portable unit"），手动 `cp -r` 进你的 drop-in 目录即可。README.md:350 自己也给了这种用法：`cp -r .agents/skills/* ~/.claude/skills/`。
3. **hooks 那部分完全用不了**，这是硬约束，不是配置问题。

---

## 10. README 1306-1720 行 Cross-Platform / Platform Support 讲了什么

这段有两个不同的章节，标题容易混：

### 10.1 `## Cross-Platform Support`（README.md:1306-1405）——其实是"跨操作系统"，不是"跨 harness"

开头一句：「ECC fully supports **Windows, macOS, and Linux**... All hooks and scripts are written in Node.js for maximum compatibility.」（README.md:1308）

这节的三个折叠块讲的都是运行时配置，跟 harness 适配关系不大，但有两个跟本维度直接相关：

**(a) hook 运行时开关（README.md:1341-1376）** —— 这些环境变量是**跨 harness 生效的统一控制面**，因为 Cursor 和 OpenCode 的适配层都实现了同一套读取逻辑（`.cursor/hooks/adapter.js:63-79`）：
```bash
export ECC_HOOK_PROFILE=standard          # minimal / standard / strict
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"
export ECC_SESSION_START_MAX_CHARS=4000   # SessionStart 注入上下文的字符上限，默认 8000
export ECC_SESSION_START_CONTEXT=off      # 低上下文/本地模型场景整个关掉
export ECC_MAX_INJECTED_INSTINCTS=6
export ECC_INSTINCT_CONFIDENCE_THRESHOLD=0.7
```

**(b) `ECC_AGENT_DATA_HOME`：多 harness 数据隔离（README.md:1379-1401）** —— 这是跨 harness 适配里一个**很实际的坑**：ECC 的记忆持久化 hook（会话摘要、学到的 skill、session 别名、成本指标）默认都写 `~/.claude`。如果你同一台机器上 Claude Code 和 Cursor 都装了 ECC，两边会互相覆盖会话文件。解法：
```bash
export ECC_AGENT_DATA_HOME="$HOME/.cursor/ecc"
```
落在这个根下的四类数据：`session-data/`、`skills/learned/`、`session-aliases.json`、`metrics/`。对应 issue [affaan-m/ECC#2065](https://github.com/affaan-m/ECC/issues/2065)。

Cursor 那边做了 4 层自动兜底（README.md:1476-1481）：sessionStart hook 注入环境变量 → hook 运行时检测到 `CURSOR_VERSION`/`CURSOR_PROJECT_DIR` 就默认 `~/.cursor/ecc` → `.cursor/ecc-agent-data.json` 项目配置 → `.cursor/rules/ecc-agent-data-home.mdc` 常驻规则提醒模型。这套 4 层实现在 `scripts/lib/agent-data-home.js` 和 `scaffolds/cursor/`（`scripts/lib/install-executor.js:208-248` 负责铺这些文件）。

### 10.2 `## Platform Support`（README.md:1404-1720）——真正的跨 harness 章节

**(a) 分发方式总表（README.md:1406-1412）**，只列了 5 家：

| Harness | ECC 分发方式 | 主指令面 | 自动化 |
|---|---|---|---|
| Claude Code | 插件 或 选择性 installer | `CLAUDE.md`、rules、skills、agents | 原生插件 hook |
| Codex | sync 流程、仓库配置、实验性 ECC marketplace | `AGENTS.md`、skills、`.codex/config.toml` | Git hooks + Codex 原生配置 |
| Cursor | 项目 adapter | `.cursor/rules/`、限定作用域的 agents | Cursor hook 适配器 |
| OpenCode | 构建后的插件 + 选择性 installer | `opencode.json`、instructions、commands | OpenCode 插件事件 |
| GitHub Copilot | 签入仓库的指令层 | `copilot-instructions.md`、prompt 文件 | **无 ECC hook 运行时** |

⚠️ 这张表**只有 5 家**，而 installer 支持 14 个 target。openclaw / hermes / kimi / qwen / zed / joycode / codebuddy / antigravity 全部不在这张"主表"里，只在 README.md:255-265 的折叠块里各占一行。

**(b) Cross-Tool Feature Parity 表（README.md:1414-1428）** —— 13 行 × 5 列的能力对照。核心数字：

| Feature | Claude Code | Cursor | Codex | OpenCode | Copilot |
|---|---|---|---|---|---|
| Agents | 67 | 共享（AGENTS.md） | 共享 | 12 | N/A |
| Commands | 94 | 共享 | 指令式 | 35 | 5 prompts |
| Skills | 281 | 共享 | 10（原生格式） | 37 | 靠 instructions |
| **Hook Events** | **8 types** | **15 types** | **None yet** | **11 types** | **None** |
| Hook Scripts | 20+ | 16（DRY adapter） | N/A | 插件 hook | N/A |
| Rules | 34 | 34（YAML frontmatter） | 指令式 | 13 instructions | 1 个常驻文件 |
| Custom Tools | 靠 hook | 靠 hook | N/A | **6 个原生 tool** | N/A |
| MCP Servers | 14 | 共享 mcp.json | 7（TOML 自动合并） | Full | N/A |
| Config Format | settings.json | hooks.json + rules/ | config.toml | opencode.json | copilot-instructions.md |
| Secret Detection | hook | beforeSubmitPrompt hook | sandbox | hook | 指令式 |

**核对结果（我逐项数了）**：

| README 声称 | 实测 | 判定 |
|---|---|---|
| Claude Skills 281 | `ls skills \| wc -l` = **281** | ✅ 准 |
| Agents 67 | `ls agents \| wc -l` = **67** | ✅ 准 |
| Commands 94 | `ls commands \| wc -l` = **94** | ✅ 准 |
| Cursor Hook Events 15 | `.cursor/hooks.json` 有 **15** 个事件 key | ✅ 准 |
| Cursor Hook Scripts 16 | `.cursor/hooks/*.js` 共 17 个，扣掉 `adapter.js` = **16** | ✅ 准 |
| **Claude Hook Events 8 types** | `hooks/hooks.json` 只有 **7** 个：PreToolUse / PreCompact / SessionStart / PostToolUse / PostToolUseFailure / Stop / SessionEnd | ❌ 差 1 |
| **Rules 34** | `find rules -type f` = **122**；Cursor 装出来 **82** 个 `.mdc` | ❌ 严重过时（34 可能是"规则类别数"，rules/ 下有 22 个语言目录） |
| **Cursor Agents 48**（README.md:1456） | 装出来 **67** 个 | ❌ 过时 |
| **Codex Skills 10（原生格式）** vs 另一处写 **32**（README.md:1551） | `.agents/skills` 实测 **39** 个目录 | ❌ README 内部自相矛盾，且都不对 |
| **MCP Servers 14** | `.mcp.json` 只有 **1** 个；`mcp-configs/mcp-servers.json` 有 **35** 个 | ❌ 对不上（14 可能是某个中间版本） |
| **"Cursor has more hook events than Claude Code (20 vs 8)"**（README.md:1490） | 同一篇 README 上面才说 Cursor 是 15 types | ❌ 内部矛盾 |

🔑 结论：README 的**结构性描述可信**（谁有 hook、谁靠指令、adapter 怎么工作），**具体数字大面积过时**。做研究引用时，数字一律以 `manifests/install-modules.json` + 实际目录计数为准，不要引 README。

**(c) 4 条 "Key architectural decisions"（README.md:1430-1433）** —— 这是全篇最有信息量的 4 行：
1. 根 `AGENTS.md` 是通用跨工具文件（Claude Code / Cursor / Codex / OpenCode 都读；Copilot 用 `.github/copilot-instructions.md`）
2. **DRY adapter pattern** 让 Cursor 复用 Claude Code 的 hook 脚本，不重复实现
3. SKILL.md（带 YAML frontmatter）格式在 Claude Code / Codex / OpenCode 之间通用
4. Codex 没有 hook，用 `AGENTS.md` + `model_instructions_file` override + sandbox 权限来补偿

**(d) 五个"in depth"折叠块**（Cursor / Codex / Zed / OpenCode / GitHub Copilot），前面已在 §4、§8 引用。补两条这里独有的：

- **Copilot 的能力天花板写得最诚实**（README.md:1713-1715）：「GitHub Copilot does not have a hook system or a subagent API, so ECC's hook automations... and agent delegation are unavailable.」并给了一张"ECC Feature → Copilot equivalent"的降级对照表（README.md:1698-1710），其中 "Hooks / automation → Not supported"、"Agents / delegation → Not supported"。Copilot 甚至**不是 installer target**，走的是"文件已经签入仓库，打开就生效"的路子（`.github/copilot-instructions.md` + `.github/prompts/*.prompt.md` + `.vscode/settings.json` 开 `chat.promptFiles`）。
- **Codex 的 hook 补偿手段**（README.md:1560）：「Codex does **not yet provide Claude-style hook execution parity**. ECC enforcement there is instruction-based via `AGENTS.md`, optional `model_instructions_file` overrides, and sandbox/approval settings.」加上 `sync-ecc-to-codex.sh` 装的**全局 git hook**（见 §6.3）——把"agent 运行时 hook"降级成"git 层 hook"。

### 10.3 一个跨 harness 设计上的洞察

README 和 `docs/architecture/cross-harness.md` 合起来给出的模型是：

> **hook 能力是不可移植的，所以 ECC 对每个平台准备一条降级路径。**

| 平台 | hook 强制手段 | 降级到 |
|---|---|---|
| Claude Code | 原生 hook | — |
| Cursor | 15 事件 + adapter 转换 | — |
| OpenCode | 20+ 插件事件 | — |
| Codex | 无 | `AGENTS.md` 指令 + **全局 git hook** + sandbox 审批 |
| Copilot | 无 | `copilot-instructions.md` 常驻指令 |
| **OpenClaw / Hermes / Kimi / Qwen / Zed / Antigravity / JoyCode** | 无 | **只有 rules/AGENTS.md 指令，连 git hook 补偿都没有** |

---

## 11. 未验证 / 存疑清单

按"我能从代码/文档确认"和"不能确认"分开列。不能确认的都给了复核动作。

### 11.1 未验证（本次只读研究无法确认）

| # | 事项 | 为什么没验证 | 怎么复核 |
|---|---|---|---|
| 1 | 真实安装出来的文件数是否等于 plan 的操作数 | 不能跑 `install-apply.js`（会写盘）；且 `scripts/install-plan.js:153` 自己声明 plan 只是 "scaffold-level"，不是 install.sh 的逐字节镜像 | 在**一次性容器/临时 HOME** 里跑 `HOME=/tmp/x node scripts/install-apply.js --dry-run --json --profile full --target openclaw` 比对 |
| 2 | `npx ecc doctor --target openclaw` 是否真存在 | `.openclaw/README.md:21` 声称有，但跑 npx 属于安装动作 | 读 `/Users/aa00158/harness-research/ECC/scripts/doctor.js`，grep `openclaw` 或看它接受哪些 `--target` |
| 3 | OpenClaw 是否真会读 `~/.openclaw/skills/` | 只有你的本地环境能确认；你的 memory 记的是 `~/.openclaw/workspace/skills/` | 在你的 OpenClaw 里放一个测试 skill 到 `~/.openclaw/skills/`，看能不能触发 |
| 4 | `ecc-universal` 这个名字是不是同一个 npm 包 | `.opencode/package.json:2` 说它是 OpenCode 插件包；`skill-unified-memory` / `ito-compute` 的模块描述说它是 "CLI runtime" | `npm view ecc-universal` 看 bin 字段 |
| 5 | 45 个 skill 装到 OpenClaw 后是否真能被发现/加载 | 涉及 OpenClaw 的 skill 发现机制，代码里看不到 | 同 #3 |
| 6 | `.agents/` 被拷进 11 个非 Codex 平台是有意还是漏过滤 | 仓库里没有说明；`PLATFORM_SOURCE_PATH_OWNERS`（helpers.js:5-18）列了 12 个平台目录唯独没 `.agents` | 去 GitHub 搜 issue/PR 里 `.agents` + `isForeignPlatformPath` |
| 7 | 英文版 `the-openclaw-guide.md` 为什么消失 | 浅克隆只有 1 个 commit，无历史 | `git log --all --full-history -- "*openclaw-guide*"`（需要完整克隆） |
| 8 | `.agents/plugins/ecc` 目录缺失是否影响 marketplace | `.agents/plugins/marketplace.json` 指向 `./plugins/ecc`，该目录不存在 | 查 README.md:1406 提到的 "experimental ECC marketplace" 的具体用法 |

### 11.2 属于"仓库声称，未在代码中核实"的说法

| README 说法 | 位置 | 状态 |
|---|---|---|
| "ECC fully supports Windows, macOS, and Linux" | README.md:1308 | 有 Node.js 全平台脚本 + `install.ps1` + `install.sh` 里的 cygpath 处理（install.sh:26-30）支撑，**部分核实**；没有实际跑过 Windows |
| "**first-class** Codex support" | README.md:1521 | 与代码矛盾：codex 在合规矩阵里被定级为 **Instruction-backed**（compliance.js:77），且 `--target codex` 拿不到 rules / commands / hooks。"first-class" 属宣传话术 |
| "Cursor has more hook events than Claude Code (**20 vs 8**)" | README.md:1490 | ❌ 实测 Cursor 15、Claude 7。同一篇 README 的对照表写的是 15 vs 8。**数字不可信** |
| "MCP Servers 14" | README.md:1423 | ❌ `.mcp.json` 1 个、`mcp-configs/mcp-servers.json` 35 个 |
| "Rules 34" | README.md:1421 | ❌ `rules/` 下 122 个文件 / 22 个语言目录 |
| "Skills 32"（Codex） | README.md:1551 | ❌ 实测 39 个 |
| "behavior-identical packaging copies"（`.agents/skills/` 和 `.cursor/skills/`） | `docs/architecture/cross-harness.md:99-101` | ❌ diff 出实质差异（见 §5.3） |
| `.openclaw/README.md` 说装 `rules/ecc/` 和 `skills/ecc/` | `.openclaw/README.md:7-8` | ❌ 实测是 `rules/` 和 `skills/<名>`，`ecc` 命名空间只有 Claude 有 |

### 11.3 我认为最值得后续深挖的 3 件事

1. **`ADAPTER_RECORDS` 与 `SUPPORTED_INSTALL_TARGETS` 没有交叉校验** —— 7 个有 installer 的平台没有合规记录，校验器不报错。这是一个"文档生成机制做得很好但覆盖面有洞"的典型案例。加一条断言就能修（`tests/lib/install-targets.test.js:852` 已经有类似的守卫写法可以照抄）。
2. **三份 skill 副本靠人工同步** —— `skills/`（281 个目录）/ `.agents/skills/`（39）/ `.cursor/skills/`（11），没有生成脚本，已实测漂移。这直接违反项目自己写的规矩（`docs/architecture/cross-harness.md:149`）。
3. **"平台覆盖优先级"靠操作顺序而非显式声明** —— `dedupeCopyFileOperations`（install-executor.js:696-719）只保留最后一条同落点写入。这意味着模块在 manifest 里的顺序会影响最终装出来的内容，但 manifest 里没有任何地方标注这一点。已经因此出过 bug（issue #2414）。

---

## 附：本次研究用到的只读复核命令（全部安全，不写盘）

```bash
cd /Users/aa00158/harness-research/ECC

# 列出所有安装 profile / 模块 / 组件
node scripts/install-plan.js --list-profiles
node scripts/install-plan.js --list-modules
node scripts/install-plan.js --list-components --json

# 看某个平台会装什么（纯计划，不写盘）
node scripts/install-plan.js --profile full --target openclaw
node scripts/install-plan.js --profile full --target openclaw --json

# 合规矩阵
node scripts/harness-adapter-compliance.js --format text
node scripts/harness-adapter-compliance.js --format markdown

# 仓库 agent 成熟度评分
node scripts/harness-audit.js --format json

# 关键常量
grep -n "SUPPORTED_INSTALL_TARGETS" scripts/lib/install-manifests.js
grep -n "PLATFORM_SOURCE_PATH_OWNERS" -A 15 scripts/lib/install-targets/helpers.js

# 验证无软链
find . -type l -not -path "./node_modules/*" -not -path "./.git/*"
```
