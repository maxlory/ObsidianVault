---
tags:
  - TradingAgents
  - AI-Agent
  - LangGraph
  - 项目拆解
  - A股投研
项目: TradingAgents-AShare
章节序号: 4
原始slug: 4-langgraph-state-machine
来源: https://zread.ai/KylinMountain/TradingAgents-AShare/4-langgraph-state-machine
抓取日期: 2026-07-30
类型: AI生成讲解-需核对
---

> 来源：[zread.ai 中文 wiki](https://zread.ai/KylinMountain/TradingAgents-AShare/4-langgraph-state-machine)（抓取于 2026-07-30）｜章节分组：Deep Dive
> ⚠️ 本文是 AI 生成的讲解，**是导航地图不是事实来源**——关键结论请点文内 GitHub 链接回源码核对。

---

TradingAgents-AShare 中的交易分析引擎由 **LangGraph `StateGraph`** 编排——这是一个有向图，其中每个节点都是一个 Agent 函数，每条边（包括条件边）都编码了路由逻辑，驱动多 Agent 工作流从数据收集、辩论直至得出最终交易决策。本页将解释该图是如何*构建*、*连线*和*执行*的，从而为后续关于条件路由、Agent 状态和各个 Agent 的页面奠定结构基础。

## 三阶段流水线概览

每次调用该图都会遵循相同的高层进程，经历三个逻辑上截然不同的阶段：

| 阶段 | 节点 | 模式 | 目的 |
| --- | --- | --- | --- |
| **1 — 并行分析** | 7 个分析师节点 + 工具节点 | 扇出 / 扇入 | 每位分析师独立收集特定领域数据并生成报告 |
| **2 — 投资辩论** | 看多研究员 ↔ 看空研究员 → 研究经理 → 交易员 | 交替循环 + 仲裁 | 结构化的多空辩论凝聚为投资计划，随后交易员将其转化为具体的交易 |
| **3 — 风险评估** | 激进分析师 ↔ 保守分析师 ↔ 中立分析师 → 风险裁判 → (交易员修订?) | 轮换三方循环 + 修订门 | 多视角风险审查，带有返回交易员的修订循环 |

下方的 Mermaid 图展示了完整的节点拓扑和所有边类型——无条件边（实线）、条件边（虚线）以及修订回边（点线）：

> **[原图为 mermaid 图；下列为从渲染 SVG 提取的节点/边标签（近拓扑序），非原始 mermaid 源码]**
>
> 阶段 3: 风险评估 → 阶段 2: 投资辩论 → 阶段 1: 并行分析 → tool_calls() → done() → rounds < max() → rounds ≥ max() → revision required() → pass() → START → 市场分析师 → 社交分析师 → 新闻分析师 → 基本面分析师 → 宏观分析师 → 聪明钱分析师 → 量价分析师 → tools_market → 市场分析师完成 → tools_social → 社交分析师完成 → tools_news → 新闻分析师完成 → tools_fundamentals → 基本面分析师完成 → tools_macro → 宏观分析师完成 → tools_smart_money → 聪明钱分析师完成 → tools_volume_price → 量价分析师完成 → 看多研究员 → 看空研究员 → 研究经理 → 交易员 → 激进分析师 → 保守分析师 → 中立分析师 → 风险裁判 → END

来源: [setup.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/setup.py#L199-L282), [trading_graph.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/trading_graph.py#L1-L130)

## 图构建：GraphSetup 构建器

该图并非声明式定义的——它由 `GraphSetup.setup_graph()` **程序化组装**而成。该类接收所有预配置的 LLM 客户端、工具节点、内存对象以及一个 `ConditionalLogic` 实例，然后逐节点、逐边地构建 `StateGraph`，最终将其编译为可执行对象。

### 延迟工厂加载

Agent 节点函数通过*工厂函数*（例如 `create_market_analyst`、`create_bull_researcher`）创建。这些工厂通过 `_load_agent_factories()` 进行延迟加载，以防止循环导入——分析师模块本身会导入 `intent_parser`，如果在包初始化期间急于导入它们，将破坏 API 请求和定时任务的模块依赖图。

```python
factories = _load_agent_factories()
analyst_nodes["market"] = factories["create_market_analyst"](
    self.quick_thinking_llm, self.data_collector
)
```

每个工厂返回一个符合 LangGraph 节点签名 `(state: AgentState) -> partial_state_update` 的**异步可调用对象**。该节点函数从状态中读取所需内容，执行分析，并返回一个被合并回状态的字典。

来源: [setup.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/setup.py#L8-L50), [setup.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/setup.py#L103-L142)

### 节点注册

每个选定的分析师在图中生成**三个节点**：分析师节点本身、一个配套的 `ToolNode`，以及一个“完成”哨兵节点。分析师节点和工具节点构成一个微型循环（调用 → 工具调用 → 调用 → 完成），而完成节点则作为扇入的同步点：

| 节点类型 | 命名约定 | 示例 |
| --- | --- | --- |
| 分析师 | `{显示名称} Analyst` | `Market Analyst` |
| 工具节点 | `tools_{analyst_type}` | `tools_market` |
| 完成哨兵 | `{显示名称} Analyst Done` | `Market Analyst Done` |
| 研究员/经理 | 直接命名 | `Bull Researcher`, `Research Manager` |
| 风险辩论者 | 直接命名 | `Aggressive Analyst`, `Risk Judge` |

显示名称是通过将 snake_case 的分析师类型键转换为 title case 派生而来的（例如，`smart_money` → `Smart Money`）。

来源: [setup.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/setup.py#L143-L200)

### 边连线

边连线遵循三阶段的拓扑结构。关键模式如下：

**从 START 扇出**：所有选定的分析师都接收一条来自 `START` 的边，使 LangGraph 能够并行执行它们：

```python
for analyst_type in selected_analysts:
    workflow.add_edge(START, f"{analyst_display_name(analyst_type)} Analyst")
```

**分析师工具调用循环**：每位分析师使用条件边来决定是调用其工具节点还是继续进入其完成节点：

```python
workflow.add_conditional_edges(
    "Market Analyst",
    self.conditional_logic.should_continue_market,
    {"continue": "tools_market", "done": "Market Analyst Done"},
)
workflow.add_edge("tools_market", "Market Analyst")  # 从工具返回
```

**扇入至辩论**：所有完成节点通过多源边汇聚到看多研究员：

```python
workflow.add_edge(
    [f"{analyst_display_name(at)} Analyst Done" for at in selected_analysts],
    "Bull Researcher",
)
```

**辩论交替**：看多和看空研究员相互路由（或在辩论轮次耗尽时路由至研究经理）：

```python
workflow.add_conditional_edges(
    "Bull Researcher",
    self.conditional_logic.should_continue_debate,
    {"Bear Researcher": "Bear Researcher", "Research Manager": "Research Manager"},
)
```

**风险修订回边**：风险裁判评估后，可以路由至 `END`，或将交易员送回进行修订——这是图中唯一的一条反向边：

```python
workflow.add_conditional_edges(
    "Risk Judge",
    self.conditional_logic.should_revise_after_risk_judge,
    {"Trader": "Trader", "END": END},
)
```

来源: [setup.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/setup.py#L202-L282)

## 编排器：TradingAgentsGraph

`TradingAgentsGraph` 是拥有图生命周期的顶层外观类。它协调六个内部组件，每个组件各司其职：

| 组件 | 类 | 职责 |
| --- | --- | --- |
| 图构建器 | `GraphSetup` | 构建并编译 `StateGraph` |
| 状态初始化器 | `Propagator` | 根据输入参数创建 `AgentState` |
| 路由逻辑 | `ConditionalLogic` | 读取状态并返回边标签的纯函数 |
| 反思引擎 | `Reflector` | 执行完成后的记忆更新 |
| 信号提取器 | `SignalProcessor` | 从最终决策文本中提取买入/卖出/持有 (BUY/SELL/HOLD) 信号 |
| 数据缓存 | `DataCollector` | 预取并缓存数据，向分析师提供滑动窗口视图 |

该编排器还管理**两个 LLM 客户端**——一个用于研究经理和风险裁判（复杂仲裁）的 `deep_thinking_llm`，以及一个用于分析师、研究员和交易员（更快速、更低成本的响应）的 `quick_thinking_llm`。特定于提供商的参数（Google 思考级别、OpenAI 推理力度）在构造时通过 `_get_provider_kwargs()` 解析。

`MemorySaver` 检查点是一个**类级别的单例**——所有 `TradingAgentsGraph` 实例共享同一个内存检查点。这种设计通过确保状态持久化集中管理而非按实例重复，从而支持并发的 API 请求。

来源: [trading_graph.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/trading_graph.py#L31-L130), [trading_graph.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/trading_graph.py#L132-L198)

## 状态生命周期：从初始化到决策

状态流经三个截然不同的阶段——创建、执行和提取：

### 1. 状态创建

`Propagator.create_initial_state()` 根据高层输入（股票代码、交易日期、用户上下文）构建一个完全初始化的 `AgentState` 字典。它会在此时执行上下文推断——派生 `InstrumentContext`（市场、交易所、货币）和 `MarketContext`（交易时段、分析模式）——以便每个下游节点都拥有可用的结构化元数据，而无需重新计算：

```python
instrument_context = infer_instrument_context(company_name)
market_context = build_market_context(company_name, str(trade_date))
normalized_user_context = normalize_user_context(user_context)
```

投资辩论状态和风险辩论状态使用空的历史记录和零计数器进行初始化；风险反馈状态以 `revision_required=False` 和 `retry_count=0` 开始。

### 2. 图执行

根据调用上下文的不同，存在两条执行路径：

| 方法 | 执行模型 | 用例 |
| --- | --- | --- |
| `propagate()` | `graph.invoke()` (同步) 或 `graph.stream()` (调试) | CLI / 评估脚本 |
| `propagate_async()` | `graph.ainvoke()` (异步) | API 服务器 / 流式端点 |

这两种方法都将初始状态和图配置（包括 `recursion_limit`、用于检查点的线程 ID 以及可选的回调）传递给 LangGraph 运行时。随后，运行时根据图拓扑自动管理节点执行顺序，并在分支点调用条件函数。

### 3. 结果提取

执行后，`SignalProcessor.process_signal()` 对 `final_trade_decision` 文本应用分层提取策略：

1. **VERDICT 标签解析** — 提取结构化的 `<!--VERDICT: {...}-->` HTML 注释
2. **显式模式匹配** — 搜索类似 `最终裁决：…` 或 `风控委员会最终裁决：…` 的行
3. **标题分类** — 扫描前 20 行以寻找 BUY/SELL/HOLD 关键词（中英文皆可）
4. **全文分类** — 退而扫描整个文本
5. **LLM 回退** — 如果没有规则匹配，则由 quick_thinking_llm 对信号进行分类

来源: [propagation.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/propagation.py#L28-L100), [trading_graph.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/trading_graph.py#L276-L360), [signal_processing.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/signal_processing.py#L50-L146)

## 条件边函数：路由层

图中的每条条件边都由 `ConditionalLogic` 中的一个**纯函数**提供支持，该函数读取当前的 `AgentState` 并返回一个与边的目标节点之一相匹配的字符串标签。这种分离将路由逻辑排除在图构建代码之外，并使其能够独立测试。

| 函数 | 源节点 | 决策逻辑 | 可能的返回值 |
| --- | --- | --- | --- |
| `should_continue_{type}` | 任意分析师 | 最后一条消息是否有 `tool_calls`？ | `"continue"` → ToolNode, `"done"` → 完成节点 |
| `should_continue_debate` | 看多 / 看空研究员 | `count ≥ 2 × max_debate_rounds` 吗？谁最后发言？ | 对手名称或 `"Research Manager"` |
| `should_continue_risk_analysis` | 激进 / 保守 / 中立分析师 | `count ≥ 3 × max_risk_discuss_rounds` 吗？谁最后发言？ | 轮换中的下一位辩论者或 `"Risk Judge"` |
| `should_revise_after_risk_judge` | 风险裁判 | `revision_required` 为真且 `retry_count ≤ max_retries` 吗？ | `"Trader"` (修订) 或 `"END"` (终止) |

辩论计数器在每位研究员/辩论者发言时递增。对于投资辩论，阈值为 `2 × max_debate_rounds`（每轮包含看多和看空各发言一次）。对于风险辩论，阈值为 `3 × max_risk_discuss_rounds`（每轮包含所有三位辩论者发言）。风险修订门使用 `safe_int()` 来防止状态中出现非整数值。

每种分析师类型都有其专属的 `should_continue_{type}` 函数，而不是共享一个通用函数。这是一个深思熟虑的设计选择——它允许未来每位分析师的工具调用终止逻辑产生差异，而无需重构条件边映射。目前所有七个实现完全相同，但这种架构预见到了未来的专门化。

来源: [conditional_logic.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/conditional_logic.py#L1-L103)

## 可选分析师名册与动态拓扑

该图的一个关键特性是其**拓扑并非固定**——它是根据传递给 `GraphSetup.setup_graph()` 的 `selected_analysts` 参数动态组装的。只有选定分析师的分析师节点、工具节点和完成节点才会被添加到图中。从 START 扇出的边和扇入至看多研究员的边都是在选定列表的循环中生成的。

这意味着同一个 `TradingAgentsGraph` 类可以生成广度不同的图。例如，快速分析可能只选择 `["market", "fundamentals"]`，生成一个 2 分析师的并行阶段；而完整分析则运行全部 7 个分析师：

```python
# 快速分析 — 2 位分析师
graph = TradingAgentsGraph(selected_analysts=["market", "fundamentals"])
 
# 完整分析 — 7 位分析师 (默认)
graph = TradingAgentsGraph(
    selected_analysts=["market", "social", "news", "fundamentals", "macro", "smart_money", "volume_price"]
)
```

空的分析师列表会在图构建时引发 `ValueError`，从而防止创建没有从 START 扇出边的退化图。

来源: [setup.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/setup.py#L96-L101), [trading_graph.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/trading_graph.py#L40-L42)

## 组件交互图

下图展示了在典型的 `propagate_async()` 调用期间，六个图层组件是如何从初始化、图执行到结果提取进行交互的：

> **[原图为 mermaid 图；下列为从渲染 SVG 提取的节点/边标签（近拓扑序），非原始 mermaid 源码]**
>
> SignalProcessor → ConditionalLogic → Compiled StateGraph → Propagator → IntentParser → DataCollector → TradingAgentsGraph → 调用者 → LangGraph 运行时驱动节点执行； → 在每个条件边调用 CL 函数 → propagate_async(ticker, date, query) → parse_intent(query, llm) → UserIntent 字典 → collect(ticker, date) → 缓存已填充 → create_initial_state(ticker, date, user_intent) → AgentState 字典 → get_graph_args() → {stream_mode, config} → ainvoke(initial_state, **args) → final_state → evict(ticker, date) → process_signal(final_state["final_trade_decision"]) → "BUY" | "SELL" | "HOLD" → {short_term: result, user_intent}

来源: [trading_graph.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/trading_graph.py#L296-L360), [propagation.py](https://github.com/KylinMountain/TradingAgents-AShare/blob/main/tradingagents/graph/propagation.py#L1-L127)

## 接下来是什么

既然你已经了解了该图作为状态机是如何构建和执行的，接下来的页面将深入探讨特定的子系统：

- **[条件路由与辩论循环 ](https://zread.ai/KylinMountain/TradingAgents-AShare/5-conditional-routing-and-debate-loops)** — 深入探讨 `ConditionalLogic` 如何管理辩论流、主张跟踪以及修订回边
- **[Agent 状态模式 ](https://zread.ai/KylinMountain/TradingAgents-AShare/6-agent-state-schema)** — `AgentState`、`InvestDebateState`、`RiskDebateState` 及所有上下文子模式的综合参考
- **[分析师团队（7 个维度） ](https://zread.ai/KylinMountain/TradingAgents-AShare/7-analyst-team-7-dimensions)** — 每个分析师工厂如何创建其节点函数并使用 DataCollector
