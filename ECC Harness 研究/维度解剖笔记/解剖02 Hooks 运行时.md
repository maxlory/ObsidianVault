# ECC Hooks 运行时 — 解剖笔记

> 研究对象：`/Users/aa00158/harness-research/ECC`（浅克隆，只读）
> 维度：Hooks 运行时
> 说明：所有论断后附 `路径:行号`。README 的自夸数字若未在代码中找到实现，标注「仓库声称，未在代码中核实」。

## 0. 术语速查（白话）

- **hook（钩子）**：Claude Code 这个 CLI 程序自己提供的机制——在特定时刻（比如"要执行 Bash 之前"）自动运行一条你指定的 shell 命令。这条命令从 stdin 收到一段 JSON（描述当前要执行什么工具、参数是什么），可以往 stdout 吐 JSON、往 stderr 吐文字（文字会作为提示塞回给模型看），并用退出码表态：`0`=放行，`2`=拦截这次工具调用。ECC 全部 hook 都是这一套。规范见 `hooks/README.md:132-162`。
- **hook 事件**：触发时刻的名字。ECC 用到 6 个：`PreToolUse`（工具执行前）、`PostToolUse`（工具执行后）、`PostToolUseFailure`（工具执行失败后）、`Stop`（Claude 一次回复结束时）、`SessionStart` / `SessionEnd`（会话开始/结束）、`PreCompact`（上下文压缩前）。
- **matcher**：这个 hook 对哪些工具生效，是个正则式字符串。`"Bash"` 只管 Bash，`"Edit|Write|MultiEdit"` 管三个，`"*"` 管全部。
- **dispatcher（分派器）**：ECC 自造的一层。原本 N 个检查要注册 N 条 hook，每条起一个 Node 进程；ECC 改成只注册 1 条，由这 1 个进程在内部按顺序调用 N 个检查函数。省的是进程启动开销。
- **profile（严格度档位）**：`minimal` / `standard` / `strict` 三档。每条 hook 在注册时写死"我属于哪几档"，运行时环境变量 `ECC_HOOK_PROFILE` 决定当前是哪档，不匹配就直接跳过。
- **plugin root**：ECC 被安装到磁盘上的那个目录。因为 hook 命令写在 `~/.claude/settings.json` 里、执行时 cwd 是用户项目目录，脚本必须先算出"ECC 装在哪"才能找到自己的文件。

---

## 1. 全景：hooks 在 ECC 里是怎么一回事

### 1.1 三层结构

ECC 的 hook 运行时是三层洋葱，从外到内：

```
[Claude Code 主程序]
   │ 到达事件（PreToolUse/Stop/...），读 settings.json，spawn 一个 shell 命令
   ▼
第 1 层：内联 node -e "..." 引导器（写死在 hooks.json 每条命令里）
   │ 唯一职责：算出 ECC 装在哪（plugin root），设 CLAUDE_PLUGIN_ROOT
   ▼
第 2 层：scripts/hooks/plugin-hook-bootstrap.js  或  内联 spawnSync 版
   │ 唯一职责：用 spawnSync 启动第 3 层，透传 stdin/stdout/stderr/退出码
   ▼
第 3 层：scripts/hooks/run-with-flags.js（统一包装器）
   │ 职责：profile 门禁 + 禁用名单 + dry-run + 路径穿越防护 + 调用真正的 hook
   ▼
第 4 层：真正干活的 hook 脚本（scripts/hooks/*.js，共 50 个文件）
```

证据：`hooks/hooks.json:10`（第 1 层的内联引导器全文）、`scripts/hooks/plugin-hook-bootstrap.js:142-157`（第 2 层 spawnNode）、`scripts/hooks/run-with-flags.js:153-266`（第 3 层 main）。

### 1.2 为什么第 1 层是一坨压成一行的 JS

`hooks/hooks.json:10` 里那条 `node -e "..."` 长达约 1.4 KB，本质是一个查找 ECC 安装目录的函数。它的查找顺序（读 `hooks/hooks.json:10` 内联代码可逐段对应）：

1. 环境变量 `CLAUDE_PLUGIN_ROOT` 有值就直接用；
2. 试 `~/.claude/scripts/lib/resolve-ecc-root.js`（手动安装场景）；
3. 依次试 `~/.claude/plugins/{ecc, ecc@ecc, marketplaces/ecc, everything-claude-code, everything-claude-code@everything-claude-code, marketplaces/everything-claude-code}`（plugin 安装场景的 6 种命名）；
4. 再扫 `~/.claude/plugins/cache/{ecc,everything-claude-code}/*/*/` 两层目录（marketplace 缓存场景）；
5. 全找不到就退回 `~/.claude`。

之所以要内联而不是 `require` 一个文件——因为在还没算出 root 之前，根本不知道那个文件在哪。这是自举（bootstrap）问题的标准解法。

`scripts/lib/resolve-ecc-root.js` 是被 `L(x)` 试探性 require 的那个模块，是判定"这个目录是不是 ECC 根"的真身。

### 1.3 数据怎么流

一次 `PreToolUse: Bash` 的完整数据流（以 `pre:bash:dispatcher` 为例）：

1. Claude Code 把 `{tool_name:"Bash", tool_input:{command:"npm run dev"}, session_id, cwd, ...}` 序列化成 JSON 写进 hook 进程的 stdin；
2. 第 1 层 `node -e` 解析出 root，然后 `require(plugin-hook-bootstrap.js)`——注意它是 **同进程 require**，不是再 spawn，`hooks/hooks.json:10` 末尾 `process.argv.splice(1,0,s);require(s)`；
3. bootstrap 用 `spawnSync` 起一个新 Node 进程跑 `scripts/hooks/pre-bash-dispatcher.js`，把 raw stdin 原样 `input` 进去（`scripts/hooks/plugin-hook-bootstrap.js:149-156`）；
4. dispatcher 内部串行跑各个子检查；
5. 输出沿原路返回：子进程 stdout → bootstrap `passthrough()` → 主进程 stdout → Claude Code。

关键设计：**stdout 必须原样回吐 stdin**，否则 Claude Code 认为 hook 改写了 payload。`scripts/hooks/plugin-hook-bootstrap.js:25-35` 的 `passthrough()` 就是干这个：子进程有 stdout 就用子进程的，没有且退出码为 0 就把原始 raw 吐回去。

---

## 2. hooks/hooks.json — 注册表全文解剖

`hooks/hooks.json` 共 269 行，声明了 **6 个事件、18 条 hook 注册项**。注意"注册项"≠"实际检查数"，因为其中 3 条是 dispatcher，各自内部还包着多个子 hook。

### 2.1 按事件统计

| 事件 | 注册条数 | hook ID | 证据行 |
|---|---|---|---|
| `PreToolUse` | 8 | `pre:bash:dispatcher` / `pre:write:doc-file-warning` / `pre:edit-write:suggest-compact` / `pre:observe:continuous-learning` / `pre:governance-capture` / `pre:config-protection` / `pre:mcp-health-check` / `pre:edit-write:gateguard-fact-force` | `hooks/hooks.json:4-98` |
| `PreCompact` | 1 | `pre:compact` | `hooks/hooks.json:99-111` |
| `SessionStart` | 2 | `session:start` / `session-start:plan-canvas-sessions` | `hooks/hooks.json:112-135` |
| `PostToolUse` | 2 | `post:dispatcher:sync` / `post:dispatcher:async` | `hooks/hooks.json:136-162` |
| `PostToolUseFailure` | 1 | `post:mcp-health-check` | `hooks/hooks.json:163-175` |
| `Stop` | 6 | `stop:format-typecheck` / `stop:check-console-log` / `stop:session-end` / `stop:evaluate-session` / `stop:cost-tracker` / `stop:desktop-notify` | `hooks/hooks.json:176-252` |
| `SessionEnd` | 1 | `session:end:marker` | `hooks/hooks.json:253-267` |
| **合计** | **21** | | |

（上表 8+1+2+2+1+6+1 = 21 条注册项；README `hooks/README.md:38-73` 的表格是按"用户可感知的功能"列的，数量对不上是因为 dispatcher 把多个功能折叠成 1 条。）

**注意 ECC 没有注册 `UserPromptSubmit`**——`hooks/hooks.json` 全文 grep 无此事件。README 主文档里提到的一些"提示注入"能力不走 hook。这点后面第 9 节还会再核。

### 2.2 两种命令封装形态

hooks.json 里的命令有两种写法，差别在于 **Stop / SessionEnd 事件用了内联 spawnSync 版，其余用 plugin-hook-bootstrap 版**：

- **A 型（bootstrap 版）**，用于 PreToolUse / PreCompact / SessionStart / PostToolUseFailure：
  `node -e "<root 解析>; require('.../plugin-hook-bootstrap.js')" node scripts/hooks/run-with-flags.js <hookId> <script> <profiles>`
  见 `hooks/hooks.json:21`、`:32`、`:43`、`:56`、`:68`、`:80`、`:91`、`:105`、`:118`、`:129`、`:169`。

- **B 型（内联 spawnSync 版）**，用于 Stop / SessionEnd：
  `node -e "…读 stdin…spawnSync(node, [run-with-flags.js, hookId, script, profiles], {timeout: N, maxBuffer:16MB})…"`
  见 `hooks/hooks.json:182`（timeout 300000ms = 5 分钟）、`:194`、`:205`、`:218`、`:231`、`:244`、`:259`。

B 型多做了两件事：① 明确设了 `maxBuffer: 16*1024*1024`（16 MB，防止 tsc 输出撑爆默认 1 MB 缓冲）；② 显式处理"spawn 失败"（`result.error || status===null || signal`）时打 `[Stop] ERROR:` 并 exit 1；③ 找不到 root 时打 `[Stop] WARNING: could not resolve ECC plugin root; skipping hook` 然后原样吐回 stdin、exit 0（fail-open）。

B 型的 `stop:format-typecheck` 单条超时是 **300 秒**（`hooks/hooks.json:183` 的 `"timeout": 300` + 内联 `timeout:300000`）——这是全仓最长的 hook 超时，见第 10 节风险评估。

### 2.3 profile 标签怎么写在注册项里

每条 A/B 型命令的最后一个参数就是这条 hook 允许的 profile 列表，逗号分隔：

- `standard,strict`：绝大多数（doc-file-warning、suggest-compact、observe、governance-capture、config-protection、mcp-health-check、gateguard、pre-compact、plan-canvas、stop:format-typecheck、stop:check-console-log、stop:desktop-notify）
- `minimal,standard,strict`：4 条，即三档全开——`stop:session-end`（`hooks/hooks.json:205`）、`stop:evaluate-session`（`:218`）、`stop:cost-tracker`（`:231`）、`session:end:marker`（`:259`）
- **hooks.json 里没写 profile 参数的两条**：
  - `pre:bash:dispatcher`（`hooks/hooks.json:10`）——直接跑 `pre-bash-dispatcher.js`，**真的绕过 run-with-flags**，门禁下沉到 `bash-hook-dispatcher.js:135` 里每个子 hook 自己判断。
  - `session:start`（`hooks/hooks.json:118`）——跑 `session-start-bootstrap.js`，它**在脚本内部补上了参数**：`spawnSync(node, [run-with-flags.js, 'session:start', 'scripts/hooks/session-start.js', 'minimal,standard,strict'])`（`scripts/hooks/session-start-bootstrap.js:45-55`）。所以它仍然过 run-with-flags，只是白名单是三档全开。

