# mattpocock/skills vs ECC：两种 skill 集合的设计对比

> 对比日期：2026-08-03　|　全部数据本机实测或来自 GitHub API
> 详细解剖见：[[01 engineering 17 个 skill 深度详解]]、[[../ECC Harness 研究/00 索引 - 从这里开始|ECC 研究总索引]]

---

## 一句话结论

**两个仓库 star 数在同一量级（200k vs 237k），但社区真实度差 6 倍，工程纪律差一个档次。** ECC 赢在目录式大而全，mattpocock 赢在每一条设计决策都有明确取舍。

---

## 一 · 硬数据对比

### 1.1 规模与形态

| 维度 | mattpocock/skills | affaan-m/ECC | 差异 |
|---|---|---|---|
| 仓库体积 | **957 KB** | 48 MB（克隆后 86 MB） | **50 倍** |
| star | 200,596 | 236,723 | 同量级 |
| fork | 17,300 | 35,987 | ECC 2 倍 |
| open issues | 270 | 117（含 PR 则更多） | — |
| 创建日期 | 2026-02-03 | 2026-01-18 | 几乎同期 |
| 许可 | MIT | MIT | 同 |
| skill 总数 | **41**（跨 6 bucket） | **281** | **6.9 倍** |
| 实际出货的 skill | **22**（plugin ship 的 promoted 集） | 281（plugin 全出）/ 43（默认 module） | — |
| agent 数 | 0 | 67 | ECC 独有 |
| command 数 | 0（skill 即命令） | 94 | 设计路线不同 |
| hook 脚本 | 0（`misc/git-guardrails-claude-code` 教你自己配） | 48 | ECC 独有 |

### 1.2 SKILL.md 的形态（最能说明设计哲学）

| 维度 | mattpocock/engineering（17 个） | ECC/workflow-quality（43 个） |
|---|---|---|
| SKILL.md 总行数 | **1,223** | 约 8,000+ |
| 中位行数 | **76** | 189 |
| 最短 | **8**（`grill-with-docs`） | 80（`repo-scan`） |
| 最长 | **135**（`diagnosing-bugs`） | **889**（`windows-desktop-e2e`） |
| 纯单文件（只有 SKILL.md） | **0 个（0%）** | **36 个（84%）** |
| 带附加文件 | **17 个（100%）** | 7 个（16%） |
| 用 `references/` 目录 | 0（用大写命名的顶层 md，如 `DEEPENING.md`） | 1（`agent-self-evaluation`） |
| 跨 harness 声明（`agents/openai.yaml`） | **17/17（100%）** | 0（另有 39/281 个 `.agents/` 平行副本） |
| 常驻 catalog 成本 | **≈800 token/轮** | **18,919 token/轮**（全装）/ 2,715（默认 module） |

> 🔑 **这组数字是全篇最重要的**：mattpocock 的 SKILL.md 中位只有 76 行、100% 有附加文件；ECC 是 189 行、84% 单文件塞满。
>
> **这才是 progressive disclosure 的正确实现**——SKILL.md 只负责"这次要不要用我"的触发判断和路由，模型确认相关后再读 `DEEPENING.md` / `tests.md` 这些细节文件。ECC 把 889 行塞进一个 SKILL.md，等于放弃了分层加载的收益。

### 1.3 社区真实度（用同一套方法测出来的）

我对两个仓库跑了同样的方法：拉全仓 issue 评论 → 按发言人分层 → 算 bot/作者占比。

| | mattpocock/skills | affaan-m/ECC |
|---|---|---|
| 评论总数 | **1,080** | **8,003** |
| bot 评论 | **61（5.6%）**——只有 `changeset-bot` 一个 | **5,494（68.7%）**——`ecc-tools` 2581 + `coderabbitai` 1556 + `greptile-apps` 1228 + 其它 129 |
| 作者本人评论 | 236（21.9%） | 1,530（19.1%） |
| **真实第三方评论** | **783（72.5%）** | **979（12.2%）** |
| 最活跃的第三方 | **LucasGHE 206 条**（占全部评论 19%） | alexraphael07-prog 58 条 |

