# ECC 维度 09：Token 优化与可观测性

> 研究对象：`/Users/aa00158/harness-research/ECC`（浅克隆，只读）
> 提交点：`e4e4163 fix(docs): restore main CI (#2623)`
> 记录时间：2026-08-01
> 读者：非程序员背景的 AI 项目主导者 —— 每节先讲"这东西实际怎么跑"，再给证据行号

---

## 0. 结论速览（TL;DR）

**ECC 没有「自动帮你省 token」的机制。它做的是观测 + 提醒：记录你花了多少、在上下文快满时喊你一声，省不省取决于你照不照做。**

九条速览：

1. README 的 "Token Optimization" 章（`README.md:1715-1789`）**90% 是在教你配置 Claude Code 自带的开关**（`model`/`MAX_THINKING_TOKENS`/`CLAUDE_CODE_SUBAGENT_MODEL`），ECC 没有代码去读这三个变量。其中 60%/70%/80% 三个节省百分比是**厂商定价的算术推论，不是测量值** → 仓库声称，未在代码中核实。

2. ECC 自己在代码里做的 token 相关的事只有三件：**SessionStart 注入截断到 8000 字符**（`scripts/hooks/session-start.js:34`）、**strategic-compact 提示**（`scripts/hooks/suggest-compact.js`）、**context/cost/loop 警告注入模型**（`scripts/hooks/ecc-context-monitor.js`）。

3. "Progressive disclosure" **在 ECC 文档里根本不存在**（全仓库唯一命中在一个讲 UI 手风琴的 Angular skill 文档里）。有一个现成的 agent 目录压缩+懒加载库 `scripts/lib/agent-compress.js`，但**没有任何生产代码调用它**（只有测试引用）——死代码。

4. `/context-budget` 那份漂亮的 token 预算报告**没有任何脚本支撑**，数字全靠模型心算（`skills/token-budget-advisor/SKILL.md:37` 原文用词 "mentally"），同一份报告跑两次数字可能不同。

5. `ecc_dashboard.py`（956 行 Tkinter）**跟 token/成本毫无关系**——grep `metrics|costs|token|session` 零命中。它是个仓库组件浏览器（列 agents/skills/commands/rules），版本号还停在 v1.10.0 而仓库已 2.1.0。唯一依赖是 Tkinter，零 pip 包。

6. 名字带 "observability"/"dashboard" 的三个 JS 脚本没一个在观测运行时：`observability-readiness.js` 是**文件存在性自评表**（跑出来 21/21 满分，含义只是「文件没被删」）；`operator-readiness-dashboard.js` 是**作者的发版待办 + MRR 目标看板**；`dashboard-web.js` 是浏览器版组件浏览器。

7. `ecc2/`（52,139 行 Rust，425 个 `#[test]`，零 `todo!()`）**不是 ECC 下一代替代品，是「装在 harness 之上的多会话控制台」**（`ecc2/README.md:16`）。跟 JS 侧的唯一接口是单向读两个 JSONL 文件。**完全没打进 npm 包、不在任何安装 manifest 里**，只能自己 `cd ecc2 && cargo run`。作者自立规矩「别拿能编译当做完了」（`ecc2/README.md:71-81`）。

8. **确认的两个真 bug**：
   - ecc2 把 `costs.jsonl` 的**累计快照**当增量累加（`ecc2/src/session/store.rs:1696-1699`），导致成本显示偏高约 (N+1)/2 倍，而且**这个错误行为被测试固化了**（`store.rs:5347-5390`）。
   - `suggest-compact` 注册在 `"matcher": "Edit|Write"`（`hooks/hooks.json:28`），**纯调研型会话（只读不写）永远收不到 compact 提示**，哪怕上下文 90% 满——而代码注释自己写着「几次大 Read 就能撑爆窗口」。

9. **零 benchmark**。ECC 对自己的性能/token 节省没有任何实测数据、任务集、对照实验。`harness-audit.js` 的 "Context Efficiency 10/10" 真实含义是「这四个 markdown 文件都在」（`scripts/harness-audit.js:447-484`）。

**默认状态**：装了 ECC，token 花费和工具活动**默认就在被记录**（`ECC_HOOK_PROFILE` 默认 `standard`），落盘到 `~/.claude/metrics/costs.jsonl` + `tool-usage.jsonl` + `/tmp/ecc-metrics-<session>.json`。详见 §2.6。

---

## 1. README 的 Token Optimization 章节讲了什么

原文位置：`README.md:1715-1789`（4 个折叠块），配套长文 `docs/token-optimization.md`（157 行）。

**一句话总结**：这一章 90% 是「教你怎么配置 Claude Code 自带的省钱开关 + 什么时候手动敲 `/compact`」，不是「ECC 内部帮你省 token 的机制」。

拆开看四个折叠块：

| 折叠块 | README 行号 | 内容性质 |
|---|---|---|
| Recommended settings | 1719-1746 | 让你往 `~/.claude/settings.json` 写 4 个 **Claude Code 官方的**环境变量（`model`/`MAX_THINKING_TOKENS`/`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`/`CLAUDE_CODE_SUBAGENT_MODEL`）。ECC 没有代码去读或设置它们 |
| Daily workflow commands | 1748-1760 | `/model` `/clear` `/compact` `/cost` —— 全是 Claude Code 内置斜杠命令，不是 ECC 的 |
| Strategic compaction | 1762-1775 | 指向 `skills/strategic-compact/SKILL.md`，这个**有对应代码**（见 §2.3） |
| Context window management | 1777-1789 | MCP 数量建议（<10 server / <80 tool）+ 指向 `/context-budget` |

四个环境变量里只有一个是 ECC 自己的：`ECC_CONTEXT_MONITOR_COST_WARNINGS`（`README.md:1740`），它由 `scripts/hooks/ecc-context-monitor.js:38` 真实读取。其余三个（`MAX_THINKING_TOKENS`、`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`、`CLAUDE_CODE_SUBAGENT_MODEL`）全仓库搜不到任何 ECC 代码读它们——它们是 Claude Code CLI 自己的开关，README 只是在做「配置建议」。

**README 里出现的百分比数字**（`README.md:1737-1740`，`docs/token-optimization.md:29-31`）：

- 「model 从 opus 换 sonnet → ~60% 成本下降」
- 「MAX_THINKING_TOKENS 从 31999 降到 10000 → 隐藏思考成本降 ~70%」
- 「subagent 用 haiku → 便宜 ~80%」

这三个数字是**厂商定价的算术推论**（Anthropic 官方价目表：opus 输入 $15/M vs sonnet $3/M，正好 5 倍 ≈ 降 80%；README 说 60% 是混合口径），不是 ECC 跑出来的测量值。仓库里没有任何脚本/数据集验证过这三个数字。→ **属「仓库声称，未在代码中核实」**。

另外一个数字：「MCP 全开会把 200k 窗口压到 ~70k」（`README.md:1780`、`README.md:1894`）。同样没有测量脚本，`skills/context-budget/SKILL.md:47` 里给的估算口径是「每个 MCP tool schema ≈ 500 token」，靠 LLM 心算，不是实测。

> 🔑 结论：**README 的 Token Optimization 章 = 使用建议文档，不是 ECC 的功能说明**。ECC 真正在代码里做的 token 相关的事，只有三件：SessionStart 注入截断、strategic-compact 提示、context-monitor 警告。逐条见 §2。

---

## 2. Token 节省机制逐条落地核查

先给结论表（"实现" = 能指到跑得起来的代码；"仅提示词" = 只有 markdown，靠模型自觉）：