这是一个容易看漏的点：`ECC_HOOK_PROFILE=minimal` **不会**关掉 Bash 前置分派器（它没有 profile 概念）和 SessionStart（它三档全开）。要关只能用 `ECC_DISABLED_HOOKS=session:start`。

### 2.4 async 与 timeout 一览

| hook ID | async | timeout(秒) | 证据 |
|---|---|---|---|
| `pre:bash:dispatcher` | 否 | 未声明（用 CC 默认） | `hooks/hooks.json:8-14` |
| `pre:write:doc-file-warning` | 否 | 未声明 | `:19-25` |
| `pre:edit-write:suggest-compact` | 否 | 未声明 | `:30-36` |
| `pre:observe:continuous-learning` | **是** | 10 | `:44-45` |
| `pre:governance-capture` | 否 | 10 | `:57` |
| `pre:config-protection` | 否 | 5 | `:69` |
| `pre:mcp-health-check` | 否 | 未声明 | `:79-84` |
| `pre:edit-write:gateguard-fact-force` | 否 | 5 | `:92` |
| `pre:compact` | 否 | 未声明 | `:104-109` |
| `session:start` | 否 | 未声明 | `:117-122` |
| `session-start:plan-canvas-sessions` | 否 | 未声明 | `:128-133` |
| `post:dispatcher:sync` | 否 | 30 | `:143` |
| `post:dispatcher:async` | **是** | 45 | `:155-156` |
| `post:mcp-health-check` | 否 | 未声明 | `:168-173` |
| `stop:format-typecheck` | 否 | **300** | `:183` |
| `stop:check-console-log` | 否 | 未声明（内联 30000ms） | `:194` |
| `stop:session-end` | **是** | 10 | `:206-207` |
| `stop:evaluate-session` | **是** | 10 | `:219-220` |
| `stop:cost-tracker` | **是** | 10 | `:232-233` |
| `stop:desktop-notify` | **是** | 10 | `:245-246` |
| `session:end:marker` | **是** | 10 | `:260-261` |

🔑 **同步阻塞的关键路径只有 3 处**：`pre:bash:dispatcher`（每次 Bash）、`post:dispatcher:sync`（每次任意工具，30s 上限）、`stop:format-typecheck`（每次回复结束，300s 上限）。其余要么 async 要么小超时。

## 3. 逐个 hook ID 与作用（核心表格）

把 hooks.json 的 21 条注册项 + dispatcher 内部的 13 个子 hook 摊平，**实际存在 31 个独立 hook ID**（`post:bash:dispatcher` 是包装层不重复计）。全部用 Node.js CommonJS 实现，唯一的 Python 文件 `insaits-security-monitor.py` 未被注册（见 3.4）。

### 3.1 PreToolUse（工具执行前，能拦截）

| # | hook ID | matcher | profile | 实现文件 | 具体干什么 | 能拦截? |
|---|---|---|---|---|---|---|
| 1 | `pre:bash:block-no-verify` | Bash | **minimal,standard,strict**（全档） | `scripts/hooks/block-no-verify.js`（546 行） | 检测 `git commit --no-verify` / `-c core.hooksPath=` 等绕过 git hook 的写法。覆盖 commit/push/merge/cherry-pick/rebase/am 等命令（`block-no-verify.js:25-31`）。目的是防止 AI 自己跳过项目的 pre-commit 检查 | **是，exit 2**（`block-no-verify.js:12-14`） |
| 2 | `pre:bash:auto-tmux-dev` | Bash | standard,strict | `auto-tmux-dev.js`（116 行） | 检测 `npm run dev`/`pnpm dev` 等开发服务器命令，**改写命令**让它在 detached tmux session 里跑（Windows 则开新 cmd 窗口），这样服务器不会占死 Claude 的 Bash 通道。先 `spawnSync('which',['tmux'])` 探测（`auto-tmux-dev.js:72`），没 tmux 就原样放行 | 不拦截，但**改写命令** |
| 3 | `pre:bash:tmux-reminder` | Bash | **strict only** | `pre-bash-tmux-reminder.js`（61 行） | 命中 `npm install/test`、`cargo build`、`docker`、`pytest`、`vitest`、`playwright` 且当前不在 tmux 里（`!process.env.TMUX`）时，回一条 additionalContext 建议用 tmux（`:14-25`） | 否，纯提示 |
| 4 | `pre:bash:git-push-reminder` | Bash | **strict only** | `pre-bash-git-push-reminder.js`（56 行） | 命中 `git push` 就吐一句"推送前先看 diff"。**纯装饰**——它自己承认 "Continuing with push"（`:17`） | 否 |
| 5 | `pre:bash:commit-quality` | Bash | **strict only** | `pre-bash-commit-quality.js`（484 行） | `git commit` 之前：`git diff --cached --name-only` 取暂存文件（`:29`）→ `git show :<file>` 读暂存内容（`:40`）→ 跑 linter、检 console.log/debugger/硬编码 secret、校验 commit message 格式 | **是，严重问题 exit 2** |
| 6 | `pre:bash:gateguard-fact-force` | Bash | standard,strict | `gateguard-fact-force.js`（1278 行） | Bash 分支：破坏性命令要求先列出目标+回滚方案+复述用户指令；常规命令每 session 复述一次指令 | **是，exit 2** |
| 7 | `pre:write:doc-file-warning` | Write | standard,strict | `doc-file-warning.js`（108 行） | 黑名单式：只对 `NOTES/TODO/SCRATCH/TEMP/DRAFT/BRAINSTORM/SPIKE/DEBUG/WIP.(md\|txt)` 且不在 `docs/ .claude/ .github/ commands/ skills/ benchmarks/ templates/ .history/ memory/` 里的路径告警（`doc-file-warning.js:23-26`）。防 AI 到处丢草稿文件 | 否，永远 exit 0（`:11`） |
| 8 | `pre:edit-write:suggest-compact` | Edit\|Write | standard,strict | `suggest-compact.js`（271 行） | 两个信号提醒手动 `/compact`：① 工具调用计数，首次 50 次然后每 25 次；② **上下文体量**——读 transcript 里最后一条 assistant 的 `usage`，与窗口比例阈值比（200k 窗口默认 160k，1M 窗口 250k），之后每增长 60k 再提醒一次（`suggest-compact.js:16-23`） | 否 |
| 9 | `pre:observe:continuous-learning` | `*` | standard,strict | `observe-runner.js`（199 行） | **async**。委托给 `skills/continuous-learning-v2/hooks/observe.sh`，用 spawnSync 起 shell，默认超时 9000ms（`observe-runner.js:8-9`）。Windows 下有 shell 探测 + 找不到就跳过的兜底 | 否 |
| 10 | `pre:governance-capture` | Bash\|Write\|Edit\|MultiEdit | standard,strict | `governance-capture.js`（334 行） | 扫工具输入里的硬编码 secret（AWS AKIA/ASIA、generic secret=、PEM 私钥头、JWT、`ghp_/gho_/ghu_/ghs_/ghr_` GitHub token，`governance-capture.js:26-32`），写进 state store 的 `governance_events` 表。**默认关闭，要 `ECC_GOVERNANCE_CAPTURE=1` 才启用**（`:15`） | 未验证（需读 334 行全文确认是否 exit 2） |
| 11 | `pre:config-protection` | Write\|Edit\|MultiEdit | standard,strict | `config-protection.js`（176 行） | 拦住对 `.eslintrc*`、`eslint.config.*`、prettier/biome 等 linter/formatter 配置文件的**修改**（新建允许）。理由：AI 常改配置让检查通过而不是改代码（`config-protection.js:5-8`） | **是，exit 2**（`:9-11`） |
| 12 | `pre:mcp-health-check` | `*` | standard,strict | `mcp-health-check.js`（749 行） | 调 MCP 工具前探测对应 MCP server 健康度。**唯一真正发网络请求的 hook**：`require('http')/require('https')`（`:18-19`），对 HTTP MCP endpoint 发探测请求，默认超时 5000ms、缓存 TTL 2 分钟、失败退避 30s→10min（`:23-26`）。stdio MCP 则 `spawn` 子进程探（`:399-406`），Windows 用 `taskkill /T /F` 清理（`:471`） | 声称能 block unhealthy MCP calls（`hooks/hooks.json:83`） |
| 13 | `pre:edit-write:gateguard-fact-force` | Edit\|Write\|MultiEdit | standard,strict | `gateguard-fact-force.js`（1278 行） | **ECC 最有攻击性的 hook**。对每个文件的第一次 Edit/Write/MultiEdit 直接 exit 2 拦下，要求模型先交出事实：谁 import 了这个文件、影响哪些公开 API、数据 schema 是什么、复述用户的原话（`gateguard-fact-force.js:3-16`）。状态存 `~/.gateguard/`（可用 `GATEGUARD_STATE_DIR` 改），30 分钟无活动过期，最多 500 条 checked 记录 / 50 个 session key（`:31-41`）。上游是 `github.com/zunoworks/gateguard`（`:20-21`） | **是，exit 2** |

### 3.2 PostToolUse（工具执行后，不能拦截）

同步组（`post:dispatcher:sync`，30s 超时）：

| # | hook ID | matcher | profile | 实现 | 干什么 |
|---|---|---|---|---|---|
| 14 | `post:edit:design-quality-check` | Edit\|Write\|MultiEdit | standard,strict | `design-quality-check.js`（131 行） | 前端文件（`.astro/.css/.html/.jsx/.scss/.svelte/.tsx/.vue`）里出现 "Get Started"、"Learn more"、`grid-cols-3/4`、`bg-gradient-to-*`、`text-center`、`font-sans/inter` 等"一眼 AI 模板"的信号就告警（`design-quality-check.js:18-25`）。注释明确说不调远程模型、不装包（`:4-6`） |
| 15 | `post:edit:accumulator` | Edit\|Write\|MultiEdit | standard,strict | `post-edit-accumulator.js`（78 行） | 把编辑过的 JS/TS 路径 `appendFileSync` 追加到 session 级临时文件（一行一条），供 Stop 时批量格式化。用 append 而非 write 是为了并发安全，去重延到 Stop 做（`:11-13`） |
| 16 | `post:edit:console-warn` | Edit | standard,strict | `post-edit-console-warn.js`（65 行） | 读刚编辑的 `.ts/.tsx/.js/.jsx` 文件，找 console.log 并报行号 |
| 17 | `post:governance-capture` | Bash\|Write\|Edit\|MultiEdit | standard,strict | `governance-capture.js` | 同 #10，后置版（扫 tool_output 里的 secret） |
| 18 | `post:session-activity-tracker` | `*` | standard,strict | `session-activity-tracker.js`（639 行） | 每次工具调用后往 `~/.claude/metrics/tool-usage.jsonl` 追加一条脱敏记录（`:5-6`），供 ECC2 指标同步。会 `spawnSync('git', ...)` 取仓库信息（`:345`） |
| 19 | `post:ecc-metrics-bridge` | `*` | **minimal,standard,strict**（全档） | `ecc-metrics-bridge.js`（284 行） | 在 `/tmp/ecc-metrics-{session}.json` 维护一份会话聚合（token、成本、改过的文件、最近 5 个工具）。存在的意义是让 statusline 和 context-monitor 不用每次扫大 JSONL（`:5-7`）。最多跟踪 200 个文件（`:20`），原子写（tmp+rename，`:125-126`） |
| 20 | `post:ecc-context-monitor` | `*` | standard,strict | `ecc-context-monitor.js`（286 行） | 读 #19 的 bridge 文件，越过阈值就往上下文注入警告：剩余上下文 <35% 提醒 / <25% 危急、成本 >$5 / >$10 / >$50 三档、改动文件 >20 个算 scope creep、同一工具重复 3 次算 loop（`:19-25`）。`ECC_CONTEXT_MONITOR_COST_WARNINGS=off` 只关成本那档 |

