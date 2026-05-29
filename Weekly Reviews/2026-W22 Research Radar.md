---
title: Research Radar Week 2026-W22
date: 2026-05-29
tags:
  - weekly-review
  - research-radar
---

# 2026-W22 Research Radar

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
> [!info]+ **今日必须看 / 93** | 谷歌I/O大会发布AI代理全套开发工具链
> **标题**：谷歌I/O大会发布AI代理全套开发工具链
> **原文链接**：🔗 [打开原文](https://x.com/GoogleAI/status/2057871583843135978)
> **source**：AI HOT / X：Google AI (@GoogleAI)
> **kind**：`product`
> **reason**：matches topics: agent, deepmind, mcp; high-value terms: agent, mcp, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：谷歌在I/O开发者大会宣布，系统性构建面向AI代理（Agent）的开发与部署工具链。核心更新包括：独立桌面应用Antigravity 2.0及其命令行工具、SDK面世；Google AI Studio新增Kotlin支持，可一键开发安卓应用并发布，同时推出移动端App。此外，Gemini API推出托管代理服务，实现一键部署；WebMCP作为开放标准在Chrome 149中推出，允许网页向代理暴露工具；Chrome DevTools也开放给AI代理以自动化调试。企业级客户可直接连接Google Cloud项目，而DeepMind的科学技能包则加速特定领域研究。此举标志着谷歌正全面打造从开发、接口到部署的完整AI代理生态系统。
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
> [!info]+ **今日必须看 / 89** | westbrookai/zipsa
> **标题**：westbrookai/zipsa
> **原文链接**：🔗 [打开原文](https://github.com/westbrookai/zipsa)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Lightweight runtime container for AI agents (Claude Code, Codex, Gemini CLI) with MCP support
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
> [!info]+ **今日必须看 / 89** | xiaolai/codex-octopus
> **标题**：xiaolai/codex-octopus
> **原文链接**：🔗 [打开原文](https://github.com/xiaolai/codex-octopus)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, codex, mcp; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：One brain, many arms — spawn multiple specialized Codex agents as MCP servers
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | Design and Report Benchmarks for Knowledge Work
> **标题**：Design and Report Benchmarks for Knowledge Work
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.23262)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, research; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.23262v1 Announce Type: new Abstract: The development of LLM agents has led to a growing body of work on knowledge-work AI, including coding, research, and healthcare. However, current knowledge-work evaluation and benchmark design still largely follow the logic of traditional NLP tasks....
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | GENSTRAT: Toward a Science of Strategic Reasoning in Large Language Models
> **标题**：GENSTRAT: Toward a Science of Strategic Reasoning in Large Language Models
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.23238)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.23238v1 Announce Type: new Abstract: Large language models (LLMs) are increasingly deployed as economic agents in marketplaces, auctions, and bidding settings. Anticipating their behavior in any specific deployment is hard. Existing strategic-reasoning benchmarks evaluate models on fixed...
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
> [!info]+ **今日必须看 / 89** | westbrookai/zipsa
> **标题**：westbrookai/zipsa
> **原文链接**：🔗 [打开原文](https://github.com/westbrookai/zipsa)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Lightweight runtime container for AI agents (Claude Code, Codex, Gemini CLI) with MCP support
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 论文到代码观察

> [!info]+ **今日必须看 / 87** | Design and Report Benchmarks for Knowledge Work
> **标题**：Design and Report Benchmarks for Knowledge Work
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.23262)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, research; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.23262v1 Announce Type: new Abstract: The development of LLM agents has led to a growing body of work on knowledge-work AI, including coding, research, and healthcare. However, current knowledge-work evaluation and benchmark design still largely follow the logic of traditional NLP tasks....
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | GENSTRAT: Toward a Science of Strategic Reasoning in Large Language Models
> **标题**：GENSTRAT: Toward a Science of Strategic Reasoning in Large Language Models
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.23238)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.23238v1 Announce Type: new Abstract: Large language models (LLMs) are increasingly deployed as economic agents in marketplaces, auctions, and bidding settings. Anticipating their behavior in any specific deployment is hard. Existing strategic-reasoning benchmarks evaluate models on fixed...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
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
> [!info]+ **今日必须看 / 87** | AgentAtlas: Beyond Outcome Leaderboards for LLM Agents
> **标题**：AgentAtlas: Beyond Outcome Leaderboards for LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.20530)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.20530v1 Announce Type: new Abstract: Large language model agents now act on codebases, browsers, operating systems, calendars, files, and tool ecosystems, but the benchmarks used to evaluate them are fragmented: each emphasizes a different unit of measurement (final task success, tool-ca...
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
> [!info]+ **今日必须看 / 86** | Constraint Decay: The Fragility of LLM Agents in Back End Code Generation
> **标题**：Constraint Decay: The Fragility of LLM Agents in Back End Code Generation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.06445)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents; strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：174 points | 86 comments
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
> [!info]+ **今日必须看 / 85** | open-covenant/covenant
> **标题**：open-covenant/covenant
> **原文链接**：🔗 [打开原文](https://github.com/open-covenant/covenant)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A capability-based operating layer for long-running autonomous software engineering agents. Rust daemon, signed capabilities, append-only audit, drift-aware memory, fail-closed sandbox dispatch, commit-scoped provenance.
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
> [!info]+ **今日必须看 / 85** | Dan1el2109/mcp-agent-search-hub
> **标题**：Dan1el2109/mcp-agent-search-hub
> **原文链接**：🔗 [打开原文](https://github.com/Dan1el2109/mcp-agent-search-hub)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Ultimate MCP Server Discovery & AI Jobs Hub 2026 – Build Smart Agents Fast
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
> [!info]+ **今日必须看 / 79** | manee1112/yachtsy-mcp-server
> **标题**：manee1112/yachtsy-mcp-server
> **原文链接**：🔗 [打开原文](https://github.com/manee1112/yachtsy-mcp-server)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🚤 Access AI-driven yacht marketplace insights with Yachtsy MCP Server. Explore 25,000+ listings and get expert sailing advice effortlessly.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 79** | monat1628/AI_agents
> **标题**：monat1628/AI_agents
> **原文链接**：🔗 [打开原文](https://github.com/monat1628/AI_agents)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🤖 Automate data analysis and machine learning with a multi-agent system that provides insights and reports efficiently using advanced AI technologies.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
