---
title: Research Radar Week 2026-W23
date: 2026-06-02
tags:
  - weekly-review
  - research-radar
---

# 2026-W23 Research Radar

## 本周趋势

> [!info]+ **今日必须看 / 100** | 阶跃星辰 Step 3.7 Flash 发布，聚焦智能体效率
> **标题**：阶跃星辰 Step 3.7 Flash 发布，聚焦智能体效率
> **原文链接**：🔗 [打开原文](https://x.com/StepFun_ai/status/2060149124117475791)
> **source**：AI HOT / X：阶跃星辰 StepFun (@StepFun_ai)
> **kind**：`model`
> **reason**：matches topics: agent, claude code, mcp; high-value terms: agent, mcp, claude code, eval
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：阶跃星辰（Step）发布了开源大模型 Step 3.7 Flash，主打智能体（Agent）工作流的效率。该模型在 ClawEval-1.1（67.1分）和 SimpleVQA Search（79.2分）评测中排名第一。其架构为 198B 参数的 MoE，约 11B 为活跃参数，支持 256K 上下文。模型具备多模态理解能力，能处理图像、文档并生成代码或调用工具执行任务。在工具使用方面，它致力于高可靠性，τ2-bench 得分超过 98%。Step 3.7 Flash 兼容 Claude Code、MCP 协议等工具链，并支持在 Mac Studio M4 Max 等设备上本地运行。模型权重以 Apache 2.0 许可开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 97** | 免费领取6个月ChatGPT Pro及AI工具思考
> **标题**：免费领取6个月ChatGPT Pro及AI工具思考
> **原文链接**：🔗 [打开原文](https://x.com/AYi_AInotes/status/2060740414273941874)
> **source**：AI HOT / X：阿易 AI Notes (@AYi_AInotes)
> **kind**：`article`
> **reason**：matches topics: agent, claude code, codex, openai; high-value terms: agent, codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OpenAI为开源项目维护者提供福利，可免费领取6个月ChatGPT Pro（价值$1200），申请无硬性Star数要求，有项目链接即可。同时，文章引用讨论了AI工具的分类：一类是"agent型"（如Claude Code、Codex），可自主运行；另一类是"实习生型"（如Cursor），需人工决策，有助于使用者以术入道、培养判断力，但受限于需人在场。作者推荐了网易的UU远程工具，称其免费两年，支持4K 144帧无延迟连接Mac并可使用原生终端，解决了"实习生型"工具的地点限制问题。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 93** | OpenAI产品支持私有MCP服务器安全连接
> **标题**：OpenAI产品支持私有MCP服务器安全连接
> **原文链接**：🔗 [打开原文](https://x.com/OpenAIDevs/status/2059703536825565499)
> **source**：AI HOT / X：OpenAI Developers (@OpenAIDevs)
> **kind**：`product`
> **reason**：matches topics: codex, openai, mcp; high-value terms: mcp, codex, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：您的团队可以在内部网络中保留MCP服务器，同时ChatGPT、Codex和Responses API通过仅出站HTTPS进行连接。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 91** | 用好 Coding Agent，重点是两头，尤其是开头的部分，如果一开始就走偏了后面怎么改都改不好。
> **标题**：用好 Coding Agent，重点是两头，尤其是开头的部分，如果一开始就走偏了后面怎么改都改不好。
> **原文链接**：🔗 [打开原文](https://x.com/dotey/status/2059773942500298934)
> **source**：AI HOT / X：宝玉 (@dotey)
> **kind**：`article`
> **reason**：matches topics: agent, claude code, codex; high-value terms: agent, codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：用好 Coding Agent 的关键在于初始规划。方法是先将需求整理后，用最强模型（如 GPT-5.5、Claude Opus 4.7）分别在 Codex、Claude Code、Cursor 的 Plan 模式下生成设计方案，选择最优方案并借鉴其他版本。对于复杂计划，可将其拆分为多个 Phases 并明确要求与验证标准，形成 Markdown 文档。执行时按 Phases 进行，并辅以人工审核纠偏。最后的代码审核（Code Review）用 GPT-5.5 审核代码质量与设计符合度即可。应避免让多个智能体交叉 Review，否则可能导致代码越改越多。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | wlsdks/oh-my-ontology
> **标题**：wlsdks/oh-my-ontology
> **原文链接**：🔗 [打开原文](https://github.com/wlsdks/oh-my-ontology)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Repo-native memory layer for Claude Code, Cursor, and Codex. Local-first markdown ontology + MCP tools so AI coding agents keep codebase context.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | JKHeadley/instar
> **标题**：JKHeadley/instar
> **原文链接**：🔗 [打开原文](https://github.com/JKHeadley/instar)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, llm; high-value terms: agent, agents, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Persistent Claude Code agents with scheduling, sessions, memory, and Telegram.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | shotkillschwul/agentic-cycle-tools
> **标题**：shotkillschwul/agentic-cycle-tools
> **原文链接**：🔗 [打开原文](https://github.com/shotkillschwul/agentic-cycle-tools)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, codex, mcp; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🚀 Vibe Kanban 2026 Pro – AI Agent Workflow Optimizer for Claude & Codex
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | constripacity/Claude-Bridge
> **标题**：constripacity/Claude-Bridge
> **原文链接**：🔗 [打开原文](https://github.com/constripacity/Claude-Bridge)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, mcp; high-value terms: agent, agents, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Real-time cross-machine MCP relay for Claude Code agents
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | amirfish1/claude-command-center
> **标题**：amirfish1/claude-command-center
> **原文链接**：🔗 [打开原文](https://github.com/amirfish1/claude-command-center)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Local-first dashboard that orchestrates Claude Code, Codex, and Gemini CLI sessions side-by-side — spawn, resume, and review from one browser tab.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | montgome753/LLM-Evaluation-Framework
> **标题**：montgome753/LLM-Evaluation-Framework
> **原文链接**：🔗 [打开原文](https://github.com/montgome753/LLM-Evaluation-Framework)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Benchmark LLM accuracy, latency, cost, and hallucination rates across models with this open-source evaluation suite.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 值得试用的工具 / 模型

> [!info]+ **今日必须看 / 100** | 阶跃星辰 Step 3.7 Flash 发布，聚焦智能体效率
> **标题**：阶跃星辰 Step 3.7 Flash 发布，聚焦智能体效率
> **原文链接**：🔗 [打开原文](https://x.com/StepFun_ai/status/2060149124117475791)
> **source**：AI HOT / X：阶跃星辰 StepFun (@StepFun_ai)
> **kind**：`model`
> **reason**：matches topics: agent, claude code, mcp; high-value terms: agent, mcp, claude code, eval
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：阶跃星辰（Step）发布了开源大模型 Step 3.7 Flash，主打智能体（Agent）工作流的效率。该模型在 ClawEval-1.1（67.1分）和 SimpleVQA Search（79.2分）评测中排名第一。其架构为 198B 参数的 MoE，约 11B 为活跃参数，支持 256K 上下文。模型具备多模态理解能力，能处理图像、文档并生成代码或调用工具执行任务。在工具使用方面，它致力于高可靠性，τ2-bench 得分超过 98%。Step 3.7 Flash 兼容 Claude Code、MCP 协议等工具链，并支持在 Mac Studio M4 Max 等设备上本地运行。模型权重以 Apache 2.0 许可开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | wlsdks/oh-my-ontology
> **标题**：wlsdks/oh-my-ontology
> **原文链接**：🔗 [打开原文](https://github.com/wlsdks/oh-my-ontology)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Repo-native memory layer for Claude Code, Cursor, and Codex. Local-first markdown ontology + MCP tools so AI coding agents keep codebase context.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | JKHeadley/instar
> **标题**：JKHeadley/instar
> **原文链接**：🔗 [打开原文](https://github.com/JKHeadley/instar)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, llm; high-value terms: agent, agents, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Persistent Claude Code agents with scheduling, sessions, memory, and Telegram.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 论文到代码观察

> [!info]+ **今日必须看 / 87** | Why LLMs Fail at Causal Discovery and How Interventional Agents Escape
> **标题**：Why LLMs Fail at Causal Discovery and How Interventional Agents Escape
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.27567)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.27567v1 Announce Type: new Abstract: Causal discovery is a cornerstone of scientific reasoning, yet whether large language models can perform it reliably remains an open question. Recent benchmarks show that even fine-tuned models plateau on simple causal graphs and degrade as complexity...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 83** | DynaSchedBench: Calibrated Dynamic Scheduling Benchmarks and Observability Paradox in LLM-based Scheduling Agents
> **标题**：DynaSchedBench: Calibrated Dynamic Scheduling Benchmarks and Observability Paradox in LLM-based Scheduling Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.27566)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.27566v1 Announce Type: new Abstract: Progress in neural combinatorial optimization for Dynamic Flexible Job Shop Scheduling Problem (DFJSP) is currently hindered by a methodological tension: static benchmarks encourage benchmark overfitting, while uncalibrated generators obscure algorith...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 83** | Learning Agent-Compatible Context Management for Long-Horizon Tasks
> **标题**：Learning Agent-Compatible Context Management for Long-Horizon Tasks
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.30785)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, research; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.30785v1 Announce Type: new Abstract: LLM agents increasingly face long-horizon tasks such as web search and deep research in real-world applications, where accumulated context can cause long-context degradation and reasoning failures. Prior work mitigates this through context management...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | JobBench: Aligning Agent Work With Human Will
> **标题**：JobBench: Aligning Agent Work With Human Will
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.26329)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.26329v1 Announce Type: new Abstract: Current benchmarks for occupational AI agents are scoped primarily by economic values, telling a replacement story. We introduce JobBench, which evaluates AI agents on the workflows that experts identify as high-priority for delegation, empowering hum...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | When LLM Reward Design Fails: Diagnostic-Driven Refinement for Sparse Structured RL
> **标题**：When LLM Reward Design Fails: Diagnostic-Driven Refinement for Sparse Structured RL
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.28918)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, api, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.28918v1 Announce Type: new Abstract: For sparse, structured reinforcement-learning tasks with semantic reward-function interfaces, LLM-generated reward shaping is better framed as debugging than one-shot generation. We study PPO-trained agents using MiniGrid as core evaluation and MuJoCo...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | LongDS-Bench: On the Failure of Long-Horizon Agentic Data Analysis
> **标题**：LongDS-Bench: On the Failure of Long-Horizon Agentic Data Analysis
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.30434)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.30434v1 Announce Type: new Abstract: Real-world data analysis is inherently iterative, yet existing benchmarks mostly evaluate isolated or short interactive tasks, leaving agents' ability to track evolving analytical context over long horizons untested. We introduce LongDS, a benchmark f...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | Anchor: Mitigating Artifact Drift in Agent Benchmark Generation
> **标题**：Anchor: Mitigating Artifact Drift in Agent Benchmark Generation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.26321)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.26321v1 Announce Type: new Abstract: AI agents are beginning to complete valuable, long-horizon business operations tasks, but training and evaluation environments for enterprise work still struggle to balance realism, verifiability, and scale. Environment and task creation frequently su...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | Your Agents Are Aging Too: Agent Lifespan Engineering for Deployed Systems
> **标题**：Your Agents Are Aging Too: Agent Lifespan Engineering for Deployed Systems
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.26302)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.26302v1 Announce Type: new Abstract: Long-lived AI agents are increasingly deployed as persistent operational systems, yet they are still evaluated like freshly initialized models. Day-one benchmarks miss a basic systems question: how long does an agent remain reliable after deployment?...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 反复出现的信号

> [!info]+ **今日必须看 / 89** | wlsdks/oh-my-ontology
> **标题**：wlsdks/oh-my-ontology
> **原文链接**：🔗 [打开原文](https://github.com/wlsdks/oh-my-ontology)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Repo-native memory layer for Claude Code, Cursor, and Codex. Local-first markdown ontology + MCP tools so AI coding agents keep codebase context.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | JKHeadley/instar
> **标题**：JKHeadley/instar
> **原文链接**：🔗 [打开原文](https://github.com/JKHeadley/instar)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, llm; high-value terms: agent, agents, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Persistent Claude Code agents with scheduling, sessions, memory, and Telegram.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | shotkillschwul/agentic-cycle-tools
> **标题**：shotkillschwul/agentic-cycle-tools
> **原文链接**：🔗 [打开原文](https://github.com/shotkillschwul/agentic-cycle-tools)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, codex, mcp; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🚀 Vibe Kanban 2026 Pro – AI Agent Workflow Optimizer for Claude & Codex
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | HIDORAKAI002/ai-workspace-archive
> **标题**：HIDORAKAI002/ai-workspace-archive
> **原文链接**：🔗 [打开原文](https://github.com/HIDORAKAI002/ai-workspace-archive)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A massive, self-updating local archive of AI tools — 11,000+ agent skills, 240+ MCP servers, 2,200+ IDE rules (Cursor/Cline), and 30+ system prompt collections. One repo to rule them all.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | bkawa-bot/planet-maiko
> **标题**：bkawa-bot/planet-maiko
> **原文链接**：🔗 [打开原文](https://github.com/bkawa-bot/planet-maiko)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A local dev tool where your agents are weird alien dogs. Would you let them in?
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | Chimdiiii/OpenMemory
> **标题**：Chimdiiii/OpenMemory
> **原文链接**：🔗 [打开原文](https://github.com/Chimdiiii/OpenMemory)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🧠 Enhance AI with OpenMemory for long-term memory storage, recall, and explanation, using a unique cognitive architecture.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | maqsam22/claude-code
> **标题**：maqsam22/claude-code
> **原文链接**：🔗 [打开原文](https://github.com/maqsam22/claude-code)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, llm, mcp; high-value terms: agent, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：👨💻 Accelerate your coding with Claude Code, an agentic tool for your terminal that streamlines tasks, explains code, and supports git workflows.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | frankxai/Starlight-Intelligence-System
> **标题**：frankxai/Starlight-Intelligence-System
> **原文链接**：🔗 [打开原文](https://github.com/frankxai/Starlight-Intelligence-System)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Starlight Intelligence System v7.x — substrate (SIP protocol, alliance forging method, attestation) + reference operational layer (6 vaults, MCP server, 7 agents, Console). MIT. Canonical: starlightintelligence.org/protocol
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 83** | Qwen3.7-Plus：多模态智能体智能
> **标题**：Qwen3.7-Plus：多模态智能体智能
> **原文链接**：🔗 [打开原文](https://qwen.ai/blog?id=qwen3.7-plus)
> **source**：AI HOT / Qwen：Blog Retrieval（API）, Hacker News
> **kind**：`model`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：Qwen Studio 提供涵盖聊天机器人、图像与视频理解、图像生成、文档处理、网页搜索集成、工具使用及制品生成的全面功能。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | DeepSWE: A contamination-free benchmark for long-horizon coding agents
> **标题**：DeepSWE: A contamination-free benchmark for long-horizon coding agents
> **原文链接**：🔗 [打开原文](https://deepswe.datacurve.ai/blog)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：59 points | 19 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