异步组（`post:dispatcher:async`，45s 超时，`async:true`）：

| # | hook ID | matcher | profile | 实现 | 干什么 |
|---|---|---|---|---|---|
| 21 | `post:bash:command-log-audit` | Bash | standard,strict | `post-bash-command-log.js`（80 行） | 往 `bash-commands.log` 追加 `[ISO时间] <命令>`。写入前脱敏：`--token=` 和 `Authorization:` 替换成 `<REDACTED>`（`:24-27`） |
| 22 | `post:bash:command-log-cost` | Bash | standard,strict | 同上，mode='cost' | 往 `cost-tracker.log` 追加 `[时间] tool=Bash command=...` |
| 23 | `post:bash:pr-created` | Bash | standard,strict | `post-bash-pr-created.js`（60 行） | 命中 `gh pr create` 就从输出里正则抓 PR URL，回一句 `gh pr review <n> --repo <r>` 提示（`:12-27`） |
| 24 | `post:bash:build-complete` | Bash | standard,strict | `post-bash-build-complete.js`（49 行） | 命中 `npm run build`/`pnpm build`/`yarn build` 就打一句"Build completed - async analysis running in background"。**注意：它并没有真的做任何分析**，只是打了这句话（`:10-16` 全部逻辑） |
| 25 | `post:quality-gate` | Edit\|Write\|MultiEdit | standard,strict | `quality-gate.js`（168 行） | 按语言跑轻量检查：JS/TS 用 Biome（若 `post-edit-format.js` 已跑过就跳过）、`.json/.md` 用 Biome、Prettier、Go 用 `gofmt -w`/`-l`、Python 用 `ruff`。全是 spawnSync 外部进程 |
| 26 | `post:observe:continuous-learning` | `*` | standard,strict | `observe-runner.js` | 同 #9 的后置版 |

### 3.3 其余生命周期事件

| # | hook ID | 事件 | profile | 实现 | 干什么 |
|---|---|---|---|---|---|
| 27 | `post:mcp-health-check` | PostToolUseFailure | standard,strict | `mcp-health-check.js` | 工具调用失败时，用正则从错误里认 401/403/429/503/ECONNREFUSED 等模式（`mcp-health-check.js:34-40`），把对应 MCP server 标为不健康、尝试重连、重探 |
| 28 | `pre:compact` | PreCompact | standard,strict | `pre-compact.js`（178 行） | 压缩前**调 `claude -p` 生成一份 LLM 摘要**写进 session 临时文件，让下个 session 拿到高质量摘要（`pre-compact.js:6-12`、`:156`）。失败就退回纯日志 |
| 29 | `session:start` | SessionStart | **minimal,standard,strict**（参数由 bootstrap 脚本补，不在 hooks.json 里，见 `session-start-bootstrap.js:47`） | `session-start-bootstrap.js`（85 行）→ run-with-flags → `session-start.js`（772 行，无 `module.exports` 所以走 spawnSync 慢路径 = **共 3 次 Node 进程启动**） | 载入最近的 session 摘要、列出可用 session 与已学 skill、检测包管理器与项目类型、注入"instincts"（学到的经验）。默认最多注入 6 条 instinct、置信度阈值 0.7、总字符上限 8000（`session-start.js:29-34`）。还会 `fs.rmSync` 清理超过 30 天的旧 session 文件（`:235`、`:34`） |
| 30 | `session-start:plan-canvas-sessions` | SessionStart | standard,strict | `plan-canvas-sessions.js`（68 行） | 如果上个会话还有没关的 Plan Canvas 浏览器评审，就在新会话开头提示可以 `plan-canvas await <file>` 续上。声明"永不阻塞，任何错误都 exit 0"（`:11-12`） |
| 31 | `stop:format-typecheck` | Stop | standard,strict | `stop-format-typecheck.js`（226 行） | 读 #15 累积的文件清单，按项目根分组跑一次 formatter、按 tsconfig 目录分组跑一次 `tsc --noEmit`，然后删掉累积文件（`:146`）。单批超时按批数摊分，总量给 Stop 预算留 90s 余量（`:13-14`）。**注册超时 300 秒** |
| 32 | `stop:check-console-log` | Stop | standard,strict | `check-console-log.js`（90 行） | `git` 查本次改动的文件，找 console.log。排除 `.test./.spec./.config.` 和 `scripts/`（`:20-24`） |
| 33 | `stop:session-end` | Stop | **minimal,standard,strict** | `session-end.js`（336 行） | 从 transcript 抽会话摘要写 session 文件。**触发 LLM**：剩余上下文 <20% 或每 50 条用户消息，就调 `claude -p`（`session-end.js:225-243`） |
| 34 | `stop:evaluate-session` | Stop | **minimal,standard,strict** | `evaluate-session.js`（100 行） | 从 transcript 里提取可复用模式，写进 learned skills 目录。注释解释为什么放 Stop 而不是 UserPromptSubmit：后者每条消息都跑太重（`:10-13`） |
| 35 | `stop:cost-tracker` | Stop | **minimal,standard,strict** | `cost-tracker.js`（239 行） | 读 transcript JSONL，把所有 assistant turn 的 usage 加总，往 `~/.claude/metrics/costs.jsonl` 追加一行。注释里有一段坦白的 bug 复盘：旧版以为 Stop payload 里有 `usage`，实际没有，导致 52 天里 2340 行全是 0（`cost-tracker.js:9-13`） |
| 36 | `stop:desktop-notify` | Stop | standard,strict | `desktop-notify.js`（261 行） | 桌面通知。macOS 优先用 iTerm2 转义序列直接写 tty（`fs.writeFileSync(tty, '\x1b]9;...')`，`:182`），否则 `osascript`（超时 5000ms，`:192`）；WSL 走 PowerShell + BurntToast |
| 37 | `session:end:marker` | SessionEnd | **minimal,standard,strict** | `session-end-marker.js`（67 行） | 清理 observer session lease、停掉 observer，然后原样吐回 stdin |

### 3.4 存在于 scripts/hooks/ 但**没有被 hooks.json 注册**的脚本

这批文件容易误以为在跑，实际不在默认 hook 图里：

| 文件 | 状态 | 证据 |
|---|---|---|
| `pre-bash-dev-server-block.js`（229 行） | **只被测试引用**，hooks.json 无注册。README `hooks/README.md:42` 还把它列成"Dev server blocker"表格第一行——文档与实现脱节 | `tests/hooks/hooks.test.js:1870-1923` |
| `post-edit-format.js`（109 行） | hooks.json 未注册；被 **Cursor 适配层** `.cursor/hooks/after-tab-file-edit.js:9` 调用。Claude Code 侧这份工作被合进了 `stop:format-typecheck` | `.cursor/hooks/after-tab-file-edit.js:9` |
| `post-edit-typecheck.js`（96 行） | 只被测试引用，同样被 Stop 批处理取代 | `tests/hooks/hooks.test.js:1937-1963` |
| `insaits-security-wrapper.js`（119 行）+ `insaits-security-monitor.py`（269 行） | 未注册。Python 的安全监控，靠 `ECC_ENABLE_INSAITS` 开关，只有测试引用 | `tests/hooks/insaits-security-wrapper.test.js` |
| `ecc-statusline.js`（168 行） | 不是 hook，是 statusline 渲染器 | `tests/hooks/ecc-statusline.test.js` |
| `cursor-session-env.js`（50 行） | Cursor 专用，注册在 `scaffolds/cursor/hooks.json:6` | `scaffolds/cursor/hooks.json:6` |
| `check-hook-enabled.js`（12 行） | CLI 小工具，给外部脚本查某个 hook 当前是否启用 | `tests/hooks/check-hook-enabled.test.js` |
| `pre-write-doc-warn.js`（10 行） | 向后兼容 shim，转发到 `doc-file-warning.js` | `tests/hooks/doc-file-warning.test.js:249-251` |
| `pretooluse-visible-output.js`（41 行） | 不是 hook，是共享工具函数（把 additionalContext 包成 Claude Code 的 hookSpecificOutput JSON） | 被 `run-with-flags.js:15` 等引用 |
| `run-with-flags-shell.sh`（36 行） | shell 版包装器，hooks.json 未用 | grep 全文无引用 |

🔑 **README 的 hook 表和实际注册表对不上**。`hooks/README.md:38-73` 那两张表描述的是**旧架构**（Dev server blocker、Prettier format、TypeScript check 各自独立注册），而 `hooks/hooks.json` 已经改成 dispatcher + Stop 批处理了。看 README 会得到错误的运行时图像。

## 4. run-with-flags.js — 统一包装器机制

文件：`/Users/aa00158/harness-research/ECC/scripts/hooks/run-with-flags.js`（271 行）

调用签名：`node run-with-flags.js <hookId> <scriptRelativePath> [profilesCsv]`（`scripts/hooks/run-with-flags.js:5-7`）

### 4.1 执行顺序（按 main() 逐步）

`scripts/hooks/run-with-flags.js:153-266`：

1. **读 stdin，上限 1 MB**（`:17` `MAX_STDIN = 1024*1024`，`:19-38`）。超了就置 `truncated=true` 并停止累积。
2. **超长保护**：如果截断了，绝不把截断后的字符串回吐 stdout——因为半截 JSON 会被 harness 判为 hook 失败从而拦住工具调用（代码注释直接引了 issue #2222，`:157-166`）。此时输出空串 + exit 0（"没意见"，fail-open）。
3. **参数缺失** → 原样回吐 + exit 0（`:168-171`）。
4. **profile / 禁用名单门禁**：`isHookEnabled(hookId, {profiles})`，不通过就原样回吐 + exit 0（`:173-176`）。
5. **dry-run**：`ECC_DRY_RUN=1` 时只往 stderr 打一行 `[DryRun] Hook "<id>" would execute: <script> (enabled=true, profiles=...) tool=... target=... command=...`，不真跑（`:178-183`、`:136-151`）。
6. **路径穿越防护**：`scriptPath` 必须以 `resolvedRoot + path.sep` 开头，否则拒绝（`:189-194`）。
7. **文件不存在** → stderr 提示 + 原样回吐 + exit 0（`:196-200`）。
8. **快路径 vs 慢路径**（性能优化的核心）：
   - 读脚本源码，正则判断是否同时含 `module.exports` 和 `run`（`:209-210`）；
   - 是 → 直接 `require(scriptPath)` 在**同进程**里调 `hookModule.run(raw, ctx)`，省掉一次 Node 进程启动，注释自估 **~50-100ms/hook**（`:203-204`，仓库自估，非实测）；
   - 否 → 回落到 `spawnSync(node, [scriptPath])` 起子进程，硬超时 30000ms（`:240-253`）。
