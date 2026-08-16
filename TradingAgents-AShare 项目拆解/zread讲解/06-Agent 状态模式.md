---
tags:
  - TradingAgents
  - AI-Agent
  - LangGraph
  - 项目拆解
  - A股投研
项目: TradingAgents-AShare
章节序号: 6
原始slug: 6-agent-state-schema
来源: https://zread.ai/KylinMountain/TradingAgents-AShare/6-agent-state-schema
抓取日期: 2026-07-30
类型: AI生成讲解-需核对
---

> 来源：[zread.ai 中文 wiki](https://zread.ai/KylinMountain/TradingAgents-AShare/6-agent-state-schema)（抓取于 2026-07-30）｜章节分组：Deep Dive
> ⚠️ 本文是 AI 生成的讲解，**是导航地图不是事实来源**——关键结论请点文内 GitHub 链接回源码核对。

---

**Agent State Schema** 是 TradingAgents-AShare 框架的中枢神经系统——它是一个单一且强类型的数据结构，在 LangGraph 状态机执行期间，每个图节点都会对其进行读写。理解该 Schema 是理解信息如何在 Agent 之间流动、条件路由决策如何制定，以及系统如何在多轮辩论中积累证据的先决条件。本页将剖析构成该 Schema 的每个 TypedDict，解释实现并行分析师追踪的累加器模式，并映射状态从初始化到最终信号提取的整个生命周期。

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L1-L185)

## Schema 架构：分层的 TypedDict 层次结构

该 Schema 被组织为一个**根状态类型** (`AgentState`)，它组合了多个**嵌套子状态类型**，每个子状态控制着 Agent 工作流的一个独立阶段。这种分层设计意味着任何图节点都可以访问完整状态，但仅修改与其职责相关的字段——分析师编写报告，研究员编写辩论历史，风险评判官编写反馈约束。

下图展示了组合层次结构。每个箭头代表“包含”关系——`AgentState` 拥有所有嵌套状态，而每个嵌套状态封装了特定工作流阶段的字段。

> **[原图为 mermaid 图；下列为从渲染 SVG 提取的节点/边标签（近拓扑序），非原始 mermaid 源码]**
>
> AgentState根状态 — 扩展自 MessagesState → InstrumentContext标的代码, 交易所, 货币 → MarketContext交易时段, 时区, 分析模式 → UserContext持仓, 风险偏好, 约束 → WorkflowContext请求来源, 分析师名册 → UserIntent解析后的自然语言查询结构 → InvestDebateState多空辩论状态 → RiskDebateState三方风险辩论状态 → RiskFeedbackState交易员修订反馈 → 分析师报告7 份报告字符串 → analyst_traces累加器: operator.add → 周期结果short_term_result / medium_term_result

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L147-L185)

## 上下文子状态：描述分析前的世界

在任何 Agent 运行之前，系统必须明确**正在分析什么标的、当前处于什么交易时段，以及用户具有哪些约束条件**。四个 TypedDict 对这一上下文基础事实进行了编码。它们在状态初始化期间被填充一次，随后被下游节点视为只读。

### InstrumentContext

标识正在分析的目标证券。`infer_instrument_context()` 函数将原始代码标准化为该结构——例如，`"600519"` 会被转换为 `{symbol: "600519", market_country: "CN", exchange: "SH", currency: "CNY", asset_type: "equity"}`。

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `symbol` | `str` | 标准化标的代码（如 `"600519"`, `"AAPL"`） |
| `security_name` | `str` | 显示名称；若无可用名称则回退到标的代码 |
| `market_country` | `str` | 市场国家代码：`"CN"`, `"US"` 或 `"UNKNOWN"` |
| `exchange` | `str` | 交易所代码（如 `"SH"`, `"SZ"`, `"US"`） |
| `currency` | `str` | 交易货币：`"CNY"`, `"USD"` 或 `"UNKNOWN"` |
| `asset_type` | `str` | 资产分类；目前始终为 `"equity"` |