| 声称机制 | 落地形态 | 证据 |
|---|---|---|
| SessionStart 注入上限 8000 字符 | ✅ 真实现（硬截断） | `scripts/hooks/session-start.js:34,192-204` |
| strategic-compact 提示 | ✅ 真实现（读 transcript 真实 token 数） | `scripts/hooks/suggest-compact.js`、`scripts/lib/transcript-context.js` |
| context 剩余量 / 成本 / 循环 警告 | ✅ 真实现（PostToolUse 注入 additionalContext） | `scripts/hooks/ecc-context-monitor.js:116-185` |
| `/context-budget` 上下文预算审计 | ❌ 仅提示词，无代码 | `skills/context-budget/SKILL.md`（单文件，同目录无脚本） |
| agent 目录「catalog 压缩 + lazy load」 | ⚠️ 有库无调用方（死代码） | `scripts/lib/agent-compress.js`，全仓库只有测试引用 |
| Progressive disclosure（渐进披露） | ❌ ECC 没有这个说法 | 全仓库 grep 只在 `skills/angular-developer/references/angular-aria.md:26` 出现（讲 UI 手风琴组件，无关） |
| MAX_THINKING_TOKENS / SUBAGENT_MODEL | ❌ 无 ECC 代码，纯配置建议 | grep 无命中（除文档） |

### 2.1 Progressive Disclosure（渐进披露）——ECC 并没有主张

「渐进披露」= 先只给模型一个目录/摘要，模型需要细节时再去读全文，避免一次性把所有文档塞进上下文。

在 ECC 仓库里 grep `progressive disclosure`，唯一命中是 `skills/angular-developer/references/angular-aria.md:26`，讲的是网页 UI 里的折叠面板，跟 token 无关。

**所以：ECC 从未把 progressive disclosure 作为 token 优化主张写出来。** 不过它事实上继承了 Anthropic Skill 格式带来的渐进披露效果：`skills/<name>/SKILL.md` 是入口，重内容放 `references/*.md`，模型只在需要时 Read。例如 `skills/angular-developer/` 目录下有 `SKILL.md` + `references/` 子目录。这属于「格式自带」，不是 ECC 的设计发明。

有一个模块**看起来**是为渐进披露写的，但没接上：

`scripts/lib/agent-compress.js` 提供 `buildAgentCatalog(agentsDir, {mode})` 三档压缩：
- `catalog`：只留 name/description/tools/model（注释称 27 个 agent ≈ 2-3k token，`scripts/lib/agent-compress.js:164`）
- `summary`：加正文第一段（≈4-5k token）
- `full`：不压缩

外加 `lazyLoadAgent(agentsDir, agentName)`（`scripts/lib/agent-compress.js:222-238`）按名字懒加载单个 agent 全文。

这正是渐进披露的标准做法。**但是**：`grep -rn "agent-compress\|buildAgentCatalog\|lazyLoadAgent"` 全仓库只命中 `tests/lib/agent-compress.test.js` 一个文件。没有任何 hook、命令、安装脚本调用它。→ **死代码 / 未接线的能力**。token 估算口径也是拍脑袋的 `JSON 字符数 / 4`（`scripts/lib/agent-compress.js:203-204`）。

### 2.2 Context 裁剪 / 压缩

ECC **没有**做「把已有对话内容裁掉/压缩后再喂回模型」这件事。Claude Code 的 auto-compact 是 harness 自带的，ECC 只能在旁边喊「该 compact 了」。

唯一沾边的是 `scripts/hooks/pre-compact.js`（PreCompact hook），它在压缩**之前**把会话状态落盘，属于「保命」不是「省 token」。

`skills/context-budget/SKILL.md` 描述的那套「扫描 agents/skills/rules/MCP，算 token，出优化建议报告」全是给模型看的**步骤说明**：

- Phase 1 扫描规则（`skills/context-budget/SKILL.md:24-53`）
- Phase 4 报告模板（`skills/context-budget/SKILL.md:71-99`）
- token 估算口径「prose 用 words × 1.3，代码用 chars / 4」（`skills/context-budget/SKILL.md:135`）

目录里只有 `SKILL.md` 一个文件（`ls skills/context-budget/` 确认），**没有任何脚本**。也就是说 `/context-budget` 跑出来的那张漂亮的表格，数字全是模型现场估的，不是程序算的。同一份报告跑两次数字可能不一样。同族的 `skills/token-budget-advisor/SKILL.md:37` 明写「Use the repository's canonical context-budget heuristics to estimate the prompt's token count **mentally**」——"mentally"（心算）是原文用词。

### 2.3 strategic-compact：唯一一个数据驱动的省 token 机制

这是 ECC 在 token 这块做得最扎实的东西，机制值得讲清楚：

**怎么跑**：`scripts/hooks/suggest-compact.js` 挂在 PreToolUse（每次模型要调工具之前触发一次）。

**两路信号**（`scripts/hooks/suggest-compact.js:15-23` 注释）：

1. **工具调用次数**（弱信号）：默认第 50 次时提醒一次，之后每 25 次再提醒（`suggest-compact.js:222-244`）。阈值可用 `COMPACT_THRESHOLD` 改。
2. **真实上下文大小**（主信号，issue #2155）：去读会话 transcript JSONL 的**尾部 256 KB**（`scripts/lib/transcript-context.js:27`），倒着找最近一条带 `message.usage` 的记录，把 `input_tokens + cache_read_input_tokens + cache_creation_input_tokens` 三者相加当作当前上下文真实占用（`transcript-context.js:104-109`）。

   这个加法口径是对的——这三个字段正好把一次请求的 prompt 分成三份（新输入 / 缓存命中 / 缓存写入），加起来就是真实 prompt 长度。

**窗口大小怎么判**（`transcript-context.js:166-194`）：
- 先看环境变量 `ECC_CONTEXT_WINDOW_TOKENS` 或 Claude Code 原生的 `CLAUDE_CODE_AUTO_COMPACT_WINDOW`
- 再看 model id 里有没有 `[1m]` 标记 → 1M 窗口
- 再查一张硬编码的已知大窗口模型表（`transcript-context.js:37-40`，目前只有 `claude-fable-5`、`claude-mythos-5` 两条）
- 都不中就看观测到的 token 数：>200k 就认定是 1M 窗口
- 兜底 200k

**什么时候喊**（`transcript-context.js:201-239`）：默认 200k 窗口下 160k token 触发（80%），1M 窗口下 250k 触发（25%）。触发后每再涨 60k 再喊一次（"bucket" 机制，避免每次工具调用都刷屏）。`COMPACT_CONTEXT_THRESHOLD=0` 可完全关掉。

**怎么把话传给模型**：不是靠 stderr（PreToolUse 的 stderr 在 exit 0 时只进 debug log，模型看不到），而是往 stdout 输出结构化 JSON `hookSpecificOutput.additionalContext`（`suggest-compact.js:246-262`）。这段注释写得很清楚，说明作者踩过坑。

**它到底省不省 token**：它本身**不省**，它只是让模型（和你）更早知道该 `/compact`。省不省取决于人/模型是否照做。这是个「提醒器」，不是「优化器」。

> 🔴 **一个自相矛盾的注册配置（本次新发现）**
>
> `hooks/hooks.json:28` 把 suggest-compact 注册在 **`"matcher": "Edit|Write"`** 上，不是 `"*"`。
>
> 后果：**只有模型调用 Edit 或 Write 时这个 hook 才会跑**。Read / Grep / Glob / Bash / Task 全都不触发。
>
> 讽刺的是代码注释自己写着（`scripts/hooks/suggest-compact.js:21-23`）：
> > "Tool count is a weak proxy for window pressure — **a few large reads can fill the window in very few calls**"
>
> 作者明知「大量 Read 会撑爆窗口」，却把这个检查器挂在了 Read 触发不到的位置。一个纯调研型会话（读几十个文件、不改代码）**永远收不到 compact 提示**，哪怕上下文已经 90% 满。
>
> 另一个连带后果：`suggest-compact.js:222-244` 那个「50 次工具调用」的计数器，实际统计的是「50 次 Edit/Write」，不是 50 次工具调用。文档和常量名都在误导。
>
> 对照：`ecc-metrics-bridge` 和 `ecc-context-monitor` 挂在 PostToolUse 的 `"*"` 上（`hooks/hooks.json` PostToolUse 两条 matcher 都是 `*`），所以那两个是全工具生效的。

### 2.4 SessionStart 注入上限——唯一的硬性 token 闸门

**这是全仓库唯一一处「ECC 主动限制自己往上下文里塞多少东西」的代码。**