9. **结果规约**：`resolveHookResult()`（`:71-90`）支持三种返回形态——字符串/Buffer 直接当 stdout；对象可带 `{stderr, exitCode, additionalContext | stdout}`；`additionalContext` 会被包成 Claude Code 的 `hookSpecificOutput` JSON（`scripts/hooks/pretooluse-visible-output.js`）。
10. **排干后再退出**：`exitWithStdout()`（`:54-69`）先 `process.stdout.write(text, cb)` 等回调，再 `process.exit()`。注释说明原因：直接 exit 会把超过 OS 管道缓冲的输出截掉，导致 harness 判 hook 失败（同样引 #2222，`:48-53`）。

### 4.2 快路径的判定是"正则匹配源码"，不是真的检查导出

`:210` 的判定条件是 `/\bmodule\.exports\b/.test(src) && /\brun\b/.test(src)`——只要源码里出现过 `run` 这个词就算数，之后再用 `typeof hookModule.run === 'function'` 兜底（`:221`）。这个宽松判定的代价是：不导出 `run` 但源码提到 `run` 的脚本会被白白 `require` 一次，副作用（模块顶层的 stdin 监听、`process.exit`）可能在父进程里跑起来。代码注释自己点了这个风险（`:204-207` SAFETY 段），但正则并没有真正防住——**这是一个潜在的坑**（详见第 12 节）。

### 4.3 谁在用 run() 快路径

实测统计（`grep 'module.exports' scripts/hooks/*.js`）：50 个脚本里 **38 个有 `module.exports`**。明确导出 `run` 的包括 `doc-file-warning.js`、`config-protection.js`、`gateguard-fact-force.js`、`suggest-compact.js`、`observe-runner.js`、`governance-capture.js`、`stop-format-typecheck.js`、`desktop-notify.js`、`session-end-marker.js`、`plan-canvas-sessions.js` 等。

**不导出 run（走 spawnSync 慢路径）**的有：`check-console-log.js`、`cost-tracker.js`、`evaluate-session.js`、`mcp-health-check.js`、`session-end.js`、`session-start.js`、`pre-bash-dev-server-block.js`、`post-edit-typecheck.js`（`grep -c module.exports` 结果为 0）。

### 4.4 run-with-flags-shell.sh

`scripts/hooks/run-with-flags-shell.sh`（36 行）是 shell 版的同名包装器，但 **hooks.json 里没有任何一条使用它**（grep 全文无 `.sh` 引用）。属于给第三方 shell hook 预留的能力。

---

## 5. dispatcher 机制（pre-bash / post-bash / posttooluse）

ECC 把"N 条 hook 注册项 = N 个 Node 进程"折叠成"1 条注册 = 1 个进程内跑 N 个函数"。三个 dispatcher：

### 5.1 bash-hook-dispatcher.js（Bash 的前后置总管）

文件：`scripts/hooks/bash-hook-dispatcher.js`（211 行）。它导出 `runPreBash` / `runPostBash`，同一份编排代码跑两组清单。

**PRE_BASH_HOOKS 清单**（`scripts/hooks/bash-hook-dispatcher.js:22-52`），按数组顺序串行：

| 序 | 子 hook ID | profile 标签 | 实现文件 |
|---|---|---|---|
| 1 | `pre:bash:block-no-verify` | `minimal,standard,strict` | `block-no-verify.js`（546 行） |
| 2 | `pre:bash:auto-tmux-dev` | 未标（默认 `standard,strict`） | `auto-tmux-dev.js` |
| 3 | `pre:bash:tmux-reminder` | **`strict` only** | `pre-bash-tmux-reminder.js` |
| 4 | `pre:bash:git-push-reminder` | **`strict` only** | `pre-bash-git-push-reminder.js` |
| 5 | `pre:bash:commit-quality` | **`strict` only** | `pre-bash-commit-quality.js`（484 行） |
| 6 | `pre:bash:gateguard-fact-force` | `standard,strict` | `gateguard-fact-force.js`（1278 行） |

未标 `profiles` 的走 `parseProfiles` 的默认 fallback `['standard','strict']`（`scripts/lib/hook-flags.js:35`）。

**POST_BASH_HOOKS 清单**（`:54-73`）：

| 序 | 子 hook ID | profile | 实现 |
|---|---|---|---|
| 1 | `post:bash:command-log-audit` | 默认 standard,strict | `post-bash-command-log.js`（mode='audit'） |
| 2 | `post:bash:command-log-cost` | 默认 standard,strict | `post-bash-command-log.js`（mode='cost'） |
| 3 | `post:bash:pr-created` | standard,strict | `post-bash-pr-created.js` |
| 4 | `post:bash:build-complete` | standard,strict | `post-bash-build-complete.js` |

**短路语义**：`runHooks()`（`:123-174`）遇到某个子 hook 返回非 0 exitCode 就**立刻停止后续子 hook 并把该退出码往上抛**（`:151-158`）。所以 `block-no-verify` 排第一位不是偶然——最硬的拦截优先。

**异常语义**：子 hook 抛异常不会中断链条，只往 stderr 加一行 `[Hook] <id> failed: <msg>` 继续（`:159-161`）。这是 fail-open 设计。

**stdout 语义的坑**：注释 `:125-129` 明说——把未修改的原始 input event 原样吐 stdout 会**过不了 Claude Code 的 hook 输出 JSON schema 校验**（报 `(root): Invalid input`），所以 pass-through 情况必须吐**空串**。这与 run-with-flags 的"原样回吐"策略正好相反，两条路径的约定不一致，是理解 ECC hook 输出行为时最容易混淆的地方。

### 5.2 posttooluse-dispatcher.js（所有工具的后置总管）

文件：`scripts/hooks/posttooluse-dispatcher.js`（294 行）。用 `process.argv[2]` 决定跑 sync 还是 async 清单（`:251`）。

**SYNC_HOOKS**（`:25-33`，注册为 `post:dispatcher:sync`，timeout 30s，非 async）：

| 序 | 子 hook ID | matcher | profile | 实现 |
|---|---|---|---|---|
| 1 | `post:edit:design-quality-check` | `Edit\|Write\|MultiEdit` | standard,strict | `design-quality-check.js` |
| 2 | `post:edit:accumulator` | `Edit\|Write\|MultiEdit` | standard,strict | `post-edit-accumulator.js` |
| 3 | `post:edit:console-warn` | `Edit` | standard,strict | `post-edit-console-warn.js` |
| 4 | `post:governance-capture` | `Bash\|Write\|Edit\|MultiEdit` | standard,strict | `governance-capture.js` |
| 5 | `post:session-activity-tracker` | `*` | standard,strict | `session-activity-tracker.js`（639 行） |
| 6 | `post:ecc-metrics-bridge` | `*` | **minimal,standard,strict** | `ecc-metrics-bridge.js` |
| 7 | `post:ecc-context-monitor` | `*` | standard,strict | `ecc-context-monitor.js` |

**ASYNC_HOOKS**（`:35-49`，注册为 `post:dispatcher:async`，timeout 45s，`async:true`）：

| 序 | 子 hook ID | matcher | profile | 实现 |
|---|---|---|---|---|
| 1 | `post:bash:dispatcher` | `Bash` | minimal,standard,strict | 内部调 `runPostBash()`（即 5.1 的 POST 清单 4 项） |
| 2 | `post:quality-gate` | `Edit\|Write\|MultiEdit` | standard,strict | `quality-gate.js` |
| 3 | `post:observe:continuous-learning` | `*` | standard,strict | `observe-runner.js` |

**matcher 在 dispatcher 内部二次过滤**：`matchesTool()`（`:55-64`）从 stdin JSON 取 `tool_name`，按 `|` 拆分精确比对。注意它是**精确字符串包含**而非正则——`matcher: 'Edit'` 不会匹配 `MultiEdit`。这与 Claude Code 原生 matcher（正则）语义不同，但因为清单是 ECC 自己写的，影响可控。

**非 0 退出不短路**：与 bash dispatcher 不同，PostToolUse dispatcher 里某个子 hook 返回非 0 只记录第一个非 0 码并**继续跑剩下的**（`:202-205`）。合理——PostToolUse 本来就不能拦截。

**多个子 hook 同时想输出 stdout 怎么办**：`mergeHookStdout()`（`:146-172`）。如果所有输出都是合法的 `hookSpecificOutput/additionalContext` JSON，就把 additionalContext 用换行拼起来合并；否则**只保留最后一个，丢弃前面的**并打警告 `[Hook] stdout from X dropped in favor of Y; raw stdout cannot be merged`。也就是说非标准输出的子 hook 会互相踩。

**passthrough 开关**：`ECC_POSTTOOLUSE_PASSTHROUGH=1` 时，如果没有任何子 hook 产出 stdout 且退出码 0 且没截断，就把原始 raw 回吐（`:244-248`、`:267-270`）。hooks.json 的两条注册命令都在内联里显式设了这个变量（`hooks/hooks.json:142`、`:154`）。

### 5.3 pre-bash-dispatcher.js / post-bash-dispatcher.js

这两个是极薄的 stdin 适配器，各 24 行：
- `scripts/hooks/pre-bash-dispatcher.js:17-24`：读 stdin → `runPreBash(raw)` → 写 stderr/stdout → 设 exitCode。
- `scripts/hooks/post-bash-dispatcher.js`：同构，调 `runPostBash`。

`pre-bash-dispatcher.js` 是 hooks.json 里**唯一一条不经过 run-with-flags 的 PreToolUse 注册**（`hooks/hooks.json:10` 末尾直接 `node scripts/hooks/pre-bash-dispatcher.js`），profile 门禁完全靠内部每个子 hook 自己的 `isHookEnabled`。

## 6. hook strictness profile（ECC_HOOK_PROFILE）

### 6.1 机制本体：它就是一个字符串比对

"strictness profile" 听着像个复杂的策略引擎，实际实现只有 `scripts/lib/hook-flags.js` 里 13 行代码（`:57-69`）：

```
isHookEnabled(hookId, {profiles}) =
    hookId 不在 ECC_DISABLED_HOOKS 里
 && parseProfiles(profiles).includes(getHookProfile())
```

- `getHookProfile()`（`:18-21`）：读 `ECC_HOOK_PROFILE`，转小写去空格，不在 `{minimal, standard, strict}` 里就一律当 `standard`。**没有报错，静默降级**——写错成 `ECC_HOOK_PROFILE=stict` 得到的是 standard，不是报错。
- `parseProfiles(rawProfiles, fallback=['standard','strict'])`（`:35-51`）：把注册时的 `"standard,strict"` 拆成数组；**没写 profiles 参数的 hook 默认是 `['standard','strict']`**，也就是 minimal 档下自动关掉。
- 三档之间**没有包含关系**。不是"strict ⊃ standard ⊃ minimal"的层级，而是每条 hook 自己声明白名单。所以 `pre:bash:tmux-reminder` 标 `strict` 就意味着它在 standard 下**不跑**，在 minimal 下也不跑。