### MarketContext

编码分析的**时间上下文**——哪个交易日、哪个交易时段，以及适合哪种分析模式。这一点至关重要，因为同一只股票在上午 9:15（盘前）和下午 3:05（盘后）可能需要不同的分析框架。`build_market_context()` 函数使用交易日历来推断交易时段和分析模式。

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `trade_date` | `str` | 请求的交易日（如 `"2025-01-15"`） |
| `timezone` | `str` | 市场时区（如 `"Asia/Shanghai"`） |
| `market_country` | `str` | 市场国家，从 InstrumentContext 传递而来 |
| `exchange` | `str` | 交易所代码，从 InstrumentContext 传递而来 |
| `market_session` | `str` | 当前时段：`"pre_market"`, `"morning"`, `"lunch_break"`, `"afternoon"`, `"closed"` |
| `market_is_open` | `bool` | 市场当前是否开放 |
| `analysis_mode` | `str` | 分析框架：`"pre_market"`, `"intraday"`, `"post_market"`, `"t_plus_1"` |
| `data_as_of` | `str` | 视为确认数据的最新日期 |
| `session_note` | `str` | 关于时段推断的人类可读解释 |

### UserContext

捕获**用户的个人交易情况**——他们的持仓、风险承受能力和硬性约束。这是最微妙的上下文，因为它部分是结构化的（如 `cash_available` 等数字字段），部分是自由格式的（`constraints` 列表和 `user_notes`）。`normalize_user_context()` 函数负责处理中文语境输入的转换：`"5万"` 变为 `50000.0`，`"半仓"` 变为 `current_position_pct` 的 `50.0`。

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `objective` | `str` | 期望操作：建仓, 加仓, 减仓, 止损, 持有处理, 观察 |
| `risk_profile` | `str` | 风险偏好：保守, 平衡, 激进 |
| `investment_horizon` | `str` | 预期持有期：短线, 波段, 中线, 长期 |
| `cash_available` | `float` | 账户可用现金 |
| `current_position` | `float` | 当前持仓股数 |
| `current_position_pct` | `float` | 当前持仓百分比 |
| `average_cost` | `float` | 每股平均持仓成本 |
| `max_loss_pct` | `float` | 最大可容忍亏损百分比 |
| `constraints` | `list[str]` | 硬性交易约束（如 `["不加杠杆", "不追高"]`） |
| `user_notes` | `str` | 自由格式的附加备注 |

### WorkflowContext

