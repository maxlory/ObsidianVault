# 渠道 01 — ECC 仓库 GitHub Issues / Discussions 实地调研

> 调研人：subagent（GitHub Issues 渠道）
> 时间：2026-08-01
> 对象：`affaan-m/ECC`（236,734 star / 35,991 fork / MIT / 创建 2026-01-18 / 最后 push 2026-07-29）
> 数据面：issue 总数 **682**（34 open / 648 closed），PR 总数 **1835**，Discussions **已开启**
> 工具：`gh` CLI（已认证账号 maxlory）；全部 682 条 issue 标题已抓取本地扫描，高价值条目逐条读正文+评论

**一句话结论**：这个渠道**极有料**，而且几乎所有的"取舍证据"都藏在 closed issue 里。核心事实是——**ECC 官方自己承认过"太多东西默认全装"是头号摩擦点**，并为此上线了 3 套裁剪机制（profile / `agent-sort` / hook env 开关）。但要注意：**几乎所有第三方抱怨都在 issue 里被"已修复/已关闭"，用户自己公开的"我最后留了什么"极少**——这个渠道能给你"该关什么、为什么关"，给不了"社区共识的最小集"。

---

## 0. 作者身份提示（读下面所有内容前先知道）

- 682 条 issue 里 **148 条（21.7%）是维护者 affaan-m 自己开的**——他把 roadmap 当 issue 用。评估"社区活跃度"时要扣掉这块。
- 出场的三个"官方侧"账号：`affaan-m`（OWNER）、`pangerlkr`（CONTRIBUTOR，实际在做 triage 和技术回复）、`haelyra`（COLLABORATOR）。
- 后两位的回复语气高度像 LLM 生成的 triage（结构化小标题 + "Our position on each"），**当"官方承诺"读可以，当"第三方实战"读不行**。

---

## A. 上下文膨胀：有实测数字（本渠道最硬的证据）

这是本次调研**唯一有多方独立量化**的维度。三个独立数字：

### A1. 12.8k tokens/turn —— 全插件加载的常驻成本

