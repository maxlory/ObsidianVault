---
title: Research Radar Week 2026-W21
date: 2026-05-19
tags:
  - weekly-review
  - research-radar
---

# 2026-W21 Research Radar

## 本周趋势

> [!info]+ **今日必须看 / 93** | 展示 HN：Statewright--通过可视化状态机提升AI智能体可靠性
> **标题**：展示 HN：Statewright--通过可视化状态机提升AI智能体可靠性
> **原文链接**：🔗 [打开原文](https://github.com/statewright/statewright)
> **source**：AI HOT / Hacker News：AI 热帖, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code; high-value terms: agent, agents, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Statewright 是一个通过状态机为AI智能体提供约束的系统，能控制其在各阶段可使用的工具，从而聚焦推理并提升可靠性。它将工作流定义为规划、实施、测试等多个阶段，自动执行工具限制与状态转换。在本地模型测试中，两个模型在5项SWE-bench子任务上应用约束后，正确率从2/10显著提升至10/10。该系统已集成到Claude Code等平台，一个修复测试失败的典型工作流可在46秒内完成。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 93** | 开源工具html-anything助力Agent生成高质量HTML
> **标题**：开源工具html-anything助力Agent生成高质量HTML
> **原文链接**：🔗 [打开原文](https://x.com/xiaohu/status/2054925632061231431)
> **source**：AI HOT / X：小互 (@xiaohu)
> **kind**：`product`
> **reason**：matches topics: agent, claude code, codex; high-value terms: agent, codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：用户分享了对开源项目html-anything的积极体验。该项目旨在让AI Agent能将任何数据转换为具有世界级设计水准的HTML代码。该项目历时三天开发，包含约一万五千行代码，支持75套Skills和9种导出格式，并能兼容包括Claude Code、Codex、OpenClaw、Hermes在内的多种代码生成Agent。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 93** | Claude Code v2.1.143 版本更新：插件管理与用户体验增强
> **标题**：Claude Code v2.1.143 版本更新：插件管理与用户体验增强
> **原文链接**：🔗 [打开原文](https://github.com/anthropics/claude-code/releases/tag/v2.1.143)
> **source**：AI HOT / Claude Code：GitHub Releases（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code; high-value terms: agent, agents, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Claude Code 发布 v2.1.143 版本，重点增强了插件管理功能，包括强制执行插件依赖关系，并新增了插件市场的预估上下文成本显示。为方便直接编辑工作副本，增加了 `worktree.bgIsolation： "none"` 设置。多项体验得到改进：后台会话唤醒后保留模型与努力级别设置；Windows PowerShell 工具默认绕过执行策略；`claude agents` 命令新增多个参数以配置默认会话。此外，本次更新修复了大量错误，包括修复损坏的 `.credentials.json` 文件导致 CLI 启动卡住、Windows Terminal 中的右键粘贴问题、后台会话错误捕获 IDE 文件引用，以及 macOS 上后台作业读取特定目录文件的权限错误等。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 90** | BestBlogs早报：AI智能体工程化实战与安全架构
> **标题**：BestBlogs早报：AI智能体工程化实战与安全架构
> **原文链接**：🔗 [打开原文](https://x.com/hongming731/status/2054701978924859865)
> **source**：AI HOT / X：洪明 (@hongming731)
> **kind**：`article`
> **reason**：matches topics: agent, codex, openai, anthropic; high-value terms: agent, codex
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：BestBlogs早报聚焦AI智能体的工程化落地。Anthropic官方指南详解Claude Computer Use最佳实践，包括解决点击偏移的根本原因、推荐分辨率策略及必须采用虚拟机隔离与人工确认门控的安全原则。OpenAI工程师分享了为Codex构建Windows安全沙箱的历程，其最终方案通过专属安全标识符和写受限令牌，实现了操作系统层面的强制文件系统隔离。早报同时指出，基准测试优异的RAG Agent在生产环境中可能出现高达30%的幻觉率。
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
> [!info]+ **今日必须看 / 89** | hunhee98/pluck
> **标题**：hunhee98/pluck
> **原文链接**：🔗 [打开原文](https://github.com/hunhee98/pluck)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：MCP-native code retrieval for AI agents — 84-88% fewer read tokens, BM25F + semantic search, AST chunks, session dedup
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
> [!info]+ **今日必须看 / 89** | eugeniughelbur/obsidian-second-brain
> **标题**：eugeniughelbur/obsidian-second-brain
> **原文链接**：🔗 [打开原文](https://github.com/eugeniughelbur/obsidian-second-brain)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Cross-CLI skill for Obsidian. Turns your vault into a living AI-first second brain across Claude Code, Codex CLI, Gemini CLI, and OpenCode. 32 commands, vault-first research, scheduled agents, write-time AI-first validator.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | aliyanfauzi361-create/skillify-skill
> **标题**：aliyanfauzi361-create/skillify-skill
> **原文链接**：🔗 [打开原文](https://github.com/aliyanfauzi361-create/skillify-skill)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, mcp; high-value terms: agent, agents, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Capture a session as a reusable agentskills.io skill file for Claude Code, Cursor, Copilot, Gemini CLI, and other agent tools
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | Wintersta7e/nexum
> **标题**：Wintersta7e/nexum
> **原文链接**：🔗 [打开原文](https://github.com/Wintersta7e/nexum)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, codex, mcp; high-value terms: agent, mcp, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Cross-agent memory for AI coding tools (Claude Code, Codex CLI), with cryptographic provenance via SSH-signed git commits
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 值得试用的工具 / 模型

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
> [!info]+ **今日必须看 / 89** | hunhee98/pluck
> **标题**：hunhee98/pluck
> **原文链接**：🔗 [打开原文](https://github.com/hunhee98/pluck)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：MCP-native code retrieval for AI agents — 84-88% fewer read tokens, BM25F + semantic search, AST chunks, session dedup
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

> [!info]+ **今日必须看 / 87** | NARRA-Gym for Evaluating Interactive Narrative Agents
> **标题**：NARRA-Gym for Evaluating Interactive Narrative Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.08503)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.08503v1 Announce Type: new Abstract: Interactive narrative tasks require LLMs to sustain a coherent, evolving story while adapting to a user over multiple turns. However, suitable benchmarks for this setting are limited: existing evaluations often focus on static prompts, isolated story...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 83** | EvolveMem:Self-Evolving Memory Architecture via AutoResearch for LLM Agents
> **标题**：EvolveMem:Self-Evolving Memory Architecture via AutoResearch for LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.13941)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, research; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.13941v1 Announce Type: new Abstract: Long-term memory is essential for LLM agents that operate across multiple sessions, yet existing memory systems treat retrieval infrastructure as fixed: stored content evolves while scoring functions, fusion strategies, and answer-generation policies...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | Collider-Bench: Benchmarking AI Agents with Particle Physics Analysis Reproduction
> **标题**：Collider-Bench: Benchmarking AI Agents with Particle Physics Analysis Reproduction
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.13950)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.13950v1 Announce Type: new Abstract: Autonomous language-model agents are increasingly evaluated on long-horizon tool-use tasks, but existing benchmarks rarely capture the complexity and nuance of real scientific work. To address this gap, we introduce Collider-Bench, a benchmark for eva...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | Deep Reasoning in General Purpose Agents via Structured Meta-Cognition
> **标题**：Deep Reasoning in General Purpose Agents via Structured Meta-Cognition
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.11388)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.11388v1 Announce Type: new Abstract: Humans intuitively solve complex problems by flexibly shifting among reasoning modes: they plan, execute, revise intermediate goals, resolve ambiguity through associative judgment, and apply formal procedures to well-specified subproblems. Current LLM...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | MemQ: Integrating Q-Learning into Self-Evolving Memory Agents over Provenance DAGs
> **标题**：MemQ: Integrating Q-Learning into Self-Evolving Memory Agents over Provenance DAGs
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.08374)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.08374v2 Announce Type: new Abstract: Episodic memory allows LLM agents to accumulate and retrieve experience, but current methods treat each memory independently, i.e., evaluating retrieval quality in isolation without accounting for the dependency chains through which memories enable th...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | Context Pruning for Coding Agents via Multi-Rubric Latent Reasoning
> **标题**：Context Pruning for Coding Agents via Multi-Rubric Latent Reasoning
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.15315)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.15315v1 Announce Type: new Abstract: LLM-powered coding agents spend the majority of their token budget reading repository files, yet much of the retrieved code is irrelevant to the task at hand. Existing learned pruners compress this context with a single-objective sequence labeler, col...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | Think Twice, Act Once: Verifier-Guided Action Selection For Embodied Agents
> **标题**：Think Twice, Act Once: Verifier-Guided Action Selection For Embodied Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.12620)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.12620v1 Announce Type: new Abstract: Building generalist embodied agents capable of solving complex real-world tasks remains a fundamental challenge in AI. Multimodal Large Language Models (MLLMs) have significantly advanced the reasoning capabilities of such agents through strong vision...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | Beyond Cooperative Simulators: Generating Realistic User Personas for Robust Evaluation of LLM Agents
> **标题**：Beyond Cooperative Simulators: Generating Realistic User Personas for Robust Evaluation of LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.12894)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.12894v1 Announce Type: new Abstract: Large Language Model (LLM) agents are increasingly deployed in settings where they interact with a wide variety of people, including users who are unclear, impatient, or reluctant to share information. However, collecting real interaction data at scal...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 反复出现的信号

> [!info]+ **今日必须看 / 93** | 展示 HN：Statewright--通过可视化状态机提升AI智能体可靠性
> **标题**：展示 HN：Statewright--通过可视化状态机提升AI智能体可靠性
> **原文链接**：🔗 [打开原文](https://github.com/statewright/statewright)
> **source**：AI HOT / Hacker News：AI 热帖, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code; high-value terms: agent, agents, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Statewright 是一个通过状态机为AI智能体提供约束的系统，能控制其在各阶段可使用的工具，从而聚焦推理并提升可靠性。它将工作流定义为规划、实施、测试等多个阶段，自动执行工具限制与状态转换。在本地模型测试中，两个模型在5项SWE-bench子任务上应用约束后，正确率从2/10显著提升至10/10。该系统已集成到Claude Code等平台，一个修复测试失败的典型工作流可在46秒内完成。
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
> [!info]+ **今日必须看 / 89** | hunhee98/pluck
> **标题**：hunhee98/pluck
> **原文链接**：🔗 [打开原文](https://github.com/hunhee98/pluck)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：MCP-native code retrieval for AI agents — 84-88% fewer read tokens, BM25F + semantic search, AST chunks, session dedup
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
> [!info]+ **今日必须看 / 85** | afterthings7/local-ai-stack
> **标题**：afterthings7/local-ai-stack
> **原文链接**：🔗 [打开原文](https://github.com/afterthings7/local-ai-stack)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🔍 Build local AI infrastructure on M2 MacBook Air with zero cloud dependencies, ensuring unlimited usage and complete privacy.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | Corryrevokable963/claude-code-book
> **标题**：Corryrevokable963/claude-code-book
> **原文链接**：🔗 [打开原文](https://github.com/Corryrevokable963/claude-code-book)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, llm, mcp; high-value terms: agent, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Explore Claude Code's Agent Harness architecture with a clear breakdown of its runtime, tools, and control flow for AI agent builders
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | device-context-protocol/dcp
> **标题**：device-context-protocol/dcp
> **原文链接**：🔗 [打开原文](https://github.com/device-context-protocol/dcp)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Device Context Protocol — bridge LLM agents to physical devices. Sub-50-byte frames, <16KB MCU footprint, capability-scoped and safe by design. Complementary to MCP.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | torsday/omnifocus-mcp
> **标题**：torsday/omnifocus-mcp
> **原文链接**：🔗 [打开原文](https://github.com/torsday/omnifocus-mcp)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：An MCP server exposing the full OmniFocus surface to LLM agents.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | elloloop/llmrouter
> **标题**：elloloop/llmrouter
> **原文链接**：🔗 [打开原文](https://github.com/elloloop/llmrouter)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, openai, anthropic; high-value terms: agent, agents, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Polyglot Go client for OpenAI, Anthropic, Bedrock, Vertex, Gemini, Cohere + 18 more. Chat, embeddings, TTS, STT, realtime sessions. One OpenAI-shaped API. Build LLM gateways and voice agents.
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

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