关于**当前执行运行**的元数据——谁请求了它、哪些分析师处于活跃状态，以及正在使用的 Schema 版本。

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `context_version` | `str` | Schema 版本，目前为 `"v1"` |
| `request_source` | `str` | 请求来源：`"api"`, `"chat"` 或 `"scheduled"` |
| `selected_analysts` | `list[str]` | 本次运行中活跃的分析师名册 |

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L48-L86), [context_utils.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/context_utils.py#L33-L88), [context_utils.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/context_utils.py#L91-L127)

## UserIntent：连接自然语言与结构化状态

当用户提交自由文本查询（如 `"分析600519短线，满仓，不加杠杆"`）时，`IntentParser`（由 LLM 提供支持）会提取出结构化的 `UserIntent` 字典。这是非结构化的人类输入与驱动 Agent 行为的类型化状态之间的桥梁。

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `raw_query` | `str` | 原始用户查询字符串 |
| `ticker` | `str` | 提取的标的代码 |
| `horizons` | `list[str]` | 请求的分析周期：`["short"]`, `["medium"]` 或两者 |
| `focus_areas` | `list[str]` | 需要强调的具体分析维度 |
| `specific_questions` | `list[str]` | 用户希望回答的具体问题 |
| `user_context` | `UserContext` | 从 LLM 解析和正则表达式回复合并推断出的用户上下文 |

意图解析器采用**两层提取策略**：首先使用结构化的系统提示词调用 LLM，然后将 LLM 的结果与基于正则表达式的回退机制（`_extract_user_context_fallback`）合并，即使 LLM 输出格式异常，该回退机制也能捕获常见的中文交易短语。合并后的结果始终通过 `normalize_user_context()` 进行标准化。

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L30-L36), [intent_parser.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/intent_parser.py#L20-L65)

## TraceItem 与累加器模式

一个微妙但在架构上至关重要的设计选择：`AgentState` 上的 `analyst_traces` 字段使用了 `Annotated[List[TraceItem], operator.add]`。这是 LangGraph 的 **reducer 注解**——这意味着当多个节点返回包含 `analyst_traces` 的部分状态更新时，这些值将通过**列表拼接进行合并**，而不是被覆盖。

这促成了以下模式：每个分析师节点仅返回自己的追踪项（例如 `{"analyst_traces": [{"agent": "market_analyst", "verdict": "看空", ...}]}`），LangGraph 会在图执行期间自动将所有追踪累加到一个列表中。如果没有 `operator.add`，每个分析师都会覆盖前一个分析师的追踪。

### TraceItem 字段

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `agent` | `str` | 分析师标识符（如 `"market_analyst"`） |
| `horizon` | `str` | 分析周期：`"short"` 或 `"medium"` |
| `data_window` | `str` | 使用的数据窗口（如 `"14天"`） |
| `key_finding` | `str` | 该分析师的主要发现 |
| `verdict` | `str` | 方向性裁决：看多, 看空, 中性 |
| `confidence` | `str` | 置信度：高, 中, 低 |

`extract_verdict()` 辅助函数解析嵌入在分析师输出中的结构化 `<!-- VERDICT: {...} -->` HTML 注释块，在提供人类可读报告的同时提供机器可读的裁决。

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L39-L46), [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L181-L181), [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L18-L27)

## 辩论子状态：InvestDebateState 和 RiskDebateState

两个辩论阶段——多空投资辩论和激进/保守/中立风险辩论——各自维护着自己独立的子状态。这些子状态追踪对话历史、主张演进和轮次目标，从而实现带有结构化证据追踪的多轮审议。

### InvestDebateState

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `bull_history` | `str` | 多方论点的运行历史 |
| `bear_history` | `str` | 空方论点的运行历史 |
| `history` | `str` | 合并的对话历史 |
| `current_speaker` | `str` | 上一位发言者的标签 |
| `current_response` | `str` | 最新的回复文本 |
| `bull_initial` | `str` | 多方的开场陈词（用于并行反驳） |
| `bear_initial` | `str` | 空方的开场陈词（用于并行反驳） |
| `bull_rebuttal` | `str` | 多方对空方开场的反驳 |
| `bear_rebuttal` | `str` | 空方对多方开场的反驳 |
| `judge_decision` | `str` | 研究经理的最终裁决 |
| `count` | `int` | 交换的总消息数（驱动轮次计数） |
| `claims` | `list[dict]` | 所有追踪中带有状态的研究主张 |
| `focus_claim_ids` | `list[str]` | 下一轮必须回应的主张 |
| `open_claim_ids` | `list[str]` | 尚未回应的主张 |
| `resolved_claim_ids` | `list[str]` | 视为已解决的主张 |
| `unresolved_claim_ids` | `list[str]` | 仍然存在实质性争议的主张 |
| `round_summary` | `str` | 最新辩论轮次的总结 |
| `round_goal` | `str` | 当前轮次的目标 |
| `claim_counter` | `int` | 用于生成唯一主张 ID 的单调计数器 |

### RiskDebateState