### 6.2 三档实际差异（从代码反推，不是从 README 抄）

| profile | 跑哪些 | 特点 |
|---|---|---|
| `minimal` | 只有 6 个：`pre:bash:block-no-verify`、`post:ecc-metrics-bridge`、`stop:session-end`、`stop:evaluate-session`、`stop:cost-tracker`、`session:end:marker`，外加不受门禁的 `pre:bash:dispatcher` 壳和 `session:start` | 只留"不可绕过的 git hook 保护"+ 会话记忆/指标持久化。所有质量检查、格式化、GateGuard 全关 |
| `standard`（默认） | 上面 6 个 + 所有标 `standard,strict` 的（约 22 个） | 关掉的只有 3 个 strict-only：`pre:bash:tmux-reminder`、`pre:bash:git-push-reminder`、`pre:bash:commit-quality` |
| `strict` | 全部 31 个 | 比 standard 多的就是那 3 个 Bash 提醒 + 提交质量检查 |

🔑 反直觉结论：**standard 和 strict 的差别很小**（3 个 hook，其中 2 个只是打印提醒文字）。真正的分水岭在 minimal↔standard 之间——GateGuard、config-protection、doc-file-warning、context-monitor、format/typecheck 全部只在 standard 及以上跑。

### 6.3 profile 门禁的两处漏网

1. `pre:bash:dispatcher`（`hooks/hooks.json:10`）**真的不经 run-with-flags**——直接调 `pre-bash-dispatcher.js`，门禁靠 `bash-hook-dispatcher.js:135` 逐个子 hook 兜住。所以 `ECC_DISABLED_HOOKS=pre:bash:dispatcher` 这个 ID 是**无效的**（没有任何代码检查这个 ID），要关得逐个关子 hook ID（`pre:bash:block-no-verify` 等）。
   `session:start` 则是在 bootstrap 脚本里补参数走了 run-with-flags，白名单 `minimal,standard,strict`（`session-start-bootstrap.js:47`），三档都跑，只能用 `ECC_DISABLED_HOOKS=session:start` 关。
2. `posttooluse-dispatcher.js` 自己实现了一份 `isEnabled()`（`:66-78`），逻辑与 `hook-flags.js` 等价但是复制的代码，不是 import。两份实现漂移的风险存在（目前一致）。

---

## 7. 环境变量总表

按"控制什么"分组，每条给出读取它的代码位置。

### 7.1 hook 总开关

| 变量 | 默认 | 作用 | 证据 |
|---|---|---|---|
| `ECC_HOOK_PROFILE` | `standard` | `minimal`/`standard`/`strict`，非法值静默变 standard | `scripts/lib/hook-flags.js:18-21` |
| `ECC_DISABLED_HOOKS` | 空 | 逗号分隔的 hook ID 黑名单，比 profile 优先级高（先查黑名单再查 profile） | `scripts/lib/hook-flags.js:23-33`、`:61-64` |
| `ECC_DRY_RUN` | 未设 | `=1` 时所有走 run-with-flags 的 hook 只打印 `[DryRun] ...` 不执行 | `scripts/lib/hook-flags.js:53-55`、`run-with-flags.js:178-183`、`posttooluse-dispatcher.js:184-187` |
| `ECC_GATEGUARD` | 未设（开） | `off`/`0`/`false`/`disabled`/`disable` 关掉 GateGuard 两个 gate；报错信息里会主动教用户这么关（`gateguard-fact-force.js:1098`、`:1131`） | `gateguard-fact-force.js:730`、`:44-45` |
| `ECC_GOVERNANCE_CAPTURE` | 未设（**关**） | `=1` 才启用治理事件捕获 | `governance-capture.js:15` |
| `ECC_ENABLE_INSAITS` | 未设（关） | insaits 安全监控开关（该 hook 未注册，实际无效） | `insaits-security-wrapper.js` |

### 7.2 SessionStart 上下文注入

| 变量 | 默认 | 作用 | 证据 |
|---|---|---|---|
| `ECC_SESSION_START_CONTEXT` | 开 | `off` 完全关掉 SessionStart 的上下文注入 | `session-start.js:107`、`:612` |
| `ECC_SESSION_START_MAX_CHARS` | `8000` | 注入上下文的字符上限；超了会截断并附一句提示告诉用户可以调大或关掉 | `session-start.js:33`、`:199` |
| `ECC_MAX_INJECTED_INSTINCTS` | `6` | 最多注入几条学到的 instinct。**注意：任务里写的 `ECC_MAX_INSTINCTS` 这个名字在仓库里不存在**，正确名字是 `ECC_MAX_INJECTED_INSTINCTS`（grep 全仓 `ECC_MAX_INSTINCTS` 零命中） | `session-start.js:31`、`:145-162`；README 文档在 `README.md:1364` |
| `ECC_INSTINCT_CONFIDENCE_THRESHOLD` | `0.7` | instinct 置信度门槛，0-1 | `session-start.js:30`、`:121-128`；`README.md:1367` |
| `ECC_SESSION_RETENTION_DAYS` | `30` | session 临时文件保留天数，设 0/off/false/disabled/never/none 关闭清理 | `session-start.js:34`；`README.md:1358-1360` |

`ECC_MAX_INJECTED_INSTINCTS` 的边界处理在测试里有覆盖：非数字、小数 `3.9`、科学计数 `1e2`、十六进制 `0x1` 都会退回默认值（`tests/hooks/hooks.test.js:515-592`，`session-start.js:159` 用 `/^\d+$/` 硬校验）。

### 7.3 各 hook 的专属旋钮

| 变量 | 默认 | 作用 | 证据 |
|---|---|---|---|
| `ECC_CONTEXT_MONITOR_COST_WARNINGS` | 开 | `off` 只关成本估算警告，保留上下文/scope/loop 警告 | `ecc-context-monitor.js:27-30`；`README.md:1370-1371` |
| `ECC_OBSERVE_RUNNER_TIMEOUT_MS` | `9000` | continuous-learning observe.sh 的超时 | `observe-runner.js:9`、`:81` |
| `ECC_QUALITY_GATE_FIX` | `false` | quality-gate 是否自动修 | `quality-gate.js:66` |
| `ECC_QUALITY_GATE_STRICT` | `false` | quality-gate 是否严格 | `quality-gate.js:67` |
| `ECC_MCP_HEALTH_TIMEOUT_MS` | `5000` | MCP 探测超时 | `mcp-health-check.js:24` |
| `ECC_MCP_HEALTH_TTL_MS` | `120000` | MCP 健康状态缓存 TTL | `mcp-health-check.js:23` |
| `ECC_MCP_HEALTH_FAIL_OPEN` | 未设 | `1/true/yes` 时探测失败也放行 | `mcp-health-check.js:587` |
| `ECC_MCP_HEALTH_STATE_PATH` | — | 健康状态落盘路径覆盖 | `mcp-health-check.js` |
| `ECC_DISABLED_MCPS` | 空 | 跳过某些 MCP server 的健康检查 | `mcp-health-check.js` |
| `GATEGUARD_STATE_DIR` | `~/.gateguard` | GateGuard 状态目录 | `gateguard-fact-force.js:32` |
| `ECC_PLAN_CANVAS_STATE_DIR` | — | Plan Canvas 状态目录 | `plan-canvas-sessions.js:23` |
| `ECC_POSTTOOLUSE_PASSTHROUGH` | hooks.json 里硬设为 `1` | PostToolUse dispatcher 是否把原始 stdin 回吐 | `posttooluse-dispatcher.js:268`；`hooks/hooks.json:142`、`:154` |
| `ECC_SESSION_ID` | — | 治理事件的会话关联 ID | `governance-capture.js:17` |
| `ECC_AGENT_DATA_HOME` | `~/.claude` | 多 harness 隔离用的数据根目录（session-data / learned skills / metrics 都在它下面） | `scripts/lib/agent-data-home.js`；`README.md:1385-1398` |

### 7.4 LLM 摘要相关（最贵的一组）

| 变量 | 默认 | 作用 | 证据 |
|---|---|---|---|
| `ECC_SKIP_LLM_SUMMARY` | 未设 | 设了就完全跳过 LLM 摘要。子进程里会被自动设成 `1` 防递归 | `scripts/lib/llm-summary.js:113`、`:159` |
| `ECC_LLM_SUMMARY_MODEL` | `haiku` | 调哪个模型 | `scripts/lib/llm-summary.js:22-24` |
| `ECC_LLM_SUMMARY_INTERVAL` | `50` | 每多少条用户消息强制生成一次 LLM 摘要 | `session-end.js:231-233` |
| `ECC_LLM_SUMMARY_CONTEXT_THRESHOLD` | `20`（%） | 剩余上下文低于此百分比就触发 LLM 摘要 | `scripts/lib/llm-summary.js:26-29` |

### 7.5 run-with-flags 传给子 hook 的变量（只读，脚本自己不用设）

`run-with-flags.js:243-250` 在 spawn 子进程时注入：`CLAUDE_PLUGIN_ROOT`、`ECC_PLUGIN_ROOT`、`ECC_HOOK_ID`、`ECC_HOOK_INPUT_TRUNCATED`（`1`/`0`）、`ECC_HOOK_INPUT_MAX_BYTES`。后两个让子 hook 知道"我收到的输入被截断了"，安全类 hook 可以据此选择拦截而不是放行。

---

## 8. memory-persistence 独立 hook 包

`hooks/memory-persistence/` 只有两个文件，共 91 行，**它不是可执行的 hook 注册文件**。

`hooks/memory-persistence/hooks.json:2` 自己写明："Reference lifecycle hook definitions for ECC memory persistence. The production hook graph is hooks/hooks.json."

它的 schema 也和真 hooks.json 完全不同——不是 `{hooks: {事件: [{matcher, hooks:[{type, command}]}]}}`，而是一个扁平的 `events` 数组，每项是 `{event, id, script, purpose, blocking}`（`hooks/memory-persistence/hooks.json:3-45`）。

它列了 6 条：`session:start`(SessionStart)、`pre:compact`(PreCompact)、`pre:observe:continuous-learning`(PreToolUse)、`post:observe:continuous-learning`(PostToolUse)、`post:session-activity-tracker`(PostToolUse)、`session:end`(SessionEnd)，全部标 `blocking: false`。

🔑 **注意 `session:end` 这一条**（`hooks/memory-persistence/hooks.json:39-45`）声明 `SessionEnd → scripts/hooks/session-end.js`，但真实的 `hooks/hooks.json` 里 `session-end.js` 是挂在 **Stop** 事件上的（`hooks/hooks.json:205` 的 `stop:session-end`），SessionEnd 上挂的是 `session-end-marker.js`。这两份文档互相矛盾——真实行为以 `hooks/hooks.json` 为准。hooks.json 的 description 甚至解释了理由："Stop carries transcript_path"（`hooks/hooks.json:210`），即 SessionEnd 事件拿不到 transcript 路径。

`hooks/memory-persistence/README.md:28-33` 给运维方的期望值得记：
> - Keep persistence local by default.
> - Avoid sending transcripts or tool traces to hosted services unless a user explicitly enables an integration.

