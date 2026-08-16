# ECC 只读研究：隔离方案与污染面清单

> 结论全部由本机实测得出，非读 README。核实时间 2026-08-01。
> 研究对象：affaan-m/ECC v2.1.0（236k star / MIT），浅克隆于 `~/harness-research/ECC`

---

## 一、当前隔离状态（已生效）

| 项目 | 状态 |
|---|---|
| 仓库位置 | `~/harness-research/ECC`（不在任何自动加载路径） |
| `~/.claude/skills` | 未变动，仍是你原有的 62 个目录 |
| `~/.claude/agents` | 未变动，仍是 cnki-researcher / github-researcher / skill-fetch-researcher |
| `~/.claude/settings.json` | 未触碰 |
| `~/.claude/rules/ecc` | 不存在（核实过） |
| 已运行的唯一写盘操作 | `npm install --ignore-scripts`，只写 `~/harness-research/ECC/node_modules` |

`package.json` 无 `preinstall` / `postinstall` 钩子（已核实），直接依赖仅 3 个：`ajv` `@iarna/toml` `sql.js`。

---

## 二、⚠️ 一个反直觉的隐性加载风险

**不安装也可能被影响**：ECC 仓库根目录自带 `CLAUDE.md` + `.claude/` 目录。
只要把 shell 的 cwd 切进 `~/harness-research/ECC`，Claude Code 就会把它们当项目配置加载。本次研究中已实测复现——`ECC/.claude/rules/node.md` 和 `everything-claude-code-guardrails.md` 被自动注入了上下文。

被自动加载的全部范围（实测，很小）：

| 路径 | 内容 |
|---|---|
| `ECC/CLAUDE.md` | 4 KB 项目说明 |
| `ECC/.claude/rules/` | 2 个 rules 文件 |
| `ECC/.claude/skills/` | 1 个 skill（everything-claude-code） |
| `ECC/.claude/commands/` | 3 个 command |

注意：仓库顶层的 `skills/`（281 个）和 `agents/`（67 个）**不会**被自动加载——Claude Code 只认 `.claude/skills`。

**规避**：研究时把工作目录设在父目录 `~/harness-research/`，而不是 `ECC/` 里面。

---

## 三、污染面清单（`install.sh --target claude` 会写什么）

实测方法：`node scripts/install-plan.js --profile <P> --target claude --json`（该脚本经 grep 核实**无任何写盘调用**，纯计算）。

### 各 profile 写入量

注意：一条"操作"是目录级的，实际落盘文件数要展开算（下表右列为实测展开值）。

| profile | 操作数 | 实际文件数 | 其中写入 `~/.claude/skills/` |
|---|---|---|---|
| minimal | 55 | **460** | 44 个 skill 目录 |
| core | 58 | **626** | 44 |
| security | 78 | — | 63 |
| research | 84 | — | 70 |
| developer | 140 | **755** | 121 |
| full | 301 | **985** | 280 |

> `minimal` 名不副实——它照样往你的全局 skills 目录塞 44 个 skill、总计 460 个文件。

### full profile 的非 skills 落点（逐条）

```
rules              -> ~/.claude/rules/ecc
.agents            -> ~/.claude/.agents
agents             -> ~/.claude/agents          （67 个 agent 文件）
AGENTS.md          -> ~/.claude/AGENTS.md
commands           -> ~/.claude/commands        （94 个）
hooks              -> ~/.claude/hooks
scripts/hooks      -> ~/.claude/scripts/hooks   （48 个 hook 脚本）
scripts/lib        -> ~/.claude/scripts/lib
.claude-plugin     -> ~/.claude                 （plugin.json / marketplace.json 等 4 个文件）
mcp-configs        -> ~/.claude/mcp-configs
the-security-guide.md -> ~/.claude/the-security-guide.md
scripts/*.js       -> ~/.claude/scripts/        （auto-update / claw / orchestrate 等 12 个）
```

安装记录：`~/.claude/ecc/install-state.json`

### 三条关键性质（均已核实到代码）

1. **全程不碰 `~/.claude/settings.json`**。`applyInstallPlan` 里唯一的 hooks 写入落在 `targetRoot/hooks/hooks.json`（`scripts/lib/install/apply.js:126`），不是 settings。
   👉 推论：**手动安装的 hook 不会自动生效**（Claude Code 从 settings.json 读 hook 注册）。真正会自动注册 hook 的是 `/plugin install ecc@ecc` 那条路。**两条安装路径的风险等级完全不同。**

2. **覆盖行为分三档**（`scripts/lib/install/apply.js:195-236`）：

   | 文件类型 | 行为 | 代码位置 |
   |---|---|---|
   | `~/.claude/skills/<name>/` 已存在 | **跳过** + 发 conflict warning（唯一的用户文件保护） | `claude-skill-migration.js:273-283` |
   | JSON 配置（mcp-configs） | `deepMergeJson` 深合并 | `apply.js:203-207` |
   | Markdown 及其它一切（commands / agents / rules） | **无条件 `writeFileSync` / `copyFileSync` 覆盖，无备份** | `apply.js:230-235` |

