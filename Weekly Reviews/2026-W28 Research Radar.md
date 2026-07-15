---
title: Research Radar Week 2026-W28
date: 2026-07-09
tags:
  - weekly-review
  - research-radar
---

# 2026-W28 Research Radar

## 本周趋势

> [!info]+ **今日必须看 / 100** | Rowboat：开源、本地优先的桌面AI助手
> **标题**：Rowboat：开源、本地优先的桌面AI助手
> **原文链接**：🔗 [打开原文](https://github.com/rowboatlabs/rowboat)
> **source**：AI HOT / Hacker News：AI 热帖, GitHub Trending
> **kind**：`product`
> **reason**：matches topics: claude code, codex, obsidian, mcp; high-value terms: mcp, codex, claude code, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Rowboat 是一个开源、本地优先的桌面 AI 助手，将邮件、会议、Slack 等数据索引为 Obsidian 风格的知识图谱，提供持久上下文记忆。内置邮件客户端、浏览器、会议记录器、代码模式（可调用 Claude Code 或 Codex 代理），并支持按事件或定时运行的背景代理。用户可通过 MCP 协议接入 Exa 搜索、GitHub 等外部工具。所有数据以纯 Markdown 格式本地存储，无供应商锁定，支持 Ollama/LM Studio 本地模型或使用 API 密钥的托管模型。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 97** | Gemini API Managed Agents 新增后台执行、远程 MCP 与自定义函数等能力
> **标题**：Gemini API Managed Agents 新增后台执行、远程 MCP 与自定义函数等能力
> **原文链接**：🔗 [打开原文](https://blog.google/innovation-and-ai/technology/developers-tools/expanding-managed-agents-gemini-api)
> **source**：AI HOT / Google Blog：AI（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Google 为 Gemini API 的 Managed Agents 新增后台执行、远程 MCP 服务器集成、自定义函数调用与凭证刷新功能。后台执行通过传入 `background： true` 异步运行任务，立即返回 ID 供轮询状态或流式获取进度。Managed Agents 可直接连接远程 MCP 服务器，无需自定义代理中间件，并能与内置沙箱工具（如 Google 搜索、代码执行）混合使用。自定义函数调用支持本地执行业务逻辑，内置工具自动在服务端运行。凭证刷新通过传递现有环境 ID 和新网络配置完成，沙箱内文件系统、已安装包和仓库保持不变。这些更新旨在帮助开发者构建可靠的生产级 AI 智能体。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 93** | OfficeCLI：为AI智能体设计的开源Office套件
> **标题**：OfficeCLI：为AI智能体设计的开源Office套件
> **原文链接**：🔗 [打开原文](https://github.com/iOfficeAI/OfficeCLI)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code; high-value terms: agent, agents, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OfficeCLI是全球首个专为AI智能体设计的开源Office套件，以单二进制文件运行，无需安装Office或任何依赖。它内置HTML渲染引擎，可将.docx/.xlsx/.pptx转换为HTML或PNG，形成"渲染→查看→修复"的视觉闭环，使AI代理能自主创建、读取和修改Word、Excel、PowerPoint文档。支持公式、图表、条件格式、RTL布局、修订追踪、表格、数据透视表等复杂功能。提供CLI命令和基于自然语言的桌面应用AionUi，并可一键安装到Claude Code、Cursor、Windsurf、GitHub Copilot等AI编码工具中。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 93** | Claude Code v2.1.202 发布
> **标题**：Claude Code v2.1.202 发布
> **原文链接**：🔗 [打开原文](https://github.com/anthropics/claude-code/releases/tag/v2.1.202)
> **source**：AI HOT / Claude Code：GitHub Releases（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, claude code, mcp; high-value terms: agent, mcp, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Claude Code v2.1.202 在 `/config` 中新增"Dynamic workflow size"设置，可控制动态工作流的 agent 数量规模（小/中/大），作为指导性建议而非硬性上限。工作流派生的 agent 现在会发射 `workflow.run_id` 和 `workflow.name` 的 OpenTelemetry 属性。修复了 mTLS 握手失败、远程控制发送命令失败、移动端发送无说明图片被静默丢弃、语音听写在麦克风故障时无限重试（改为暂停输入）、重载已有技能导致重复指令等问题。改进了工作流 agent 列表布局，MCP 错误消息更清晰。`/review ` 恢复为快速单次审查，多 agent 审查请使用 `/code-review`。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 91** | Elvis Saravia 通过 HITL 和 DialAgent 提升 agentic loops 可靠性
> **标题**：Elvis Saravia 通过 HITL 和 DialAgent 提升 agentic loops 可靠性
> **原文链接**：🔗 [打开原文](https://x.com/omarsar0/status/2074506169352180108)
> **source**：AI HOT / X：Elvis Saravia (@omarsar0, DAIR.AI)
> **kind**：`article`
> **reason**：matches topics: agent, codex, mcp; high-value terms: agent, mcp, codex
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Elvis Saravia 介绍使用 human-in-the-loop（HITL）来提升 agentic loops 的可靠性。他所有 Claude 和 Codex agent 会话都通过 @DialAgent MCP 服务器，该服务器为 agent 提供专属号码，支持语音、SMS、iMessage 作为原生工具。当循环/自动化处理 PR 或新功能时，agent 会通过简短电话将决策升级给人类，尤其适合在路上或离开电脑时。用户可粘贴指令让 agent 拨打电话测试。DialAgent 提供 $5 免费额度：http://getdial.ai
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | imbflool/cc-plugin-eval
> **标题**：imbflool/cc-plugin-eval
> **原文链接**：🔗 [打开原文](https://github.com/imbflool/cc-plugin-eval)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, llm; high-value terms: agent, agents, claude code, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🚀 Automate the evaluation of Claude Code plugin components to ensure accurate triggering of skills, agents, commands, and hooks.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | 777genius/infinity-context
> **标题**：777genius/infinity-context
> **原文链接**：🔗 [打开原文](https://github.com/777genius/infinity-context)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, obsidian, mcp; high-value terms: agent, agents, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Reliable memory and context infrastructure for AI coding agents: source-backed facts, review-gated learning, MCP/SDK/UI, and replaceable Qdrant/Graphiti retrieval.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 87** | MedCalc-Pro: Solving Complex Medical Calculations with LLM Agents
> **标题**：MedCalc-Pro: Solving Complex Medical Calculations with LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.02879)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.02879v1 Announce Type: new Abstract: Current benchmarks for evaluating large language models (LLMs) in medical calculation are largely based on simplified settings, where each patient case corresponds to a single calculator and the required tool is explicitly specified in the query. Howe...
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
> [!info]+ **今日必须看 / 85** | kritird/Mnemex-Context-Graph
> **标题**：kritird/Mnemex-Context-Graph
> **原文链接**：🔗 [打开原文](https://github.com/kritird/Mnemex-Context-Graph)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, llm, mcp; high-value terms: agent, mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Agent memory for LLMs — a self-pruning knowledge context graph modeled on human memory. Long-term, navigable, context-budget-aware memory with no vector DB, no embeddings, no server. Markdown + git. Claude Code plugin.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 值得试用的工具 / 模型

> [!info]+ **今日必须看 / 89** | imbflool/cc-plugin-eval
> **标题**：imbflool/cc-plugin-eval
> **原文链接**：🔗 [打开原文](https://github.com/imbflool/cc-plugin-eval)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, llm; high-value terms: agent, agents, claude code, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🚀 Automate the evaluation of Claude Code plugin components to ensure accurate triggering of skills, agents, commands, and hooks.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 89** | 777genius/infinity-context
> **标题**：777genius/infinity-context
> **原文链接**：🔗 [打开原文](https://github.com/777genius/infinity-context)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, obsidian, mcp; high-value terms: agent, agents, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Reliable memory and context infrastructure for AI coding agents: source-backed facts, review-gated learning, MCP/SDK/UI, and replaceable Qdrant/Graphiti retrieval.
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

## 论文到代码观察

> [!info]+ **今日必须看 / 87** | MedCalc-Pro: Solving Complex Medical Calculations with LLM Agents
> **标题**：MedCalc-Pro: Solving Complex Medical Calculations with LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.02879)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.02879v1 Announce Type: new Abstract: Current benchmarks for evaluating large language models (LLMs) in medical calculation are largely based on simplified settings, where each patient case corresponds to a single calculator and the required tool is explicitly specified in the query. Howe...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 83** | APeB: Benchmarking Personalization Ability of Large Language Model Agents
> **标题**：APeB: Benchmarking Personalization Ability of Large Language Model Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.03162)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.03162v1 Announce Type: new Abstract: LLM-powered agents struggle with personalization when users issue raw, underspecified queries. In this setting, agents must infer latent intent, extract preferences from noisy interaction histories, and select among competing alternatives. Existing be...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | Evaluating Generative Agents with Actions Grounded in Socially Distributed Task Environments using Incognita
> **标题**：Evaluating Generative Agents with Actions Grounded in Socially Distributed Task Environments using Incognita
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.02975)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, benchmark; high-value terms: benchmark, agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.02975v1 Announce Type: new Abstract: Effective agency in social environments depends on when an agent seeks knowledge, when it acts, and whether its actions are justified by acquired information. Existing grounded benchmarks provide executable actions, persistent state, and verifiable ou...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 77** | ASK in the Dark: Uncertainty-Gated LLM Assistance under Partial Observability
> **标题**：ASK in the Dark: Uncertainty-Gated LLM Assistance under Partial Observability
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.02686)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.02686v1 Announce Type: new Abstract: Reinforcement learning agents operating under partial observability must act on incomplete information, making them natural candidates for guidance from small language models (SLMs) that carry broad reasoning priors. Yet integrating SLM guidance into...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 70** | Organizational Memory for Agentic Business Process Execution
> **标题**：Organizational Memory for Agentic Business Process Execution
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.03228)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.03228v1 Announce Type: new Abstract: LLM-based agents offer new opportunities for automating business process execution beyond the limits of rule-based systems. However, general-purpose LLMs lack the organization-specific knowledge required for reliable execution, which is typically frag...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 70** | Object-Centric Environment Modeling for Agentic Tasks
> **标题**：Object-Centric Environment Modeling for Agentic Tasks
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.02846)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.02846v1 Announce Type: new Abstract: Large language model (LLM) agents can improve through accumulated experience, but free-form textual memories become difficult to maintain, validate, and reuse as interactions grow. Recent symbolic approaches learn executable skills or programmatic wor...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 70** | SwarmResearch: Orchestrating Coding Agents for Open-Ended Discovery
> **标题**：SwarmResearch: Orchestrating Coding Agents for Open-Ended Discovery
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.02807)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, research; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.02807v1 Announce Type: new Abstract: Long-running coding agents such as autoresearch can persistently discover optimizations for open-ended problems. However, they tend to converge onto a single high-level approach, then proceed with low-level edits while missing other superior approache...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 69** | Beyond Forecasting: The Belief-to-Trade Layer in Prediction-Market Agents
> **标题**：Beyond Forecasting: The Belief-to-Trade Layer in Prediction-Market Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2607.03015)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2607.03015v1 Announce Type: new Abstract: Forecasting future events has attracted growing attention as a testbed for general-purpose AI. A natural way to ground this evaluation is let the models trade in the prediction markets. Trading, however, requires more than forecasting. Moreover, recen...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 反复出现的信号

> [!info]+ **今日必须看 / 100** | Rowboat：开源、本地优先的桌面AI助手
> **标题**：Rowboat：开源、本地优先的桌面AI助手
> **原文链接**：🔗 [打开原文](https://github.com/rowboatlabs/rowboat)
> **source**：AI HOT / Hacker News：AI 热帖, GitHub Trending
> **kind**：`product`
> **reason**：matches topics: claude code, codex, obsidian, mcp; high-value terms: mcp, codex, claude code, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Rowboat 是一个开源、本地优先的桌面 AI 助手，将邮件、会议、Slack 等数据索引为 Obsidian 风格的知识图谱，提供持久上下文记忆。内置邮件客户端、浏览器、会议记录器、代码模式（可调用 Claude Code 或 Codex 代理），并支持按事件或定时运行的背景代理。用户可通过 MCP 协议接入 Exa 搜索、GitHub 等外部工具。所有数据以纯 Markdown 格式本地存储，无供应商锁定，支持 Ollama/LM Studio 本地模型或使用 API 密钥的托管模型。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 93** | OfficeCLI：为AI智能体设计的开源Office套件
> **标题**：OfficeCLI：为AI智能体设计的开源Office套件
> **原文链接**：🔗 [打开原文](https://github.com/iOfficeAI/OfficeCLI)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）, Hacker News
> **kind**：`product`
> **reason**：matches topics: agent, agents, claude code; high-value terms: agent, agents, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OfficeCLI是全球首个专为AI智能体设计的开源Office套件，以单二进制文件运行，无需安装Office或任何依赖。它内置HTML渲染引擎，可将.docx/.xlsx/.pptx转换为HTML或PNG，形成"渲染→查看→修复"的视觉闭环，使AI代理能自主创建、读取和修改Word、Excel、PowerPoint文档。支持公式、图表、条件格式、RTL布局、修订追踪、表格、数据透视表等复杂功能。提供CLI命令和基于自然语言的桌面应用AionUi，并可一键安装到Claude Code、Cursor、Windsurf、GitHub Copilot等AI编码工具中。
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
> [!info]+ **今日必须看 / 79** | jkershawrh/deepfield-multimodal
> **标题**：jkershawrh/deepfield-multimodal
> **原文链接**：🔗 [打开原文](https://github.com/jkershawrh/deepfield-multimodal)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, reasoning
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Agentic Signal Classification Engine — Three-tier agent cascade (nano/micro/macro) on Intel Xeon 6 with Red Hat OpenShift. Deterministic nanoagents compress on CPU. LLM reasoning when you need it. Agents earn their tier through empirical validation.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 78** | openai/openai-agents-python
> **标题**：openai/openai-agents-python
> **原文链接**：🔗 [打开原文](https://github.com/openai/openai-agents-python)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, openai, llm; high-value terms: agent, agents
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A lightweight, powerful framework for multi-agent workflows
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 72** | tharindu-dhanushka/dataagent-client
> **标题**：tharindu-dhanushka/dataagent-client
> **原文链接**：🔗 [打开原文](https://github.com/tharindu-dhanushka/dataagent-client)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🗣️ Simplify data interaction with a Microsoft Teams app for natural language queries to Fabric Data Agents, featuring real-time responses and multi-agent support.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 72** | dbtlr/norn
> **标题**：dbtlr/norn
> **原文链接**：🔗 [打开原文](https://github.com/dbtlr/norn)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, obsidian; high-value terms: agent, agents
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Your Markdown vault as a deterministic, queryable graph — validation, drift detection, and plan/apply repair for humans and coding agents"
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 72** | haanhthai/tmux-sidecar
> **标题**：haanhthai/tmux-sidecar
> **原文链接**：🔗 [打开原文](https://github.com/haanhthai/tmux-sidecar)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🤖 Enhance your terminal experience with Tmux Sidecar, an AI-native workspace that seamlessly integrates AI agents for coding assistance without heavy setup.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 72** | aliasunder/vault-cortex
> **标题**：aliasunder/vault-cortex
> **原文链接**：🔗 [打开原文](https://github.com/aliasunder/vault-cortex)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, obsidian, mcp; high-value terms: agent, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Standalone MCP server for Obsidian vaults — hybrid search, structured memory, task queries, and full vault access for any AI agent
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 69** | anthropics/claude-plugins-official
> **标题**：anthropics/claude-plugins-official
> **原文链接**：🔗 [打开原文](https://github.com/anthropics/claude-plugins-official)
> **source**：GitHub Search, GitHub Trending
> **kind**：`github_repo`
> **reason**：matches topics: anthropic, mcp; high-value terms: mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