这条"默认本地、不外传"的承诺在代码里**基本成立但有例外**：唯一的外发路径是 `llm-summary.js` 调 `claude -p`，它把最近 25 轮对话（最多 7000 字符，`scripts/lib/llm-summary.js:18-19`）送去 Anthropic 的模型生成摘要。这是"复用 Claude Code 自己的登录态"，不算第三方托管，但确实把 transcript 片段发出去了。用户如果不接受，需要显式设 `ECC_SKIP_LLM_SUMMARY=1`。

## 9. hooks 如何进入运行时：plugin 模式 vs 手动模式

🔑 **先纠正一个前提：ECC 的 hooks 从来不写进 `settings.json`。** 两条安装路径都不碰它，README 反复强调"不要往 settings.json 里抄"。

### 9.1 plugin 模式（`/plugin install`）

- 走 marketplace / plugin 安装，ECC 落到 `~/.claude/plugins/<某个名字>/` 下。
- **Claude Code v2.1+ 按约定自动加载任何已安装 plugin 目录下的 `hooks/hooks.json`**，不需要在 `plugin.json` 里声明（`README.md:409`）。
- 反过来，**在 `.claude-plugin/plugin.json` 里加 `"hooks"` 字段会报错**：`Duplicate hooks file detected: ./hooks/hooks.json resolves to already-loaded file`（`docs/de-DE/README.md:809-812`、`:1090`，关联 issue #29/#52/#103）。
- 实际验证：`.claude-plugin/plugin.json` 里确实**只有 `skills` 和 `commands` 两个路径字段，没有 `hooks`**（`.claude-plugin/plugin.json:26-31`）。`hooks` 只出现在 `keywords` 数组里（`:16`）。
- 这种模式下 `CLAUDE_PLUGIN_ROOT` 由 Claude Code 注入，第 1 层内联引导器的第一个分支就命中，root 解析零成本。

### 9.2 手动/选择性安装模式（`install.sh --modules hooks-runtime`）

流程在 `scripts/lib/install/apply.js`：

1. `hooks-runtime` 模块声明要复制三个路径：`hooks`、`scripts/hooks`、`scripts/lib`（`manifests/install-modules.json` 里 `id: hooks-runtime` 那项）。适用 target：`claude`、`claude-project`、`cursor`、`opencode`、`codebuddy`。
2. 用只读 planner 实测（`node scripts/install-plan.js --profile full --target claude --json`）：targetRoot = `~/.claude`，301 个复制操作，其中 hook 相关两条 —— `hooks → ~/.claude/hooks` 和 `scripts/hooks → ~/.claude/scripts/hooks`。**没有任何一条目标是 `settings.json`。**
3. `buildResolvedClaudeHooks()`（`scripts/lib/install/apply.js:119-143`）对 claude / claude-project 两个 target 做一件额外的事：把 hooks.json 里的 `${CLAUDE_PLUGIN_ROOT}` 占位符全部替换成实际的 `targetRoot`（`replacePluginRootPlaceholders`，`:84-106`），再写到 `~/.claude/hooks/hooks.json`（`:238-244`）。
4. 校验：`hooks` 字段必须是 JSON 对象，否则抛错（`:134-136`）。

> 补充观察：当前 `hooks/hooks.json` 文本里其实**一个 `${CLAUDE_PLUGIN_ROOT}` 占位符都没有**（grep 全文零命中）——它靠第 1 层内联引导器在运行时算 root，不靠安装时替换。所以第 3 步的占位符替换在当前版本是空转，属于给未来/给其他 target 留的能力。这也解释了为什么内联引导器要写那 6 个候选目录名——它得在两种安装模式下都能自己找到家。

### 9.3 为什么 README 反复警告"不要两边都装"

`README.md:395` / `:409` / `:1842`：如果你已经 `/plugin install` 了，Claude Code 自动加载 plugin 里的 hooks.json；这时再往 `settings.json` 或 `~/.claude/hooks/hooks.json` 复制一份，同一个 hook 会**被注册两次、执行两次**。对于 `pre:edit-write:gateguard-fact-force` 这种会 exit 2 拦截的 hook，双份执行的后果不只是慢，还可能状态互相打架（GateGuard 状态文件按 session key 存，两次调用会看到彼此写的状态）。

### 9.4 其他 harness 的 hook 分发

| harness | hook 载体 | 证据 |
|---|---|---|
| Claude Code | 原生 plugin hooks / `~/.claude/hooks/hooks.json` | 上文 |
| Cursor | `.cursor/hooks.json` + `.cursor/hooks/*.js` 适配层，复制到 targetRoot（`install-executor.js:429-437`）；`cursor-session-env.js` 从 `scaffolds/cursor/hooks.json:6` 注册；`after-tab-file-edit.js` 转调 `post-edit-format.js` | `scripts/lib/install-executor.js:233-237`、`:429-437` |
| OpenCode | plugin events，但 **默认 profile 故意排除 hooks-runtime**，要用户显式 `--modules hooks-runtime` 才装（`install-manifests.js:141-143`：`"OpenCode defaults intentionally exclude hooks-runtime until users opt in."`） | `scripts/lib/install-manifests.js:141-143`；`manifests/install-profiles.json:15` |
| Codex | 无 hook 运行时，靠 git hooks + 指令层 | `README.md:1414`（平台支持表） |
| GitHub Copilot | 无 hook 运行时 | 同上 |

### 9.5 README 声称的数字 vs 实测

`README.md:1425-1426` 的跨工具对照表写：Claude Code "Hook Events: 8 types"、"Hook Scripts: 20+ scripts"。

实测：`hooks/hooks.json` 注册了 **6 个事件**（PreToolUse / PreCompact / SessionStart / PostToolUse / PostToolUseFailure / Stop / SessionEnd —— 数一下是 7 个）。等一下，精确重数：PreToolUse、PreCompact、SessionStart、PostToolUse、PostToolUseFailure、Stop、SessionEnd = **7 个事件**。README 说 8 个，差 1 个，README 数字**未在代码中核实**（可能把 Notification 或 UserPromptSubmit 算进去了，但两者都不在 hooks.json 里）。

`scripts/hooks/` 下 50 个文件，其中被 hooks.json 或 dispatcher 实际用到的约 34 个——"20+ scripts"这个说法成立。

## 10. 安全与性能评估：网络 / 写盘 / 卡死风险

方法：对 `scripts/hooks/*.js` 全量 grep 三类特征——`require('http'|'https')`/`fetch(`/`axios`（网络）、`writeFileSync|appendFileSync|mkdirSync|rmSync|unlinkSync|renameSync`（写盘）、`spawnSync|execSync|execFileSync|spawn(`（起外部进程）。逐项对照代码确认。

### 10.1 网络请求：只有 2 处，但第 2 处很贵

| hook | 网络行为 | 目标 | 超时 | 风险 |
|---|---|---|---|---|
| `pre:mcp-health-check` / `post:mcp-health-check` | **直接发 HTTP/HTTPS 请求**探测 MCP endpoint 可达性 | 用户自己配的 MCP server URL（不是 ECC 的服务器） | `ECC_MCP_HEALTH_TIMEOUT_MS`，默认 5000ms，`req.setTimeout` 硬设（`mcp-health-check.js:282`、`:305`） | 中。有 TTL 缓存 2 分钟（`:23`）、有失败退避 30s→10min（`:25-26`），不会每次工具调用都探。但它 matcher 是 `*` 且**不是 async**，探测慢时会同步卡住工具调用 |
| `stop:session-end` / `pre:compact`（经 `scripts/lib/llm-summary.js`） | **`spawnSync('claude', ['--model','haiku','-p'])`** —— 起一个完整的 Claude Code CLI 子进程做 LLM 调用，间接产生 API 网络请求 | Anthropic API（复用 Claude Code 自己的登录态，不需要额外 key，`llm-summary.js:5-7`） | `LLM_TIMEOUT_MS = 90000`（**90 秒**，`llm-summary.js:19`） | **高，见下** |

⚠️ **LLM 摘要是这套 hook 里最贵、最危险的一环**：

- 触发条件（`session-end.js:225-238`）：剩余上下文 <20%（`ECC_LLM_SUMMARY_CONTEXT_THRESHOLD`）**或** 用户消息数是 50 的整数倍（`ECC_LLM_SUMMARY_INTERVAL`）。也就是说长会话必然会撞上。
- 它把最近 25 轮对话、最多 7000 字符（`llm-summary.js:18-19`）作为 prompt 发出去，**用户看不到、不会被问**。
- 成本：每次一个 haiku 调用。默认关不掉，要显式 `ECC_SKIP_LLM_SUMMARY=1`。
- 递归防护有：子进程 env 里强制 `ECC_SKIP_LLM_SUMMARY=1` 且清空 `CLAUDECODE`（`llm-summary.js:157-160`），防止子进程的 Stop hook 再触发一次摘要。这个防护写得对。
- **但超时对不上**：`stop:session-end` 在 hooks.json 里声明 `timeout: 10`（10 秒，`hooks/hooks.json:207`）、内联 spawnSync 也设 `timeout:30000`（30 秒，`:205`），而 LLM 调用自己给了 90 秒。结论：**90 秒的 LLM 调用永远等不到自然完成，会先被外层 30 秒（或 harness 的 10 秒）杀掉**。这条链路在 Stop 上大概率是"跑 10-30 秒然后被砍、白花钱、拿不到摘要"。因为它是 `async: true`，不阻塞用户，但白烧 token。信心度：中（~65%）——我读到的是超时数字的矛盾，没有实测。可自行验证：设 `ECC_LLM_SUMMARY_INTERVAL=1` 跑一轮，看 `~/.claude/session-data/` 里有没有 LLM 生成的摘要块（含 `<!-- ECC:SUMMARY:START -->` 标记）。
- `pre:compact` 那条走 PreCompact 事件，hooks.json 里**没声明 timeout**（`hooks/hooks.json:104-109`），走 Claude Code 默认（通常 60 秒）。所以 PreCompact 那次 LLM 调用比 Stop 那次更可能真跑完，但也可能在压缩前卡你 60 秒。

**除此之外，全仓 hook 脚本没有任何一处直接连 ECC 自己的服务器、没有遥测上报、没有 `fetch`。** 这一点与 `hooks/memory-persistence/README.md:29` 的承诺一致。

### 10.2 写盘：13 个 hook 会写，全部写在临时目录或用户数据目录