3. **⚠️ 撞名实测：2 处**（早先"0 个"的结论是错的——统计脚本用 `isDirectory()` 过滤，漏掉了 symlink 形式的 skill）：

   | 撞名项 | 你的版本 | ECC 版本 | 后果 |
   |---|---|---|---|
   | `skills/deep-research` | symlink → `~/.codex/skills/deep-research` | ECC 自带同名 skill | ✅ **安全**，命中跳过保护 |
   | `commands/learn.md` | 你的 `/learn`（5644 字节，"persist learnings into rules/memory/skills"） | ECC 的 `/learn`（1725 字节，"extract patterns as candidate skills"） | 🔴 **先被无条件覆盖，卸载时再被删除** |

   👉 `commands/learn.md` 的双重损失已核实到代码：写入端 `apply.js:230-235` 无存在性检查直接覆盖；install-state 从不记录 `previousContent`（`apply.js` 里 grep 无写入点）；卸载端 `install-lifecycle.js:650-668` 的 `copy-file` 分支**只有删除、没有还原**（`render-template` 等其它 kind 才有还原分支，但同样无内容可还原）。装前不备份 = 永久丢失。

   `~/.claude/AGENTS.md`、`marketplace.json`、`plugin.json`、`the-security-guide.md` 你都没有，这几个不冲突；3 个自建 agent 也不撞名。

---

## 四、试用路径分级（风险从低到高）

| 档 | 做法 | 写盘范围 | 可逆性 |
|---|---|---|---|
| **0 · 纯读**（当前） | 只读仓库文件 | 无 | — |
| **1 · 计划预览** | `node scripts/install-plan.js --profile full --target claude --json` | 无（核实过无写盘调用） | — |
| **2 · dry-run** | `node scripts/install-apply.js --profile core --target claude --dry-run` | 无（`install-apply.js:169` 在 `applyInstallPlan` 前 return） | — |
| **3 · 项目级沙箱**（推荐试用档） | `--target claude-project`，在一个空目录里跑 | 只写 `<该目录>/.claude/` | 删目录即完全消失 |
| **3b · HOME 重定向沙箱** | `HOME=/tmp/ecc-fake node scripts/install-apply.js --profile full --target claude` | 只写 `/tmp/ecc-fake/.claude/` | 删目录即消失。这是 ECC 自己测试套件用的隔离法（`tests/scripts/install-apply.test.js:15-34`） |
| **4 · 单点取用** | **手动 `cp -R skills/<name> ~/.claude/skills/`** | 你指定的那一个 | 删掉即可 |
| **5 · 全局安装** | `./install.sh --profile full` | 983 个文件进 `~/.claude` | 靠 install-state 卸载，但**被覆盖的文件不还原** |
| **6 · 插件安装** | `/plugin install ecc@ecc` | 插件缓存 + **自动注册 hooks** | 风险最高，会引入自动执行代码 |

### ⚠️ 三个命令级陷阱（都已核实到代码）

**陷阱 1：`./install.sh --dry-run` 拦不住 npm install。**
`install.sh:18-21` 在 `node_modules` 缺失时会先无条件跑 `npm install`，再把 `--dry-run` 透传下去。想真正零写盘，绕开 shell 壳子直接调 Node：
```bash
node ~/harness-research/ECC/scripts/install-apply.js --profile core --target claude --dry-run
```

**陷阱 2：`--skills <name>` 不是装一个 skill。**
15 个预定义的 `skill:*` 组件指向的是**整个 module** 而不是单个目录。实测：

| 命令 | 实际装入 |
|---|---|
| `--skills tdd-workflow` | **44 个 skill 目录 / 85 个文件** |
| `--skills security-review` | **63 个目录 / 106 个文件** |
| `--skills deep-research` | **9 个目录 / 16 个文件** |
| `--skills code-tour`（无预定义组件，走合成路径） | 1 个目录 / 1 个文件 |

想真的只要一个 skill，用 `cp -R` 手动拷，别用 `--skills`。

**陷阱 3：卸载不带 `--target` 会扫全部 14 个安装目标。**
`install-lifecycle.js:29-32` 的 `normalizeTargets` 在参数为空时返回**所有** adapter，且 `uninstall.js:76-81` 把 `HOME` 和 `process.cwd()` 一起传进去。对你尤其危险——你有真实的 `~/.codex`（57 个文件）和 `~/.openclaw`。卸载务必显式指定：
```bash
node ~/harness-research/ECC/scripts/uninstall.js --target claude --dry-run   # 先看
```

### 档 3 的具体命令（沙箱试用）

```bash
mkdir -p ~/harness-research/ecc-sandbox && cd ~/harness-research/ecc-sandbox
node ~/harness-research/ECC/scripts/install-apply.js --profile core --target claude-project
```

实测确认 `--target claude-project` 的 `targetRoot` = **当前工作目录下的 `.claude/`**，301 条操作全部落在那里面，不外溢。
在该目录里开 Claude Code 会话即可试用；`rm -rf ~/harness-research/ecc-sandbox` 就是完整卸载。

---

## 五、待核实项

- [ ] 卸载命令的实际行为与残留（install-state 能否干净回滚）
- [ ] `/plugin install` 路径注册了哪些 hook、每次工具调用的实际开销
- [ ] `ecc ito find` 那个算力询价桥接是否会自动触发
