---
title: Research Radar Week 2026-W26
date: 2026-06-24
tags:
  - weekly-review
  - research-radar
---

# 2026-W26 Research Radar

## 本周趋势

> [!info]+ **今日必须看 / 100** | Show HN：Oak--专为代理设计的 Git 替代方案
> **标题**：Show HN：Oak--专为代理设计的 Git 替代方案
> **原文链接**：🔗 [打开原文](https://oak.space/oak/oak)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Oak 是开源版本控制系统，专为 AI 智能体（Claude Code、Codex、Cursor）设计。采用 BLAKE3 内容哈希、内容定义分块、diff/merge 及 Blob/Manifest/Commit/Tree 数据模型，可选 SQLite 和 git 后端。以分支-会话为基本工作单元，用分支描述替代逐次提交，通过内容寻址懒加载使智能体数秒内编辑任意仓库。速度远超 git。已发布公开测试版 v0.99.0，支持 macOS（Apple Silicon）、Linux（x86_64）及 Windows，可通过 curl 或 cargo 安装，Apache-2.0 开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 93** | IBM 开源 CUGA：轻量级智能体框架，提供二十余个单文件示例应用
> **标题**：IBM 开源 CUGA：轻量级智能体框架，提供二十余个单文件示例应用
> **原文链接**：🔗 [打开原文](https://huggingface.co/blog/ibm-research/cuga-apps)
> **source**：AI HOT / Hugging Face：Blog（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, openai, mcp; high-value terms: agent, mcp, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：IBM 开源了 CUGA（Configurable Generalist Agent），一个处理规划、执行循环、工具调用和状态管理的轻量级智能体框架。开发者只需提供工具列表和提示词即可构建 CugaAgent。内置计划-执行-反思循环，在 AppWorld（2025年7月-2026年2月）和 WebArena（2025年2月-9月）基准上排名第一。支持 Fast / Balanced / Accurate 三种推理模式，代码执行可在本地、Docker 或 E2B 沙箱中运行。可互换工具支持 OpenAPI、MCP 和 LangChain 函数，通过环境变量一键切换 OpenAI、watsonx、Ollama 等提供商。随框架发布二十余个单文件示例应用，涵盖电影推荐、IBM Cloud 架构顾问等场景，每个应用仅需一个 FastAPI 文件。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 91** | 开源教程《Deep Agents 实战》发布
> **标题**：开源教程《Deep Agents 实战》发布
> **原文链接**：🔗 [打开原文](https://x.com/shao__meng/status/2068306942184034471)
> **source**：AI HOT / X：邵猛 (@shao__meng)
> **kind**：`article`
> **reason**：matches topics: agent, agents, claude code; high-value terms: agent, agents, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：LangChain 官方认证大使 @zhanghaili0610 推出开源教程《Deep Agents 实战》，基于 LangChain / LangGraph 生态，讲解如何用 Deep Agents Harness 框架构建真实 Agent 应用。核心是"三层架构"：Runtime（LangGraph）、Framework（LangChain）、Harness（Deep Agents）。技术内核为上下文工程，通过虚拟文件系统实现按需读取、中间结果落盘、大文件局部读取。教程共 8 章 + 2 准备篇，覆盖虚拟文件系统（六大工具）、任务规划、子 Agent 委派（异步并行）及 Skills 复用（可在 Claude Code、Cursor 等 30+ 工具中通用）。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 90** | Seed2.1 正式发布，深入 AI 生产力
> **标题**：Seed2.1 正式发布，深入 AI 生产力
> **原文链接**：🔗 [打开原文](https://seed.bytedance.com/zh/blog/seed2-1-%E6%AD%A3%E5%BC%8F%E5%8F%91%E5%B8%83-%E6%B7%B1%E5%85%A5-ai-%E7%94%9F%E4%BA%A7%E5%8A%9B)
> **source**：AI HOT / 字节 Seed：Research Feed（网页内嵌数据）
> **kind**：`model`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, api
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：字节Seed发布Seed2.1系列，面向真实生产力场景的智能体，强化通用Agent能力、代码工程交付与多模态理解。Seed2.1 Pro在GDPval基准获最高分，Agents' Last Exam位列参评模型第一梯队；MobileWorld手机GUI任务最高分，CreativeWork多环境任务表现突出。多模态在CharXiv-RQ等多项基准取得SOTA。代码能力上，Seed2.1 Pro在NL2Repo-Bench表现良好，开发者评测相比Claude Opus 4.6获59.1%胜率。模型已在豆包、TRAE上线，API通过火山方舟提供。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | omnigent-ai/omnigent
> **标题**：omnigent-ai/omnigent
> **原文链接**：🔗 [打开原文](https://github.com/omnigent-ai/omnigent)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Omnigent is an open-source AI agent framework and meta-harness: orchestrate Claude Code, Codex, Cursor, Pi, and custom agents — swap harnesses without rewriting, enforce policies and sandboxing, and collaborate in real time from any device.
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
> [!info]+ **今日必须看 / 89** | velvetmonkey/flywheel-memory
> **标题**：velvetmonkey/flywheel-memory
> **原文链接**：🔗 [打开原文](https://github.com/velvetmonkey/flywheel-memory)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, obsidian, mcp; high-value terms: agent, agents, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Compounding knowledge-graph memory for AI agents over open markdown. Hybrid search, self-correcting wikilinks, decision-surface retrieval. MCP server.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | fengaiyunzi/Skills-Manager
> **标题**：fengaiyunzi/Skills-Manager
> **原文链接**：🔗 [打开原文](https://github.com/fengaiyunzi/Skills-Manager)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：⚙️ Manage and sync AI coding assistant skills across multiple platforms to streamline development with Claude Code, Codex, and Opencode.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents
> **标题**：Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.19704)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: benchmark, agent, agents, mcp
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.19704v1 Announce Type: new Abstract: Agent benchmarks are growing fast, but no single benchmark touches more than four or five of the dimensions that deployment exposes. This paper aggregates the largest coordinated deep-dive of one MCP-based industrial-agent benchmark to date: fourteen...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | Can LLMs Be CEOs? Benchmarking Strategic Resource Reallocation with Multi-Role Agent Simulation
> **标题**：Can LLMs Be CEOs? Benchmarking Strategic Resource Reallocation with Multi-Role Agent Simulation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17459)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, llm, research, benchmark; high-value terms: benchmark, agent, eval, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17459v1 Announce Type: new Abstract: Evaluating the decision-making capabilities of large language models (LLMs) is a growing research priority, yet existing benchmarks focus on isolated cognitive tasks such as reasoning, knowledge retrieval, and economic rationality in stylized settings...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 值得试用的工具 / 模型

> [!info]+ **今日必须看 / 90** | Seed2.1 正式发布，深入 AI 生产力
> **标题**：Seed2.1 正式发布，深入 AI 生产力
> **原文链接**：🔗 [打开原文](https://seed.bytedance.com/zh/blog/seed2-1-%E6%AD%A3%E5%BC%8F%E5%8F%91%E5%B8%83-%E6%B7%B1%E5%85%A5-ai-%E7%94%9F%E4%BA%A7%E5%8A%9B)
> **source**：AI HOT / 字节 Seed：Research Feed（网页内嵌数据）
> **kind**：`model`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, api
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：字节Seed发布Seed2.1系列，面向真实生产力场景的智能体，强化通用Agent能力、代码工程交付与多模态理解。Seed2.1 Pro在GDPval基准获最高分，Agents' Last Exam位列参评模型第一梯队；MobileWorld手机GUI任务最高分，CreativeWork多环境任务表现突出。多模态在CharXiv-RQ等多项基准取得SOTA。代码能力上，Seed2.1 Pro在NL2Repo-Bench表现良好，开发者评测相比Claude Opus 4.6获59.1%胜率。模型已在豆包、TRAE上线，API通过火山方舟提供。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | omnigent-ai/omnigent
> **标题**：omnigent-ai/omnigent
> **原文链接**：🔗 [打开原文](https://github.com/omnigent-ai/omnigent)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Omnigent is an open-source AI agent framework and meta-harness: orchestrate Claude Code, Codex, Cursor, Pi, and custom agents — swap harnesses without rewriting, enforce policies and sandboxing, and collaborate in real time from any device.
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

## 论文到代码观察

> [!info]+ **今日必须看 / 87** | Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents
> **标题**：Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.19704)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: benchmark, agent, agents, mcp
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.19704v1 Announce Type: new Abstract: Agent benchmarks are growing fast, but no single benchmark touches more than four or five of the dimensions that deployment exposes. This paper aggregates the largest coordinated deep-dive of one MCP-based industrial-agent benchmark to date: fourteen...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | Can LLMs Be CEOs? Benchmarking Strategic Resource Reallocation with Multi-Role Agent Simulation
> **标题**：Can LLMs Be CEOs? Benchmarking Strategic Resource Reallocation with Multi-Role Agent Simulation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17459)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, llm, research, benchmark; high-value terms: benchmark, agent, eval, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17459v1 Announce Type: new Abstract: Evaluating the decision-making capabilities of large language models (LLMs) is a growing research priority, yet existing benchmarks focus on isolated cognitive tasks such as reasoning, knowledge retrieval, and economic rationality in stylized settings...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | Deontic Policies for Runtime Governance of Agentic AI Systems
> **标题**：Deontic Policies for Runtime Governance of Agentic AI Systems
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.19464)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, security
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.19464v1 Announce Type: new Abstract: Autonomous agentic AI systems driven by Large Language Models (LLMs) introduce a new class of security, privacy, and compliance challenges: an agent that can invoke tools, manipulate data, install software, and coordinate with peer agents across organ...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | Hidden Anchors in Multi-Agent LLM Deliberation
> **标题**：Hidden Anchors in Multi-Agent LLM Deliberation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.19494)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.19494v1 Announce Type: new Abstract: Multi-agent LLM deliberation, where agents exchange and revise answers over several rounds, is increasingly used to improve reasoning and accuracy, yet how and why it works is rarely modelled. Such deliberation mirrors how humans reach decisions. As s...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | DeXposure-Claw: An Agentic System for DeFi Risk Supervision
> **标题**：DeXposure-Claw: An Agentic System for DeFi Risk Supervision
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.19501)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.19501v1 Announce Type: new Abstract: Decentralized finance exposes supervisors to fast-moving, networked credit risks. General-purpose LLM agents fit this setting poorly: they over-read weak evidence and recommend high-stakes interventions, while existing evaluations offer no regulator-a...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | MemTrace: Probing What Final Accuracy Misses in Long-Term Memory
> **标题**：MemTrace: Probing What Final Accuracy Misses in Long-Term Memory
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17328)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17328v1 Announce Type: new Abstract: LLM agents increasingly maintain long-term memory of user facts across sessions. Yet such memory is usually evaluated by aggregating accuracy over question rows or episodes. Because this approach scores question rows independently, even when several q...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | SEAGym: An Evaluation Environment for Self-Evolving LLM Agents
> **标题**：SEAGym: An Evaluation Environment for Self-Evolving LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17546)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17546v1 Announce Type: new Abstract: Self-evolving LLM-based agents improve mainly by changing their agent harness: the structured execution layer around a base model, including prompts, memory, tools, middleware, runtime state, and the model-tool interaction loop. Existing evaluations o...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | MapSatisfyBench: Benchmarking Satisfaction-Aware Map Agents through Behavior-Grounded Implicit Decision Factors
> **标题**：MapSatisfyBench: Benchmarking Satisfaction-Aware Map Agents through Behavior-Grounded Implicit Decision Factors
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17453)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17453v1 Announce Type: new Abstract: Large language model agents are increasingly integrated into map services. Since map services are embedded in everyday-life scenarios rather than professional task settings, users often express their needs informally, resulting in underspecified queri...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 反复出现的信号

