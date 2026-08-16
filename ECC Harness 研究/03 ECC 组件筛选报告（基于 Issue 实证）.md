# ECC 组件筛选报告：基于 682 条 Issue + 8003 条评论的实证分析

> 分析日期：2026-08-01　|　对象：affaan-m/ECC v2.1.0
> 语料：682 条 issue（编号 13~2648，含正文，2.0 MB）+ 8003 条评论（33 MB）
> 全部本地计算，脚本见 `analyze.js` / `sentiment.js`

---

## 一、方法论（可复现）

### 步骤

```bash
# 1. 拉全部 issue 含正文，排除 PR
gh api "repos/affaan-m/ECC/issues?state=all&per_page=100" --paginate \
  --jq '.[] | select(.pull_request == null) | {number,title,body,state,comments,created_at,user:.user.login,labels:[.labels[].name]}' \
  > issues-raw.jsonl

# 2. 一次拉全仓评论（不必逐个 issue 调用）
gh api "repos/affaan-m/ECC/issues/comments?per_page=100" --paginate \
  --jq '.[] | {issue:(.issue_url|split("/")|last), user:.user.login, body, created_at}' \
  > comments-raw.jsonl

# 3. 本地：语料分层 → 组件名匹配 → 句子级情感提取
node analyze.js && node sentiment.js
```

### 三个关键设计

**① 语料分层——先剔除噪音源。** 8003 条评论的发言人分布：

| 发言人 | 条数 | 性质 |
|---|---|---|
| `ecc-tools[bot]` | 2581 | 作者自己的 bot |
| `coderabbitai[bot]` | 1556 | AI 代码审查 bot |
| `affaan-m` | 1530 | 作者本人 |
| `greptile-apps[bot]` | 1228 | AI 代码审查 bot |
| 其它 bot | 129 | dependabot / github-actions / cubic-dev-ai |
| **真实第三方用户** | **≈979** | 416 个不同用户 |

**bot + 作者占 88%。** 只对剩下 12% 做分析，否则结论会被 bot 噪音淹没。

**② 组件名匹配分强弱。** 281 skill + 67 agent + 94 command 共 348 个名字。
- 强匹配：`skills/<name>`、`` `<name>` ``、`<name>/SKILL.md`、`/<name>`、`ecc:<name>`
- 弱匹配（仅用于含连字符且 ≥8 字符的名字，不会误报）：裸词 + 词边界
- 通用词（`taste`/`ck`/`council`/`plan`/`blueprint` 等）只用强匹配，避免大量假阳性

**③ 句子级情感提取。** 把文档切句，只保留同时命中「组件名 + 表态词」的句子。表态词表分正负两组（`false positive`/`disabled`/`broken`/`too many` vs `works well`/`useful`/`saved me`）。

### ⚠️ 方法论的硬局限（必须先说）

**情感提取结果：负面 177 句，正面 2 句。**

GitHub issue **结构上不产生好评**——没人去 issue 里夸东西。所以这套方法：

- ✅ **能可靠回答**「哪些组件有确认的问题」
- ❌ **不能直接回答**「哪些组件好用」

下面第四节用两个**替代信号**来逼近后者，但它们是弱信号，我会明确标注置信度。

---

## 二、强证据：确认有问题的组件

以下每条都能追到 issue 编号 + 发言人 + 原句。

### 🔴 第一梯队：bug 密度最高

**`continuous-learning-v2` / `continuous-learning`（19 + 16 句负面，涉及 17 + 14 个 issue）**

这是全仓 bug 之王，而且是**跨平台全面失败**：