与 `InvestDebateState` 类似，但有三位发言者而非两位：

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `aggressive_history` | `str` | 激进辩手的论点历史 |
| `conservative_history` | `str` | 保守辩手的论点历史 |
| `neutral_history` | `str` | 中立辩手的论点历史 |
| `history` | `str` | 合并的对话历史 |
| `latest_speaker` | `str` | 最后发言的分析师标签 |
| `current_aggressive_response` | `str` | 激进分析师的最新回复 |
| `current_conservative_response` | `str` | 保守分析师的最新回复 |
| `current_neutral_response` | `str` | 中立分析师的最新回复 |
| `judge_decision` | `str` | 风险经理的最终裁决 |
| `count` | `int` | 总消息数（轮次控制：`count >= 3 * max_risk_discuss_rounds`） |
| `claims` | `list[dict]` | 特定于风险的追踪主张 |
| `focus_claim_ids` / `open_claim_ids` / `resolved_claim_ids` / `unresolved_claim_ids` | `list[str]` | 主张生命周期追踪（与 InvestDebateState 相同） |
| `round_summary` / `round_goal` / `claim_counter` | `str` / `str` / `int` | 轮次管理字段 |

`count` 字段是**轮次控制机制**。对于 InvestDebateState，当 `count >= 2 * max_debate_rounds`（2 位发言者 × 轮次）时辩论终止。对于 RiskDebateState，当 `count >= 3 * max_risk_discuss_rounds`（3 位发言者 × 轮次）时终止。`ConditionalLogic` 类直接从状态中读取这些值以做出路由决策。

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L88-L132), [conditional_logic.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/conditional_logic.py#L71-L92)

## RiskFeedbackState：交易员修订循环

在风险评判官做出裁决后，系统必须决定是接受交易员的计划、拒绝它，还是将其退回修订。`RiskFeedbackState` 对该决策及其约束进行编码，从而实现一个**受控的修订循环**，在该循环中可以要求交易员最多修订 `max_retries` 次。

| 字段 | 类型 | 描述 |
| --- | --- | --- |
| `retry_count` | `int` | 交易员被退回的次数 |
| `max_retries` | `int` | 允许的最大修订次数（默认：1） |
| `revision_required` | `bool` | 交易员是否必须修订 |
| `latest_risk_verdict` | `str` | 裁决：`"pass"`, `"revise"` 或 `"reject"` |
| `hard_constraints` | `list[str]` | 来自风险评判官的不可协商约束 |
| `soft_constraints` | `list[str]` | 建议性（可协商）约束 |
| `execution_preconditions` | `list[str]` | 执行前必须满足的条件 |
| `de_risk_triggers` | `list[str]` | 需要立即降低风险的触发器 |
| `revision_reason` | `str` | 计划被拒绝的原因说明 |

`should_revise_after_risk_judge()` 条件会读取 `revision_required` 并比较 `retry_count <= max_retries`，以决定是路由回交易员节点还是在 `END` 处终止图。这形成了一个有界的反馈循环：

> **[原图为 mermaid 图；下列为从渲染 SVG 提取的节点/边标签（近拓扑序），非原始 mermaid 源码]**
>
> RFS → Yes → No → Risk Judge写入 RiskFeedbackState → revision_requiredANDretry_count ≤ max_retries? → Trader修订计划 → END

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L135-L144), [conditional_logic.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/conditional_logic.py#L94-L102)

## AgentState：根状态类型

`AgentState` 扩展了 LangGraph 的 `MessagesState`（它提供了带有自身 reducer 的 `messages` 字段），并将所有子状态、报告字段和结果累加器组合到一个 TypedDict 中。`StateGraph(AgentState)` 中的每个节点都会接收此完整状态，并返回一个由 LangGraph 合并回去的部分字典。

| 分类 | 字段 | 写入模式 |
| --- | --- | --- |
| **身份** | `company_of_interest`, `trade_date`, `sender`, `horizon` | 初始化时设置一次；`sender` 按节点更新 |
| **上下文** | `instrument_context`, `market_context`, `user_context`, `workflow_context`, `user_intent` | 初始化时设置一次（此后只读） |
| **分析师报告** | `market_report`, `sentiment_report`, `news_report`, `fundamentals_report`, `macro_report`, `smart_money_report`, `volume_price_report` | 每位分析师精确写入一个报告字符串 |
| **辩论** | `investment_debate_state`, `investment_plan` | 研究员和研究经理增量修改 |
| **交易** | `trader_investment_plan`, `final_trade_decision` | 交易员和风险评判官分别写入 |
| **风险** | `risk_debate_state`, `risk_feedback_state` | 风险辩手和风险评判官增量修改 |
| **累加器** | `analyst_traces` (`operator.add`) | 每位分析师追加一个 TraceItem |
| **周期结果** | `short_term_result`, `medium_term_result` | 图完成后设置 |
| **元数据** | `metadata` | 任意运行时元数据 |

`analyst_traces` 上的 `operator.add` 注解是默认 `MessagesState` reducer 之外的**唯一 reducer**。所有其他字段均使用 LangGraph 默认的“最后一次写入获胜”语义——这意味着如果两个节点都返回 `{"market_report": "..."}`，则第二个会覆盖第一个。这是安全的，因为每位分析师都写入一个独特的报告字段。

来源: [agent_states.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/agents/utils/agent_states.py#L147-L185)

## 状态初始化与生命周期

`Propagator.create_initial_state()` 方法在图开始执行之前构造完整的初始 `AgentState` 字典。每个字段都会接收一个显式的默认值——报告为空字符串，主张为空列表，计数器为零——确保没有任何节点会遇到 `KeyError`。

初始化序列遵循以下顺序：

1. **推断** `InstrumentContext`：通过 `infer_instrument_context()` 从标的代码推断
2. **构建** `MarketContext`：通过 `build_market_context()` 从标的代码和交易日构建
3. **标准化** `UserContext`：通过 `normalize_user_context()` 从可选的用户输入标准化
4. **总结** 将上述三个上下文总结为人类可读的字符串，成为对话中的第一条消息
5. **初始化** `InvestDebateState`：使用空历史、零计数、空主张和 `default_round_goal("investment", 1)` 初始化
6. **初始化** `RiskDebateState`：通过 `build_empty_risk_debate_state()` 初始化
7. **初始化** `RiskFeedbackState`：以 `retry_count=0`, `max_retries=1`, `revision_required=False` 初始化
8. **设置** 所有报告字段为 `""`，追踪为 `[]`，结果字段为 `None`

图完成后，`SignalProcessor` 使用基于规则的关键词提取器从 `final_trade_decision` 中提取最终的买入/卖出/持有决策，如果未找到结构化裁决，则回退到 LLM 调用。随后，`Reflector` 消耗已完成的状态以更新每个 Agent 的长期记忆。

来源: [propagation.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/propagation.py#L30-L111), [signal_processing.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/signal_processing.py#L18-L46), [reflection.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/reflection.py#L45-L93)

## 接下来去哪

现在你已经了解了状态是如何构建和传播的，自然的进展是探索**条件路由如何读取该状态**以引导图流程，以及**每个 Agent 如何读写其状态切片**：

- [条件路由与辩论循环 ](https://zread.ai/KylinMountain/TradingAgents-AShare/5-conditional-routing-and-debate-loops) — `ConditionalLogic` 如何读取 `InvestDebateState.count` 和 `RiskFeedbackState.revision_required` 来路由图
- [主张驱动的多空辩论 ](https://zread.ai/KylinMountain/TradingAgents-AShare/8-claim-driven-bull-bear-debate) — `InvestDebateState.claims` 和 `focus_claim_ids` 如何驱动结构化审议
- [三方风险辩论与修订 ](https://zread.ai/KylinMountain/TradingAgents-AShare/9-three-way-risk-debate-and-revision) — `RiskDebateState` 和 `RiskFeedbackState` 如何实现有界修订循环
- [记忆与反思系统 ](https://zread.ai/KylinMountain/TradingAgents-AShare/18-memory-and-reflection-system) — `Reflector` 如何消耗已完成的状态以更新 Agent 记忆