> [!info]+ **今日必须看 / 100** | Show HN：Oak--专为代理设计的 Git 替代方案
> **标题**：Show HN：Oak--专为代理设计的 Git 替代方案
> **原文链接**：🔗 [打开原文](https://oak.space/oak/oak)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Oak 是开源版本控制系统，专为 AI 智能体（Claude Code、Codex、Cursor）设计。采用 BLAKE3 内容哈希、内容定义分块、diff/merge 及 Blob/Manifest/Commit/Tree 数据模型，可选 SQLite 和 git 后端。以分支-会话为基本工作单元，用分支描述替代逐次提交，通过内容寻址懒加载使智能体数秒内编辑任意仓库。速度远超 git。已发布公开测试版 v0.99.0，支持 macOS（Apple Silicon）、Linux（x86_64）及 Windows，可通过 curl 或 cargo 安装，Apache-2.0 开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | omnigent-ai/omnigent
> **标题**：omnigent-ai/omnigent
> **原文链接**：🔗 [打开原文](https://github.com/omnigent-ai/omnigent)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Omnigent is an open-source AI agent framework and meta-harness: orchestrate Claude Code, Codex, Cursor, Pi, and custom agents — swap harnesses without rewriting, enforce policies and sandboxing, and collaborate in real time from any device.
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
> [!info]+ **今日必须看 / 87** | Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents
> **标题**：Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.19704)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: benchmark, agent, agents, mcp
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.19704v1 Announce Type: new Abstract: Agent benchmarks are growing fast, but no single benchmark touches more than four or five of the dimensions that deployment exposes. This paper aggregates the largest coordinated deep-dive of one MCP-based industrial-agent benchmark to date: fourteen...
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
> [!info]+ **今日必须看 / 85** | hamidoubodian21-netizen/ai-engineering-production
> **标题**：hamidoubodian21-netizen/ai-engineering-production
> **原文链接**：🔗 [打开原文](https://github.com/hamidoubodian21-netizen/ai-engineering-production)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：AI Engineering Roadmap 2026 🚀 | Build & Deploy AI Apps from Scratch
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | xbtlin/ai-berkshire
> **标题**：xbtlin/ai-berkshire
> **原文链接**：🔗 [打开原文](https://github.com/xbtlin/ai-berkshire)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, llm, mcp; high-value terms: agent, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：AI 时代的伯克希尔：基于 Claude Code 的价值投资研究框架。巴菲特·芒格·段永平·李录四大师方法论 + 多Agent并行研究。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | bunhine0452/Ocul-PM
> **标题**：bunhine0452/Ocul-PM
> **原文链接**：🔗 [打开原文](https://github.com/bunhine0452/Ocul-PM)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, llm; high-value terms: agent, agents, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Local-first AI project manager (AI PM) for AI coding agents — auto-journals, verifies & organizes what Claude Code · Cursor · Gemini CLI do. 코딩 에이전트용 로컬-우선 AI PM · 작업 자동 기록·검증·정리.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 83** | voidly-ai/voidly-pay
> **标题**：voidly-ai/voidly-pay
> **原文链接**：🔗 [打开原文](https://github.com/voidly-ai/voidly-pay)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: release, agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Off-chain credit ledger + hire marketplace for AI agents. Ed25519-signed envelopes, atomic settlement, hire-and-release escrow. https://voidly.ai/pay
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | Show HN: Persona.js – a vanilla-JS agent UI library with native WebMCP (MIT)
> **标题**：Show HN: Persona.js – a vanilla-JS agent UI library with native WebMCP (MIT)
> **原文链接**：🔗 [打开原文](https://www.persona-chat.dev/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：12 points | 16 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
