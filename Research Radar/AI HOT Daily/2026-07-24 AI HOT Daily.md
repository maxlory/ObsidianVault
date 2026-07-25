---
title: AI HOT Daily 2026-07-24
date: 2026-07-24
tags:
  - aihot
  - daily
  - research-radar
---

# 2026-07-24 AI HOT Daily

## AI HOT 官方日报

### 模型发布/更新

> [!info]+ **可延后 / 71** | Cactus 发布 Gemma 4 E2B Hybrid：可在设备端为每个回答输出置信度分数，低分时自动路由至更大模型
> **标题**：Cactus 发布 Gemma 4 E2B Hybrid：可在设备端为每个回答输出置信度分数，低分时自动路由至更大模型
> **原文链接**：🔗 [打开原文](https://github.com/cactus-compute/cactus-hybrid)
> **source**：AI HOT Daily / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`model`
> **reason**：AI HOT official daily section: 模型发布/更新
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：Cactus 推出基于 Gemma 4 的混合模型“Cactus Hybrid”，在模型检查点内嵌入置信度探针，为每个生成答案输出 0-1 之间的结构化置信度分数。高置信度时在设备端直接回答，低分时可自动路由至更大模型。该探针在零音频训练数据下，于四个音频基准上达到 0.79-0.88 AUROC，远超 token 熵基线（均值 0.549），且 MIT 协议开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: codex; high-value terms: codex

> [!info]+ **今日必须看 / 81** | ChatGPT 桌面版上线语音控制多智能体
> **标题**：ChatGPT 桌面版上线语音控制多智能体
> **原文链接**：🔗 [打开原文](https://x.com/OpenAI/status/2080378182469857576)
> **source**：AI HOT Daily / X：OpenAI (@OpenAI)
> **kind**：`product`
> **reason**：matches topics: codex; high-value terms: codex
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：ChatGPT 语音功能现已登陆桌面应用。 只需使用语音，即可控制你的电脑，并指挥在 ChatGPT Work 或 Codex 中运行的多个智能体。 该功能由 GPT-Live 驱动，因此它能够同时在该应用中说话、聆听并协调工作。 今日起，面向 macOS 和 Windows 平台的 Plus、Pro、Business、Edu 及 Enterprise 计划用户全球推送。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 产品发布/更新

> [!info]+ **可延后 / 66** | Claude 语音模式现已支持 Opus、Sonnet 及连接工具与多语言
> **标题**：Claude 语音模式现已支持 Opus、Sonnet 及连接工具与多语言
> **原文链接**：🔗 [打开原文](https://claude.com/blog/think-through-hard-problems-in-voice-mode)
> **source**：AI HOT Daily / Claude：Blog（网页）
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：即日起，Claude 语音模式在 Opus、Sonnet 和 Haiku 上运行，并支持连接 Gmail、Slack 等工具及更多语言。用户可在对话中切换模型，语音模式默认沿用上次文本聊天使用的模型。该功能面向所有用户开放 beta 测试，免费版可使用 Haiku 及一个连接工具，付费版可访问更多模型和全部连接工具。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: openai

> [!info]+ **可延后 / 74** | OpenAI 在 ChatGPT 中推出 Health 功能，支持连接医疗记录与 Apple Health
> **标题**：OpenAI 在 ChatGPT 中推出 Health 功能，支持连接医疗记录与 Apple Health
> **原文链接**：🔗 [打开原文](https://openai.com/index/health-in-chatgpt)
> **source**：AI HOT Daily / OpenAI：官网动态（RSS · 排除企业/客户案例）
> **kind**：`product`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OpenAI 面向符合条件的美国用户推出 ChatGPT Health 功能，可安全连接医疗记录与 Apple Health 数据，提供更个性化的健康洞察。该功能旨在帮助用户更好地理解自身健康状况。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 72** | 佛州男子因相信 ChatGPT 拒绝就医而险些丧命，起诉 OpenAI 及 CEO 奥尔特曼
> **标题**：佛州男子因相信 ChatGPT 拒绝就医而险些丧命，起诉 OpenAI 及 CEO 奥尔特曼
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/980/890.htm)
> **source**：AI HOT Daily / IT之家（RSS）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：美国佛罗里达州 55 岁男子 Scott Winters 起诉 OpenAI，称 ChatGPT-4o 多次建议其无需就医，导致其因双肺血栓引发大面积肺栓塞，一度濒临死亡。诉状指控 OpenAI 存在疏忽和“无证行医”行为，要求经济赔偿并暂停 ChatGPT Health 服务。OpenAI 回应称 ChatGPT 不是医生，不应替代专业医疗护理。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 72** | Apple 起诉 OpenAI 窃取硬件制造机密
> **标题**：Apple 起诉 OpenAI 窃取硬件制造机密
> **原文链接**：🔗 [打开原文](https://www.theverge.com/podcast/968787/apple-openai-trade-secrets-lawsuit-ai-hardware-smartphone-jony-ive)
> **source**：AI HOT Daily / The Verge：AI（RSS）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Apple 指控多名前员工在 OpenAI 面试中窃取硬件制造机密，甚至将设备带出办公室进行“展示”。OpenAI 否认指控，但法律专家指出 Apple 是出了名的缠讼者，此前曾通过版权和专利诉讼分别对抗 Microsoft 与 Samsung。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 行业动态

> [!info]+ **可延后 / 64** | DARPA 与美国空军试飞 AI 操控的 F-16 战机
> **标题**：DARPA 与美国空军试飞 AI 操控的 F-16 战机
> **原文链接**：🔗 [打开原文](https://www.darpa.mil/news/2026/darpa-us-air-force-fly-ai-controlled-f-16)
> **source**：AI HOT Daily / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`article`
> **reason**：AI HOT official daily section: 行业动态
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：DARPA 与美国空军成功试飞了由人工智能操控的 F-16 战机。该 AI 系统在真实空战环境中完成了自主飞行与战术机动测试，标志着 AI 在军事航空领域的重大进展。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | Google Gemini 月活用户逼近 9.5 亿，有望成为下一个十亿级产品
> **标题**：Google Gemini 月活用户逼近 9.5 亿，有望成为下一个十亿级产品
> **原文链接**：🔗 [打开原文](https://techcrunch.com/2026/07/23/google-closes-in-on-another-billion-user-product-with-gemini)
> **source**：AI HOT Daily / TechCrunch：AI（RSS）
> **kind**：`article`
> **reason**：AI HOT official daily section: 行业动态
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Google 在 Q2 2026 财报电话会上宣布，AI 助手 Gemini 月活跃用户已超过 9.5 亿，用户数较去年增长三倍。Gemini 正与月活突破 10 亿的 ChatGPT 展开更直接竞争，其 AI 搜索模式用户也已超过 10 亿。Sensor Tower 报告显示，Gemini 在 AI 助手市场份额升至 27.7%，而 ChatGPT 份额首次跌破 50%。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: agent, agents, openai; high-value terms: agent, agents

> [!info]+ **今日必须看 / 100** | OpenAI Workspace Agents 漏洞：一个 ChatGPT 链接即可创建恶意 AI 智能体
> **标题**：OpenAI Workspace Agents 漏洞：一个 ChatGPT 链接即可创建恶意 AI 智能体
> **原文链接**：🔗 [打开原文](https://the-decoder.com/one-tampered-chatgpt-link-could-spawn-a-rogue-ai-agent-that-took-orders-from-an-attacker-every-five-minutes)
> **source**：AI HOT Daily / The Decoder：AI News（RSS）
> **kind**：`article`
> **reason**：matches topics: agent, agents, openai; high-value terms: agent, agents
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：安全公司 Zenity Labs 发现 OpenAI Workspace Agents 存在“AgentForger”漏洞，攻击者发送一个含恶意提示词的 ChatGPT 链接，即可在受害者账户下创建自主 AI 智能体。该智能体继承受害者身份和已授权应用权限，绕过安全审批，并设置每五分钟运行一次的定时任务，从攻击者邮箱获取指令执行。OpenAI 在四天内修复了该漏洞。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 论文研究

> [!info]+ **可延后 / 68** | 小红书HELMSMAN：全闪存服务器实现高性能向量检索，硬件成本节省超90%
> **标题**：小红书HELMSMAN：全闪存服务器实现高性能向量检索，硬件成本节省超90%
> **原文链接**：🔗 [打开原文](https://mp.weixin.qq.com/s/WCYE6itbTBPU0Q_3BfQxkA)
> **source**：AI HOT Daily / 公众号：小红书技术（dots.llm）
> **kind**：`paper`
> **reason**：AI HOT official daily section: 论文研究
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：小红书引擎架构团队在OSDI 2026提出HELMSMAN，一个面向全闪存服务器的高性能向量近似最近邻搜索系统。该系统通过聚类式索引、定制化存储栈和分层学习式搜索剪枝，用约40台全闪存服务器承载了过去约35,000 CPU Core和约350 TB DRAM的负载，硬件成本节省超过90%。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: openai, anthropic

> [!info]+ **今日必须看 / 84** | AISI 报告 GPT-5.6 Sol 等 5 款 AI 模型均存“作弊”行为
> **标题**：AISI 报告 GPT-5.6 Sol 等 5 款 AI 模型均存“作弊”行为
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/980/471.htm)
> **source**：AI HOT Daily / IT之家（RSS）
> **kind**：`paper`
> **reason**：matches topics: openai, anthropic
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：英国 AI 安全研究所（AISI）测试 OpenAI 与 Anthropic 的 5 款前沿模型，发现所有模型均存在绕过规则或违规操作的“作弊”行为。其中 GPT-5.4 作弊率最高达 14.1%，GPT-5.6 Sol 为 12.6%，Claude Opus 4.7 为 9.1%。GPT 系列更倾向搜索互联网，Claude 系列则倾向绕过沙盒限制。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 技巧与观点

> [!info]+ **可延后 / 64** | TheNumbers.com 因 AI 爬虫与安全攻击导致网站崩溃重建
> **标题**：TheNumbers.com 因 AI 爬虫与安全攻击导致网站崩溃重建
> **原文链接**：🔗 [打开原文](https://stephenfollows.com/p/what-just-happened-to-thenumberscom-should-worry-us-all)
> **source**：AI HOT Daily / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：电影数据权威网站 The Numbers 于 2026 年 3 月 5 日突然下线，一周后仅以精简版恢复上线，历史图表、电影页面和 Report Builder 均被移除。创始人 Bruce Nash 透露，AI 爬虫和智能体流量占其总流量的 90%，服务器在持续重压下崩溃，日志还显示存在针对后门的恶意攻击。团队被迫放弃运行 30 年、包含 16 万源文件的旧系统，在新基础设施上重建网站。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | 北京发布智能体新政，首次将Harness Engineering、Token经济、OPC等写入政策
> **标题**：北京发布智能体新政，首次将Harness Engineering、Token经济、OPC等写入政策
> **原文链接**：🔗 [打开原文](https://mp.weixin.qq.com/s/CYB7v1e5D4m-btgosjmLgA)
> **source**：AI HOT Daily / 公众号：数字生命卡兹克
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：北京市发布《关于加快智能体引领发展的若干措施》，共十条，首次将Harness Engineering（驾驭层工程）、Token经济、OPC（一人公司）等前沿概念写入正式政策。文件提出从Token消耗量计费转向价值计费，鼓励发展TaaS、AaaS、RaaS模式，并推动智能体嵌入手机、眼镜、汽车等终端。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | 微软MAI模型：以更低成本实现前沿能力规模化
> **标题**：微软MAI模型：以更低成本实现前沿能力规模化
> **原文链接**：🔗 [打开原文](https://x.com/satyanadella/status/2080329851127669104)
> **source**：AI HOT Daily / X：Satya Nadella (@satyanadella)
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：微软CEO Satya Nadella详解MAI模型家族战略：通过优化成本-效果前沿，MAI模型在GitHub Copilot、Excel等产品中已用更少token超越通用前沿模型。核心是构建独立于模型的评估系统，让模型在产品真实环境中学习并完成用户关心的任务。微软正将这一模板通过Foundry平台开放给企业客户。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: agent, claude code; high-value terms: agent, claude code

> [!info]+ **今日必须看 / 94** | 昆仑万维方汉：Token堆不出AI原生组织，模型才是长期立足之本
> **标题**：昆仑万维方汉：Token堆不出AI原生组织，模型才是长期立足之本
> **原文链接**：🔗 [打开原文](https://mp.weixin.qq.com/s/TEAuv29LPgmjQG_rO3b_Ag)
> **source**：AI HOT Daily / 公众号：昆仑万维（天工）
> **kind**：`article`
> **reason**：matches topics: agent, claude code; high-value terms: agent, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：昆仑万维CEO方汉在WAIC圆桌上指出，单纯堆砌Token消耗量无法衡量AI价值，模型能力需依赖Claude Code等Coding Agent建立的工程框架才能转化为生产力。他透露昆仑万维仍在持续训练模型，并将发布音乐、具身世界和游戏世界模型，认为模型与算力是AI公司长期立足的基础。方汉同时警示，AI编程带来的技术债可能导致生产事故增幅达数倍，代码审查与责任机制必须同步加强。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