`scripts/hooks/session-start.js` 在新会话开始时会把上次会话摘要、learned skills、instincts 等注入上下文。上限：

- `DEFAULT_SESSION_START_CONTEXT_MAX_CHARS = 8000`（`session-start.js:34`）—— 注意单位是**字符不是 token**，8000 字符英文约 2000 token
- 超了就硬截断，尾部贴一条提示告诉你怎么调大（`session-start.js:192-204`）
- `ECC_SESSION_START_MAX_CHARS` 可改，设 0 = 关闭注入（`session-start.js:111-117, 614`）
- `ECC_SESSION_START_CONTEXT=off` 也能整体关（`session-start.js:106-109`）

还有几个更细的条数上限：
- 注入的 instinct（学到的行为直觉）最多 6 条，且置信度需 ≥0.7（`session-start.js:30-31, 127-163`）
- 注入的 learned skill 最多 6 条，每条摘要截到 220 字符（`session-start.js:32-33, 475-480`）

README 对应说法在 `README.md:1894`，与代码一致。→ **这条主张核实通过**。

### 2.5 ecc-context-monitor：把警告注入给模型

**这是「可观测性反哺给模型」的那条链路**，值得单独讲，因为它是 ECC 观测体系里唯一一个 agent 能感知到的输出。

数据流是三段式：

```
Stop hook           PostToolUse hook         PostToolUse hook
cost-tracker.js  →  ecc-metrics-bridge.js →  ecc-context-monitor.js → 模型上下文
      ↓                     ↓                        ↑
~/.claude/metrics/   /tmp/ecc-metrics-           读 bridge 判阈值
   costs.jsonl        {session}.json
```

1. **`scripts/hooks/cost-tracker.js`**（Stop hook，模型每答完一轮触发）：拿 stdin 里的 `transcript_path`，扫整个会话 JSONL，把所有 assistant 轮次的 usage 加总，用硬编码费率表（`cost-tracker.js:73-77`）算出美元数，append 一行到 `~/.claude/metrics/costs.jsonl`（`cost-tracker.js:227`）。

   两个已修的坑值得记（注释里写得很坦白）：
   - 早期版本以为 Stop 的 payload 里直接带 usage/model，结果**连续 52 天写了 2340 行全是 0 的数据**（`cost-tracker.js:10-13`）。
   - Claude Code 一个 API response 会写多行 JSONL（每个 content block 一行）且每行重复同一份 usage，直接按行加会**虚高 2.5-3 倍**（实测 704 行只有 286 个唯一 message.id，$867 vs $333，`cost-tracker.js:96-102`）。现在按 `message.id` 去重。
   - 还有一条更可靠的通道：如果 statusline 把 Claude Code 给的权威 `cost.total_cost_usd` 写到 `/tmp/harness-cost-<session>.json` 且 ≤300 秒新鲜，就优先用它（`cost-tracker.js:24-35, 202-210`）。

2. **`scripts/hooks/ecc-metrics-bridge.js`**（PostToolUse，每次工具调用后）：维护 `/tmp/ecc-metrics-{session}.json` 聚合文件——工具调用计数、改过的文件集合（上限 200，`ecc-metrics-bridge.js:20`）、最近 5 次工具调用的哈希环形缓冲（用于检测死循环，`ecc-metrics-bridge.js:21, 252-256`）、从 costs.jsonl 读回的累计花费。

   注意：`context_remaining_pct` 这个字段在 bridge 初始化时是 `null`（`ecc-metrics-bridge.js:230`），bridge 自己**从不填它**。谁填？见 §4 statusline。

3. **`scripts/hooks/ecc-context-monitor.js`**（PostToolUse）：读 bridge，按阈值生成警告，取前两条拼成文本，通过 `hookSpecificOutput.additionalContext` 注给模型（`ecc-context-monitor.js:259-266`）。

   阈值全是硬编码常量（`ecc-context-monitor.js:18-25`）：

   | 常量 | 值 | 含义 |
   |---|---|---|
   | `CONTEXT_WARNING_PCT` | 35 | 剩余 ≤35% 发 warning |
   | `CONTEXT_CRITICAL_PCT` | 25 | 剩余 ≤25% 发 critical |
   | `COST_NOTICE_USD` | 5 | 花超 $5 提示 |
   | `COST_WARNING_USD` | 10 | 花超 $10 警告 |
   | `COST_CRITICAL_USD` | 50 | 花超 $50 严重警告 |
   | `FILES_WARNING_COUNT` | 20 | 改超 20 个文件警告"改动太散" |
   | `LOOP_THRESHOLD` | 3 | 最近 5 次里同一工具+同参数出现 3 次 = 疑似死循环 |
   | `STALE_SECONDS` | 60 | bridge 超过 60 秒没更新，context 数据作废（成本/范围/循环仍判） |

   去重逻辑值得一提：早期版本按「每 N 次调用」重发，导致同一条 cost 警告在一轮里刷了 ~20 次；现在改成**按消息文本变化去重**，只有内容变了或首次升到 critical 才再发（`ecc-context-monitor.js:239-253`）。

   `ECC_CONTEXT_MONITOR_COST_WARNINGS=off` 只关成本那三条，context/scope/loop 照常（`ecc-context-monitor.js:141, 219`）——README `1759` 行的说法与代码一致，✅ 核实通过。

> 🔑 一句话：ECC 的 token"优化"其实是**观测 + 提醒**，不是自动裁剪。真正决定省不省的是你（或模型）看到警告后做什么。

### 2.6 这些 hook 默认开不开？（已核实：开）

ECC 用一个「档位」概念控制 hook 开关：`ECC_HOOK_PROFILE` 三档 `minimal / standard / strict`，**默认 `standard`**（`scripts/lib/hook-flags.js:19`）。每个 hook 声明自己在哪些档下生效，不在档里就跳过（`hook-flags.js:56-69`）。还可以用 `ECC_DISABLED_HOOKS=id1,id2` 单独禁用（`hook-flags.js:23-32`）。

本维度相关的 5 个 hook 的注册情况：

| hook id | 事件 | matcher | 生效档位 | 默认(standard)开？ |
|---|---|---|---|---|
| `post:ecc-metrics-bridge` | PostToolUse | `*` | minimal,standard,strict | ✅ 永远开 |
| `stop:cost-tracker` | Stop | — | minimal,standard,strict | ✅ 永远开 |
| `post:ecc-context-monitor` | PostToolUse | `*` | standard,strict | ✅ 开（minimal 档关） |
| `post:session-activity-tracker` | PostToolUse | `*` | standard,strict | ✅ 开（minimal 档关） |
| `pre:edit-write:suggest-compact` | PreToolUse | **`Edit|Write`** | standard,strict | ⚠️ 开，但只在编辑文件时触发（见上方红框） |

证据：PostToolUse 三个走的是 `scripts/hooks/posttooluse-dispatcher.js:25-33` 的 `SYNC_HOOKS` 表（hooks.json 里 PostToolUse 只注册了这一个 dispatcher，由它内部路由）；`stop:cost-tracker` 在 `hooks/hooks.json:231-237`；`suggest-compact` 在 `hooks/hooks.json:28-36`。

> 🔑 结论：**装了 ECC 就默认在记录你的 token 花费和工具活动**，数据落在 `~/.claude/metrics/costs.jsonl` 和 `~/.claude/metrics/tool-usage.jsonl`，以及 `/tmp/ecc-metrics-<session>.json`。想全关：`ECC_HOOK_PROFILE=minimal` 只保留 metrics-bridge + cost-tracker，或者 `ECC_DISABLED_HOOKS=post:ecc-metrics-bridge,stop:cost-tracker,...` 精确关。

---

## 3. ecc_dashboard.py 解剖

### 3.1 一句话定性

**它跟 token / 成本 / 可观测性一点关系都没有。** 它是一个 Tkinter 桌面 GUI，功能等于「ECC 组件浏览器」——列出仓库里的 agents / skills / commands / rules，可以搜索、点开看内容、切主题、改字体。

