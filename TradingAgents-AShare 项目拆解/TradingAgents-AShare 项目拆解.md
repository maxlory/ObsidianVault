---
tags:
  - TradingAgents
  - AI-Agent
  - LangGraph
  - 项目拆解
  - A股投研
  - MOC
项目: TradingAgents-AShare
仓库: https://github.com/KylinMountain/TradingAgents-AShare
建立日期: 2026-07-30
---

A 股多智能体投研系统的完整拆解资料库。上游是 [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents)（94.9k★, Apache-2.0），这个 fork 加了 Web 前后端和 A 股数据源。我打算以它为底座做二次开发。

## ⚠️ 先读这个

[[⚠️ 勘误表-zread讲解vs源码实测]] —— zread 的讲解有 **4 处与源码实际不符**，其中 2 处会直接影响改造方案（"工具调用循环"根本不存在、BM25 中文检索实际失效）。**不先读这个，会照着错误的架构理解去设计改造。**

两类材料的可信度不同：

| 材料 | 性质 | 怎么用 |
|---|---|---|
| [[五问深拆报告-源码实测]] | 我的源码实测，结论带文件行号 | **高置信**，冲突时以此为准 |
| `zread讲解/` 18 篇 | AI 生成的讲解 | 导航地图——查"东西在哪、长什么样"极快；凡"在实践中…""设计意图是…"一律回源码核对 |

## 按目的读（不用通读 18 篇）

**想先建立整体认知** → [[01-概述]] → [[03-架构概述]] → [[04-LangGraph 状态机]]

**要改数据源**（接自有数据 / 换 akshare / 加港美股）
→ [[11-提供者注册表模式]]、[[10-数据收集器与缓存]]
🔑 关键坑：fallback 链会**自动追加你没配置的 provider**，A 股可能静默降级到 yfinance 拿到垃圾数据且无溯源标记。见 [[五问深拆报告-源码实测]] 方向①。

**要改 Agent 团队结构**（加减 Agent / 改辩论轮次 / 换裁决）
→ [[06-Agent 状态模式]]、[[07-分析师团队-7个维度]]、[[05-条件路由与辩论循环]]、[[08-主张驱动的多空辩论]]、[[09-三方风险辩论与修正]]
🔑 关键坑：`setup.py` 里 7 组 ToolNode + 条件边**是不可达死代码**，加 Agent 时那 2 处是"不写会崩、写了不执行"的仪式代码。**读这几篇前先看勘误表冲突 1、2**，否则会照着不存在的 ReAct 循环去设计。

**要加 LLM / 成本 / 输出约束**
→ [[13-多提供者 LLM 工厂]]、[[14-深度快速思考与提供者配置]]、[[17-配置参考]]
🔑 关键事实：一次分析约 18 次 LLM 调用（16 quick + 2 deep），固定可数。`trading_graph.py#L89` 已有 callbacks 透传通道，是加成本统计最干净的切入点。约束机制已存在但**全是 prompt 级软约束，零代码校验**。

**想知道"从历史决策学习"能不能用** → [[18-记忆与反思系统]] + 勘误表冲突 3、4
🔑 结论：当前 Web 部署下这条链路一次也没跑过（`reflect_and_remember` 零调用 + memory 无持久化）。

## 全部章节

| # | 章节 | 分组 |
|---|---|---|
| 01 | [[01-概述]] | Get Started |
| 02 | [[02-快速开始]] | Get Started |
| 03 | [[03-架构概述]] | Deep Dive |
| 04 | [[04-LangGraph 状态机]] | Deep Dive |
| 05 | [[05-条件路由与辩论循环]] | Deep Dive |
| 06 | [[06-Agent 状态模式]] | Deep Dive |
| 07 | [[07-分析师团队-7个维度]] | Deep Dive |
| 08 | [[08-主张驱动的多空辩论]] | Deep Dive |
| 09 | [[09-三方风险辩论与修正]] | Deep Dive |
| 10 | [[10-数据收集器与缓存]] | Deep Dive |
| 11 | [[11-提供者注册表模式]] | Deep Dive |
| 12 | [[12-自然语言意图解析器]] | Deep Dive |
| 13 | [[13-多提供者 LLM 工厂]] | Deep Dive |
| 14 | [[14-深度快速思考与提供者配置]] | Deep Dive |
| 15 | [[15-REST API 与身份验证]] | Deep Dive |
| 16 | [[16-流式辩论可视化]] | Deep Dive |
| 17 | [[17-配置参考]] | Deep Dive |
| 18 | [[18-记忆与反思系统]] | Deep Dive |

## ⚠️ 商用红线

license 是**双许可**：从上游继承的核心逻辑是 Apache-2.0，但**新增的 `api/`、`frontend/` 和改造过的 `tradingagents/` 是 PolyForm Noncommercial 1.0.0**。个人自用无碍；一旦要进公司产品或对外服务，这部分一行都不能逐字用。详见 [[五问深拆报告-源码实测]] 开头的 license 节。

## 素材位置

- 本 vault：分章讲解 + 勘误表 + 拆解报告（可搜索、可双链）
- `~/dev-learning-path/zread-wiki/TradingAgents-AShare/FULL.md`：300 KB 全 18 章合并单文件，**整份喂给 AI 提问**用（没导入 vault，Obsidian 开这么大文件会卡）
- `~/dev-learning-path/拆解报告/2026-07-29-TradingAgents-AShare.md`：拆解报告原件
- 每篇讲解内的源码链接都已改写成可直接点开的 GitHub `blob/main` 链接（讲解 397 个 + 拆解报告 31 个 = 428 个），点开就能回源码核对