| issue | 发言人 | 问题 |
|---|---|---|
| [#2452](https://github.com/affaan-m/ECC/issues/2452) | @HJJ50 | "On Windows (Git Bash / MSYS2) the continuous-learning-v2 observer is **broken by two independent regressions**" |
| [#2417](https://github.com/affaan-m/ECC/issues/2417) | @AccountZero | "On macOS the background observer **fails to produce instincts unattended, and the failure is self-masking**" |
| [#2431](https://github.com/affaan-m/ECC/issues/2431) | @Guykaganovsky1 | "breaks from the installed location (`~/.claude/skills/continuous-learning-v2/`)" |
| [#2489](https://github.com/affaan-m/ECC/issues/2489) | @Hochmah | Windows 手动安装时又发现另一个 bug |
| [#1231](https://github.com/affaan-m/ECC/issues/1231) | — | "Instincts are stored but **never proactively injected** into session context" |

> 🔑 **"failure is self-masking"（失败会自我掩盖）** 是最要命的一条——它坏了你不会知道，只会以为"还没学到东西"。
> 这和我们解剖笔记 05 的代码级发现完全一致：confidence 分数由 LLM 自己填、衰减逻辑零实现、注入不做相关性匹配、默认 `enabled: false`。
> **两条独立证据链指向同一结论：ECC 最大的卖点 instincts 实际不工作。**

### 🟠 第二梯队：单点明确问题

| 组件 | issue | 原话 / 问题 |
|---|---|---|
| `code-reviewer`（agent） | [#1486](https://github.com/affaan-m/ECC/issues/1486) @ahmed-fawzy99 | "I have used the code-reviewer agent **extensively** over the last few days... it flags many parts of the code as HIGH priority, but **most of the times, they are false positives**." ← 重度用户的一手反馈 |
| `cost-report`（command） | [#2276](https://github.com/affaan-m/ECC/issues/2276) @nhosler | "reads **wrong path/schema** and never finds the cost-tracker data"——报告永远说 tracker 没配置，而 tracker 一直在写数据 |
| `auto-update`（command） | [#1247](https://github.com/affaan-m/ECC/issues/1247) @oryband | 你手动禁用 MCP 的修改**会被 auto-update 覆盖回去** |
| `backend-patterns`（skill） | [#54](https://github.com/affaan-m/ECC/issues/54) @aliyome / [#1213](https://github.com/affaan-m/ECC/issues/1213) | "has **wrong implementation**"；且 response envelope 段与 `api-design` 重复 |
| `mcp-server-patterns`（skill） | [#1213](https://github.com/affaan-m/ECC/issues/1213) | "68 lines, mostly defers to external docs"——**v2.1.0 实测 70 行，四个月只多了 2 行** |
| `gateguard`（skill） | [#2608](https://github.com/affaan-m/ECC/issues/2608) | 对每个新文件的首次 Edit/Write 都拦，应改为按会话 N 次后停止。**仍 OPEN** |
| `suggest-compact`（hook） | [#2461](https://github.com/affaan-m/ECC/issues/2461) / [#2290](https://github.com/affaan-m/ECC/issues/2290) | 在大窗口模型上把 context 百分比算错（Opus 4.x 400k 窗口翻倍） |
| 翻译版 SKILL.md | [#2630](https://github.com/affaan-m/ECC/issues/2630) | **22 个翻译 SKILL.md 的 frontmatter 没有任何 YAML 解析器能接受**。仍 OPEN |
| Windows hooks | [#2368](https://github.com/affaan-m/ECC/issues/2368) [#2043](https://github.com/affaan-m/ECC/issues/2043) [#1985](https://github.com/affaan-m/ECC/issues/1985) [#1484](https://github.com/affaan-m/ECC/issues/1484) [#1454](https://github.com/affaan-m/ECC/issues/1454) | 五条独立报告：内联 `node -e` 引号处理在 Git Bash 下路径变成 `C:\c\Users\` |

---

## 三、两份第三方系统性审计（本报告最有价值的发现）

有两位用户自己做了完整的组件审计，方法比我这套更硬。

### #1430 @Petrusreno（2026-04-14）— agent 冗余审计

用知识图谱工具（graphify v0.4.13，AST + LLM 语义抽取，覆盖 299 个文件）分析 `~/.claude/agents/`，结论：**9 个语言专属 reviewer 应合并成 1 个语言感知的 `code-reviewer`**。

**我核实的部分（成立）**：9 个 reviewer 的 frontmatter 逐字相同——

```
code-reviewer        tools: Read, Grep, Glob, Bash    model: sonnet
python-reviewer      tools: Read, Grep, Glob, Bash    model: sonnet
typescript-reviewer  tools: Read, Grep, Glob, Bash    model: sonnet
rust-reviewer        tools: Read, Grep, Glob, Bash    model: sonnet
go-reviewer          tools: Read, Grep, Glob, Bash    model: sonnet
java-reviewer        tools: Read, Grep, Glob, Bash    model: sonnet
kotlin-reviewer      tools: Read, Grep, Glob, Bash    model: sonnet
cpp-reviewer         tools: Read, Grep, Glob, Bash    model: sonnet
flutter-reviewer     tools: Read, Grep, Glob, Bash    model: sonnet
```

**⚠️ 我核实后要修正它的部分（它夸大了）**：#1430 说 "Same template, same tools, same model — only the language label changes"。但我算了正文行级重合度：

| agent | 正文行数 | 与 code-reviewer 逐字相同 | 重合率 |
|---|---|---|---|
| cpp-reviewer | 61 | 11 | 18% |
| go-reviewer | 64 | 11 | 17% |
| python-reviewer | 75 | 12 | 16% |
| typescript-reviewer | 93 | 11 | 12% |
| java-reviewer | 150 | 10 | **7%** |

**重合只有 7-18%**，那 10-17 行重合的是「防注入模板 6 条 + 输出结构标题」。各语言的检查清单是**真实不同的内容**。

👉 **修正后的正确结论**：合并 17 个 reviewer 不是"零损失去重"，会真丢掉语言专属检查清单。但对**你**的意义反而更明确：**不是因为它们重复所以不该装，而是因为你不写那些语言所以纯属浪费。**

**#1430 明确指出该保留独立的**（我认同）：
- `security-reviewer` —— 范围不同（OWASP、密钥），工具含 Write/Edit 可修复
- `database-reviewer` —— PostgreSQL/Supabase schema/migration
- `refactor-cleaner` —— 删除优先的动作（knip/depcheck/ts-prune），不是 review

**建议采纳情况：未采纳，且问题扩大了。** 2026-04 时是 9 个语言 reviewer + 6 个 build-resolver；v2.1.0（2026-08）变成 **17 个 reviewer + 10 个 build-resolver**。

### #1213 @OnoSendai13（2026-04-03）— skill 冗余审计

用 ECC **自带的 `skill-stocktake` skill** 审计了当时全部 ~88 个 skill，方法五步：库存扫描 → 内容重叠分析 → 时效性检查 → 使用频率 → 综合判断，且"三批全部由 sub-agent 并行独立评估"。

**建议删除 4 个，我核实 v2.1.0 的采纳情况**：

| 建议删除 | 理由 | v2.1.0 现状 |
|---|---|---|
| `continuous-learning`（v1） | 被 v2 完全取代，v1 自己列的"潜在 v2 增强"在 v2 里已实现 | ❌ **仍在** |
| `coding-standards` | 与 `frontend-patterns`/`api-design`/`backend-patterns` 大量重叠；通用原则（KISS/DRY/YAGNI）该进 rules 不该做 skill | ❌ **仍在** |
| `project-guidelines-example` | 不是可复用 skill，是 Zenith 项目专属模板，含硬编码路径 | ✅ 已删 |
| `playwright-cli` | 279 行 CLI 目录，不是 skill | ✅ 已删 |

**还给了 8 个 skill 的具体改进项**（我实测了当前行数）：

| skill | #1213 指出的问题 | v2.1.0 行数 |
|---|---|---|
| `tdd-workflow` | 非常 TypeScript/Next.js 专属但没标注 | 583 |
| `backend-patterns` | envelope 段重复 api-design；Supabase 例子对通用标题太窄 | 562 |
| `django-verification` | 12 个阶段太重；pip-audit 与 safety 检查冗余 | 470 |
| `plankton-code-quality` | 80% 在讲一个外部工具的架构 | 237 |
| `iterative-retrieval` | 伪代码不能直接用；外部引用（2025-01）已过时 | 212 |
| `verification-loop` | 与 `springboot-verification` 结构高度重叠 | 129 |
| `mcp-server-patterns` | 68 行，大部分只转引外部文档 | 70 |

**这份审计的结局**：被作者用批量模板关闭——
> "Thanks for opening this. I am closing it in the cleanup pass because it is stale, broad, or has been superseded by newer cleanup/fix work."

同样模板关闭的还有 #1486（那条 code-reviewer 重度用户反馈）。

**⚠️ 关于这个信号我要给准确的量级，不夸大**：这类模板回复全仓只有 **36 条，占作者 1530 条评论的 2.4%**——**不是普遍做法**。但 #1213 和 #1486 这两条高质量反馈确实在这 36 条里。

---

## 四、逼近"哪些好用"：两个替代信号（弱证据，标注置信度）

既然 issue 里没有好评，我用行为信号替代。

### A 组：被 ≥3 个第三方 issue 提到、且零负面表态

**含义**：有人在用（提到了）+ 没人报问题。**置信度：低（约 30%）**——因为"被提到"很多时候只是出现在文件列表或安装日志里，不代表在用。

**agent（仅 3 个）**：`architect`（11 issue）、`chief-of-staff`（4）、`code-architect`（3）

**skill（26 个，摘对你可能相关的）**：`rules-distill`(7)、`delivery-gate`(6)、`growth-log`(6)、`security-review`(6)、`skill-comply`(6)、`context-budget`(5)、`browser-qa`(4)、`plan-orchestrate`(4)、`search-first`(4)、`skill-scout`(3)、`iterative-retrieval`(3)

**command（22 个，摘相关的）**：`quality-gate`(11)、`feature-dev`(7)、`harness-audit`(7)、`learn`(7)、`multi-plan`(6)、`build-fix`(5)、`plan-canvas`(3)、`skill-health`(3)、`project-init`(3)

### B 组：从未被任何第三方用户提到（348 个里 104 个）

**含义有歧义**：可能"没人用"，也可能"用了没问题"。但因为 issue 是 bug 报告场所，**更偏向前者。置信度：中（约 60%）**。

- **agent 17 个**：`code-simplifier`、`csharp-reviewer`、`django-build-resolver`、`django-reviewer`、`fastapi-reviewer`、`fsharp-reviewer`、`harmonyos-app-resolver`、`marketing-agent`、`mle-reviewer`、`performance-optimizer`、`pr-test-analyzer`、`react-build-resolver`、`react-reviewer`、`seo-specialist`、`silent-failure-hunter`、`type-design-analyzer`、`vue-reviewer`
- **skill 77 个**：含全部 `ito-*`（5 个预测市场/算力）、全部 `scientific-*`（5 个）、全部 `*-ops`（8 个运营后台）、`quarkus-*`（4）、`laravel-*`（4）、`springboot-*`（3）、`golang-*`（2）、`rust-*`（2）、`deep-research`、`exa-search`、`github-ops`、`benchmark` 等
- **command 10 个**：全部 7 个 `epic-*` + 3 个 `react-*`

> 🔑 **`epic-*` 全部 7 个命令零第三方提及**，这套「Epic 协作」体系可能完全没人用。

---

## 五、给你的筛选清单

前提：你是金融背景、不写传统代码、靠 AI 主导项目、Python 用于量化、已有 79 个自建 skill。

### ✅ 值得单独取用的（用 `cp -R`，不要用 `--skills`）

**理由分级**：作者默认推荐 > 第三方审计认可 > A 组弱信号 > 与你需求匹配

| 组件 | 类型 | 为什么 | 证据强度 |
|---|---|---|---|
| `search-first` | skill | 先调研再动手，与你的 github-research 思路同源可对照 | A 组 4 issue 零负面 + 作者列入 Core 推荐 |
| `skill-stocktake` | skill | 审计你自己那 79 个 skill 的臃肿情况——**#1213 那份审计就是用它做的，已被实战验证** | 高：有公开产出物为证 |
| `config-gc` / `skill-scout` | skill | 同上，配置垃圾回收 + skill 发现 | 中 |
| `rules-distill` | skill | 从会话里蒸馏规则，和你的 `/learn` 互补 | A 组最高分 7 issue 零负面 |
| `strategic-compact` | skill | 上下文策略性压缩。**全仓仅 2 条正面表态之一** | 中（唯一有正面表态的） |
| `architect` | agent | A 组 agent 最高分 11 issue 零负面 | A 组 |
| `code-explorer` / `code-architect` | agent | 读懂陌生代码库（你研究别人仓库时用） | A 组 |
| `python-reviewer` | agent | 你唯一会用的语言 | 与需求匹配 |
| `gateguard` 的**设计思想** | — | 「问模型『你违规了吗』永远答没有；问『列出所有 import 这个模块的文件』会逼它真去 Grep」——**这个洞察值得移植进你自己的 hook，但别装它本体**（#2608 过度拦截未修） | 高（思想）/ 低（实现） |

### ❌ 明确不要装

| 不装什么 | 数量 | 理由 |
|---|---|---|
| `continuous-learning` + `continuous-learning-v2` | 2 | **全仓 bug 之王**（35 句负面/31 个 issue），Windows 全坏、macOS 后台失败且 self-masking、instinct 从不主动注入。两条独立证据链（issue + 代码）都指向"不工作" |
| 除 `python-reviewer` 外的 16 个语言 reviewer | 16 | 你不写那些语言。正文重合仅 7-18% 说明它们不是冗余副本，但对你就是纯浪费 |
| 全部 10 个 `build-resolver` | 10 | 你不做构建 |
| 全部 7 个 `epic-*` command | 7 | 零第三方提及，可能完全没人用 |
| 全部 `docs-*` 翻译 module | 8 | `cost: heavy` + [#2630](https://github.com/affaan-m/ECC/issues/2630) 22 个翻译 SKILL.md 的 frontmatter 无法被 YAML 解析器接受，**仍 OPEN** |
| `cost-report` | 1 | [#2276](https://github.com/affaan-m/ECC/issues/2276) 读错路径，永远找不到数据 |
| `auto-update` | 1 | [#1247](https://github.com/affaan-m/ECC/issues/1247) 会覆盖你手动禁用 MCP 的设置 |
| `backend-patterns` / `coding-standards` / `mcp-server-patterns` | 3 | #1213 审计明确指出内容质量问题，作者未采纳修改 |
| `framework-language` 里除 Python 外的 67 个 | 67 | Django/Laravel/Quarkus/SpringBoot/Kotlin/Rust/C#/F#/Perl/Vue/Angular/Nuxt… 全部与你无关 |
| `supply-chain-domain`(8) / `prediction-market`(6) / `ito-*`(5) / `media-generation`(8) / `devops-infra`(16) / `swift-apple`(7) | 50 | 与你需求无关，且 `ito-*` 和多数 `*-ops` 零第三方提及 |
| `deep-research`（ECC 版） | 1 | 会与你现有的 `deep-research` skill 撞名（虽然会命中跳过保护，但没必要） |

### 📊 筛选后的量级

| | 全装 | 筛选后 |
|---|---|---|
| skill | 281 | **约 8-10 个** |
| agent | 67 | **约 4 个** |
| 常驻 catalog 成本 | 18.9k token/轮 | **< 1k token/轮** |

---

## 六、可信度总评

**判断：这个仓库有真实使用者，但规模远小于 236k star 暗示的量级。置信度：中高（约 75%）**

支撑依据：

1. **88% 的评论来自 bot 或作者本人**（7024 / 8003）。真实第三方评论约 979 条，来自 416 个用户。
2. 但**确实存在高质量第三方反馈**：#2482（实测 12.8k token/轮）、#1579（发现重复打包官方 skill）、#1213（用 skill-stocktake 审计 88 个 skill）、#1430（用知识图谱审计 299 个 agent 文件）——这四条的技术深度不可能是刷的。
3. **作者对高质量反馈的处理不一致**：#1579 两项建议全部采纳（legacy shim 清零、官方 skill 重复打包移除）；#1213 只采纳一半并被批量模板关闭；#1430 完全未采纳且问题扩大（9→17 reviewer）。
4. **组件覆盖率**：348 个组件中，被任何第三方提到过的只有 244 个（70%），**104 个从未被任何真实用户提及**。

> 🔑 最能说明问题的一条：ECC 自带 `skill-stocktake`（技能库存审计）和 `config-gc`（配置垃圾回收）两个"治臃肿"的工具，而 #1213 用前者审出的删除建议，四个月后只落实了一半。**框架提供了治臃肿的工具，但自己没用它治自己。**