> **来源**：[Issue #2482](https://github.com/affaan-m/ECC/issues/2482) — @webdes83（第三方，NONE）2026-07-09
> 环境：everything-claude-code **2.0.0-rc.1** / Claude Code / macOS

原话（翻译）：

> 启用插件会把 **全部 182 个 skill description（约 10.3k token）+ 48 个 agent description（约 2.5k token）≈ 12.8k token 注入每一个 turn**，作为常驻的发现列表（在 2.0.0-rc.1 上实测）。**对只依赖 15–20 个 skill 的用户来说，这笔每轮成本有约 90% 是浪费的。**

他还指出了一个**结构性死结**（这是本条最有价值的部分）：

> 这笔成本**没法靠"禁用插件"来降**，因为同一个插件同时提供了 MCP server（github / exa / memory / playwright / context7 / sequential-thinking）、agents 和 hooks。所以今天想砍掉 skill description 的 token，唯一办法是连 MCP server 和 agents 一起丢掉——这是不可接受的交换。**Claude Code 的 settings.json 里没有 per-skill 开关。**

他提了三个方案（按偏好排序）：① plugin config 选择哪些 skill 类别进常驻列表；② "lean skills" 模式只广播 skill **名字**不广播 description；③ 把 bundle 拆成 `-core`（MCP/agents/hooks）和 `-skills` 两个包。

**结局**：他自己 45 分钟后撤回了 issue，理由是——

> 已发布的 manifest 驱动的 selective-install（developer / minimal profile 保留 platform-configs/MCP + agents，同时丢掉重型领域 skill 模块）在我们这边已经覆盖了实际需求，运行时 lazy-load 方向在 #657 跟踪。

👉 **可直接抄的取舍结论**：想要 MCP + agents 但不要 281 个 skill 的人，正确做法是**走 selective-install 的 developer 或 minimal profile，而不是装 plugin**。这是第三方实测后自己采用的方案。

### A2. ~26.3k tokens —— agent description 曾触发 Claude Code 官方性能警告

> **来源**：[Issue #434](https://github.com/affaan-m/ECC/issues/434) — @TK7684（第三方）2026-03-15，ECC v1.8.0

用户贴出 Claude Code 自己弹的警告：

```
⚠Large cumulative agent descriptions will impact performance (~26.3k tokens > 15.0k)
```

当时是 16 个 agent 文件 ~1853 行（`planner.md` 单文件 213 行）。用户明确写了那个两难：

> 想同时开 ECC 和其它插件的用户只能二选一：接受性能警告，或者**禁用 ECC（失去那些有价值的 agent）**。

**维护者回复**（affaan-m，OWNER，2026-03-20）：

> Closing as already fixed on `main`. Agent description compression and lazy-load support landed in merged PR #688, **which reduces the catalog footprint from ~26k tokens to ~2-3k**.

⚠️ 这个数字**是官方自称**，issue 里没有第三方复测确认。信任度：中。验证动作——装完后看 Claude Code 是否还弹 `Large cumulative agent descriptions` 警告。

### A3. 12,000 tokens → 5,100 tokens —— 唯一公开的"最小可用集"实测

> **来源**：[Issue #916](https://github.com/affaan-m/ECC/issues/916) — @Kavya071（第三方）2026-03-25，ECC v1.9.0

这条是**整个渠道最贴近你问题的一条**。他先复述了问题（ECC v1.9.0 有 291 个东西：28 agent / 125 skill / 60 command / 3 context / 65 rule / 40+ script）：

> - **Plugin 安装**把全部 200+ skill 载入上下文窗口，**每条消息烧约 12,000 token 在 skill description 上**。**Claude Code 的 skill 预算上限会静默丢弃 skill——用户不知道哪些被丢了。**
> - **`install-plan.js` 的 profile** 是模块粒度的，但 `framework-language` 是一整块——想只装 TypeScript skill 就必须连 Perl / Rust / C++ / Django / Spring Boot 一起装。
> - **手工挑**：读完 291 个条目要 1–2 小时，会漏，会挑错（给 TypeScript 项目装了 Python rules）。

他做的方案 `/agent-sort`：6 个并行 agent 分别读 agents / skills / commands / rules / hooks / extras，**对项目代码库 grep 取证**，分成 DAILY（常驻）和 LIBRARY（放进一个关键词路由器，不触发时 0 token）。

**他在真实项目（React Native + TypeScript + Supabase）上的实测结果**：

```
装插件前：  200+ skills，约 12,000 token 开销
agent-sort 后： 51 个 daily 条目，约 5,100 token 开销
Library：   168 个 reference 文件，触发前 0 token
耗时：约 8 分钟（6 个 agent 并行）
```

他还提到一个跨栈常量：**"~35 个 always-daily 基线，不随技术栈变化"**——这是本渠道最接近"最小可用集大小"的数字（约 35 个组件）。可惜他**没有点名列出这 35 个是哪些**。

**结局**：维护者 2026-04-06 把它合进 main（commit `a1e37d7`），成为 `skills/agent-sort/SKILL.md` + `/agent-sort` 命令，挂在 `workflow-quality` 安装模块下。

👉 **对你的直接价值**：ECC 仓库里自带一个叫 `agent-sort` 的 skill，就是官方认可的"按项目证据自动裁剪"工具。你本地 `~/harness-research/ECC/skills/agent-sort/SKILL.md` 就有，可以直接读它的分类标准，而不必安装。

### A4. 官方自述："当前抱怨集中在默认加载太多上下文"

> **来源**：[Issue #657](https://github.com/affaan-m/ECC/issues/657) — @affaan-m（OWNER）2026-03-20，标题 `feat: token optimization — module manifests, lazy-load skills, context scoping`

原话：

> 当前的抱怨集中在**默认加载了太多上下文、对窄用例塞了太多内容、以及运行时噪音大**。

工作项里有一条值得注意：**"5. 记录什么时候该用重型 skill、什么时候该用轻量 command"**——说明官方自己也认为很多 skill 是"重型且不该默认用"的。

**结局**：2026-05-11 被 affaan-m 自己关掉，理由是"这属于 roadmap 规划工作，不是活跃的 GitHub 支持问题，应该挪到统一产品路线图"。👉 **也就是说 lazy-load skill routing 这条主线在公开渠道断了跟踪**，落地程度不透明。

---

## B. 关不掉 / 误触发 / 拖慢：hook 层是最大重灾区

### B1. 头号 issue：hook 不能单独关、无条件触发

> **来源**：[Issue #248](https://github.com/affaan-m/ECC/issues/248) — @Vvkmnn（第三方）2026-02-18，Claude Code v2.1.45 / ECC 1.4.1 / macOS
> 标题：**Plugin hooks cannot be individually disabled & fire unconditionally**

这是全库信息密度最高的一条抱怨。他列了 5 个具体故障：

| # | 症状 | 涉及组件 |
|---|---|---|
| 1 | **每条 Bash 命令**（`ls`、`git status`）都报 `PreToolUse:Bash hook error` | `pre-bash-tmux-reminder`（用 `console.error()` 输出提示信息，Claude Code 把所有 stderr 当 hook error） |
| 2 | 白名单外的**任何 `.md` 文件**都写不了，连 `/tmp/` 都不行 | doc blocker hook |
| 3 | **Claude Code 原生 plan mode 直接坏掉**——它要写 `.claude/plans/`，被 doc blocker 拦 | doc blocker hook |
| 4 | `gh issue create --body "...npm run dev..."` 被拦（正则匹配了 heredoc 里的正文） | `pre-bash-dev-server-block` |
| 5 | 按官方 `hooks/README.md` 在 `~/.claude/settings.json` 里写 `hooks: []` 想覆盖——**无效**，Claude Code 是合并配置不是替换 | Claude Code 核心限制 |

他自己的黑色幽默注脚（在 #352 里）：

> 这个 bug 是自我证明的——**我没法从 Claude Code 里提交这个 issue，因为 hook 匹配到了 issue 正文里的文字。**

**官方给出的解法（这是你最该记的部分）**：

- @pangerlkr（CONTRIBUTOR）2026-02-24：新增 **`ECC_SKIP_HOOKS=1` 环境变量可以绕过所有 hook**。
- @affaan-m（OWNER）2026-03-05 结案：
  > Resolved in ECC v1.8.0. Hook gating now supports profile and explicit disable controls (**`ECC_HOOK_PROFILE`** and **`ECC_DISABLED_HOOKS`**), and script-level flag checks ensure disabling intent is respected. Shipped in PR #334.

👉 **三个官方 hook 关闭开关（第三方逼出来的）**：`ECC_SKIP_HOOKS=1` / `ECC_HOOK_PROFILE` / `ECC_DISABLED_HOOKS`。
👉 issue 里还提到 **PR #245「Prune plugin 43→12」**——官方自己把插件 hook 从 43 个砍到 12 个。

### B2. 逐条误触发实例（都是第三方定位到代码行的）

| Issue | 组件 | 误触发内容 | 状态 |
|---|---|---|---|
| [#2514](https://github.com/affaan-m/ECC/issues/2514) @thejesh23 2026-07-14 | `scripts/hooks/pre-bash-tmux-reminder.js:16` | 正则里 `yarn (install\|test)?` 的 `?` 放错位置，导致 **`yarn` 后面跟任何东西都触发 tmux 提醒**（`yarn --version` 都提醒）。npm/pnpm/bun 都正常，只有 yarn 过宽 | 已修 PR #2517 |
| [#352](https://github.com/affaan-m/ECC/issues/352) @joshuajbrunner 2026-03-07 | `scripts/hooks/pre-bash-dev-server-block.js` | `splitShellSegments()` 不处理 heredoc，导致 `glab mr create --description "<<EOF ...run the dev script... EOF"` 被当成起 dev server 拦掉 | 已关 |
| [#2120](https://github.com/affaan-m/ECC/issues/2120) @ahnineamine 2026-06-02 | `ecc-metrics-bridge.js` + `ecc-context-monitor.js` | ① `hashToolCall` 对 Edit/Write/MultiEdit **只用 `file_path` 做指纹**，忽略 old_string/new_string → 同一文件改 3 个不同地方就报 `LOOP WARNING: Tool 'Edit' called 3 times with same parameters`；② 同一条 `COST WARNING` 在一个 turn 内**原样打印约 20 次**（成本数据每 turn 才更新一次，但 monitor 每次 PostToolUse 都读，debounce 又按调用计数而非内容去重） | 已修 |
| [#1530](https://github.com/affaan-m/ECC/issues/1530) @ShunmeiCho 2026-04-21 | `gateguard-fact-force.js` | `routineBashMsg` **触发了 Claude Code 运行时自带的 anti-prompt-injection 过滤器** | 已关 |
| [#1534](https://github.com/affaan-m/ECC/issues/1534) @vishnu-UH 2026-04-21 | SessionStart hook | `/compact` 恢复后**旧的 skill ARGUMENTS 会被重新执行** | 已关 |
| [#1053](https://github.com/affaan-m/ECC/issues/1053) @kuqili 2026-03-31 | SessionStart hook | 没有 cwd/project 过滤，**把别的项目的 session 注入进来** | 已关 |

作者的原话总结（#2120）：

> 这些都是噪音问题——它们不弄坏什么，但它们让警告变得不可信，**而不可信的警告比没有警告更糟**。

### B3. agent 层的质量抱怨

> [Issue #1486](https://github.com/affaan-m/ECC/issues/1486) — @ahmed-fawzy99（第三方）2026-04-18，点名 **`code-reviewer` agent**：
>
> 我这几天大量使用 code-reviewer agent，注意到的一点是：**它把代码里的很多地方标成 HIGH priority，但大多数时候都是误报。它表现得像是每次被调用都必须标点什么出来，哪怕根本没东西可标。**

**维护者处理**：2026-05-11 以"stale / broad"为由在清库中关闭，**没有修**。

> [Issue #173](https://github.com/affaan-m/ECC/issues/173) — @Warshoow 2026-02-08：`model:` 字段在 Claude Code 里不生效，**13 个 agent 全部跑 opus**，尽管 CONTRIBUTING.md 推荐 sonnet。👉 这是成本层面的坑。

---

## C. 与 Claude Code 原生能力的冲突/重叠

这一节直接回答你的问题 3。

| Issue | 冲突点 | 官方说法 |
|---|---|---|
| [#84](https://github.com/affaan-m/ECC/issues/84) @Hor1zonZzz 2026-01-26 | **ECC `planner` agent vs Claude Code 内置 Plan 模式**命名/功能撞车 | affaan-m 第一条回复很坦白：「**我要么把它设成一条 rule，要么直接用默认的 claude planner，再写一条覆盖同样功能的 rule**」。后来才补上"插件 agent 有 namespace（`everything-claude-code:planner`）"的正式解释，并说「我会考虑在未来版本改名成 `ecc-planner`」 |
| [#641](https://github.com/affaan-m/ECC/issues/641) / [#297](https://github.com/affaan-m/ECC/issues/297) | 装了 ECC 之后**在 CC 里打 `/plan` 会走 ECC 的 plan 命令**，不是原生的 | affaan-m 确认：「装成 plugin 的话 `/plan` 会用 ECC 的 plan 命令」 |
| [#248](https://github.com/affaan-m/ECC/issues/248) | ECC 的 doc blocker hook **拦住了 Claude Code 原生 plan mode 写 `.claude/plans/`** | 已修（whitelist） |
| [#745](https://github.com/affaan-m/ECC/issues/745) @VISDE 2026-03-22 | **目录撞名**：ECC 的 `save-session` 往 `~/.claude/sessions/` 写 `*-session.tmp`，而 Claude Code 自己也用这个目录放 PID lock，启动清理时**静默删掉 ECC 的所有 session 文件**。`/resume-session` 什么都找不到，用户以为自己忘了存 | affaan-m：已改到 `~/.claude/session-data/`，`~/.claude/sessions/` 只留作 legacy 读路径 |
| [#1520](https://github.com/affaan-m/ECC/issues/1520) @itniuma2026 2026-04-21 | cursor target 往 `.cursor/AGENTS.md` 写 **ECC 的自我介绍**，通过 Cursor 的嵌套 AGENTS.md 机制**污染宿主项目上下文** | 已关 |
| [#105](https://github.com/affaan-m/ECC/issues/105) / [#1845](https://github.com/affaan-m/ECC/issues/1845) | **Duplicate hooks 加载警告**——`plugin.json` 里的 hooks 字段和自动加载的 `./hooks/hooks.json` 重复 | 已修（删 plugin.json 的 hooks 字段） |
| [#180](https://github.com/affaan-m/ECC/issues/180) @AhYi8 2026-02-09 | `/everything-claude-code:` 相关插件**每次重启电脑就消失** | 已关 |

👉 一条实用推论：**装 plugin 会静默改变 `/plan` 的行为**。你如果本来就有自己的 plan 工作流（plannotator + ExitPlanMode hook），这是硬冲突。

---

## D. 安装冲突 / 覆盖 / 卸载

### D1. 「Install Rules」其实装了 18 个模块（第三方逐模块列出来打脸）

> **来源**：[Issue #1487](https://github.com/affaan-m/ECC/issues/1487) — @yangswld（第三方）2026-04-18，ECC v1.10.0 / Windows 11
> 标题：`install.ps1 --profile full modifies far more than "rules" — misleading README causes unintended system changes`
> 标签：bug / documentation / Community

他把 `manifests/install-profiles.json` 里 full profile 的 18 个模块逐个列表，指出 **17/18 跟 "rules" 无关**，并给出实测清单：

- `commands-core` 80 个 slash command / `agents-core` 51 个 agent / `hooks-runtime` 30 个 hook 脚本 / `framework-language` 153 个 skill …
- **installer 会改 `~/.claude/settings.json`**，注入 **40 个 hook**（横跨 PreToolUse / PostToolUse / Stop / SessionStart / SessionEnd / PreCompact / PostToolUseFailure）+ 3 个 ECC 环境变量 + 自定义 marketplace 配置。
- **好几个 hook 用 `*` matcher（匹配所有工具调用），也就是每一次工具调用都执行**，对上下文窗口和延迟有显著影响。
- **内容重复**：Step 1 已经通过 plugin 装了 agents/skills/commands（在 `~/.claude/plugins/cache/`），Step 2 的脚本安装又往 `~/.claude/` 根目录复制一遍，**用户手里是两份**。
- **没有回滚路径**：没有 `uninstall.ps1`。

🔑 **这条和你[[02 ECC 隔离方案与污染面清单]] 的实测结论有冲突，必须标出来**：
你的笔记（基于 v2.1.0 代码）说「**全程不碰 `~/.claude/settings.json`**」，而这位用户（v1.10.0 / Windows / `install.ps1`）说 installer 往 settings.json 注入了 40 个 hook。
两种可能：① v1.10.0→v2.1.0 之间行为变了；② `install.ps1`（PowerShell）和 `install.sh`/`install-apply.js`（Node）行为不同。
**置信度：中低（~40%）判断是版本差异**。验证动作：`grep -rn "settings.json" /Users/aa00158/harness-research/ECC/install.ps1`。

**维护者结案**（2026-05-11）：README 现在把 plugin 安装设为默认路径，**明确警告不要叠加 plugin + full install**，并且不再把 full installer 说成"必需的 rules-only 步骤"。

### D2. 覆盖用户自己的文件 —— 反复出现 5 次的老问题

| Issue | 报告人 | 内容 |
|---|---|---|
| [#1505](https://github.com/affaan-m/ECC/issues/1505) @xiaowoonly 2026-04-20 | 第三方 | ECC 用 `fs.copyFileSync` 直接往 `~/.claude/rules/` 和 `~/.claude/skills/` 写，**无冲突检测**。「**每次 ECC 更新都会无警告地覆盖这些目录里的所有文件。如果你在旁边加过自己的 rules/skills，下次更新就静默丢了。**」他的方案：装到 `~/.claude/rules/ecc/` 和 `~/.claude/skills/ecc/` 子目录。**这条 issue 至今 0 条评论**——ECC 采纳了 rules 那一半（今天确实装到 `~/.claude/rules/ecc/`），skills 那一半没采纳 |
| [#1879](https://github.com/affaan-m/ECC/issues/1879) @zomians 2026-05-13 | 第三方 | `rules/README.md` 明确警告不要用 `cp -r .../<lang>/*` 扁平复制，**但 `skills/configure-ecc/SKILL.md` L237-243 和大部分翻译版 README 恰恰在教这个写法**（zh-CN 15 处 / ja-JP 多处 / ko-KR 6 处 / pt-BR 5 处）。照做的结果是 **只剩最后一个语言的 rules，`common/` 也被覆盖，`../common/` 相对引用全断，且全程无报错** |
| [#186](https://github.com/affaan-m/ECC/issues/186) / [#164](https://github.com/affaan-m/ECC/issues/164) / [#432](https://github.com/affaan-m/ECC/issues/432) / [#446](https://github.com/affaan-m/ECC/issues/446) / [#236](https://github.com/affaan-m/ECC/issues/236) | 多人 | 同一根因的重复报告：多语言 rules 扁平安装互相覆盖 |

### D3. 卸载

> [Issue #1425](https://github.com/affaan-m/ECC/issues/1425) — @lklkxcxc 2026-04-14，标题就叫 **「how uninstall ecc」**，正文一句话：
> **「装了 ecc 插件之后用 Claude 编程非常慢。」**

评论里第三方 @chAwater 的原话（**这是本渠道少数几条真正的"用户为什么放弃"的自述**）：

> 我遇到一些 bug，可能是和其它插件冲突导致的（只是感觉，我不想深挖），**而且有很多不必要的功能在吃 token，所以我就把它删了**。你可以用 `node ./scripts/uninstall.js --target claude`……另外记得把 settings 里的 marketplace 也删掉。

维护者给的官方卸载路径：

```bash
ecc uninstall --target claude --dry-run   # 先看
ecc uninstall --target claude             # 再删
# 没有 ecc 在 PATH 时：
node ./scripts/uninstall.js --target claude
```

⚠️ 加一句 affaan-m 自己说的：**如果是 marketplace 装的，还必须在 Claude Code 里 `/plugin` 移除**，否则 plugin bundle 依旧自动加载。手动删 `~/.claude/plugins/...` 只应该是"正常路径已经坏了"的兜底。

### D4. 官方承认的"用户把自己搞乱了"的典型形态

> **来源**：[Issue #1554](https://github.com/affaan-m/ECC/issues/1554) — @affaan-m 自己 2026-04-23（这是维护者对社区反馈的总结，价值很高）

原话（翻译）：

> 最近的用户反馈是一致的：
> - **用户把 plugin 安装和 manual/full 安装叠着装**
> - 用户最后在 `~/.claude` 和项目目录里**有重复的 ECC 表面**
> - 用户不知道已经有 uninstall/reset 路径
> - **用户觉得 hook 很侵入，因为他们不知道怎么干净地只禁用或移除 ECC 管的那层 hook**
> - README 的安装章节在给出简单推荐之前太深、分叉太多

> **来源**：[Issue #1557](https://github.com/affaan-m/ECC/issues/1557) — @affaan-m 2026-04-23，标题 `install: add low-context / no-hooks path and hook opt-out guidance`

> 用户即使技术上照着 README 做也还是被烧到，**因为 ECC 没有为想要最小 footprint 的人定义一条清晰的 low-context、low-hook 安装路径**。
> 用户面的问题：
> - **hook 感觉侵入性强、太全局**
> - 用户复制的 rule pack 比实际需要的多
> - **token/上下文用量增长，因为"安全最小路径"没有说清楚**
> - 想要 skill/command 但不要 hook 的用户，没有清晰记录的 opt-out 方案

⚠️ **#1557 在 2026-04-30 被关闭，但 issue 下 0 条评论、没有说关联到哪个 PR。**「no-hooks 安装路径」是否真的落地，本渠道**无法确认**。验证动作：`grep -rn "no-hooks\|ECC_SKIP_HOOKS\|ECC_HOOK_PROFILE" /Users/aa00158/harness-research/ECC/README.md`。

---

（下一节：反面意见 / 供应链 / 渠道盲区 —— 补充中）