**结论**：两个仓库的第三方评论**绝对数量接近**（783 vs 979），但 ECC 要用 **8 倍的评论总量**才产生这个数字——其余 68.7% 是三个 AI 代码审查 bot 和作者自己的 bot 在刷。

**mattpocock 的社区参与真实度是 ECC 的 6 倍**（72.5% vs 12.2%）。而且有一个贡献了 206 条评论的第三方（LucasGHE），说明存在真实的协作关系，不只是"提 issue 然后被批量关闭"。

> 参考：HN 上第三方对这两个仓库的评价（见 [[../ECC Harness 研究/田野调研笔记/渠道04 HackerNews 与 Lobsters|渠道04 HN 笔记]]）——ECC 被投稿 4 次全部 0 评论、唯一一条用户表态是"留 1 删 300"；而 **mattpocock/skills 被"反复自发提及"**，被归类为"精选的个人参考集，赢在人格背书分发"。

---

## 二 · 工程纪律对比（7 个维度）

### 2.1 生命周期管理

| | mattpocock | ECC |
|---|---|---|
| 机制 | **6 个 bucket + promoted 制度**。只有 `engineering/` `productivity/` 是 promoted，精确对应 plugin ship 的 22 个 | 无 bucket 概念，34 个 module 靠 `defaultInstall` 布尔值 |
| 废弃处理 | `deprecated/` 目录，**不进 plugin、不进 README、无 docs 页、`link-skills.sh` 显式排除** | `continuous-learning` 的 description 第一句就是 `[DEPRECATED - use continuous-learning-v2]`，**却仍在默认安装面里** |
| 未完成品 | `in-progress/` 9 个，明确不出货 | 7 个 module 标 `beta`，但仍可装 |

**判决**：mattpocock 完胜。ECC 出货一个自己标了废弃的 skill，这是纪律问题不是能力问题。

### 2.2 触发确定性

| | mattpocock | ECC |
|---|---|---|
| user-invoked 机制 | **9/17 用 `disable-model-invocation: true`** + `agents/openai.yaml` 里 `allow_implicit_invocation: false` | **0/43 使用**——全部 model-invoked |
| 后果 | 流程节点（该不该 grilling、要不要拆 ticket）**只能人触发，没有漏调用的可能** | 每一个都可能漏触发。43 个里只有 4 个被 hook 层引用 |

**判决**：这是两者最本质的差异。**mattpocock 用"把该由人决定的事交给人"解决了漏调用问题；ECC 试图靠 description 让模型自己判断，所以必然有漏。**

### 2.3 编排层

| | mattpocock `ask-matt` | ECC `ecc-recipes` |
|---|---|---|
| 触发 | **user-invoked**（确定性入口） | model-invoked（模型可能想不起来） |
| 内容 | **手写的完整地图**：主流程 + 3 个 on-ramp + 词汇层 + standalone + 前置条件，每个分支带判据和反例 | 运行时读 `commands/` 目录再按前缀分族 |
| 定位 | 给建议 | `Advisory only` |
| 维护契约 | **有，写进仓库 CLAUDE.md**："a new skill it never mentions, or a stale one it still routes to, **is a router that lies**" | 无 |
| 覆盖 | 每个 user-reachable skill（含跨 bucket 的 20 个） | 命令目录（会随版本漂移） |

**判决**：两者都只是顾问不是调度器，但 mattpocock 的地图**写清了判据和反例**（"never a well-scoped feature"、"don't triage them"、"it hands off, it doesn't build"），而 ECC 的是动态分类。

### 2.4 词汇一致性