证据：在 `ecc_dashboard.py` 里 grep `metrics|costs|token|session`，**零命中**。它从不读 `~/.claude/metrics/costs.jsonl`，不读 `/tmp/ecc-metrics-*.json`，不读任何会话数据。

### 3.2 怎么跑

```bash
npm run dashboard      # → python3 ./ecc_dashboard.py   (package.json:456)
npm run dashboard:web  # → node scripts/dashboard-web.js (package.json:457) 浏览器版，无需 Tkinter
```

必须在**仓库根目录**跑：
- `get_project_path()` 返回脚本自身所在目录（`ecc_dashboard.py:39-41`），所以数据源锁死在 ECC 仓库自己
- logo 用的是相对路径 `assets/images/ecc-logo.png`（`ecc_dashboard.py:299, 362`），cwd 不对就加载不到（有 try/except 兜底，不会崩）
- 顶部 `from scripts.lib.ecc_dashboard_runtime import launch_terminal, maximize_window`（`ecc_dashboard.py:31`）依赖仓库根在 `sys.path` 上

### 3.3 依赖

| 依赖 | 说明 |
|---|---|
| Python 3 + **Tkinter** | 唯一硬依赖。缺了会打印一段各平台安装指引然后 `sys.exit(1)`（`ecc_dashboard.py:8-23`），并推荐改用 `npm run dashboard:web` |
| 第三方 pip 包 | **零**。只用标准库 `tkinter/os/json/pathlib/logging/webbrowser/platform/subprocess` |

⚠️ 坑：仓库根的 `pyproject.toml` **不是给 dashboard 用的**。它声明的是一个叫 `llm-abstraction` 的包，源码在 `src/llm`，依赖 `anthropic`/`openai`（`pyproject.toml:2, 22-25, 47`）。跟 dashboard 完全无关，别被误导去 `pip install -e .`。

### 3.4 读哪些数据源

四个 loader，全部是**扫仓库目录 + 手写解析 YAML frontmatter**（没用 pyyaml，是字符串切割）：

| loader | 行号 | 扫描目标 | 提取什么 |
|---|---|---|---|
| `load_agents` | `ecc_dashboard.py:44-106` | `agents/*.md` | frontmatter 的 `name` / `description` |
| `load_skills` | `ecc_dashboard.py:108-186` | `skills/*/SKILL.md` | 目录名 + description |
| `load_commands` | `ecc_dashboard.py:188-235` | `commands/*.md` | 命令名 + description |
| `load_rules` | `ecc_dashboard.py:237-281` | `rules/**/*.md` | 按语言分组 |

`load_agents` 有个值得注意的注释（`ecc_dashboard.py:46-49`）：「目录是唯一真源，`AGENTS.md` 是手维护的、会漂移」——作者自己承认文档和实际不同步。

另一个 code smell：`load_agents` 在扫不到目录时会 fallback 到一份**硬编码的 19 个 agent 列表**（`ecc_dashboard.py:82-104`）。仓库现在有 67+ 个 agent，这份 fallback 早就过时了；一旦触发，GUI 会显示一份假数据而不报错。

### 3.5 UI 结构

`class ECCDashboard(tk.Tk)`（`ecc_dashboard.py:287`），5 个 tab：

1. Agents（`:397`）— 搜索框 + Treeview 列表 + 详情面板
2. Skills（`:502`）— 加分类筛选
3. Commands（`:624`）
4. Rules（`:674`）— 加语言筛选
5. Settings（`:751`）— 项目路径 / 明暗主题 / 字体族 / 字号 / 4 个快捷按钮（开终端、开 README、开 AGENTS.md、刷新）

安全性上做过一轮加固：`_open_project_doc`（`ecc_dashboard.py:841-851`）用 `os.path.commonpath` 拦目录穿越；`launch_terminal`（`scripts/lib/ecc_dashboard_runtime.py:64-70`）用 list argv 且不开 `shell=True`。

### 3.6 明显失修的痕迹

| 现象 | 证据 |
|---|---|
| 版本号写死且过时 | 标题栏写 `v1.10.0`（`ecc_dashboard.py:370`），About 框里同时写 `v1.0.0` 和 `Version: 1.10.0`（`ecc_dashboard.py:814-821`）；仓库 `VERSION` 文件是 `2.1.0` |
| 无测试 | `tests/` 下没有任何 `.py` 测试覆盖它 |
| 硬编码 fallback agent 列表过时 | `ecc_dashboard.py:82-104`，19 条 vs 实际 67+ |

> 🔑 一句话：`ecc_dashboard.py` 是个「看看仓库里有哪些 agent/skill」的静态浏览器，2026 年的 ECC 已经不靠它了。想看运行时数据要去 §4 的三个 JS 脚本。

---

## 4. scripts/ 下的可观测性工具

⚠️ **先破一个误解**：名字里带 "observability" / "dashboard" 的这三个脚本，**没有一个是在观测你的 agent 运行时**。真正的运行时观测在 §2.5 的 hook 三件套 + `/cost-report` 命令里。

| 脚本 | 行数 | 它实际在干什么 |
|---|---|---|
| `scripts/observability-readiness.js` | 462 | 检查**仓库里有没有某些文件、文件里有没有某些字符串**，打个 21 分的满分表 |
| `scripts/operator-readiness-dashboard.js` | 1191 | 维护者自己的**发版待办清单 + MRR 增长目标**看板 |
| `scripts/dashboard-web.js` | 951 | 本地 HTTP 组件浏览器（agents/skills/commands/rules/mcp/hooks），无运行时数据 |

### 4.1 observability-readiness.js —— 一个「文档存在性」自评表，不是观测工具

```bash
npm run observability:ready              # package.json:438
node scripts/observability-readiness.js --format json
```

**机制本体**：它定义 10 条 check（`observability-readiness.js:141-351`），每条 check 的 `pass` 判定就是两件事：

1. `fs.existsSync(某个文件)` —— 文件在不在
2. `text.includes('某个字符串')` —— 文件里有没有这段字面量

举个具体例子，`ecc2-tool-risk-ledger` 这条（3 分，`observability-readiness.js:229-239`）的通过条件是：
- `ecc2/src/observability/mod.rs` 文件存在
- 该文件文本里同时含有 `ToolCallEvent`、`RiskAssessment`、`ToolLogger` 三个词
- `ecc2/src/session/store.rs` 里含有 `insert_tool_log`、`query_tool_logs`
- `ecc2/src/session/manager.rs` 里含有 `sync_tool_activity_metrics`、`tool-usage.jsonl`

也就是说：**只要这些字符串出现在文件里（哪怕在注释里、哪怕函数体是 `todo!()`），这条就 PASS 3 分。** 它不编译、不跑测试、不检查行为。

更极端的是 `release-safety-evidence`（3 分，`observability-readiness.js:285-340`）：判定条件是 8 个文档文件存在 + 在这些文档里 grep 出 `TanStack`、`Mini Shai-Hulud`、`GitGuardian`、`npm audit signatures` 等约 30 个字面量。这就是**「我写了文档所以我合规」**。

**实测（本次只读研究里跑过）**：

```
Observability Readiness: 21/21
Ready: yes
（10 条全 PASS）
```

满分。但这个满分**唯一的含义是「作者写的文件都还在原地、关键词没被删」**。它是防止重构时误删的回归检查（作者原话 `deterministic: true`，`observability-readiness.js:386`），把它当成「ECC 的可观测性做得好」的证据是错的。

评分口径写死在 `RUBRIC_VERSION = '2026-05-11'`（`observability-readiness.js:7`）。

### 4.2 operator-readiness-dashboard.js —— 维护者的发版 OKR 看板

```bash
npm run operator:dashboard                                        # package.json:439
node scripts/operator-readiness-dashboard.js --format text --skip-github
```

跑出来长这样（本次实测节选）：