| 写入者 | 写到哪 | 内容 | 增长控制 |
|---|---|---|---|
| `post:edit:accumulator` | `os.tmpdir()/ecc-edited-<sessionId>.txt` | 编辑过的文件路径，一行一条，`appendFileSync` | Stop hook 读完就 `unlinkSync` 删掉（`stop-format-typecheck.js:146`） |
| `post:ecc-metrics-bridge` | `/tmp/ecc-metrics-{session}.json` | 会话聚合指标 | 最多 200 个文件（`ecc-metrics-bridge.js:20`），原子写 tmp+rename（`:125-126`） |
| `post:ecc-context-monitor` | 同上目录的状态文件 | 已发过的警告状态（去重用） | tmp+unlink（`ecc-context-monitor.js:81-85`） |
| `post:session-activity-tracker` | `~/.claude/metrics/tool-usage.jsonl` | 每次工具调用一行 | **没看到轮转/上限**——每次工具调用都追加一行，长期使用会无限增长。未在代码中找到清理逻辑（我 grep 了该文件的 rm/rotate，无命中）。风险：低（文本行小），但需要用户自己清 |
| `stop:cost-tracker` | `~/.claude/metrics/costs.jsonl` | 每次 Stop 一行累计成本 | 同上，无轮转 |
| `post:bash:command-log-*` | `bash-commands.log` / `cost-tracker.log` | 每条 Bash 命令一行（token/Authorization 已脱敏，`post-bash-command-log.js:24-27`） | 无轮转 |
| `pre:edit-write:gateguard-fact-force` | `~/.gateguard/`（可用 `GATEGUARD_STATE_DIR` 改） | 会话 gate 状态 | 有上限：500 条 checked、50 个 session key、30 分钟过期（`gateguard-fact-force.js:37-40`），过期文件会 `unlinkSync`（`:949`） |
| `pre:mcp-health-check` | `ECC_MCP_HEALTH_STATE_PATH` 指定处 | MCP 健康状态 JSON | 按 server 一条，规模有限 |
| `session:start` | — | **会删**超过 30 天的旧 session `.tmp` 文件（`session-start.js:235` `fs.rmSync`），受 `ECC_SESSION_RETENTION_DAYS` 控制 | 有 |
| `pre:edit-write:suggest-compact` | — | 同样有 `fs.rmSync` 清理逻辑（`suggest-compact.js:106`） | 有 |
| `stop:session-end` / `pre:compact` | `~/.claude/session-data/`（受 `ECC_AGENT_DATA_HOME` 影响） | session 摘要 markdown | 靠 session:start 的 30 天清理 |
| `stop:desktop-notify` | 直接写终端 tty 设备文件（`fs.writeFileSync(tty, '\x1b]9;...')`，`desktop-notify.js:182`） | iTerm2 通知转义序列 | 不是持久化写入 |

🔑 **没有任何 hook 写用户的项目源码文件**——除了 formatter。`stop:format-typecheck` 会跑 `biome check --write` / `prettier --write`（`stop-format-typecheck.js:59-61`），`post:quality-gate` 会跑 `gofmt -w`（`quality-gate.js:108`）和 `ruff`（`:127`）。这些是**真的会改你的源文件**的。想关就 `ECC_DISABLED_HOOKS=stop:format-typecheck,post:quality-gate`。

### 10.3 会不会拖慢 / 卡住会话——逐场景判断

**场景 A：每次 Bash 工具调用**（同步阻塞）
- 跑 `pre:bash:dispatcher`：1 个 Node 进程（第 1 层内联）+ 1 个 spawnSync（bootstrap → dispatcher）= **2 次 Node 启动，约 100-200ms 底噪**（Node 冷启动典型 50-100ms，仓库注释也这么估，`run-with-flags.js:203-204`）。
- dispatcher 内部 6 个子 hook 全部同进程 require，不再额外起进程。
- 其中 `pre:bash:auto-tmux-dev` 会 `spawnSync('which', ['tmux'])`（`auto-tmux-dev.js:72`）——**这个 spawnSync 没设 timeout**，理论上可以挂住。实际 `which` 不会挂，风险极低。
- strict 档下多一个 `pre:bash:commit-quality`，遇到 `git commit` 会跑 `git diff --cached` + 每个暂存文件一次 `git show :<file>`（`pre-bash-commit-quality.js:29`、`:40`）+ linter。**大提交（几十个文件）时这里会明显变慢**，且 `:264` 的 spawnSync 需要确认是否设了 timeout（未逐行验证）。
- 加上 `pre:mcp-health-check`（matcher `*`，同步）：命中缓存时几乎免费，缓存过期时最多 5 秒。
- **结论：正常情况每次 Bash 加 150-300ms；最坏情况（MCP 探测超时 + 大提交质量检查）可达 10 秒级。**

**场景 B：每次 Edit/Write**（同步阻塞）
- `pre:edit-write:gateguard-fact-force`（timeout 5s）+ `pre:config-protection`（5s）+ `pre:write:doc-file-warning` + `pre:edit-write:suggest-compact` + `pre:mcp-health-check`，各自一个 Node 进程链。
- 这里是**进程数最多的路径**：5 条独立注册项 × 每条 2 次 Node 启动 = 10 次进程创建。粗估 **500ms-1s 的固定开销**。这是 ECC hook 设计里最明显的性能债——PreToolUse 没有像 PostToolUse 那样做 dispatcher 合并。
- 然后 `post:dispatcher:sync`（30s 上限）再跑 7 个子 hook，其中 `post:session-activity-tracker` 会 `spawnSync('git', ...)`（`session-activity-tracker.js:345`）。
- **GateGuard 的第一次拦截是设计内的"卡住"**：它会 exit 2 拒绝你对某个文件的第一次编辑，逼模型先去调查。对人类用户这表现为"Claude 编辑前多绕了一圈"。要关：`ECC_GATEGUARD=off`。

**场景 C：每次 Claude 回复结束（Stop）**
- `stop:format-typecheck` 声明 **300 秒超时**，内部预算 `TOTAL_BUDGET_MS = 270_000`（270 秒，`stop-format-typecheck.js:29`）按批数均摊（`:175`）。批 = 项目根数 + tsconfig 目录数。
- ⚠️ **这是全套 hook 里最可能让人以为"Claude 卡死了"的地方**：在大型 monorepo 里，`tsc --noEmit` 单次几十秒很正常，而且是**同步、非 async**，用户会眼睁睁看着回复结束后卡住。
- 缓解设计有三条：① 累积器把 N 次编辑合成 Stop 时一次（`post-edit-accumulator.js:8-10` 的动机说明）；② 按 tsconfig 分组避免重复 tsc；③ 预算均摊防止总时长爆掉。
- 但 270 秒的预算本身就意味着"设计者接受最多 4.5 分钟的 Stop 停顿"。
- 其余 5 个 Stop hook 都是 `async:true` + 10 秒超时，不阻塞。

**场景 D：会话开始（SessionStart）**
- `session:start` 在 hooks.json 里无 timeout 声明（走 Claude Code 默认），bootstrap 内部 spawnSync 给 30 秒（`session-start-bootstrap.js:53`）。
- 进程链最长：内联引导 → bootstrap → run-with-flags → session-start.js，**4 次 Node 启动**（`session-start.js` 没有 `module.exports`，吃不到 run() 快路径）。
- 772 行脚本要扫 session 目录、读 learned skills、检测包管理器与项目类型、清 30 天旧文件。首次在积累很久的 `~/.claude` 上跑会有可感延迟。可用 `ECC_SESSION_START_CONTEXT=off` 关掉上下文注入部分，或 `ECC_SESSION_START_MAX_CHARS` 压小注入量。
- 三档 profile 都跑（白名单 `minimal,standard,strict`），调低 profile 无效。

### 10.4 fail-open 纪律：整体做得好

统计下来，ECC 在几乎所有出错分支都选择"放行"：

- `run-with-flags.js:268-271`：顶层 catch 后 `process.exit(0)`。
- `run-with-flags.js:232-235`：`run()` 抛异常 → 打 stderr → 原样回吐 → exit 0。
- `bash-hook-dispatcher.js:159-161`：子 hook 抛异常 → 记一行 → **继续跑下一个**。
- `posttooluse-dispatcher.js:206-208`：同上。
- `plugin-hook-bootstrap.js:242-246`：bootstrap 解析失败 → 回吐 → exit 0。
- `hooks/hooks.json:182` 等 Stop 内联版：找不到 plugin root → 打 WARNING → 回吐 → exit 0。
- `plan-canvas-sessions.js:11-12`：明确声明"任何错误都 exit 0"。

例外（**故意 fail-closed**）：`config-protection` 在 stdin 被截断时仍可选择拦截（`run-with-flags.js:157-162` 的注释点名了这个设计）。

### 10.5 安全面的加分与减分

**加分**：
- 两处独立的路径穿越防护：`run-with-flags.js:189-194`、`plugin-hook-bootstrap.js:51-61`、`observe-runner.js:24-31`。
- stdin 1 MB 硬上限，三个入口都实现了（`run-with-flags.js:17`、`bash-hook-dispatcher.js:20`、`posttooluse-dispatcher.js:23`）。
- 日志写盘前脱敏 token / Authorization（`post-bash-command-log.js:24-27`）。
- `block-no-verify` 挂在 minimal 档，意味着"就算你把 ECC 调到最低档，绕过 git hook 依然被拦"——这个默认值选得对。
- Windows 上 `plugin-hook-bootstrap.js:96-109` 的 shell 探测要求"能 spawn **且** 退出码为 0"，专门防 System32 那个 WSL 存根 bash.exe 误判。

**减分 / 需要注意**：
1. **hooks.json 里那 1.4 KB 的内联 JS 出现 21 次**，完全重复。任何一处要改都得改 21 处；而且它是 `node -e` 字符串，语法错了不会被 lint 发现。`session-start-bootstrap.js:8-17` 的注释就记录了一次真实事故：内联 JS 里的 `!` 触发了 bash 历史展开，导致 "SessionStart:startup hook error"，最后靠把逻辑挪出到独立文件解决——但其余 20 处内联仍在。
2. **`run-with-flags.js:210` 的 run() 探测是正则匹配源码文本**，不是真的检查导出。任何含 `module.exports` 且文本里出现 `run` 的脚本都会被 `require()` 进主进程。如果那个脚本在模块顶层做了 `process.exit()` 或注册 stdin listener，会污染 run-with-flags 自己的进程。当前 shipped 的脚本没踩这个雷（都用 `if (require.main === module)` 守卫了），但对第三方扩展是个陷阱。
3. **`ECC_MCP_RECONNECT_COMMAND` 会被 `spawnSync(command, {shell: true})` 直接执行**（`mcp-health-check.js:563-569`），且支持 `{server}` 模板替换（`:551-554`）。这是一个环境变量 → shell 执行的路径。只要攻击者能控制环境变量就能执行任意命令。默认未设，风险取决于部署环境。
4. `stop:format-typecheck` 在 Windows 上用 `shell: true` 跑 `.cmd`（`stop-format-typecheck.js:69`），代码里有 `UNSAFE_PATH_CHARS` 检查（`:32`）来防注入——说明作者意识到了这个风险并做了缓解，但缓解方式是"路径含特殊字符就跳过整批"，不是转义。

---

## 11. 测试覆盖（tests/hooks）

`tests/hooks/` 有 **48 个测试文件、21687 行**（`wc -l tests/hooks/*.js`）。这是仓库里 hook 系统可信度最高的证据来源——比 README 可信得多。

主要文件：

