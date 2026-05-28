---
title: Research Radar Week 2026-W22
date: 2026-05-28
tags:
  - weekly-review
  - research-radar
---

# 2026-W22 Research Radar

## 本周趋势

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
> [!info]+ **今日必须看 / 89** | BrianV1981/aim
> **标题**：BrianV1981/aim
> **原文链接**：🔗 [打开原文](https://github.com/BrianV1981/aim)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, claude code, codex; high-value terms: agent, agents, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A.I.M. – Actual Intelligent Memory: zero-token exoskeleton for long-running Gemini CLI/Chat GPT Codex/Claude Code agents. External SQLite memory, GitOps guardrails, DataJack cartridges. Alpha, trenches-first.
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
> [!info]+ **今日必须看 / 88** | 社会科学中的编码智能体
> **标题**：社会科学中的编码智能体
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/research/coding-agents-social-sciences)
> **source**：AI HOT / Anthropic：Research（发表成果 · 网页）, Anthropic
> **kind**：`paper`
> **reason**：matches topics: claude code, codex, anthropic; high-value terms: codex, claude code
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：一项针对1260名定量社会科学家的调查显示，虽然81%的受访者用过AI聊天机器人，但仅有20%将Claude Code、Codex等编码智能体常规应用于工作。采用率存在显著差异：以男性名字命名的研究者使用率是女性研究者的两倍；顶尖大学研究者可能性高出40%。用户产出更多工作论文和基金申请，但这可能反映早期采用者自身差异。研究者对AI助力撰写可发表论文更乐观，但对重塑整个社会科学领域持保留态度。这是一项初步调查，更深入研究仍在进行中。
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

## 论文到代码观察

> [!info]+ **今日必须看 / 88** | 社会科学中的编码智能体
> **标题**：社会科学中的编码智能体
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/research/coding-agents-social-sciences)
> **source**：AI HOT / Anthropic：Research（发表成果 · 网页）, Anthropic
> **kind**：`paper`
> **reason**：matches topics: claude code, codex, anthropic; high-value terms: codex, claude code
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：一项针对1260名定量社会科学家的调查显示，虽然81%的受访者用过AI聊天机器人，但仅有20%将Claude Code、Codex等编码智能体常规应用于工作。采用率存在显著差异：以男性名字命名的研究者使用率是女性研究者的两倍；顶尖大学研究者可能性高出40%。用户产出更多工作论文和基金申请，但这可能反映早期采用者自身差异。研究者对AI助力撰写可发表论文更乐观，但对重塑整个社会科学领域持保留态度。这是一项初步调查，更深入研究仍在进行中。
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
> [!info]+ **今日必须看 / 83** | POLAR-Bench: A Diagnostic Benchmark for Privacy-Utility Trade-offs in LLM Agents
> **标题**：POLAR-Bench: A Diagnostic Benchmark for Privacy-Utility Trade-offs in LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.19127)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.19127v1 Announce Type: new Abstract: LLM agents increasingly have access to private user data and act on the user's behalf when interacting with third-party systems. The user defines what may and must not be shared, and the agent must robustly follow that intent even when third-party sys...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
> [!info]+ **今日必须看 / 81** | Trustworthy Agent Network: Trust in Agent Networks Must Be Baked In, Not Bolted On
> **标题**：Trustworthy Agent Network: Trust in Agent Networks Must Be Baked In, Not Bolted On
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.19035)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, api, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.19035v1 Announce Type: new Abstract: The rapid advancement of Large Language Models has given rise to autonomous LLM-based agents capable of complex reasoning and execution. As these agents transition from isolated operation to collaborative ecosystems, we witness the emergence of the Ag...
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
> [!info]+ **今日必须看 / 88** | 社会科学中的编码智能体
> **标题**：社会科学中的编码智能体
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/research/coding-agents-social-sciences)
> **source**：AI HOT / Anthropic：Research（发表成果 · 网页）, Anthropic
> **kind**：`paper`
> **reason**：matches topics: claude code, codex, anthropic; high-value terms: codex, claude code
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：一项针对1260名定量社会科学家的调查显示，虽然81%的受访者用过AI聊天机器人，但仅有20%将Claude Code、Codex等编码智能体常规应用于工作。采用率存在显著差异：以男性名字命名的研究者使用率是女性研究者的两倍；顶尖大学研究者可能性高出40%。用户产出更多工作论文和基金申请，但这可能反映早期采用者自身差异。研究者对AI助力撰写可发表论文更乐观，但对重塑整个社会科学领域持保留态度。这是一项初步调查，更深入研究仍在进行中。
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
> [!info]+ **今日必须看 / 83** | POLAR-Bench: A Diagnostic Benchmark for Privacy-Utility Trade-offs in LLM Agents
> **标题**：POLAR-Bench: A Diagnostic Benchmark for Privacy-Utility Trade-offs in LLM Agents
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.19127)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm, benchmark; high-value terms: benchmark, agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.19127v1 Announce Type: new Abstract: LLM agents increasingly have access to private user data and act on the user's behalf when interacting with third-party systems. The user defines what may and must not be shared, and the agent must robustly follow that intent even when third-party sys...
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
> [!info]+ **今日必须看 / 81** | Trustworthy Agent Network: Trust in Agent Networks Must Be Baked In, Not Bolted On
> **标题**：Trustworthy Agent Network: Trust in Agent Networks Must Be Baked In, Not Bolted On
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2605.19035)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, api, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2605.19035v1 Announce Type: new Abstract: The rapid advancement of Large Language Models has given rise to autonomous LLM-based agents capable of complex reasoning and execution. As these agents transition from isolated operation to collaborative ecosystems, we witness the emergence of the Ag...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 下周观察清单

- 跟进本周最高分工具的 release / issue 活跃度。
- 检查高频模型是否出现可本地部署版本或明确 license。
- 将真正有用的条目转成永久笔记，不保留纯链接堆积。