```
ECC Operator Readiness Dashboard: work remaining
Dashboard ready: true
Publication ready: false

Growth baseline:
  MRR: $1,728/mo -> $10,000/mo (gap $8,272/mo)

Requirements:
  IN_PROGRESS public-pr-budget: Keep public PRs below 20
  COMPLETE   completion-dashboard: ...
  CURRENT    release-video-suite: Produce the ECC 2.0 release video suite
  IN_PROGRESS partner-sponsor-talks-pack: Prepare sponsor, partner, consulting, podcast, talk copy
  ...
```

**这是作者本人的项目管理面板**：PR/issue 队列水位、赞助商话术准备度、发版视频套件、Linear roadmap 同步、甚至 MRR 从 $1,728 涨到 $10,000 的差距（`operator-readiness-dashboard.js:593-625` 的 `extractGrowthBaseline` / `buildGrowthSummary`）。

数据来源同样是**读文档 grep 关键词**（`buildRequirements`，`operator-readiness-dashboard.js:627-970`），外加可选的 `gh` CLI 实时查 PR/issue（`--skip-github` 可关）。

对使用者而言这个脚本没有任何价值——它衡量的是「ECC 这个开源项目自己发版准备好了没」，不是「你的 agent 跑得怎么样」。

### 4.3 dashboard-web.js —— 浏览器版组件目录

```bash
npm run dashboard:web       # package.json:457 → node scripts/dashboard-web.js
# 默认 http://127.0.0.1:3456
```

起一个 Node 原生 `http` server（无 express 无任何第三方依赖），把 `agents/` `skills/` `commands/` `rules/` `.mcp.json` `hooks/hooks.json` 扫一遍，渲染成一个单页 HTML（`dashboard-web.js:161` 的 `renderHTML`）。带搜索、多语言、主题切换、最近浏览（localStorage）。

安全上做过加固：强制绑定 loopback，`ECC_DASHBOARD_HOST` 只接受 `127.0.0.1`/`localhost`/`::1`，否则直接抛错（`dashboard-web.js:25-34`），并有 Host / Origin 头校验（`scripts/lib/loopback-guard.js`）。

**同样零运行时数据**：grep `metrics|costs.jsonl|token|session_id|tool-usage` 在这个文件里零命中。它跟 `ecc_dashboard.py` 是同一个东西的两种 UI（一个 Tkinter 一个浏览器），`ecc_dashboard.py:21` 的错误提示里也明说了这层替代关系。

### 4.4 真正能看运行时数据的东西

这几个才是「观测你的 agent」：

| 工具 | 数据源 | 说明 |
|---|---|---|
| `/cost-report`（`commands/cost-report.md`） | `~/.claude/metrics/costs.jsonl` | **实际可用的花费报表**。注意它不是脚本，是一段写在 markdown 里让模型执行的 `node -e '...'` 内联代码 |
| `scripts/hooks/ecc-statusline.js` | `/tmp/ecc-metrics-{session}.json` + Claude Code stdin | 状态栏：`model │ 当前任务 │ $花费 N工具 N文件 时长 │ 目录 ██░░ N%` |
| `scripts/hooks/session-activity-tracker.js` | 写 `~/.claude/metrics/tool-usage.jsonl` | 每次工具调用记一行（工具名、参数摘要、文件 diff 预览），有 secret 脱敏（`:31`）和 220 字符截断（`:45`） |
| `scripts/session-inspect.js` / `scripts/sessions-cli.js` / `scripts/loop-status.js` / `scripts/status.js` | 各类会话/编排状态文件 | 查看会话、循环、编排状态 |

关于 `/cost-report`（`commands/cost-report.md:20-24`）：它明确指出 costs.jsonl 每行是**该会话的累计快照**，所以聚合时要「按 session_id 取最新一行再求和」，直接把所有行加起来会重复计数。这个坑跟 §2.5 讲的 cost-tracker 演化史是同一个。

⚠️ **一个未接线的洞**：`scripts/hooks/cost-tracker.js:24-35` 定义了一份「harness-cost 契约」——如果 statusline 把 Claude Code 给的权威 `cost.total_cost_usd` 写到 `/tmp/harness-cost-<session>.json`，cost-tracker 就优先用那个准确值。但全仓库 grep `harness-cost` 只命中 cost-tracker 自己和它的测试，**ECC 自带的 `ecc-statusline.js` 并不写这个文件**。结论：开箱即用状态下，你看到的所有金额都是**基于硬编码费率表的估算值**（`cost-tracker.js:73-77`），不是账单真值。费率表也没有 Opus 长上下文 2x 档、1 小时缓存 2x 档（作者在 `cost-tracker.js:30-33` 自己承认会低估）。

---

## 5. ecc2/ Rust runtime 解剖

### 5.1 定位：不是「ECC 下一代」，是「ECC 之上的一层」

`ecc2/README.md:1-5` 自述：

> `ecc2/` is the current Rust-based ECC 2.0 control-plane scaffold. It is usable as an alpha for local experimentation, but it is **not** the finished ECC 2.0 product yet.

关键定位句在 `ecc2/README.md:16`：

> **ECC 2.0 is the layer above individual harness installs.**

白话：主仓库那堆 JS 是「装进 Claude Code 里的插件」，ecc2 是「在你的终端里另开一个程序，同时管很多个 Claude Code 会话」。crate 名字叫 `ecc-tui`（`ecc2/Cargo.toml:2`），描述是「Agentic IDE control plane with TUI dashboard」。

所以**它不是要替换 JS 那套**，两者是上下层关系。目标能力（`ecc2/README.md:18-23`）：一个界面管多个 agent 会话 / 保持会话状态、输出、风险可见 / 编排 + worktree 管理 + review 控制。

README 里还有一节叫 **"Repo Rule"**（`ecc2/README.md:71-81`），原文：

> Do not market `ecc2/` as done just because the scaffold builds.

作者自己给自己立了「别拿能编译当做完了」的规矩。这点在这个仓库里算难得的诚实。

### 5.2 代码行数统计

```
find src -name "*.rs" | xargs wc -l    → 16 个文件，52,139 行
```

| 文件 | 总行 | 测试行(估) | 生产行(估) | `#[test]` 数 |
|---|---:|---:|---:|---:|
| `src/tui/dashboard.rs` | 15,165 | ~5,712 | ~9,453 | 134 |
| `src/main.rs` | 12,595 | ~4,137 | ~8,458 | 114 |
| `src/session/manager.rs` | 8,175 | ~3,963 | ~4,212 | 30 |
| `src/session/store.rs` | 7,113 | ~2,043 | ~5,070 | 35 |
| `src/worktree/mod.rs` | 2,677 | ~1,050 | ~1,627 | 21 |
| `src/config/mod.rs` | 1,806 | ~902 | ~904 | 32 |
| `src/session/daemon.rs` | 1,322 | ~824 | ~498 | 19 |
| `src/session/mod.rs` | 984 | ~261 | ~723 | 14 |
| `src/notifications.rs` | 636 | ~201 | ~435 | 12 |
| `src/observability/mod.rs` | 423 | ~124 | ~299 | 5 |
| `src/tui/widgets.rs` | 382 | ~87 | ~295 | 5 |
| `src/session/runtime.rs` | 379 | ~129 | ~250 | 2 |
| `src/session/output.rs` | 172 | ~32 | ~140 | 2 |
| `src/comms/mod.rs` | 156 | 0 | 156 | 0 |
| `src/tui/app.rs` | 151 | 0 | 151 | 0 |
| `src/tui/mod.rs` | 3 | 0 | 3 | 0 |
| **合计** | **52,139** | **~19,465** | **~32,674** | **425** |

（生产/测试拆分法：取每个文件最后一处 `#[cfg(test)]` 或 `mod tests` 行号到文件末尾算测试，是估算不是精确值。）

**425 个 `#[test]`，测试占 ~37% 代码量**——这不是 vaporware。作为参照，主仓库的 JS 测试目录 `tests/` 也有几十个文件，但 Rust 这边测试密度更高。

### 5.3 有没有 stub

`grep -rn "todo!()\|unimplemented!()\|not yet implemented\|stub" ecc2/src/` → **零命中**。