| 测试文件 | 覆盖对象 | 值得看的点 |
|---|---|---|
| `tests/hooks/hooks.test.js` | 综合，最大的一份 | `:515-592` 覆盖 `ECC_MAX_INJECTED_INSTINCTS` / `ECC_INSTINCT_CONFIDENCE_THRESHOLD` 的非法输入（`3.9`、`1e2`、`0x1`、`not-a-number`）；`:1713-1963` 覆盖 `post-edit-format.js` / `pre-bash-dev-server-block.js` / `post-edit-typecheck.js` 这三个**已经不在 hooks.json 里**的脚本 |
| `tests/hooks/hook-flags.test.js` | profile / 禁用名单逻辑 | profile 机制的规格说明书 |
| `tests/hooks/run-with-flags-truncation.test.js` | 1 MB 截断行为 | 对应 #2222 那个 issue 的回归测试 |
| `tests/hooks/stop-hooks-stdout.test.js`（336 行） | Stop hook 的 stdout 契约 | 验证"回吐原始 raw vs 输出空串"的边界 |
| `tests/hooks/posttooluse-dispatcher.test.js` | dispatcher 编排、matcher、stdout 合并 | `mergeHookStdout` 的丢弃行为 |
| `tests/hooks/bash-hook-dispatcher.test.js` | pre/post bash 编排 + 短路 | |
| `tests/hooks/gateguard-fact-force.test.js` | GateGuard | 最大的单 hook（1278 行）配了独立测试 |
| `tests/hooks/plugin-hook-bootstrap.test.js` | root 解析 + Windows 路径归一 | `normalizePluginRootForPlatform` |
| `tests/hooks/suggest-compact.test.js`（900 行） | 压缩建议的两个信号 | 对 `#2155` 的完整回归 |
| `tests/hooks/mcp-health-check.test.js` | MCP 探测 | |
| `tests/hooks/observe-*.test.js`（6 个） | continuous-learning observer | 竞态、超时、子目录检测、entrypoint 白名单 |
| `tests/hooks/test_insaits_security_monitor.py` | 唯一的 Python 测试 | 对应未注册的 Python hook |

跑法（`CLAUDE.md` 的 Running Tests 段）：`node tests/run-all.js`，或单跑 `node tests/hooks/hooks.test.js`。

🔑 **测试覆盖率与"是否在跑"是两回事**：`pre-bash-dev-server-block.js` 有 50+ 行测试、README 把它列为第一个 PreToolUse hook，但它**不在 hooks.json 里**。测试通过 ≠ 该 hook 在你的会话里生效。

## 12. 结论与可疑点

### 12.1 一句话总结

ECC 的 hook 系统是一个**四层包装、31 个 hook ID、全 Node.js 实现、以 fail-open 为纪律的自动执行层**；它真正的设计重点不是"多做检查"，而是"**把检查从每次工具调用挪到批量时刻**"（PostToolUse dispatcher 合并、编辑累积到 Stop 批处理），以及"**用 GateGuard 强制模型在动手前交出事实**"。

### 12.2 三个最值得记住的机制

1. **GateGuard fact-forcing**（`gateguard-fact-force.js`，1278 行）是整套东西里唯一一个"改变模型行为"而不是"检查模型产物"的 hook。它对每个文件的第一次编辑直接 exit 2，逼模型去查 importer / API / schema / 复述用户原话。作者的原话很值得引：*"Instead of asking 'are you sure?' (which LLMs always answer 'yes'), this hook demands concrete facts"*（`:5-7`）。这是 harness 设计上的一个真 idea。
2. **编辑累积 → Stop 批处理**：`post:edit:accumulator` 每次编辑只 append 一行路径（微秒级），`stop:format-typecheck` 在回复结束时按项目根/tsconfig 分组一次性跑 formatter 和 tsc。把"每次编辑跑一次 tsc"（可能几十次）变成"每轮回复跑一次"。这是本仓最实在的性能设计。
3. **run() 快路径**：`run-with-flags.js:203-237` 用 `require()` 替代 `spawnSync` 省一次 Node 启动。设计对，但探测方式（正则匹配源码）粗糙。

### 12.3 可疑点 / 我会 push back 的地方

| # | 问题 | 证据 | 严重度 |
|---|---|---|---|
| 1 | **LLM 摘要的超时链条自相矛盾**：`llm-summary.js` 给 90 秒，外层 spawnSync 给 30 秒，hooks.json 给 10 秒。90 秒的调用不可能跑完 | `scripts/lib/llm-summary.js:19` vs `hooks/hooks.json:205,207` | 高——白烧 token 且拿不到结果。信心中（~65%），需实测 |
| 2 | **README 的 hook 表描述的是旧架构**，列了 Dev server blocker / Prettier format / TypeScript check 三个已经不在 hooks.json 里的 hook | `hooks/README.md:42,57,58` vs `hooks/hooks.json` 全文 | 中——误导读者 |
| 3 | **1.4 KB 内联 JS 复制粘贴 21 次**。已有一次真实事故（`!` 触发 bash 历史展开）靠抽出文件解决，剩下 20 处没抽 | `hooks/hooks.json` 各条 command；事故记录 `session-start-bootstrap.js:8-17` | 中——维护性债 |
| 4 | **PreToolUse 没做 dispatcher 合并**，Edit/Write 路径上有 5 条独立注册项 = 10 次 Node 进程启动。PostToolUse 已经合并了，PreToolUse 没有 | `hooks/hooks.json:4-98` 有 8 条注册；对比 `:136-162` 只有 2 条 | 中——每次编辑 500ms-1s 固定开销 |
| 5 | **`ECC_MCP_RECONNECT_COMMAND` 环境变量 → `spawnSync(shell:true)`** 任意命令执行路径 | `mcp-health-check.js:546-569` | 中——默认未设，但是个明确的攻击面 |
| 6 | **run() 快路径靠正则判断**，第三方 hook 若在模块顶层有副作用会污染包装器进程 | `run-with-flags.js:210` | 低-中 |
| 7 | **两份 profile 门禁实现**：`hook-flags.js:57-69` 和 `posttooluse-dispatcher.js:66-78` 是复制不是复用 | 对比两处 | 低——目前一致，有漂移风险 |
| 8 | **`~/.claude/metrics/*.jsonl` 无轮转**：`tool-usage.jsonl` 每次工具调用一行、`costs.jsonl` 每次 Stop 一行，未找到清理逻辑 | `session-activity-tracker.js:5-6`、`cost-tracker.js:6-8` | 低——文件小但无限增长 |
| 9 | **`post:bash:build-complete` 是空壳**：它打印 "async analysis running in background"，但代码里没有任何分析 | `post-bash-build-complete.js:10-16`（全部逻辑就这 7 行） | 低——但属于对模型撒谎，会误导模型以为有分析结果可等 |
| 10 | **README 说 "Hook Events: 8 types"，实测 7 个** | `README.md:1425` vs `hooks/hooks.json` 顶层 7 个 key | 低——仓库声称，未在代码中核实 |
| 11 | **`hooks/memory-persistence/hooks.json` 与真 hooks.json 冲突**：前者说 `session-end.js` 挂 SessionEnd，后者实际挂在 Stop | `hooks/memory-persistence/hooks.json:39-45` vs `hooks/hooks.json:205` | 低——参考文档过期 |

### 12.4 "省 token"类宣传的核实

README 通篇未在 hook 章节给出具体的省 token 百分比数字（我 grep 了 hook 相关段落）。与 hook 相关的可核实性能主张只有两条：

- "`stop:format-typecheck` runs once at Stop instead of after every Edit"（`hooks/hooks.json:186`）——**代码核实成立**，累积器机制在 `post-edit-accumulator.js` + `stop-format-typecheck.js:135-175`。
- "This eliminates one Node.js process spawn (~50-100ms savings per hook)"（`run-with-flags.js:203-204`）——**机制成立，但 50-100ms 这个数字是代码注释里的自估，没有 benchmark 支撑**。属于"仓库声称，未在代码中核实"。

### 12.5 如果你要用这套 hook，最小配置建议

```bash
# 关掉 LLM 摘要（省钱 + 避开超时矛盾）
export ECC_SKIP_LLM_SUMMARY=1

# 关掉最重的两个：Stop 时的 tsc 批处理 + 编辑前的事实门
export ECC_DISABLED_HOOKS="stop:format-typecheck,pre:edit-write:gateguard-fact-force"
# 或只关 GateGuard：
export ECC_GATEGUARD=off

# 想彻底最小化：
export ECC_HOOK_PROFILE=minimal   # 只留 block-no-verify + 指标/会话持久化
```

先用 `ECC_DRY_RUN=1` 跑一轮观察 stderr 里的 `[DryRun] Hook "..." would execute: ...`，能看到当前 profile 下究竟有哪些 hook 会跑，再决定关谁（`run-with-flags.js:178-183`）。注意 dry-run 覆盖不到 `pre:bash:dispatcher` 内部的子 hook（它们经 `bash-hook-dispatcher.js` 而非 run-with-flags），也覆盖不到 `session:start`。

### 12.6 上游已知坑（ECC 自己整理的）

`docs/hook-bug-workarounds.md` 记录了 Claude Code 本体（不是 ECC）的 5 个 hook 相关 bug：
- 成功的 hook 被误标 `Hook Error` → 对策：hook 开头就消费 stdin、简单 allow/block hook 别往 stdout 写东西、诊断信息走 stderr（`docs/hook-bug-workarounds.md:21-34`）。这解释了为什么 ECC 的 dispatcher 要那么小心地区分"吐原始 raw"和"吐空串"。
- **改了 hook 不会热重载，必须重启 session**（`:53-58`）。研究这套东西时改配置要记得重启。
- MCP 认证在 compaction 后失效（`:44-50`）。
- 529 Overloaded 的缓解（`:60-67`）。

---

## 附：关键文件绝对路径清单

- `/Users/aa00158/harness-research/ECC/hooks/hooks.json` — 唯一的可执行 hook 注册表，269 行
- `/Users/aa00158/harness-research/ECC/hooks/README.md` — hook 文档（部分已过期）
- `/Users/aa00158/harness-research/ECC/hooks/memory-persistence/hooks.json` — 参考性生命周期契约，非可执行
- `/Users/aa00158/harness-research/ECC/scripts/hooks/run-with-flags.js` — 统一包装器
- `/Users/aa00158/harness-research/ECC/scripts/hooks/plugin-hook-bootstrap.js` — 第 2 层引导
- `/Users/aa00158/harness-research/ECC/scripts/hooks/bash-hook-dispatcher.js` — Bash 前后置编排
- `/Users/aa00158/harness-research/ECC/scripts/hooks/posttooluse-dispatcher.js` — PostToolUse 编排
- `/Users/aa00158/harness-research/ECC/scripts/lib/hook-flags.js` — profile / 禁用名单机制（79 行，全部机制在这）
- `/Users/aa00158/harness-research/ECC/scripts/lib/llm-summary.js` — 唯一发起 LLM 调用的地方
- `/Users/aa00158/harness-research/ECC/scripts/lib/install/apply.js` — 安装时写 `~/.claude/hooks/hooks.json` 的代码
- `/Users/aa00158/harness-research/ECC/scripts/hooks/gateguard-fact-force.js` — 最大最有攻击性的 hook
- `/Users/aa00158/harness-research/ECC/scripts/hooks/stop-format-typecheck.js` — 最可能卡住会话的 hook
- `/Users/aa00158/harness-research/ECC/tests/hooks/` — 48 个测试文件，21687 行，可信度最高的行为规格
- `/Users/aa00158/harness-research/ECC/docs/hook-bug-workarounds.md` — 上游 Claude Code hook bug 对策
- `/Users/aa00158/harness-research/ECC/README.md:1340-1400` — hook 运行时环境变量文档段
