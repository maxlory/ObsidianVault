---
title: Research Radar Week 2026-W27
date: 2026-06-29
tags:
  - weekly-review
  - research-radar
---

# 2026-W27 Research Radar

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
> [!info]+ **今日必须看 / 100** | 阿德拉菲尼尔：仅在AI agent工作时阻止Mac睡眠的菜单栏工具
> **标题**：阿德拉菲尼尔：仅在AI agent工作时阻止Mac睡眠的菜单栏工具
> **原文链接**：🔗 [打开原文](https://github.com/kageroumado/adrafinil)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Adrafinil 是一款 macOS 菜单栏应用，仅在 Claude Code、Codex、Cursor、Gemini CLI、Aider、Hermes、OpenCode、Cline、Pi 等 9 种 AI coding agent 持有活跃会话时阻止系统睡眠（包括合盖睡眠）。无 agent 工作时，合盖后 Mac 正常睡眠。它通过各 agent 的钩子系统调用 CLI，往返延迟低于 50ms，支持引用计数断言、热切出（温度阈值强制释放）、空闲释放及进程嗅探。需要 macOS Tahoe 26.4，Xcode 26+ 构建，以签名公证的磁盘映像提供。
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
> [!info]+ **今日必须看 / 89** | unitedideas/nothumansearch
> **标题**：unitedideas/nothumansearch
> **原文链接**：🔗 [打开原文](https://github.com/unitedideas/nothumansearch)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Search engine for AI agents — ranks sites by agentic readiness (llms.txt, OpenAPI, MCP, ai-plugin). MCP server, REST API, full-text search. 8,000+ indexed sites.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | wisechef-ai/loopskill-api
> **标题**：wisechef-ai/loopskill-api
> **原文链接**：🔗 [打开原文](https://github.com/wisechef-ai/loopskill-api)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Open-core 'Spotify for AI agents' — a self-hostable registry of runnable agent artifacts: skills, bundles, loops, and personalities. docker compose up → working registry in 60s, zero signup. MCP-native, MPL-2.0.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | selectess/fde-consultants-protocoles
> **标题**：selectess/fde-consultants-protocoles
> **原文链接**：🔗 [打开原文](https://github.com/selectess/fde-consultants-protocoles)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, codex, mcp; high-value terms: agent, mcp, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：FDE Consultants Protocoles — open-source Forward Deployed Engineer + DeepSCR protocol: turn any coding agent (Claude Code, Codex, Cursor) into a certifying engineer with verifiable AI Assurance Scores, MCP tools, and a public trust registry. Apache-2.0.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | HubbaishRana/agent-quickstart
> **标题**：HubbaishRana/agent-quickstart
> **原文链接**：🔗 [打开原文](https://github.com/HubbaishRana/agent-quickstart)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, mcp; high-value terms: agent, agents, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🤖 Build and prototype custom coding agents with this self-hostable quickstart app, inspired by Claude Code for agile development.
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
> [!info]+ **今日必须看 / 89** | sandsower/memento-vault
> **标题**：sandsower/memento-vault
> **原文链接**：🔗 [打开原文](https://github.com/sandsower/memento-vault)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, mcp, codex
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Persistent knowledge capture for coding agents. Auto-triages sessions into searchable Zettelkasten notes with epistemic metadata. Works with Claude Code, Codex, Cursor, Windsurf via hooks or MCP. Local-only, markdown + git.
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
> [!info]+ **今日必须看 / 89** | unitedideas/nothumansearch
> **标题**：unitedideas/nothumansearch
> **原文链接**：🔗 [打开原文](https://github.com/unitedideas/nothumansearch)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Search engine for AI agents — ranks sites by agentic readiness (llms.txt, OpenAPI, MCP, ai-plugin). MCP server, REST API, full-text search. 8,000+ indexed sites.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 论文到代码观察

> [!info]+ **今日必须看 / 87** | How Do Tool-Augmented LLM Agents Perform on Real-World Energy Analytics Tasks?
> **标题**：How Do Tool-Augmented LLM Agents Perform on Real-World Energy Analytics Tasks?
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.26346)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.26346v1 Announce Type: new Abstract: Agentic benchmarks have emerged across general-purpose and domain-specific settings, including finance, coding, law, and drug discovery, yet energy-domain evaluations remain largely limited to static knowledge recall. This is a critical gap for a sect...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | OpenFinGym: A Verifiable Multi-Task Gym Environment for Evaluating Quant Agents
> **标题**：OpenFinGym: A Verifiable Multi-Task Gym Environment for Evaluating Quant Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.26350)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.26350v1 Announce Type: new Abstract: Although large language model agents are increasingly applied to quantitative-finance workflows, their evaluation remains fragmented across isolated tasks, while the financial relevance of benchmark tasks is often overlooked. Yet financial workflows a...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 73** | RIFT-Bench: Dynamic Red-teaming For Agentic AI Systems
> **标题**：RIFT-Bench: Dynamic Red-teaming For Agentic AI Systems
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.23927)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, llm; high-value terms: agent, api, security, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.23927v1 Announce Type: new Abstract: Agentic AI systems powered by large language models (LLMs) are rapidly evolving into autonomous decision-making systems, exposing attack vectors beyond those of traditional LLM vulnerabilities. Existing security evaluations are often tied to specific...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 70** | Critique of Agent Model
> **标题**：Critique of Agent Model
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.23991)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.23991v1 Announce Type: new Abstract: What is an agent? What constitutes agency? With the rise of Large Language Model (LLM) systems marketed as ``coding agents'', ``AI co-scientists'', and other ``agentic" tools that promise to drive up productivity, and at the same time, ``existential"...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 69** | When Agents Meet Electric Bus Fleet Operations: Pricing Behavior, Trade-offs, and Policy Implications in an Aggregator Framework
> **标题**：When Agents Meet Electric Bus Fleet Operations: Pricing Behavior, Trade-offs, and Policy Implications in an Aggregator Framework
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.26400)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, pricing
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.26400v1 Announce Type: new Abstract: Agentic systems are changing how complex operational tasks are coordinated, introducing a new paradigm for connecting heterogeneous data sources and automating processes. Electric bus fleets provide a relevant test case. Their operation requires conti...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 69** | The Verification Horizon: No Silver Bullet for Coding Agent Rewards
> **标题**：The Verification Horizon: No Silver Bullet for Coding Agent Rewards
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.26300)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.26300v1 Announce Type: new Abstract: A classical intuition holds that verifying a solution is easier than producing one. For today's coding agents, this intuition is being inverted: as foundation models develop stronger reasoning capabilities and engineering harnesses grow more sophistic...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 69** | When Retrieval Metrics Mislead: Measuring Policy Signal in Long-Horizon Tool-Use Agents
> **标题**：When Retrieval Metrics Mislead: Measuring Policy Signal in Long-Horizon Tool-Use Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.23937)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.23937v1 Announce Type: new Abstract: Exact-match retrieval recall is often used as a proxy for whether a retriever supplies useful policy context to a downstream decision model. We test this proxy for pre-action policy classification in tau-bench using Qwen2.5-3B/7B classifiers. Under go...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 66** | 思考即回忆：推理如何解锁LLM中的参数化知识
> **标题**：思考即回忆：推理如何解锁LLM中的参数化知识
> **原文链接**：🔗 [打开原文](https://research.google/blog/thinking-to-recall-how-reasoning-unlocks-parametric-knowledge-in-llms)
> **source**：AI HOT / Google Research：Blog（网页）
> **kind**：`paper`
> **reason**：matches topics: llm, research
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：Google Research研究发现，推理（chain-of-thought）能帮助大语言模型（LLM）回忆简单事实，即使这些事实无需复杂推导。在Gemini-2.5 Flash和Pro以及Qwen3-32B上，启用推理后模型能够回答原本无法直接回答的简单问题，pass@k显示正确事实存在于输出分布中。该现象由两个机制驱动：一是生成的推理token充当计算缓冲，允许模型进行隐藏计算以提取参数化知识；二是推理过程中产生的相关事实起到启动效应（factual priming），帮助模型激活正确答案。
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
> [!info]+ **今日必须看 / 100** | 阿德拉菲尼尔：仅在AI agent工作时阻止Mac睡眠的菜单栏工具
> **标题**：阿德拉菲尼尔：仅在AI agent工作时阻止Mac睡眠的菜单栏工具
> **原文链接**：🔗 [打开原文](https://github.com/kageroumado/adrafinil)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Adrafinil 是一款 macOS 菜单栏应用，仅在 Claude Code、Codex、Cursor、Gemini CLI、Aider、Hermes、OpenCode、Cline、Pi 等 9 种 AI coding agent 持有活跃会话时阻止系统睡眠（包括合盖睡眠）。无 agent 工作时，合盖后 Mac 正常睡眠。它通过各 agent 的钩子系统调用 CLI，往返延迟低于 50ms，支持引用计数断言、热切出（温度阈值强制释放）、空闲释放及进程嗅探。需要 macOS Tahoe 26.4，Xcode 26+ 构建，以签名公证的磁盘映像提供。
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
> [!info]+ **今日必须看 / 89** | unitedideas/nothumansearch
> **标题**：unitedideas/nothumansearch
> **原文链接**：🔗 [打开原文](https://github.com/unitedideas/nothumansearch)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Search engine for AI agents — ranks sites by agentic readiness (llms.txt, OpenAPI, MCP, ai-plugin). MCP server, REST API, full-text search. 8,000+ indexed sites.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | wisechef-ai/loopskill-api
> **标题**：wisechef-ai/loopskill-api
> **原文链接**：🔗 [打开原文](https://github.com/wisechef-ai/loopskill-api)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Open-core 'Spotify for AI agents' — a self-hostable registry of runnable agent artifacts: skills, bundles, loops, and personalities. docker compose up → working registry in 60s, zero signup. MCP-native, MPL-2.0.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | How Do Tool-Augmented LLM Agents Perform on Real-World Energy Analytics Tasks?
> **标题**：How Do Tool-Augmented LLM Agents Perform on Real-World Energy Analytics Tasks?
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.26346)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.26346v1 Announce Type: new Abstract: Agentic benchmarks have emerged across general-purpose and domain-specific settings, including finance, coding, law, and drug discovery, yet energy-domain evaluations remain largely limited to static knowledge recall. This is a critical gap for a sect...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | hiper2d/marlow
> **标题**：hiper2d/marlow
> **原文链接**：🔗 [打开原文](https://github.com/hiper2d/marlow)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, research; high-value terms: agent, agents, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Long-loop agent experiment: Werewolf ops + AI safety research stream. Runs on Claude Code.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | Goodeye-Labs/goodeye-cli
> **标题**：Goodeye-Labs/goodeye-cli
> **原文链接**：🔗 [打开原文](https://github.com/Goodeye-Labs/goodeye-cli)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：CLI for Goodeye: turn business outcomes into verified AI workflows that agents run reliably.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 85** | stevesolun/ctx
> **标题**：stevesolun/ctx
> **原文链接**：🔗 [打开原文](https://github.com/stevesolun/ctx)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, obsidian; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Not an Amazon-style catalog or marketplace. ctx is a recommendation layer: bring your org tools or use the shipped graph to load the right skills, agents, MCPs, and harnesses only for the current dev window, cutting token bills and local compute waste: 79,958-node LLM-wiki graph...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | OpenFinGym: A Verifiable Multi-Task Gym Environment for Evaluating Quant Agents
> **标题**：OpenFinGym: A Verifiable Multi-Task Gym Environment for Evaluating Quant Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.26350)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.26350v1 Announce Type: new Abstract: Although large language model agents are increasingly applied to quantitative-finance workflows, their evaluation remains fragmented across isolated tasks, while the financial relevance of benchmark tasks is often overlooked. Yet financial workflows a...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
