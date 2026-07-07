---
title: Research Radar Week 2026-W28
date: 2026-07-07
tags:
  - weekly-review
  - research-radar
---

# 2026-W28 Research Radar

## 本周趋势

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
> [!info]+ **今日必须看 / 89** | hellOoSaksit/ai-project-scaffold
> **标题**：hellOoSaksit/ai-project-scaffold
> **原文链接**：🔗 [打开原文](https://github.com/hellOoSaksit/ai-project-scaffold)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：AI-friendly project scaffold & knowledge architecture for AI coding agents (Claude Code, Cursor, Copilot, Codex, Gemini). One CLAUDE.md router, AGENTS.md + llms.txt entry points, job-first docs with frontmatter, SSOT registries, standalone lifecycle, docs-lint CI — for new or ex...
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
> [!info]+ **今日必须看 / 85** | mar-schmidt/Podium
> **标题**：mar-schmidt/Podium
> **原文链接**：🔗 [打开原文](https://github.com/mar-schmidt/Podium)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Thin orchestration layer for local LLM agents. Durable sessions, profiles, scheduling. Leans on native MCP/tools/memory instead of replacing them.
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
> [!info]+ **今日必须看 / 79** | alex-mextner/agent-tools
> **标题**：alex-mextner/agent-tools
> **原文链接**：🔗 [打开原文](https://github.com/alex-mextner/agent-tools)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Portable agent skills, agent & git hooks, CI gates & MCP slots for AI coding agents — universal and by-project-type, with a shared JSONL log lib
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 79** | stacklok/toolhive-studio
> **标题**：stacklok/toolhive-studio
> **原文链接**：🔗 [打开原文](https://github.com/stacklok/toolhive-studio)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：ToolHive is an application that allows you to install, manage and run MCP servers and connect them to AI agents
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 79** | Misaya0/MCP-Agent-Template
> **标题**：Misaya0/MCP-Agent-Template
> **原文链接**：🔗 [打开原文](https://github.com/Misaya0/MCP-Agent-Template)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, llm, mcp; high-value terms: agent, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：🤖 Build multi-agent, retrieval-augmented AI workflows with ready-to-use components for document serving, Q/A bots, and agent orchestration.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 值得试用的工具 / 模型

> [!info]+ **今日必须看 / 89** | hellOoSaksit/ai-project-scaffold
> **标题**：hellOoSaksit/ai-project-scaffold
> **原文链接**：🔗 [打开原文](https://github.com/hellOoSaksit/ai-project-scaffold)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：AI-friendly project scaffold & knowledge architecture for AI coding agents (Claude Code, Cursor, Copilot, Codex, Gemini). One CLAUDE.md router, AGENTS.md + llms.txt entry points, job-first docs with frontmatter, SSOT registries, standalone lifecycle, docs-lint CI — for new or ex...
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

> [!info]+ **今日必须看 / 77** | Contrastive Reflection for Iterative Prompt Optimization
> **标题**：Contrastive Reflection for Iterative Prompt Optimization
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.30840)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.30840v1 Announce Type: new Abstract: LLM agents are becoming central to information retrieval: they issue retrieval queries, synthesize answers, and increasingly serve as judges for IR evaluation. Improving the prompts that control these agents is an optimization problem, but in applied...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 76** | OpenLife: Toward Open-World Artificial Life with Autonomous LLM Agents
> **标题**：OpenLife: Toward Open-World Artificial Life with Autonomous LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.31046)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, research; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.31046v1 Announce Type: new Abstract: Artificial life has explored life-like behavior on many computational substrates, but mostly in researcher-designed closed worlds. We argue that large language model (LLM) agents, with persistent memory, tool use, network access, and payment, now make...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 73** | Reference-Based Prosody and Rhythm Evaluation for Spoken Dialogue Systems
> **标题**：Reference-Based Prosody and Rhythm Evaluation for Spoken Dialogue Systems
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.31055)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, api, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.31055v1 Announce Type: new Abstract: Speech-to-speech (S2S) AI agents are advancing rapidly, yet evaluation lacks interpretable speech-native measures for conversational prosody and rhythm. Because $F_0$, speaking rate, articulation rate, and pausing shift with model-predicted speaker tr...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 70** | A Single Rewrite Suffices: Empirical Lessons from Production Skill Description Optimization
> **标题**：A Single Rewrite Suffices: Empirical Lessons from Production Skill Description Optimization
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.30775)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.30775v1 Announce Type: new Abstract: Enterprise AI agents route user queries to specialized skills by matching queries against natural language skill descriptions. When two skills share overlapping descriptions, the routing LLM misroutes queries, a failure we term skill collision. As age...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 70** | Investigating Multi-Agent Deliberation in Law
> **标题**：Investigating Multi-Agent Deliberation in Law
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.30906)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.30906v1 Announce Type: new Abstract: Artificial Intelligence is increasingly applied to the field of law, and has the potential to increase access to justice. One particular movement that is gaining traction is that of agentic AI, wherein AI agents, based on Large Language Models (LLMs)...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 69** | DDIAgents: Mechanism-Conditioned Context Flow for Drug-Drug Interaction Prediction
> **标题**：DDIAgents: Mechanism-Conditioned Context Flow for Drug-Drug Interaction Prediction
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.31085)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.31085v1 Announce Type: new Abstract: Drug-drug interaction (DDI) prediction is essential for medication safety, yet it requires reasoning over heterogeneous biomedical evidence whose relevance changes across interaction mechanisms. We propose DDIAgents, a mechanism-conditioned multi-agen...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 63** | Bridging Scientific Heritage: An Arabic--Russian Parallel Corpus and LLM Benchmark for Sustainable Knowledge Transfer
> **标题**：Bridging Scientific Heritage: An Arabic--Russian Parallel Corpus and LLM Benchmark for Sustainable Knowledge Transfer
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.30943)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm, research, benchmark; high-value terms: benchmark
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.30943v1 Announce Type: new Abstract: Russian and Arabic are among the major languages of scientific communication. Language barriers impede the exchange of research results between these communities, which affects international collaboration and the progress of sustainability-related res...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **可延后 / 62** | Indi-RomCoM: Code-Mixed Benchmark for Evaluating LLMs on Romanized Indic-English Instructions
> **标题**：Indi-RomCoM: Code-Mixed Benchmark for Evaluating LLMs on Romanized Indic-English Instructions
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.30790)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm, benchmark; high-value terms: benchmark, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.30790v1 Announce Type: new Abstract: Romanized Code Mixing (RCM), where bilingual speakers fluidly blend local languages with English in Roman script, has emerged as the dominant form of communication across multilingual communities. While Large Language Models (LLMs) perform strongly on...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 反复出现的信号

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
> [!info]+ **今日必须看 / 85** | mar-schmidt/Podium
> **标题**：mar-schmidt/Podium
> **原文链接**：🔗 [打开原文](https://github.com/mar-schmidt/Podium)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Thin orchestration layer for local LLM agents. Durable sessions, profiles, scheduling. Leans on native MCP/tools/memory instead of replacing them.
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
> [!info]+ **今日必须看 / 78** | Agents In Biology
> **标题**：Agents In Biology
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/research/agents-in-biology)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: agent, agents, anthropic; high-value terms: agent, agents
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
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
> [!info]+ **今日必须看 / 76** | craft-ai-agents/craft-agents-oss
> **标题**：craft-ai-agents/craft-agents-oss
> **原文链接**：🔗 [打开原文](https://github.com/craft-ai-agents/craft-agents-oss)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
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

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