也就是说这 5 万行里没有「留个空函数占位」的地方。**但注意**：没有 `todo!()` 不等于功能齐全，只说明作者写的东西都写完了；README 自己列了一长串 "What Is Still Missing"（`ecc2/README.md:60-69`）：多 agent 编排、agent 之间显式委派与摘要、可视化 worktree/diff review、更强的跨 harness 兼容、更深的 memory 与 roadmap 规划层、发布打包与安装故事。

⚠️ 本次研究**没有执行 `cargo build` / `cargo test`**（会往仓库写 `target/`，违反只读约束）。所以「能编译、425 个测试全绿」这件事**未验证**——只是 `CHANGELOG.md:73` 声称 `cargo build --manifest-path ecc2/Cargo.toml` 在基线上通过。要自己验证的话：`cd /Users/aa00158/harness-research/ECC/ecc2 && cargo test`（会下载依赖并生成 target/，非只读）。

### 5.4 模块逐个核查

#### `src/observability/mod.rs`（423 行，299 生产行）—— 真实现

三块东西：

1. **`ToolCallEvent`**（`:7-17`）：一条工具调用记录，字段有 session_id / tool_name / 输入摘要 / 输入参数 JSON / 输出摘要 / 触发摘要 / 耗时 ms / **风险分**。

2. **风险打分 `compute_risk`**（`:60-104`）：四个维度加权求和后 clamp 到 [0,1]：

   | 维度 | 规则 | 行号 |
   |---|---|---|
   | 工具基础风险 | bash +0.20 / write,multiedit +0.15 / edit +0.10 / 其他 +0.05 | `:121-131` |
   | 文件敏感度 | 输入含 `.env`/`secret`/`token`/`id_rsa`/`.pem` 等 +0.25；含 `package.json`/`dockerfile`/`.github/workflows`/`migration`/`production` 等 +0.15 | `:133-169` |
   | 爆炸半径 | 含 `git push --force`/`rm -rf /`/`origin main` 等 +0.35；含 `**`/`--all`/`--recursive`/`find `/`xargs` 等 +0.25 | `:171-205` |
   | 不可逆性 | 含 `rm -rf`/`git reset --hard`/`drop table`/`shred ` +0.45；含 `rm -f`/`delete from` +0.40 | `:207-233` |

   分数映射到四档动作 `Allow / Review / RequireConfirmation / Block`（`:107-119`），阈值从 config 读（默认见 `src/config/mod.rs:333-335`，advisory 0.50 / warning 0.75 / critical 0.90 —— 注意这组是 budget 阈值；risk 阈值是 `Config::RISK_THRESHOLDS`）。

   这就是**纯字符串匹配的启发式**，不是模型判断。优点是确定性、零成本；缺点是绕过很容易（比如 `rm -r -f` 就匹配不到 `rm -rf`）。

3. **`ToolLogger`**（`:261-297`）：写入/分页查询 SQLite。`query` 对 `page_size == 0` 会 bail（`:287-289`）。

调用方：`src/session/manager.rs:23, 1484, 1500`——真接上了。

#### `src/session/store.rs`（7,113 行）—— 真实现，但有一个真 bug

SQLite 状态库（rusqlite bundled，`Cargo.toml:20`），默认落在 `~/.claude/ecc2.db`（`src/config/mod.rs:288`）。

**它怎么跟 JS 那边的数据接上**——两个 sync 函数，都是「读 JS hook 写的 JSONL 文件，聚合进 SQLite」：

| 函数 | 读什么 | 行号 |
|---|---|---|
| `sync_cost_tracker_metrics` | `<db同级>/metrics/costs.jsonl`（默认 `~/.claude/metrics/costs.jsonl`，正是 `scripts/hooks/cost-tracker.js` 写的那个文件） | `store.rs:1654-1724`，路径 `config/mod.rs:342-349` |
| `sync_tool_activity_metrics` | `~/.claude/metrics/tool-usage.jsonl`（`scripts/hooks/session-activity-tracker.js` 写的） | `store.rs:1726+`，路径 `config/mod.rs:351-356` |

> 🔴 **确认的 bug：ecc2 会把成本算高好几倍。**
>
> `store.rs:1696-1699` 对同一个 session_id 的每一行做 `+=` 累加：
> ```rust
> aggregate.input_tokens = aggregate.input_tokens.saturating_add(row.input_tokens);
> aggregate.output_tokens = aggregate.output_tokens.saturating_add(row.output_tokens);
> aggregate.cost_usd += row.estimated_cost_usd;
> ```
> 但 `costs.jsonl` 的每一行是**该会话到当前为止的累计快照**，不是增量。这一点在三个地方都白纸黑字写着：
> - `scripts/hooks/cost-tracker.js:19-23`：「Each row therefore represents the cumulative session total up to that point. To get per-session cost, take the last row per session_id.」
> - `commands/cost-report.md:20-24`：「summing every row would multiply-count」
> - `/cost-report` 的实现确实是取 latest per session（`commands/cost-report.md` 的 `node -e` 片段里 `if(!p||String(r.timestamp)>String(p.timestamp))bySession.set(k,r)`）
>
> **量级**：一个会话有 N 次 Stop（N 轮对话），累计值线性增长时，ecc2 报出的数是真值的约 (N+1)/2 倍。20 轮对话的会话会显示成真实花费的 ~10 倍。
>
> 更糟的是**测试把错误行为固化了**：`store.rs:5347-5390` 的 `sync_cost_tracker_metrics_aggregates_usage_into_sessions` 用两行（100+40 tokens、$0.11+$0.05）断言结果是 140 tokens / $0.16。所以这个 bug 有测试保护，不会被偶然修掉。
>
> 对照组：`sync_tool_activity_metrics` 就做对了——它用 `seen_event_ids.insert(row.id)` 去重（`store.rs:1795-1797`），因为 tool-usage.jsonl 每行是独立事件。说明作者知道去重这回事，只是没意识到 costs.jsonl 语义不同。

调用点：`main.rs:2780-2781`（CLI 启动时同步一次）、`tui/dashboard.rs:520-523`（TUI 启动）、`tui/dashboard.rs:4028-4039`（TUI 内刷新）。

#### `src/tui/dashboard.rs`（15,165 行）—— 真实现，最大的模块

ratatui 写的终端界面。跟 token 相关的部分：

- `TokenMeter`（`src/tui/widgets.rs:74-124`）：两种模式 `tokens()` 和 `currency()`，画一个 Gauge 进度条
- `BudgetState`（`widgets.rs:9-66`）：六档 `Unconfigured / Normal / Alert50 / Alert75 / Alert90 / OverBudget`，带颜色（青→黄→亮红→红）和加粗
- 默认预算（`src/config/mod.rs:311-312`）：**成本 $10、token 50 万**；告警阈值 50% / 75% / 90%（`config/mod.rs:333-335`）
- 使用点：`dashboard.rs:1343`（token meter）、`:1352`（currency meter）、`:4155-4168`（聚合汇总行）、`:6409-6411`（单会话 input/output/total）

也就是说 **ecc2 的 TUI 里真的有一个「本会话烧了多少 token / 多少钱 / 占预算多少」的仪表盘**——这是整个 ECC 里唯一一处可视化的 token 预算。但因为 §上面那个 bug，上面显示的数字偏高。

#### `src/session/`（manager 8175 / store 7113 / daemon 1322 / mod 984 / runtime 379 / output 172）—— 真实现

会话生命周期：start / stop / resume / 后台 daemon / 输出捕获（带 `OUTPUT_BUFFER_LIMIT` 环形缓冲，`session/output.rs`）。

#### `src/worktree/mod.rs`（2,677 行）—— 真实现

git2 绑定（`Cargo.toml:23`，带 vendored-openssl feature），做 git worktree 的创建/状态/合并队列/清理。CLI 里有 `worktree-status`、`worktree-resolution`、`merge-worktree`、`merge-queue`、`prune-worktrees`（`main.rs:292-351`）。

#### `src/main.rs`（12,595 行）—— CLI 入口，命令非常多