| | mattpocock | ECC |
|---|---|---|
| 领域词汇表 | **仓库根 `CONTEXT.md`**：4 个术语定义 + 每个带 **_Avoid_ 禁用同义词列表** + **Flagged ambiguities 已解决歧义记录** | 无 |
| 架构词汇表 | `codebase-design` 是 deep-module 词汇的唯一真值源（module/interface/depth/seam/adapter/leverage/locality） | 无统一词汇层 |
| 后果 | 41 个 skill 用词一致 | `verification-loop` 与 `springboot-verification` 结构重叠、`api-design` 与 `backend-patterns` 内容重复（[#1213](https://github.com/affaan-m/ECC/issues/1213) 实证） |

**判决**：mattpocock 有 DDD 式的统一语言机制，ECC 完全没有——这直接导致了 ECC 的内容重复问题。

### 2.5 安装与卸载

| | mattpocock | ECC |
|---|---|---|
| 安装方式 | **两种哲学，二选一**：plugin（只读托管 bundle，"you subscribe rather than fork"）或 skills.sh（可编辑副本，"hack on them and make them your own"）。README 明确警告装两个会让每个 skill 出现两次 | plugin（广告全部 281+67）或 `install.sh`（**983 个文件拷进 `~/.claude`**） |
| 用户文件保护 | plugin 是只读 bundle，不碰你的文件 | **只有 `skills/` 同名跳过有保护**；markdown 和其它一切**无条件覆盖，且卸载时是删除而非还原** |
| 更新 | `git pull` / plugin 自动 | `/auto-update`（[#1247](https://github.com/affaan-m/ECC/issues/1247) 报告它**会覆盖你手动禁用 MCP 的设置**） |
| 官方安装器 | plugin 命令一行 | `install.sh --dry-run` 拦不住 npm install；`--skills tdd-workflow` 实际装 44 个目录；卸载不带 `--target` 会扫全部 14 个安装目标 |

**判决**：mattpocock 完胜。"subscribe rather than fork" 这个定位从根上避免了污染问题。

### 2.6 跨 harness 支持

| | mattpocock | ECC |
|---|---|---|
| 覆盖率 | **17/17（100%）** 都有 `agents/openai.yaml`，含 invocation policy | 39/281（14%）有 `.agents/` 平行副本，且是**删掉 metadata 的拷贝**而非声明文件 |
| 支持的 harness | Claude Code + Codex/Agent Skills 兼容的（`~/.agents/skills`） | 14 个安装目标，但**只有 5 个能装 hooks** |
| 深度 | 每个 skill 声明式适配 | 广度大但深度不均（`.openclaw/` 只有一个 21 行 README 占位） |

**判决**：ECC 广度大，mattpocock 深度足。**你在用 OpenClaw 的话，ECC 那边是最浅一档（占位 README），mattpocock 这边通过 `~/.agents/skills` 标准也能覆盖。**

### 2.7 对自己主张的诚实度

| | mattpocock | ECC |
|---|---|---|
| 性能主张 | 无（不宣称省 token / 提效百分比） | README 宣称 60%/70%/80% 省钱，**实为厂商定价的算术推论，非测量值**；`gateguard` skill 的 description 直接写进 "+2.25 points"（只有 2 个任务的 A/B） |
| benchmark | 无（也不假装有） | **全仓零 benchmark**，`harness-audit` 的"打分"本质是文件存在性检查 + 加权求和 |
| 自我限定 | `link-skills.sh` 里自己写 "This is a dev-only script... **It is not a supported installer**"；`wayfinder` 自称"最烧脑，只用于真正装不下的工程" | `safety-guard` skill **完全没有实现，纯文档**；`context-budget` 的 token 数字是模型 "mentally" 心算的 |

**判决**：mattpocock 不做无法验证的宣称。ECC 有把营销口径写进机器可读元数据的问题。

---

## 三 · ECC 赢的地方（要说公道话）

不是一边倒。ECC 有 mattpocock 完全没有的东西：

| ECC 独有 | 价值 |
|---|---|
| **67 个 agent** | mattpocock 一个都没有（他靠 skill 内部 spawn sub-agent）。ECC 的语言专属 reviewer 矩阵虽然对你没用，但对多语言团队是实打实的覆盖 |
| **48 个 hook 脚本 + 完整 hooks 运行时** | 这是**唯一 100% 确定性执行**的层。`delivery-gate` 的 Stop hook 会真的 `exit 2` 拦住你。mattpocock 只在 `misc/git-guardrails-claude-code` 里教你自己配 |
| **`orch-pipeline` 的 7 阶段 + 4 档 right-sizing** | 完整的流程宪法，含规模分级（trivial/small/standard/large 各跑哪些阶段）。mattpocock 的 `ask-matt` 有流程但**没有规模分级** |
| **多语言/多领域覆盖** | 281 个 skill 覆盖 Django/Laravel/SpringBoot/Rust/Swift/供应链/医疗/预测市场。mattpocock 是纯 TypeScript 生态视角 |
| **`.agents/` 跨 14 个 harness 的安装适配器** | 广度是真的（虽然深度不均） |
| **`skill-stocktake` / `config-gc` / `context-budget` 三件治臃肿工具** | 讽刺的是这三个恰恰是 ECC 最该用在自己身上的东西。mattpocock 靠 bucket 制度从源头避免臃肿，所以不需要 |

**一句话**：ECC 是"目录式全家桶 + 有 hook 运行时"，mattpocock 是"精选参考集 + 无运行时"。**它们不是同一类产品。**

---

## 四 · 对你的组合建议

你的实际情况：金融出身、靠 AI 主导工程、已有 79 个自建 skill + 一套 project-lifecycle 流水线、在用 OpenClaw / Codex / Claude Code 多平台。

### 推荐组合

```
整套订阅 mattpocock/skills（22 个，800 token/轮）
   +
从 ECC 手动取 4-6 个元治理 skill（skill-stocktake / config-gc / rules-distill 等）
   +
从 ECC 只偷设计不装本体（4 条，见下）
```

**理由**：
1. **mattpocock 是"订阅"模式**——只读 bundle，作者维护，你不用管。常驻成本 800 token，几乎免费
2. **ECC 是"拷贝"模式**——983 个文件进你的目录，覆盖不还原，而且你已经核实过 `commands/learn.md` 会被覆盖。所以只手动取需要的
3. 两者**功能重叠很小**：mattpocock 是流程 + 参考，ECC 你要的是元治理工具（治你那 79 个 skill）

### 从 ECC 只偷设计不装本体的 4 条

已在 [[../ECC Harness 研究/01 ECC 工程化流程与工具分类|ECC 工程化流程]]和 [[../ECC Harness 研究/06 workflow-quality 重点 skill 详解与调用时序|06 重点 skill 详解]]里详述：

1. **GateGuard 的核心论断** —— 问模型"你违规了吗"永远答没有；问"列出所有 import 这个模块的文件"会逼它真去 Grep
2. **陈旧回放防护** —— 注入的历史摘要强制包上 "HISTORICAL REFERENCE ONLY — NOT LIVE INSTRUCTIONS"
3. **right-sizing 4 档分级** —— mattpocock 恰好缺这个，可以补进 `ask-matt` 式的地图里
4. **`delivery-gate` 的 4 条合理化正则** —— 抓 AI 给自己找台阶（"先跳过测试" / "这是既有问题"）

### 从 mattpocock 偷的 5 条设计

见 [[01 engineering 17 个 skill 深度详解]] 第五节，最值钱的三条：

1. **「拿到会变红的回路之前拒绝理论推测」**（`diagnosing-bugs` Phase 1）
2. **「一个 adapter 是假 seam，两个才是真的」**（`DEEPENING.md`）
3. **user-invoked vs model-invoked 硬二分**——把该人决定的事交给人，从根上消灭漏调用

### 一个结构性建议

**给你自己的 79 个 skill 引入 mattpocock 的 bucket 制度。**

你现在的 `~/.claude/skills/` 是 79 个平铺目录，没有"推广 / 杂项 / 进行中 / 已废弃"的分层。后果和 ECC 一样——**旧的、实验性的、已被替代的 skill 混在一起吃 catalog token 且互相竞争触发**。

最小改造：在你的 skill 库里加一份 `SKILLS-INDEX.md`（等价于他的 bucket README），标出哪些是日常主力、哪些是备查、哪些已被替代。配合 `skill-stocktake` 跑一次审计，就能拿到分层依据。