`clap` derive 定义的 subcommand 有 **40+ 个**（`main.rs:109-438` 主命令组，加上 comms/schedule/remote/graph 等子组）。README 只列了 7 个（dashboard/start/sessions/status/stop/resume/daemon），实际远不止：`delegate`、`assign`、`drain-inbox`、`auto-dispatch`、`coordinate-backlog`、`rebalance-team`、`log-decision`、`decisions`、`graph`、`migrate`、`schedule`、`remote`、`export-otel`、`messages`、`run-session`……

> 🟢 **值得单独点名：`export-otel`**（`main.rs:403-409`，实现 `main.rs:8126-8250+`）
>
> 把 SQLite 里的会话和工具调用导出成 **OTLP JSON**（OpenTelemetry 的标准 trace 格式），可以喂给 Jaeger / Grafana Tempo 之类的追踪系统。
>
> 数据结构：一个 session = 一个 root span，每次工具调用 = 一个 child span。span 属性带 `ecc.metrics.input_tokens` / `output_tokens` / `tokens_used` / `tool_calls` / `files_changed` / `duration_secs` / `cost_usd`（`main.rs:8188-8197`）和 `tool.risk_score`（`main.rs:8235`）。委派出去的子会话通过 span link 连回父会话 trace（`main.rs:8199-8209`）。
>
> ⚠️ 注意口径：它是**离线导出**（写 stdout 或文件），不是活的 OTLP exporter——没有引 `opentelemetry` crate，是手写 serde struct 拼 OTLP JSON 结构（`Cargo.toml` 依赖列表里没有任何 otel 库）。所以你得自己把导出的 JSON 灌进采集器。
>
> 另外它导出的 token/cost 数字来自 SQLite，也就继承了上面那个累加 bug。

#### `src/comms/mod.rs`（156 行）/ `src/tui/app.rs`（151 行）—— 薄，且零测试

这两个是全仓库仅有的两个没有任何 `#[test]` 的非平凡文件。comms 是会话间消息传递，app.rs 是 TUI 应用外壳。

#### `src/notifications.rs`（636 行）—— 真实现

桌面通知 + webhook 通知（`DesktopNotifier` / `WebhookNotifier`，被 `dashboard.rs:18` 引用）。

### 5.5 和主仓库 JS 的关系

| 维度 | 结论 | 证据 |
|---|---|---|
| 打包 | **完全没打进 npm 包**。`package.json` 的 `files` 数组里没有 `ecc2`，全文 grep `ecc2` 零命中 | `grep -n "ecc2" package.json` → 无输出 |
| 安装 | **不在任何安装 manifest 里**。`manifests/` 和 `scripts/lib/install*.js` grep `ecc2` 零命中 | 同上 |
| npm scripts | **没有** `npm run ecc2` 之类的入口。只能 `cd ecc2 && cargo run` | `ecc2/README.md:38-42` |
| 版本 | **独立版本线**。`ecc2/Cargo.toml:3` 是 `0.1.0`，主仓库 `VERSION` 是 `2.1.0`。CHANGELOG 明说「Kept `ecc2/` versioning independent for now」 | `CHANGELOG.md:41` |
| 数据接口 | **单向：JS 写 JSONL → Rust 读进 SQLite**。Rust 不回写任何 JS 侧文件 | `store.rs:1654, 1726` + `config/mod.rs:342-356` |
| 语言分工 | JS = 装进 harness 的 hook/skill/command；Rust = 跨会话的控制面 TUI | `ecc2/README.md:16` |

**所以「ecc2 是 ECC 下一代形态吗？」的答案是：**

不完全是「下一代」，是「新增的上层」。JS 那套 hook/skill/agent 没有被替代（它们是 ecc2 的**数据源**）。ecc2 想做的是一个「多会话控制台」，跟 JS 那套是互补关系。但从工程投入看（52k 行 Rust vs 主仓库 JS 侧脚本量），作者显然把未来押在 ecc2 上。

**完成度打分（我的判断，置信度中 ~65%）**：

- 代码真实性：高。425 个测试、零 `todo!()`、模块间调用链完整
- 可用性：中低。没有打包、没有安装路径、必须自己装 Rust 工具链 `cargo run`；`rust-toolchain.toml` 存在但本次未读版本
- 正确性：有已确认缺陷（成本重复累加），且被测试固化
- 与主仓库整合度：低。除了读两个 JSONL 文件外，两边基本各过各的

---

## 6. Benchmark 与评测数据

### 6.1 直接结论

**ECC 没有任何针对自己的性能/token benchmark 数据。** 一条都没有。

在仓库里找了这些地方，全部落空：
- `find . -iname "*bench*" -o -iname "*eval*"` 的所有命中，都是**给用户项目用的 skill**（`skills/benchmark/`、`skills/eval-harness/`、`skills/agent-eval/`、`skills/benchmark-methodology/`），不是 ECC 自测数据
- `docs/releases/*/` 下的 evidence 文档（如 `docs/releases/2.0.0-rc.1/publication-evidence-*.md`）是**发版检查清单证据**（npm audit 跑没跑、Dependabot 有没有 alert），不含性能数字
- `research/` 只有一个文件 `research/ecc2-codebase-analysis.md`，是代码统计不是性能测量
- README grep `faster|speedup|benchmark|latency|throughput|measured` 只有 4 处命中，全是提到某个 skill 的名字或 §1 那三个定价推论

`skills/benchmark/SKILL.md` 是让 agent 去测**你的**网页 Core Web Vitals / API p50-p99 / 构建时间的工作流说明，跟 ECC 自身性能无关。

### 6.2 所谓的 "score" 全是文件存在性打分

ECC 有三套「打分器」，跑出来都会给你一个漂亮的分数。**它们的评分函数没有一个在测量运行时行为**：

| 打分器 | 满分 | 评分方式 |
|---|---|---|
| `scripts/observability-readiness.js` | 21 | `existsSync` + `text.includes(字面量)`（见 §4.1） |
| `scripts/harness-audit.js` | 未细数 | 同上 |
| `scripts/operator-readiness-dashboard.js` | 状态枚举 | 同上 + 可选 `gh` 查询（见 §4.2） |

`harness-audit.js` 里那两个跟本维度直接相关的分类，评分标准如下：

**Context Efficiency（上下文效率）** —— 10 分，四条：
- 3 分：`skills/strategic-compact/SKILL.md` 文件存在（`harness-audit.js:447-454`）
- 3 分：`scripts/hooks/suggest-compact.js` 文件存在（`:455-464`）
- 2 分：`commands/model-route.md` 文件存在（`:465-474`）
- 2 分：`docs/token-optimization.md` 文件存在（`:475-484`）

**Cost Efficiency（成本效率）** —— 10 分，三条：
- 4 分：`skills/cost-aware-llm-pipeline/SKILL.md` 存在（`harness-audit.js:625-634`）
- 3 分：`docs/token-optimization.md` 存在（`:635-644`）
- 3 分：`commands/model-route.md` 存在（`:645-654`）

> 🔑 也就是说：**「我的 harness 上下文效率 10/10」的真实含义是「我这四个 markdown 文件都在」**。跟你实际烧多少 token 没有任何关系。`docs/token-optimization.md` 这一个文件同时被两个分类各计一次分。
>
> 这个 rubric 用来做 CI 回归检查（防止重构误删文件）是合理的；用来当「ECC 让你省 token」的证据是不成立的。

### 6.3 那 §1 里的百分比数字能信吗

三个数（60% / 70% / 80%）都属于**厂商定价的算术推论**，可以自己核对但核对的是 Anthropic 价目表，不是 ECC：

| 声称 | 可核对的依据 | 我的判断 |
|---|---|---|
| opus→sonnet ~60% 降本 | Anthropic 定价 opus $15/$75 per M vs sonnet $3/$15 per M（这个费率表 ECC 自己也硬编码在 `scripts/hooks/cost-tracker.js:73-77`） | 方向对，但 60% 这个具体数取决于你的 input/output 比例和缓存命中率，**属于口径不明的估算**。纯按 token 单价算应该是 80% |
| MAX_THINKING_TOKENS 31999→10000 降 ~70% | 31999→10000 正好是降 68.75% —— 但这是**上限**降了 70%，不是实际花费降 70%（大多数请求根本用不满 31999 个思考 token） | **数字上是算出来的，语义上有误导**。置信度：中高 ~75% 认为实际省下的远低于 70% |
| subagent 用 haiku 便宜 ~80% | haiku $0.80/$4 vs sonnet $3/$15，input 便宜 73%，output 便宜 73% | 大致合理 |

**要自己验证的动作**：查 https://www.anthropic.com/pricing 的当期价目表，和 `scripts/hooks/cost-tracker.js:73-77` 里那张硬编码表对一下。

### 6.4 能不能复现

| 想复现的东西 | 能不能 | 怎么做 |
|---|---|---|
| 三个 readiness 分数 | ✅ 能，而且必然复现（纯文件检查，`deterministic: true`） | `node scripts/observability-readiness.js --format json` |
| ECC 自身的 token 节省效果 | ❌ 不能，仓库里没有对照实验、没有任务集、没有基线数据 | 无 |
| 你自己的花费数据 | ✅ 能（但数字是估算不是账单） | 装好 hook 跑几个会话 → `~/.claude/metrics/costs.jsonl` → `/cost-report` |
| ecc2 的 425 个测试 | ⚠️ 未验证（本次没跑，会写 target/） | `cd ecc2 && cargo test`（需 Rust 1.96，见 `ecc2/rust-toolchain.toml:3`） |

### 6.5 一个附带发现：`research/ecc2-codebase-analysis.md` 已经严重过时

这份 2026-03-26 的内部分析报告（172 行）说 ecc2 有 **4,417 行、15 个文件、29 个测试函数**（`research/ecc2-codebase-analysis.md:4, 80`）。

现在实测是 **52,139 行、16 个文件、425 个 `#[test]`**。差了 12 倍。

它列的很多「gap」也已经过时了，例如：
- 「3.6 No Metrics Aggregation」——现在 `tui/dashboard.rs:4155-4168` 有聚合视图了
- 「3.1 Comms Module — Send Without Receive」——comms 从 36 行涨到 156 行，CLI 里也有了 `drain-inbox`/`inbox`（`ecc2/src/main.rs:177, 460`）
- 「3.5 git2 是可删依赖」——`ecc2/Cargo.toml:23` 里 git2 还在，且加了 vendored-openssl feature，看起来是在用了

**教训**：这个仓库里的分析型文档要按「写作日期」打折看，不能当作当前状态的证据。

---

## 7. 结论：哪些主张站得住，哪些站不住

### ✅ 站得住（代码里能验证）

1. **SessionStart 注入上限 8000 字符** —— 硬截断，可配置，README 描述与代码一致（`scripts/hooks/session-start.js:34, 192-204`）
2. **strategic-compact 用真实上下文大小触发** —— 读 transcript 的 usage 三字段求和，窗口自适应，bucket 去重（`scripts/lib/transcript-context.js`）
3. **context-monitor 把警告注入回模型** —— PostToolUse 的 `additionalContext`，阈值明确，按文本去重（`scripts/hooks/ecc-context-monitor.js`）
4. **`ECC_CONTEXT_MONITOR_COST_WARNINGS=off` 只关成本警告** —— 代码与文档一致（`ecc-context-monitor.js:141, 219`）
5. **`/cost-report` 能出真报表** —— 且正确处理了累计快照语义（`commands/cost-report.md:20-24`）
6. **ecc2 的风险打分和 OTLP 导出是真实现** —— 有 425 个测试，无 stub（`ecc2/src/observability/mod.rs`、`ecc2/src/main.rs:8126+`）
7. **ecc2 TUI 有 token/成本预算仪表盘** —— 默认 $10 / 50 万 token，50/75/90% 三档告警（`ecc2/src/config/mod.rs:311-313, 333-335`）

### ⚠️ 半真半假

1. **「Token Optimization」章** —— 内容真实有用，但它教的是 **Claude Code 自带的开关**，不是 ECC 的功能。README 把它放在自己的 feature 章节下容易让人误会
2. **60%/70%/80% 三个节省百分比** —— 定价推论，非测量；「思考 token 降 70%」尤其有误导（降的是上限不是实际用量）→ **仓库声称，未在代码中核实**
3. **`/context-budget` 上下文预算审计** —— 报告模板漂亮，但全靠模型心算（`skills/token-budget-advisor/SKILL.md:37` 原文 "mentally"），无脚本、无可复现性
4. **ecc2 完成度** —— 代码是真的（52k 行 / 425 测试 / 零 stub），但没打包、没安装路径、必须自己 `cargo run`；且本次未验证能否编译通过

### ❌ 站不住

1. **「observability-readiness 21/21 满分」不代表可观测性好** —— 它只检查文件存在和字符串出现（`scripts/observability-readiness.js:141-351`）
2. **`harness-audit` 的 Context/Cost Efficiency 满分** —— 同样只是「四个 markdown 文件都在」（`scripts/harness-audit.js:447-484, 625-654`）
3. **「ECC 帮你省 token」缺乏任何实证** —— 无 benchmark、无任务集、无对照实验、无基线数据
4. **`ecc_dashboard.py` / `dashboard-web.js` 是「可观测性工具」** —— 两者都零运行时数据，是静态组件浏览器
5. **`operator-readiness-dashboard.js` 对使用者有价值** —— 它是作者的发版 OKR 看板（含 MRR 目标）

### 🔴 确认的缺陷

| 缺陷 | 位置 | 后果 |
|---|---|---|
| ecc2 把 costs.jsonl 的累计快照当增量累加 | `ecc2/src/session/store.rs:1696-1699`，测试固化在 `:5347-5390` | TUI 和 export-otel 显示的成本/token 数偏高约 (N+1)/2 倍（N=会话内 Stop 次数） |
| `harness-cost` 契约无写入方 | `scripts/hooks/cost-tracker.js:24-35` 定义，无人实现（`ecc-statusline.js` 不写） | 所有金额永远是估算，不是账单真值；Opus 长上下文 2x / 1h 缓存 2x 档缺失，作者自承会低估（`cost-tracker.js:30-33`） |
| `agent-compress.js` 死代码 | `scripts/lib/agent-compress.js`，只有测试引用 | 现成的 agent 目录压缩 / 懒加载能力没接线 |
| `ecc_dashboard.py` 硬编码 fallback 过时 | `ecc_dashboard.py:82-104` | 目录扫不到时静默显示 19 个假 agent（实际 67+） |
| `research/ecc2-codebase-analysis.md` 过时 12 倍 | 全文 | 按它做判断会严重低估 ecc2 |

---

## 8. 遗留问题 / 未验证项

1. **`cd ecc2 && cargo test` 是否全绿** —— 本次未跑（会生成 `target/`，违反只读约束）。`CHANGELOG.md:73` 只声称 `cargo build` 通过，没提 test。要验证：需 Rust 1.96（`ecc2/rust-toolchain.toml:3`）
2. **ecc2 成本累加 bug 的实际放大倍数** —— 我按「累计值线性增长」推算约 (N+1)/2 倍，这是理论推导。真实倍数取决于会话内 Stop 事件分布，**未实测**（置信度 中 ~60%）
3. ~~hooks.json 默认开哪些~~ —— **已核实，见 §2.6**
4. **`scripts/hooks/pre-compact.js` 到底存了什么** —— 只确认它在 compact 前落盘会话状态，具体内容和恢复路径未细读
5. **`ECC_SESSION_START_MAX_CHARS` 的 8000 字符实际对应多少 token** —— 英文约 2000 token，中文约 4000+ token（中文字符 token 密度更高）。仓库没做这个换算，**未验证**它在中文场景下是否偏松
6. **ecc2 的 `sync_cost_tracker_metrics` 在 `session_id` 不匹配时的行为** —— JS 侧 session id 和 ecc2 自己创建的 session id 是不是同一套命名空间？如果不是，这个 sync 可能根本匹配不到任何行（那样反而不会触发累加 bug）。**未核实**，需要实跑 ecc2 建一个会话看 SQLite 里的 id 格式

