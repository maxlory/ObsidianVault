---
title: Daily Intelligence 2026-06-18
date: 2026-06-18
tags:
  - daily-briefing
  - research-radar
  - workbuddy
---

# 2026-06-18 Daily Intelligence

## 今日概览

- 今日信号总数：261
- 今日必须看：19
- 可延后：57
- 处理建议：先看高分条目的 README / paper / release notes，再决定是否建立永久笔记。

## AI HOT 官方日报

### 模型发布/更新

> [!info]+ **可延后 / 71** | MolmoMotion：语言引导的3D运动预测模型
> **标题**：MolmoMotion：语言引导的3D运动预测模型
> **原文链接**：🔗 [打开原文](https://huggingface.co/blog/allenai/molmomotion)
> **source**：AI HOT Daily / Hugging Face：Blog（RSS）
> **kind**：`model`
> **reason**：AI HOT official daily section: 模型发布/更新
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：MolmoMotion基于Molmo 2骨干网络，输入视频帧、物体上的3D点标记及文字动作指令（如“移动并旋转桌上放水果的木碗”），预测未来数秒内这些点的3D轨迹。提供两个变体：自回归的MolmoMotion-AR逐步预测坐标，流匹配的MolmoMotion-FM通过连续空间变换处理多可能性运动。同时发布MolmoMotion-1M数据集（含116万视频的3D点轨迹及动作描述）和PointMotionBench基准测试（2700个人工验证视频片段）。模型权重、数据集和基准测试均已开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 71** | Grok 4.3 在 Amazon Bedrock 正式可用
> **标题**：Grok 4.3 在 Amazon Bedrock 正式可用
> **原文链接**：🔗 [打开原文](https://x.ai/news/grok-amazon-bedrock)
> **source**：AI HOT Daily / xAI：News（网页）
> **kind**：`model`
> **reason**：AI HOT official daily section: 模型发布/更新
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：6 月 17 日，xAI 宣布 Grok 4.3 在 Amazon Bedrock 上全面可用。该模型在前沿模型中达成最低幻觉率，支持 100 万 token 上下文窗口，并提供可配置推理努力（none/low/medium/high）。在 Artificial Analysis Omniscience 基准排名第一，在 Tau2 Telecom 基准评估客服智能体真实工具调用性能排名第一，在 Vals AI Case Law 和 Corporate Finance 基准的复杂文档理解任务排名第一。定价为输入每百万 token 1.25 美元、输出每百万 token 2.50 美元，每美元智能度是其他前沿模型的 2–10 倍。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: agent, mcp; high-value terms: agent, mcp, api

> [!info]+ **今日必须看 / 100** | Vercel 发布开源 AI 智能体框架 Eve：每个智能体就是一个文件目录
> **标题**：Vercel 发布开源 AI 智能体框架 Eve：每个智能体就是一个文件目录
> **原文链接**：🔗 [打开原文](https://www.marktechpost.com/2026/06/17/vercel-releases-eve)
> **source**：AI HOT Daily / MarkTechPost（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, mcp; high-value terms: agent, mcp, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Vercel 发布开源 AI 智能体框架 Eve（npm 包，Apache-2.0 许可）。Eve 采用文件系统优先设计：每个智能体对应一个磁盘目录，目录结构直接映射模型、指令、工具、技能、连接、子智能体等能力，无需额外注册代码。内置六大生产级能力：持久执行（每步检查点，崩溃后可恢复）、沙箱计算、人机审批、安全连接（支持 MCP 和 OpenAPI）、多通道（Slack、Discord、Teams 等）以及追踪与评估（OpenTelemetry）。Vercel 内部运行了上百个智能体，包括数据分析工具 d0（月处理超3万查询）、自动销售代理 Lead Agent（年费约5000美元、回报32倍）和支持智能体 Vertex（自主解决9…
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: claude code, codex; high-value terms: codex, claude code

> [!info]+ **今日必须看 / 96** | Omnigent开源：AI智能体团队元框架
> **标题**：Omnigent开源：AI智能体团队元框架
> **原文链接**：🔗 [打开原文](https://x.com/Yuchenj_UW/status/2067273020352380950)
> **source**：AI HOT Daily / X：Yuchen Jin (@Yuchenj_UW)
> **kind**：`product`
> **reason**：matches topics: claude code, codex; high-value terms: codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：编程的未来不是单一智能体，而是一个完整的AI团队。 Omnigent让你在一个实时会话中运行一个智能体团队：Claude Code、Codex、Cursor、Pi，以及你自己的智能体。 它是一个面向AI智能体的元框架，基于我们内部的Databricks开发工具构建，现已开源给所有人。 由传奇人物@matei_zaharia和Databricks AI团队打造。没错，Matei仍然编写大量代码，包括Omnigent和我们产品的前端代码。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 产品发布/更新

> [!info]+ **可延后 / 66** | Google发布99美元Gemini智能音箱
> **标题**：Google发布99美元Gemini智能音箱
> **原文链接**：🔗 [打开原文](https://techcrunch.com/2026/06/17/google-bets-on-gemini-to-reinvent-the-smart-home-speaker)
> **source**：AI HOT Daily / TechCrunch：AI（RSS）
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Google推出首款专为Gemini打造的智能音箱Google Home Speaker，售价99.99美元。支持自然语言请求和多步指令，可在说话中途纠正，并具备连续对话功能。内置10种新声音。高级AI功能需订阅Google Home Premium（月费10美元或年费100美元），包括Gemini Live自由对话、Nest摄像头活动摘要等。即日起预售，本月发货。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Wolfram 语言和 Mathematica 15 版发布：内置 AI 助手、符号音乐等新功能
> **标题**：Wolfram 语言和 Mathematica 15 版发布：内置 AI 助手、符号音乐等新功能
> **原文链接**：🔗 [打开原文](https://writings.stephenwolfram.com/2026/06/launching-version-15-of-wolfram-language-mathematica-built-in-useful-ai-lots-of-new-core-functionality)
> **source**：AI HOT Daily / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：在 Mathematica 诞生近 38 年后，Wolfram 语言与 Mathematica 发布 Version 15。每个笔记本内置 AI 助手，支持从 AI 环境中直接调用 Wolfram 技术。新增符号音乐系统、大规模时间序列与事件序列处理、分类数据计算、模型拟合超函数 ModelFit。笔记本支持千兆字节级大小与实时查找，首次引入侧边栏、视觉主题及弃用功能样式。强化了表格连接、多点可视化、图形刻度绘制与轨道运行计算等功能。DSolve 拐角处获得 AI 方法辅助，支持偏微分方程曲线坐标求解。扩充了矩阵分解、多元 zeta 函数与调和数、流线型部分分式分解。强化了 WebSocket 实时连接、Python 交互改进，支持…
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | 阿里云发布HappyOyster 1.0：一句话生成可实时交互的数字世界
> **标题**：阿里云发布HappyOyster 1.0：一句话生成可实时交互的数字世界
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/652.htm)
> **source**：AI HOT Daily / IT之家（RSS）
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：6月17日，阿里云发布开放式世界模型HappyOyster 1.0（快乐生蚝）。该产品基于原生多模态架构，支持多模态输入与音视频联合生成，可在生成过程中持续接收用户指令并实时响应画面。它深度学习物理世界状态转移规律，保持人物和环境长程一致性。官网开放“实时导演”与“世界探索”两种玩法：前者可随时叫停改写故事、与虚拟男友实时互动等；后者支持自由漫游、滑板冲刺、翼装滑翔、骑马奔驰、攻击打怪等交互。该产品已于今年4月16日开放内测，即日起至7月17日官网不定期掉落体验积分。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Claude Design与Replit联动，设计变应用
> **标题**：Claude Design与Replit联动，设计变应用
> **原文链接**：🔗 [打开原文](https://x.com/Replit/status/2067328501003497684)
> **source**：AI HOT Daily / X：Replit (@Replit)
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：在Claude中设计。在Replit中构建。 你现在可以将Claude Design中的设计发送到Replit，将其变成一个可工作的应用。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: claude code; high-value terms: claude code

> [!info]+ **今日必须看 / 81** | Claude Design 更新：跨项目保持品牌一致，与Claude Code协同
> **标题**：Claude Design 更新：跨项目保持品牌一致，与Claude Code协同
> **原文链接**：🔗 [打开原文](https://claude.com/blog/claude-design-stays-on-brand-for-daily-work)
> **source**：AI HOT Daily / Claude：Blog（网页）
> **kind**：`product`
> **reason**：matches topics: claude code; high-value terms: claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：6月17日，Claude Design 更新，支持跨项目使用统一设计系统，并与Claude Code同步工作流。用户可直接拖拽、对齐和缩放画布元素，编辑器稳定性大幅提升。设计系统可从GitHub、设计文件或原始上传导入，团队管理员可锁定标准系统防止篡改。新增桌面端侧边栏入口及独立网页端claude.ai/design。使用限制与聊天、Claude Cowork、Claude Code共享，每次任务消耗更少token，错误率下降。支持导出PDF、PPT，集成Adobe、Canva、Gamma等工具。发布首周用户超一百万。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: agent, hugging face; high-value terms: agent

> [!info]+ **今日必须看 / 89** | Strands Robots SDK：用单一智能体打通 Hugging Face Hub 到物理机器人
> **标题**：Strands Robots SDK：用单一智能体打通 Hugging Face Hub 到物理机器人
> **原文链接**：🔗 [打开原文](https://huggingface.co/blog/amazon/strands-lerobot-hub-to-hardware)
> **source**：AI HOT Daily / Hugging Face：Blog（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, hugging face; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：AWS（Apache 2.0）开源的 Strands Robots SDK 将 LeRobot 栈封装为 AgentTools，构建统一智能体。默认用 MuJoCo 模拟（无需硬件），mode="real" 切换至真实机器人。可记录演示数据为 LeRobotDataset 并推送 Hugging Face Hub，运行 GR00T 或 LerobotLocal 策略推理，经 Zenoh mesh 广播命令到多台机器人。模拟与硬件代码完全一致，只需改一个关键字参数。示例可在笔记本（Python 3.12+，Linux/macOS）无硬件、无 GPU 运行。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: anthropic, deepmind

> [!info]+ **今日必须看 / 80** | Anthropic与DeepMind CEO呼吁G7组建AI联盟排除中国
> **标题**：Anthropic与DeepMind CEO呼吁G7组建AI联盟排除中国
> **原文链接**：🔗 [打开原文](https://x.com/kimmonismus/status/2067310431669223425)
> **source**：AI HOT Daily / X：Kim (@kimmonismus)
> **kind**：`article`
> **reason**：matches topics: anthropic, deepmind
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Dario Amodei（Anthropic）与Demis Hassabis（Google DeepMind）在G7闭门会议上呼吁组建美国主导的联盟，为人工智能制定全球规则和标准。Amodei指出，该联盟应以前沿模型和硬件（包括芯片及其他关键组件）的访问权限为手段，将中国排除在外。这一主张被评论为高技术新冷战的开端，竞争方将从根本上被剥夺参与权。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: openai

> [!info]+ **可延后 / 72** | 泄露文件显示OpenAI年营收130亿但亏损远超收入
> **标题**：泄露文件显示OpenAI年营收130亿但亏损远超收入
> **原文链接**：🔗 [打开原文](https://arstechnica.com/ai/2026/06/leaked-financial-docs-show-openai-is-losing-billions-of-dollars-a-year)
> **source**：AI HOT Daily / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OpenAI 2025年营收130.7亿美元（2024年37亿），但研发成本达191.8亿（含向微软支付105.9亿），收入成本（推理计算）75亿，销售营销成本57.3亿，运营亏损209.2亿。2025年净亏损约390亿，扣除约300亿一次性会计费用后约80亿。2025年3月获1220亿融资（估值8520亿）。ChatGPT周活超9亿，付费约5000万。为控制成本已关闭Sora视频模型并削减非核心业务。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 72** | 消息称 OpenAI 今年一季度现金消耗达 37 亿美元，超同期收入的一半
> **标题**：消息称 OpenAI 今年一季度现金消耗达 37 亿美元，超同期收入的一半
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/335.htm)
> **source**：AI HOT Daily / IT之家（RSS）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OpenAI 在 2026 年第一季度现金消耗达 37 亿美元，超过同期 57 亿美元收入的一半。数据来自一份向股东披露的文件，直观体现 AI 大模型研发与规模化落地的巨额成本。OpenAI 正筹备上市，已在美国保密递交 IPO 申请，最早或于 9 月完成，估值最高可达 1 万亿美元。头部 AI 企业持续重金投入算力、模型研发与人才招募以维持竞争优势。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 76** | LifeSciBench 发布
> **标题**：LifeSciBench 发布
> **原文链接**：🔗 [打开原文](https://openai.com/index/introducing-life-sci-bench)
> **source**：AI HOT Daily / OpenAI：官网动态（RSS · 排除企业/客户案例）
> **kind**：`paper`
> **reason**：matches topics: openai
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：2026 年 6 月，OpenAI 联合 173 位博士级生命科学家发布 LifeSciBench 评测基准，涵盖 750 个真实研究任务，覆盖证据处理、分析、设计优化等七个工作流及七个生物领域。每项任务配有约 25 条细化评分标准（共 19,020 条），评估模型的科学正确性与实用价值。79% 的任务需多步推理，53% 要求解读图表、PDF 等附件数据，旨在衡量 AI 在复杂、不确定的研究任务中的实际能力，而非仅回答结构化问题。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 76** | OpenAI 与 Molecule.one 合作：GPT‑5.4 自主优化 Chan‑Lam 偶联反应
> **标题**：OpenAI 与 Molecule.one 合作：GPT‑5.4 自主优化 Chan‑Lam 偶联反应
> **原文链接**：🔗 [打开原文](https://openai.com/index/ai-chemist-improves-reaction)
> **source**：AI HOT Daily / OpenAI：官网动态（RSS · 排除企业/客户案例）
> **kind**：`paper`
> **reason**：matches topics: openai
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：OpenAI 将 GPT‑5.4 接入 Molecule.one 的自主化学智能体 Maria，用于优化药物化学中的 Chan‑Lam 偶联反应。GPT‑5.4 独立识别伯磺酰胺为高价值挑战性底物，并建议使用 TEMPO 等温和氧化剂。经两轮实验，88% 的硼酸和 83% 的磺酰胺底物产率提升，平均产率从 16.6% 升至 25.2%，产率超 30% 的反应占比从 15.6% 增至 37.5%。人类化学家后续验证，14 对底物中 11 对产率提高，多数提升超两倍。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 行业动态

> [!info]+ **可延后 / 64** | 中国加紧筹建世界人工智能合作组织
> **标题**：中国加紧筹建世界人工智能合作组织
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/248.htm)
> **source**：AI HOT Daily / IT之家（RSS）
> **kind**：`article`
> **reason**：AI HOT official daily section: 行业动态
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：中国正加紧筹建世界人工智能合作组织，欢迎各方加入。2025年7月26日，中国政府倡议成立该组织，作为践行多边主义、推动共商共建共享全球治理的举措，旨在弥合数字和智能鸿沟、促进人工智能向善普惠发展。初步考虑总部设在上海。同日，2025世界人工智能大会发表《人工智能全球治理行动计划》，呼吁各方遵循向善为民、尊重主权、发展导向、安全可控、公平普惠、开放合作的原则，协力推进全球人工智能发展与治理。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | Databricks 扩大对 Snowflake 的领先优势
> **标题**：Databricks 扩大对 Snowflake 的领先优势
> **原文链接**：🔗 [打开原文](https://www.tomtunguz.com/databricks-widens-lead)
> **source**：AI HOT Daily / Tomer Tunguz 博客（VC 分析）
> **kind**：`article`
> **reason**：AI HOT official daily section: 行业动态
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Databricks 年化经常性收入（ARR）达 69 亿美元，同比增长 80%；同期 Snowflake ARR 约 53 亿美元，增速 34%。两者差距从 3 月的 4.9 亿美元扩大至 16 亿美元。AI 产品年化收入 17 亿美元，占总 ARR 的 25%，六个月前为 10 亿美元。Salesforce 以 36 亿美元收购 Fin，其 AI 智能体年收入 1 亿美元，同样占比约 25%，同比增长 350%。Databricks 私人估值 1340 亿美元，80% 的增长率远超 CrowdStrike（26%）和 Shopify（34%）等同行。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: agent; high-value terms: agent

> [!info]+ **今日必须看 / 79** | 谷歌发布Agentic Resource Discovery（ARD）开放规范
> **标题**：谷歌发布Agentic Resource Discovery（ARD）开放规范
> **原文链接**：🔗 [打开原文](https://developers.googleblog.com/announcing-the-agentic-resource-discovery-specification)
> **source**：AI HOT Daily / Google Developers Blog（RSS）
> **kind**：`article`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Agentic Resource Discovery（ARD）是一项开放规范，用于在Web上发布、发现和验证AI工具、技能与智能体。它基于两个原语：组织在其自有域名下托管catalog描述可用能力，registry作为搜索引擎索引catalog并响应发现请求。ARD支持加密验证，使客户端与端点连接前确认发布者身份，然后直接通过原生协议调用能力。Google Cloud的Gemini Enterprise Agent Platform通过Agent Registry提供企业级支持，包括URN命名、出站策略、工具固定和基于Agent Identity的信任验证。该规范现已发布，开发者可通过托管`ai-catalog.json`文件使其服…
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: anthropic

> [!info]+ **可延后 / 72** | Claude Opus 4.8 Build Day黑客马拉松获奖项目揭晓
> **标题**：Claude Opus 4.8 Build Day黑客马拉松获奖项目揭晓
> **原文链接**：🔗 [打开原文](https://claude.com/blog/meet-the-winners-of-our-claude-opus-4-8-build-day-hackathon)
> **source**：AI HOT Daily / Claude：Blog（网页）
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：6月13日，Anthropic在旧金山举办12小时黑客马拉松，310名参与者使用Opus 4.8和$500 credits完成原型。第一名Tekton：输入历史建筑照片后，Claude自动搜集图纸等资料，跨339个施工步骤重建3D模型，每个构件附带证据链；自纠循环反复检查直至20项测试全部通过。第二名Sim Francisco：基于美国人口普查数据生成10,000名合成市民，各具独立世界观，实时对新闻投票，精准预测选举结果。第三名Custom Universe：用手机拍摄物件照片，Opus 4.8将其转为可拖放、实时渲染的3D物体，支持文本指令重设风格。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: claude code, anthropic; high-value terms: claude code

> [!info]+ **今日必须看 / 87** | Anthropic 在首尔开设办公室并宣布多项韩国AI生态合作
> **标题**：Anthropic 在首尔开设办公室并宣布多项韩国AI生态合作
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/seoul-office-partnerships-korean-ai-ecosystem)
> **source**：AI HOT Daily / Anthropic：Newsroom（网页）
> **kind**：`article`
> **reason**：matches topics: claude code, anthropic; high-value terms: claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Anthropic 正式启用首尔办公室，并宣布与韩国AI生态的多项合作。NAVER 在全公司部署 Claude Code，数千工程师用于提升编码效率；Nexon 工程团队用 Claude Code 编写游戏代码。LG CNS 将 Claude 推广至数千员工并计划覆盖整个 LG 集团；Hanwha Solutions 通过 AWS Bedrock 部署 Claude 满足数据驻留与安全要求；Samsung SDS 向三星电子员工部署 Claude（包括 Claude Cowork 和 Claude Code）。初创公司 Channel Corp 用 Claude 驱动客户AI平台 Channel Talk。Anthropic 与韩国…
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: codex; high-value terms: codex

> [!info]+ **今日必须看 / 83** | NVIDIA GEAR实验室发布ENPIRE：8个Codex智能体自主控制机器人完成物理实验
> **标题**：NVIDIA GEAR实验室发布ENPIRE：8个Codex智能体自主控制机器人完成物理实验
> **原文链接**：🔗 [打开原文](https://x.com/DrJimFan/status/2067283904986517866)
> **source**：AI HOT Daily / X：Jim Fan (@DrJimFan)
> **kind**：`paper`
> **reason**：matches topics: codex; high-value terms: codex
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：NVIDIA GEAR实验室推出ENPIRE系统，首次实现物理世界自主研究。系统让8个Codex智能体控制8台机器人，配备GPU和token预算。安全方面采用硬运动极限切断和扭矩受限夹爪两层硬件保障，支持通宵无人运行。奖励函数通过视觉分类器离线固定并冻结，防止智能体作弊。实时监测机器人利用率（MRU）、token利用率（MTU）和GPU利用率，以Tokens-to-Success和Time-to-Success评估效率。ENPIRE自主完成扎带、整理细针、安装GPU等高精度任务，发现8机器人并行探索显著更快。系统将开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 论文研究

> [!info]+ **可延后 / 68** | 用SGLang-JAX在TPU上优化Ling-2.6-1T：一个Pallas核将MoE数据移动隐藏在计算中
> **标题**：用SGLang-JAX在TPU上优化Ling-2.6-1T：一个Pallas核将MoE数据移动隐藏在计算中
> **原文链接**：🔗 [打开原文](https://www.lmsys.org/blog/2026-06-17-ling-2-6-tpu)
> **source**：AI HOT Daily / LMSYS：Blog（Chatbot Arena 团队）
> **kind**：`paper`
> **reason**：AI HOT official daily section: 论文研究
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：SGLang-JAX现已支持inclusionAI的Ling-2.6-1T（1T稀疏MoE，63B激活参数，256路由专家，top-8路由加共享专家）在TPU v7x上高效推理。团队开发了Fused MoE V2——一个融合scatter、专家FFN和gather的Pallas核，通过将MoE数据移动隐藏在计算中，使MoE预填充延迟从5.16ms降至2.42ms（降幅53%），解码核延迟从0.249ms降至0.211ms（降幅约15%）。仅替换MoE核即提升预填充吞吐量24.8%，解码吞吐量18.5%–35.3%。在SGLang解码基准测试中，16块TPU v7x芯片输出吞吐量达16块H200 GPU的1.29倍（mc=128）至1…
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 68** | Google 医学推理 AI 系统 AMIE 新研究：从诊断迈向长期疾病管理
> **标题**：Google 医学推理 AI 系统 AMIE 新研究：从诊断迈向长期疾病管理
> **原文链接**：🔗 [打开原文](https://blog.google/innovation-and-ai/models-and-research/google-research/amie-for-disease-management-in-nature)
> **source**：AI HOT Daily / Google Blog：AI（RSS）
> **kind**：`paper`
> **reason**：AI HOT official daily section: 论文研究
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：今日发表在《自然》杂志上的研究展示了 Google 的医学推理 AI 系统 AMIE（Articulate Medical Intelligence Explorer）从单次诊断对话演进到长期疾病管理的能力。AMIE 利用 Gemini 模型的长上下文能力，整合共情对话智能体和深度思考管理推理智能体，可交叉引用数百页临床指南。在盲测中，AMIE 与 21 名初级保健医生相比，在整体管理推理上匹配临床医生，在计划精确性和指南一致性上得分显著更高。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 技巧与观点

> [!info]+ **可延后 / 64** | Matt Pocock 开源 skills v1：将技能描述 Token 成本降低 63%
> **标题**：Matt Pocock 开源 skills v1：将技能描述 Token 成本降低 63%
> **原文链接**：🔗 [打开原文](https://x.com/AYi_AInotes/status/2067327021005656135)
> **source**：AI HOT Daily / X：阿易 AI Notes (@AYi_AInotes)
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Matt Pocock（Total TypeScript 作者）开源了 skills v1，将技能描述的 Token 成本降低 63%。该工具包将技能分为模型可调用和用户可调用，新增 /codebase-design、/domain-modeling、/grilling 三项技能；重写 /writing-great-skills；将 /diagnose 更新为 /diagnosing-bugs 并改为模型可调用；新增 /ask-matt 路由技能，帮助 AI 自动判断时机触发合适工程流程。主推文评价其将 prompt 从咒语拆解为纪律性流程。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | baoyu-design 本地动画视频导出功能更新
> **标题**：baoyu-design 本地动画视频导出功能更新
> **原文链接**：🔗 [打开原文](https://x.com/dotey/status/2067039941960327204)
> **source**：AI HOT Daily / X：宝玉 (@dotey)
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：baoyu-design（本地运行 Claude Design 的 Skill）新增动画视频导出功能。其声明式动画引擎基于 f(t) 设计：任意时间点 t 可绝对确定画面状态。导出采用无头 Chromium 逐帧截图 + ffmpeg 编码，每帧等待两帧 requestAnimationFrame 确保渲染完成。截图以 2 倍 DPR（3840×2160）再缩回 1080p，保证细节清晰。95 秒 30fps 动画需 2850 次截图循环，帧帧精确。项目已开源（MIT），获 1.2K star。此前 baoyu-design 已支持 PPT 本地生成和导出可编辑 PPTX。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | 预训练还不够“苦涩”
> **标题**：预训练还不够“苦涩”
> **原文链接**：🔗 [打开原文](https://blog.ml.cmu.edu/2026/06/17/pre-training-isnt-bitter-enough)
> **source**：AI HOT Daily / CMU：Machine Learning Blog
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Richard Sutton的“苦涩教训”通常被解读为警告不要在AI系统中编码过多人类知识，最终胜出的方法是能吸收更多算力和数据的一般性方法。现代基础模型预训练表面上是这一教训的胜利：采用通用架构、海量数据、简单的自监督目标（语言模型预测下一个token，视觉模型重建掩码块等）。但问题在于，训练目标仍由人类在训练循环外选定——完成一次大规模预训练后评估下游表现，再调整方案重新运行。这个控制环路非常粗糙。该论文探讨能否让这一环路变得更高效。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | 博客现状，2026年中
> **标题**：博客现状，2026年中
> **原文链接**：🔗 [打开原文](https://www.interconnects.ai/p/state-of-the-blog-mid-2026)
> **source**：AI HOT Daily / Nathan Lambert：Interconnects（RSS）
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Nathan Lambert 在 Interconnects 博客创办约三年后更新规划。他当前三大目标：为前沿模型演进提供清晰度、创建开放模型生态、建立支撑机构。博客定位为原始、高辨识度的独立声音，避免成为全职分析平台。已披露与 Arcee AI 和 Mercor 签署咨询协议，以深入后训练领域并推动透明评测与开放生态。订阅者突破 7 万，付费约 900 人；运营实体 Interconnects AI, LLC 已成立，但银行账户数月余额接近零，收入再投入业务，近期不打算全职运营。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: mcp; high-value terms: mcp

> [!info]+ **今日必须看 / 79** | Google 分享 A2UI 与 MCP Apps 三种集成架构模式
> **标题**：Google 分享 A2UI 与 MCP Apps 三种集成架构模式
> **原文链接**：🔗 [打开原文](https://developers.googleblog.com/a2ui-and-mcp-apps)
> **source**：AI HOT Daily / Google Developers Blog（RSS）
> **kind**：`article`
> **reason**：matches topics: mcp; high-value terms: mcp
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Google 分享了三种集成 A2UI 与 MCP Apps 的架构模式，旨在结合两者优势。A2UI 采用声明式框架，通过 JSON payload 定义 UI，由宿主原生渲染，确保一致性与安全性，但受限于预定义组件库。MCP Apps 在 iframe 中使用标准 Web 技术提供自定义界面，但存在设计碎片化、性能与安全挑战。三种模式包括：通过 MCP 服务器提供 A2UI，利用 MCP Resources 或 Tool 调用传递 JSON，实现“一次编写，原生渲染”的跨平台能力；以及静态与动态交付方案。Google 正考虑扩展 MCP 以原生支持 A2UI。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: anthropic, llm

> [!info]+ **今日必须看 / 80** | 特朗普向Anthropic提出不可能的要求
> **标题**：特朗普向Anthropic提出不可能的要求
> **原文链接**：🔗 [打开原文](https://garymarcus.substack.com/p/breaking-trump-asks-the-impossible)
> **source**：AI HOT Daily / Gary Marcus：The Road to AI We Can Trust（RSS）
> **kind**：`article`
> **reason**：matches topics: anthropic, llm
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：特朗普要求Anthropic完成不可能的任务，暴露了生成式AI安全护栏的根本困境。早在2024年1月，Gary Marcus就指出任何护栏都难以在过于严格和过于宽松之间找到平衡。如今这一判断得到验证：基于next-token predictor的大语言模型本质上不适合安全控制。要么对LLM加以限制直至出现更好的技术，要么承受后果。问题并非Anthropic独有，而是整个生成式AI面临的挑战。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 今日必须看

> [!info]+ **今日必须看 / 91** | microsoft/skills
> **标题**：microsoft/skills
> **原文链接**：🔗 [打开原文](https://github.com/microsoft/skills)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp; strong public engagement
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Skills, MCP servers, Custom Agents, Agents.md for SDKs to ground Coding Agents
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 89** | thegoodguysla/myco-brain
> **标题**：thegoodguysla/myco-brain
> **原文链接**：🔗 [打开原文](https://github.com/thegoodguysla/myco-brain)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Self-hosted, source-traceable memory for AI agents: on your own Postgres, keyless to run. Facts earn confidence from independent corroboration and supersede on contradiction, never silently overwritten. MCP-native (Claude, Cursor, Windsurf). Reproducible LongMemEval in-repo: 73....
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 89** | jeremylongshore/intent-eval-lab
> **标题**：jeremylongshore/intent-eval-lab
> **原文链接**：🔗 [打开原文](https://github.com/jeremylongshore/intent-eval-lab)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, codex, mcp; high-value terms: agent, mcp, codex, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Vendor-neutral research umbrella for measuring AI plugin, agent, and MCP server quality across CLI runtimes (Claude Code, Gemini CLI, Copilot CLI, Codex CLI).
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 87** | marcusquinn/aidevops
> **标题**：marcusquinn/aidevops
> **原文链接**：🔗 [打开原文](https://github.com/marcusquinn/aidevops)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents, api, security; strong public engagement
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Vibe-Coding is easy. DevOps is hard. OpenCode & Git token-efficient AI agent automation for your app, business, and personal development. Opinionated tools, services, CLI & API stack for speed, security, and 24/7 results. Open-source first. SOTA everything. Try on your repos for...
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

> [!info]+ **今日必须看 / 85** | Claude Code v2.1.181 发布
> **标题**：Claude Code v2.1.181 发布
> **原文链接**：🔗 [打开原文](https://github.com/anthropics/claude-code/releases/tag/v2.1.181)
> **source**：AI HOT / Claude Code：GitHub Releases（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, claude code; high-value terms: agent, claude code, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Claude Code v2.1.181 发布，新增 `/config key=value` 语法允许在提示中直接设置任意配置项，新增 `sandbox.allowAppleEvents` 选项使沙盒命令支持 Apple Events，新增 `CLAUDE_CLIENT_PRESENCE_FILE` 环境变量用于抑制移动端推送通知。内置 Bun 运行时升级至 1.4，改进了长段落流式输出（逐行显示）和 API 连接中断后自动重试。子 agent 面板优化：空闲 agent 30 秒自动隐藏、列表最多 5 行。修复了提示缓存读取、Write/Edit 在网络驱动器产生 0 字节文件、启动性能回归（约 120ms）、启动阻塞（最长 15 秒）、macOS TUI 冻结、子 agent 时长显示错误、API 重试指示器残留、AWS 凭证刷新等问题。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 85** | Vercel 发布开源 AI 智能体框架 Eve：每个智能体就是一个文件目录
> **标题**：Vercel 发布开源 AI 智能体框架 Eve：每个智能体就是一个文件目录
> **原文链接**：🔗 [打开原文](https://www.marktechpost.com/2026/06/17/vercel-releases-eve)
> **source**：AI HOT / MarkTechPost（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, mcp; high-value terms: agent, mcp, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Vercel 发布开源 AI 智能体框架 Eve（npm 包，Apache-2.0 许可）。Eve 采用文件系统优先设计：每个智能体对应一个磁盘目录，目录结构直接映射模型、指令、工具、技能、连接、子智能体等能力，无需额外注册代码。内置六大生产级能力：持久执行（每步检查点，崩溃后可恢复）、沙箱计算、人机审批、安全连接（支持 MCP 和 OpenAPI）、多通道（Slack、Discord、Teams 等）以及追踪与评估（OpenTelemetry）。Vercel 内部运行了上百个智能体，包括数据分析工具 d0（月处理超3万查询）、自动销售代理 Lead Agent（年费约5000美元、回报32倍）和支持智能体 Vertex（自主解决92%工单）。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 85** | wolfieman/lodestar
> **标题**：wolfieman/lodestar
> **原文链接**：🔗 [打开原文](https://github.com/wolfieman/lodestar)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, llm, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Lodestar — a 2026 agentic, RAG-grounded HBCU career-advice assistant (model-agnostic, hybrid RAG, MCP server). Evolved from IgniteAI, the first-place HP FOWA 2024 custom GPT (faithfully reproduced here).
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

> [!info]+ **今日必须看 / 79** | keepwhatworks/trinity
> **标题**：keepwhatworks/trinity
> **原文链接**：🔗 [打开原文](https://github.com/keepwhatworks/trinity)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: codex, llm, mcp; high-value terms: mcp, codex, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Ask all three. Keep what works. A local-first cross-provider council layer for Claude, Codex, and Gemini — one prompt, all three answer, a chairman synthesizes the verdict. No new app, no API key; your transcripts never leave your machine.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 79** | microservices-sh/mcp
> **标题**：microservices-sh/mcp
> **原文链接**：🔗 [打开原文](https://github.com/microservices-sh/mcp)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Local stdio MCP server for microservices.sh modules and guarded agent workflows.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 79** | houstongolden/youmd
> **标题**：houstongolden/youmd
> **原文链接**：🔗 [打开原文](https://github.com/houstongolden/youmd)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents, mcp; high-value terms: agent, agents, mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Identity context protocol for the agent internet -- an MCP where the context is you.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 79** | daniyalazhar123/AI-Employee-Vault
> **标题**：daniyalazhar123/AI-Employee-Vault
> **原文链接**：🔗 [打开原文](https://github.com/daniyalazhar123/AI-Employee-Vault)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: claude code, obsidian, mcp; high-value terms: mcp, claude code, api
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Personal AI Employee — Gold Tier | Odoo 19 Docker | Gmail API | MCP Servers | Claude Code | Hackathon 0
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 78** | Omnigent开源：AI智能体团队元框架
> **标题**：Omnigent开源：AI智能体团队元框架
> **原文链接**：🔗 [打开原文](https://x.com/Yuchenj_UW/status/2067273020352380950)
> **source**：AI HOT / X：Yuchen Jin (@Yuchenj_UW)
> **kind**：`product`
> **reason**：matches topics: claude code, codex; high-value terms: codex, claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：编程的未来不是单一智能体，而是一个完整的AI团队。 Omnigent让你在一个实时会话中运行一个智能体团队：Claude Code、Codex、Cursor、Pi，以及你自己的智能体。 它是一个面向AI智能体的元框架，基于我们内部的Databricks开发工具构建，现已开源给所有人。 由传奇人物@matei_zaharia和Databricks AI团队打造。没错，Matei仍然编写大量代码，包括Omnigent和我们产品的前端代码。
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

> [!info]+ **今日必须看 / 77** | Determinism as a feature: when to let your agent call a math API instead of reasoning
> **标题**：Determinism as a feature: when to let your agent call a math API instead of reasoning
> **原文链接**：🔗 [打开原文](https://dev.to/whatsonyourmind/determinism-as-a-feature-when-to-let-your-agent-call-a-math-api-instead-of-reasoning-10mf)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents, api, reasoning
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：LLM agents are great at deciding what to do and unreliable at computing it. Ask one to allocate...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 76** | millionco/react-doctor
> **标题**：millionco/react-doctor
> **原文链接**：🔗 [打开原文](https://github.com/millionco/react-doctor)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents; strong public engagement
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Your agent writes bad React. This catches it
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 可延后

> [!info]+ **可延后 / 74** | Lightpanda Agent and PandaScript – LLM at buildtime, not runtime
> **标题**：Lightpanda Agent and PandaScript – LLM at buildtime, not runtime
> **原文链接**：🔗 [打开原文](https://lightpanda.io/blog/posts/introducing-lightpanda-agent-and-pandascript)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：8 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 72** | thelminson/brain-kit
> **标题**：thelminson/brain-kit
> **原文链接**：🔗 [打开原文](https://github.com/thelminson/brain-kit)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, claude code, obsidian; high-value terms: agent, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Sistema de investigación/conocimiento para Claude Code (agentes + skills + estructura), domain-agnostic. Plantilla reutilizable para cualquier área.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 71** | 苹果 Xcode 27 核心首次深度集成 AI 智能体：支持自然语言修 Bug、构建 App
> **标题**：苹果 Xcode 27 核心首次深度集成 AI 智能体：支持自然语言修 Bug、构建 App
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/734.htm)
> **source**：AI HOT / IT之家（RSS）
> **kind**：`product`
> **reason**：matches topics: openai, anthropic; high-value terms: api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：在 2026 年 WWDC 期间，苹果发布 Xcode 27，其核心组件首次整合 AI 智能体，能理解 Swift 语言并通过多轮自然语言对话辅助开发。AI 可跨多个文件修改整个代码库，也能根据提示与资源生成应用设计并独立构建完整应用，建成后仍可通过对话添加特效、动画等。Xcode 27 支持接入 Anthropic、OpenAI 和 Google 等第三方 AI 模型，同时引入 Core AI 框架提供现代 Swift API 调用端侧模型，并升级开源框架 MLX。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 71** | Strands Robots SDK：用单一智能体打通 Hugging Face Hub 到物理机器人
> **标题**：Strands Robots SDK：用单一智能体打通 Hugging Face Hub 到物理机器人
> **原文链接**：🔗 [打开原文](https://huggingface.co/blog/amazon/strands-lerobot-hub-to-hardware)
> **source**：AI HOT / Hugging Face：Blog（RSS）
> **kind**：`product`
> **reason**：matches topics: agent, hugging face; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：AWS（Apache 2.0）开源的 Strands Robots SDK 将 LeRobot 栈封装为 AgentTools，构建统一智能体。默认用 MuJoCo 模拟（无需硬件），mode="real" 切换至真实机器人。可记录演示数据为 LeRobotDataset 并推送 Hugging Face Hub，运行 GR00T 或 LerobotLocal 策略推理，经 Zenoh mesh 广播命令到多台机器人。模拟与硬件代码完全一致，只需改一个关键字参数。示例可在笔记本（Python 3.12+，Linux/macOS）无硬件、无 GPU 运行。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 71** | askalf/deepdive
> **标题**：askalf/deepdive
> **原文链接**：🔗 [打开原文](https://github.com/askalf/deepdive)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, anthropic, llm, research; high-value terms: agent
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Local research agent. One command, cited answer. Plan → search → headless browser fetch → extract → synthesize. Every LLM call goes through your own router (dario / Anthropic-compat). Zero hosted dependencies.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 70** | 借助 Workload Identity Federation 安全访问 Claude Platform
> **标题**：借助 Workload Identity Federation 安全访问 Claude Platform
> **原文链接**：🔗 [打开原文](https://claude.com/blog/workload-identity-federation)
> **source**：AI HOT / Claude：Blog（网页）
> **kind**：`product`
> **reason**：matches topics: claude code; high-value terms: claude code, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Workload Identity Federation （WIF） 已在 Claude Platform 上全面可用。WIF 兼容任何 OIDC 身份提供者，覆盖所有 Claude API 端点（包括第一方 SDK 和 Claude Code）。WIF 用短生命期凭证替代静态 API 密钥，并引入服务账户，每个工作负载拥有独立身份、角色和审计日志。Claude Console 提供引导设置流程，支持 Admin API 进行组织管理。API 密钥可并行使用以便逐步迁移。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 70** | Kickart 3.0发布，让广告视频创作更精准高效
> **标题**：Kickart 3.0发布，让广告视频创作更精准高效
> **原文链接**：🔗 [打开原文](https://mp.weixin.qq.com/s/e6SOMeRbdMB_zATsNcYmUQ)
> **source**：AI HOT / 公众号：火山引擎
> **kind**：`product`
> **reason**：matches topics: agent; high-value terms: agent, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：火山引擎一站式营销创作平台Kickart 3.0（原"创作Agent"）正式上线，升级为对话式视频生成模式，用户可通过多轮对话调整商品图、故事板等，用自然语言生成营销视频。新增"爆款裂变"能力，上传视频链接后自动拆解爆款逻辑并重构至新商品视频，支持抖音电商内容合规与质量预审核。平台开放SaaS、API及Skill等多种交付方式，并已接入Seedance 2.0 mini，助力降低广告营销成本。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 70** | Closing the Feedback Loop: From Experience Extraction to Insight Governance in Verbal Reinforcement Learning
> **标题**：Closing the Feedback Loop: From Experience Extraction to Insight Governance in Verbal Reinforcement Learning
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17591)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents, llm; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17591v1 Announce Type: new Abstract: Training-free verbal reinforcement learning enables LLM agents to learn from world feedback -- objective signals such as dynamic task outcomes, market returns, or demand forecasts -- by extracting verbal rules from experience and injecting them as con...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 69** | 如何在 OpenRouter 上使用 OpenAI Codex CLI
> **标题**：如何在 OpenRouter 上使用 OpenAI Codex CLI
> **原文链接**：🔗 [打开原文](https://openrouter.ai/blog/tutorials/codex-cli-openrouter)
> **source**：AI HOT / OpenRouter：Announcements（RSS）
> **kind**：`article`
> **reason**：matches topics: codex, openai; high-value terms: codex
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Codex CLI 支持自定义 OpenAI 兼容提供商，只需在 config.toml 中配置即可将请求路由到 OpenRouter。用户无需修改 Codex 本身，就能获得提供商故障转移、使用跟踪以及跨所有模型的统一密钥。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Show HN: Tyto – find where audio breaks your voice-agent calls
> **标题**：Show HN: Tyto – find where audio breaks your voice-agent calls
> **原文链接**：🔗 [打开原文](https://call-analysis.ai-coustics.com/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：14 points | 2 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Strands Shell: Give your agent a shell without giving it keys to your machine
> **标题**：Strands Shell: Give your agent a shell without giving it keys to your machine
> **原文链接**：🔗 [打开原文](https://github.com/strands-agents/shell)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：9 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Eve – The Framework for Building Agents
> **标题**：Eve – The Framework for Building Agents
> **原文链接**：🔗 [打开原文](https://vercel.com/eve)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：9 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Show HN: RewardHackBench: Using sandboxes to stop agents from cheating
> **标题**：Show HN: RewardHackBench: Using sandboxes to stop agents from cheating
> **原文链接**：🔗 [打开原文](https://github.com/islo-labs/reward-hack-bench)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：8 points | 3 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Show HN: Visual Agents with Code Mode
> **标题**：Show HN: Visual Agents with Code Mode
> **原文链接**：🔗 [打开原文](https://www.vlm.run/blog/orion-2)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：7 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Show HN: Relaymux, a tmux-based meta-harness for local coding agents
> **标题**：Show HN: Relaymux, a tmux-based meta-harness for local coding agents
> **原文链接**：🔗 [打开原文](https://github.com/mupt-ai/relaymux)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：7 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Show HN: Agenthatch – Compile any skill into a standalone Python agent
> **标题**：Show HN: Agenthatch – Compile any skill into a standalone Python agent
> **原文链接**：🔗 [打开原文](https://github.com/agenthatch/agenthatch)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：7 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | My AI agent got dumber mid-session. I measured the context window before blaming MCP.
> **标题**：My AI agent got dumber mid-session. I measured the context window before blaming MCP.
> **原文链接**：🔗 [打开原文](https://dev.to/rapls/my-ai-agent-got-dumber-mid-session-i-measured-the-context-window-before-blaming-mcp-4c3l)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent, llm, mcp; high-value terms: agent, mcp
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：There's a particular way an AI coding agent goes bad. Not a crash, not an error. It just gets duller....
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 65** | NVIDIA GEAR实验室发布ENPIRE：8个Codex智能体自主控制机器人完成物理实验
> **标题**：NVIDIA GEAR实验室发布ENPIRE：8个Codex智能体自主控制机器人完成物理实验
> **原文链接**：🔗 [打开原文](https://x.com/DrJimFan/status/2067283904986517866)
> **source**：AI HOT / X：Jim Fan (@DrJimFan)
> **kind**：`paper`
> **reason**：matches topics: codex; high-value terms: codex
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：NVIDIA GEAR实验室推出ENPIRE系统，首次实现物理世界自主研究。系统让8个Codex智能体控制8台机器人，配备GPU和token预算。安全方面采用硬运动极限切断和扭矩受限夹爪两层硬件保障，支持通宵无人运行。奖励函数通过视觉分类器离线固定并冻结，防止智能体作弊。实时监测机器人利用率（MRU）、token利用率（MTU）和GPU利用率，以Tokens-to-Success和Time-to-Success评估效率。ENPIRE自主完成扎带、整理细针、安装GPU等高精度任务，发现8机器人并行探索显著更快。系统将开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | dreamide/dream
> **标题**：dreamide/dream
> **原文链接**：🔗 [打开原文](https://github.com/dreamide/dream)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Dream is an IDE built for AI coding.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | dsbarik/rag-research-assistant
> **标题**：dsbarik/rag-research-assistant
> **原文链接**：🔗 [打开原文](https://github.com/dsbarik/rag-research-assistant)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: llm, research; high-value terms: api, eval
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A modular, high-performance Retrieval-Augmented Generation (RAG) system that runs 100% locally. It features a dual interface (FastAPI & Gradio), persistent vector storage with ChromaDB, and context-aware conversations using Ollama and Sentence Transformers.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | bloknayrb/tandem
> **标题**：bloknayrb/tandem
> **原文链接**：🔗 [打开原文](https://github.com/bloknayrb/tandem)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: claude code, mcp; high-value terms: mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Collaborative AI-human document editor — real-time annotations, suggestions, and review powered by Claude Code via MCP
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | JSungMin/vs-token-safer
> **标题**：JSungMin/vs-token-safer
> **原文链接**：🔗 [打开原文](https://github.com/JSungMin/vs-token-safer)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: claude code, mcp; high-value terms: mcp, claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Token-safe C++/C# code search via clangd/Roslyn index instead of grep. Local-only, no IDE required. Claude Code plugin + vts CLI.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 63** | Claude Design 更新：跨项目保持品牌一致，与Claude Code协同
> **标题**：Claude Design 更新：跨项目保持品牌一致，与Claude Code协同
> **原文链接**：🔗 [打开原文](https://claude.com/blog/claude-design-stays-on-brand-for-daily-work)
> **source**：AI HOT / Claude：Blog（网页）
> **kind**：`product`
> **reason**：matches topics: claude code; high-value terms: claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：6月17日，Claude Design 更新，支持跨项目使用统一设计系统，并与Claude Code同步工作流。用户可直接拖拽、对齐和缩放画布元素，编辑器稳定性大幅提升。设计系统可从GitHub、设计文件或原始上传导入，团队管理员可锁定标准系统防止篡改。新增桌面端侧边栏入口及独立网页端claude.ai/design。使用限制与聊天、Claude Cowork、Claude Code共享，每次任务消耗更少token，错误率下降。支持导出PDF、PPT，集成Adobe、Canva、Gamma等工具。发布首周用户超一百万。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 63** | The Slot-Machine Was the Point
> **标题**：The Slot-Machine Was the Point
> **原文链接**：🔗 [打开原文](https://dev.to/arthurpro/the-slot-machine-was-the-point-4fm1)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent, llm; high-value terms: agent; strong public engagement
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Lars Faye's Agentic Coding Is a Trap — published Sunday, May 3, picked up on Hacker News at 398 points and 316 comments — is the best single compendium of the cognitive-debt evidence base anyone…
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 62** | Anthropic与DeepMind CEO呼吁G7组建AI联盟排除中国
> **标题**：Anthropic与DeepMind CEO呼吁G7组建AI联盟排除中国
> **原文链接**：🔗 [打开原文](https://x.com/kimmonismus/status/2067310431669223425)
> **source**：AI HOT / X：Kim (@kimmonismus)
> **kind**：`article`
> **reason**：matches topics: anthropic, deepmind
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Dario Amodei（Anthropic）与Demis Hassabis（Google DeepMind）在G7闭门会议上呼吁组建美国主导的联盟，为人工智能制定全球规则和标准。Amodei指出，该联盟应以前沿模型和硬件（包括芯片及其他关键组件）的访问权限为手段，将中国排除在外。这一主张被评论为高技术新冷战的开端，竞争方将从根本上被剥夺参与权。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 62** | Distributed General-Purpose Agent Networks: Architecture, Key Mechanisms, and Prototypes
> **标题**：Distributed General-Purpose Agent Networks: Architecture, Key Mechanisms, and Prototypes
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17368)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17368v1 Announce Type: new Abstract: Large language models have accelerated the transition from passive conversational assistants to autonomous agents that can understand goals, plan actions, invoke tools, and execute multi-step tasks. Yet the capability of a single agent remains constra...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 62** | MemSlides: A Hierarchical Memory Driven Agent Framework for Personalized Slide Generation with Multi-turn Local Revision
> **标题**：MemSlides: A Hierarchical Memory Driven Agent Framework for Personalized Slide Generation with Multi-turn Local Revision
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17162)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17162v1 Announce Type: new Abstract: Personalized presentation generation requires more than conditioning on a current prompt or template: agents must preserve stable user preferences across tasks, retain newly introduced preferences and constraints during multi-turn revision, and carry...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 62** | ProCUA-SFT Technical Report
> **标题**：ProCUA-SFT Technical Report
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17321)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17321v1 Announce Type: new Abstract: Training computer-use agents (CUAs) -- models that interact with graphical desktops through screenshots and keyboard/mouse actions -- requires large-scale, diverse trajectory data collected in full desktop environments. The largest public resource, Ag...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 61** | Google 分享 A2UI 与 MCP Apps 三种集成架构模式
> **标题**：Google 分享 A2UI 与 MCP Apps 三种集成架构模式
> **原文链接**：🔗 [打开原文](https://developers.googleblog.com/a2ui-and-mcp-apps)
> **source**：AI HOT / Google Developers Blog（RSS）
> **kind**：`article`
> **reason**：matches topics: mcp; high-value terms: mcp
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Google 分享了三种集成 A2UI 与 MCP Apps 的架构模式，旨在结合两者优势。A2UI 采用声明式框架，通过 JSON payload 定义 UI，由宿主原生渲染，确保一致性与安全性，但受限于预定义组件库。MCP Apps 在 iframe 中使用标准 Web 技术提供自定义界面，但存在设计碎片化、性能与安全挑战。三种模式包括：通过 MCP 服务器提供 A2UI，利用 MCP Resources 或 Tool 调用传递 JSON，实现"一次编写，原生渲染"的跨平台能力；以及静态与动态交付方案。Google 正考虑扩展 MCP 以原生支持 A2UI。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 61** | 谷歌发布Agentic Resource Discovery（ARD）开放规范
> **标题**：谷歌发布Agentic Resource Discovery（ARD）开放规范
> **原文链接**：🔗 [打开原文](https://developers.googleblog.com/announcing-the-agentic-resource-discovery-specification)
> **source**：AI HOT / Google Developers Blog（RSS）
> **kind**：`article`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Agentic Resource Discovery（ARD）是一项开放规范，用于在Web上发布、发现和验证AI工具、技能与智能体。它基于两个原语：组织在其自有域名下托管catalog描述可用能力，registry作为搜索引擎索引catalog并响应发现请求。ARD支持加密验证，使客户端与端点连接前确认发布者身份，然后直接通过原生协议调用能力。Google Cloud的Gemini Enterprise Agent Platform通过Agent Registry提供企业级支持，包括URN命名、出站策略、工具固定和基于Agent Identity的信任验证。该规范现已发布，开发者可通过托管`ai-catalog.json`文件使其服务可发现。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 61** | DeusData/codebase-memory-mcp
> **标题**：DeusData/codebase-memory-mcp
> **原文链接**：🔗 [打开原文](https://github.com/DeusData/codebase-memory-mcp)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：matches topics: mcp; high-value terms: mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 61** | Panniantong/Agent-Reach
> **标题**：Panniantong/Agent-Reach
> **原文链接**：🔗 [打开原文](https://github.com/Panniantong/Agent-Reach)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 59** | LLM benchmarks are answering someone else's question
> **标题**：LLM benchmarks are answering someone else's question
> **原文链接**：🔗 [打开原文](https://danlevy.net/llm-evals-are-broken/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: llm, benchmark; high-value terms: benchmark
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 58** | Three LangGraph Rewrites: What Stateful Agents Actually Cost in Production
> **标题**：Three LangGraph Rewrites: What Stateful Agents Actually Cost in Production
> **原文链接**：🔗 [打开原文](https://dev.to/elenarevicheva/three-langgraph-rewrites-what-stateful-agents-actually-cost-in-production-25on)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Originally published on AIdeazz — cross-posted here with canonical link. For three weeks, our intake...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 58** | Why Most AI Agents Fail in Production And the Architecture Patterns That Actually Work
> **标题**：Why Most AI Agents Fail in Production And the Architecture Patterns That Actually Work
> **原文链接**：🔗 [打开原文](https://dev.to/jacobjerryarackal/why-most-ai-agents-fail-in-production-and-the-architecture-patterns-that-actually-work-dbo)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent, agents; high-value terms: agent, agents
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Building an AI agent is like the difference between reading a cookbook and actually running a busy...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 57** | FadedCantCode/Fabrica-RUNE
> **标题**：FadedCantCode/Fabrica-RUNE
> **原文链接**：🔗 [打开原文](https://github.com/FadedCantCode/Fabrica-RUNE)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, llm; high-value terms: agent
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Portable agent spec format with a divergence-risk linter.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 57** | heng1234/claude-web
> **标题**：heng1234/claude-web
> **原文链接**：🔗 [打开原文](https://github.com/heng1234/claude-web)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: claude code, llm; high-value terms: claude code
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Web UI for Claude Code CLI - token streaming, tool visualization, checkpoint, multi-session management
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 57** | mjacobs/auto-review
> **标题**：mjacobs/auto-review
> **原文链接**：🔗 [打开原文](https://github.com/mjacobs/auto-review)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent, obsidian; high-value terms: agent
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Three sibling Python tools that synthesize git diffs, agent CLI traces, and notes captures into daily Obsidian recaps.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 56** | Leaked financial docs show OpenAI is losing billions of dollars a year
> **标题**：Leaked financial docs show OpenAI is losing billions of dollars a year
> **原文链接**：🔗 [打开原文](https://arstechnica.com/ai/2026/06/leaked-financial-docs-show-openai-is-losing-billions-of-dollars-a-year/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: openai; strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：277 points | 190 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 56** | Leaked OpenAI financials show $38.5B loss and compute burn
> **标题**：Leaked OpenAI financials show $38.5B loss and compute burn
> **原文链接**：🔗 [打开原文](https://runtimewire.com/article/openai-leaked-financials-altman-compute-burn)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: openai; strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：219 points | 255 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 56** | Anthropic employees accuse Trump administration of targeting them
> **标题**：Anthropic employees accuse Trump administration of targeting them
> **原文链接**：🔗 [打开原文](https://www.nytimes.com/2026/06/17/technology/anthropic-trump-administration-fable.html)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic; strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：167 points | 180 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 55** | NarrativeWorldBench: A Frontier-Saturated Benchmark and a Latent World Model for Long-Horizon Co-Creative Audio Drama
> **标题**：NarrativeWorldBench: A Frontier-Saturated Benchmark and a Latent World Model for Long-Horizon Co-Creative Audio Drama
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17391)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm, benchmark; high-value terms: benchmark
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17391v1 Announce Type: new Abstract: Long-form serialized audio drama, with arcs that run for 200 to 800 episodes, is a major creative medium and a setting where frontier large language models (LLMs) fail. We benchmark 21 models, spanning classical, fine-tuned, open-frontier, closed-fron...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 55** | Scaling Enterprise Agent Routing: Degradation, Diagnosis, and Recovery
> **标题**：Scaling Enterprise Agent Routing: Degradation, Diagnosis, and Recovery
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17519)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, llm; high-value terms: agent
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17519v1 Announce Type: new Abstract: Production LLM assistants route user requests to growing libraries of specialized tools, but how does routing accuracy degrade as the catalog scales? We study single-step routing on a 110-agent, 584-tool catalog from a deployed enterprise productivity...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 55** | Verified Detection and Prevention of Concurrency Anomalies in Multi-Agent Large Language Model Systems
> **标题**：Verified Detection and Prevention of Concurrency Anomalies in Multi-Agent Large Language Model Systems
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17182)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent, llm; high-value terms: agent
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17182v1 Announce Type: new Abstract: Multi-agent LLM systems share state through memory stores, vector indices, and tool registries. We model such sharing as long-running read-generate-write operations under deterministic-generation semantics -- the regime durable-execution engines enfor...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 54** | Noam Shazeer 离开 Google 加入 OpenAI
> **标题**：Noam Shazeer 离开 Google 加入 OpenAI
> **原文链接**：🔗 [打开原文](https://x.com/Yuchenj_UW/status/2067401895178817999)
> **source**：AI HOT / X：Yuchen Jin (@Yuchenj_UW)
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：两年前谷歌花 27 亿美元请回的 AI 传奇 Noam Shazeer 已离开谷歌，加入 OpenAI。 对 Gemini 来说是个残酷的消息。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 54** | 泄露文件显示OpenAI年营收130亿但亏损远超收入
> **标题**：泄露文件显示OpenAI年营收130亿但亏损远超收入
> **原文链接**：🔗 [打开原文](https://arstechnica.com/ai/2026/06/leaked-financial-docs-show-openai-is-losing-billions-of-dollars-a-year)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OpenAI 2025年营收130.7亿美元（2024年37亿），但研发成本达191.8亿（含向微软支付105.9亿），收入成本（推理计算）75亿，销售营销成本57.3亿，运营亏损209.2亿。2025年净亏损约390亿，扣除约300亿一次性会计费用后约80亿。2025年3月获1220亿融资（估值8520亿）。ChatGPT周活超9亿，付费约5000万。为控制成本已关闭Sora视频模型并削减非核心业务。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 54** | 消息称 OpenAI 今年一季度现金消耗达 37 亿美元，超同期收入的一半
> **标题**：消息称 OpenAI 今年一季度现金消耗达 37 亿美元，超同期收入的一半
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/335.htm)
> **source**：AI HOT / IT之家（RSS）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OpenAI 在 2026 年第一季度现金消耗达 37 亿美元，超过同期 57 亿美元收入的一半。数据来自一份向股东披露的文件，直观体现 AI 大模型研发与规模化落地的巨额成本。OpenAI 正筹备上市，已在美国保密递交 IPO 申请，最早或于 9 月完成，估值最高可达 1 万亿美元。头部 AI 企业持续重金投入算力、模型研发与人才招募以维持竞争优势。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 54** | google-research/timesfm
> **标题**：google-research/timesfm
> **原文链接**：🔗 [打开原文](https://github.com/google-research/timesfm)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：matches topics: research
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 54** | Nexus-JPF/note-companion
> **标题**：Nexus-JPF/note-companion
> **原文链接**：🔗 [打开原文](https://github.com/Nexus-JPF/note-companion)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: obsidian; strong public engagement
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Note Companion: AI assistant for Obsidian that goes beyond just a chat. (prev File Organizer 2000)
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 54** | When Rules Learn: A Self-Evolving Agent for Legal Case Retrieval
> **标题**：When Rules Learn: A Self-Evolving Agent for Legal Case Retrieval
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17220)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent; high-value terms: agent, eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17220v1 Announce Type: new Abstract: Legal case retrieval remains challenging due to the complexity of legal language and the need for precise lexical alignment between queries and relevant cases. Although dense retrieval models have achieved notable progress, empirical studies show that...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 54** | Quantifying Consistency in LLM Logical Reasoning via Structural Uncertainty
> **标题**：Quantifying Consistency in LLM Logical Reasoning via Structural Uncertainty
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17312)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm; high-value terms: eval, reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17312v1 Announce Type: new Abstract: Large language models can arrive at the same answer through reasoning paths that are unstable, contradictory, or difficult to rank consistently -- a failure mode especially prevalent in multi-step deductive reasoning. Existing methods assess reliabili...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 53** | MolmoMotion：语言引导的3D运动预测模型
> **标题**：MolmoMotion：语言引导的3D运动预测模型
> **原文链接**：🔗 [打开原文](https://huggingface.co/blog/allenai/molmomotion)
> **source**：AI HOT / Hugging Face：Blog（RSS）
> **kind**：`model`
> **reason**：AI HOT selected item
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：MolmoMotion基于Molmo 2骨干网络，输入视频帧、物体上的3D点标记及文字动作指令（如"移动并旋转桌上放水果的木碗"），预测未来数秒内这些点的3D轨迹。提供两个变体：自回归的MolmoMotion-AR逐步预测坐标，流匹配的MolmoMotion-FM通过连续空间变换处理多可能性运动。同时发布MolmoMotion-1M数据集（含116万视频的3D点轨迹及动作描述）和PointMotionBench基准测试（2700个人工验证视频片段）。模型权重、数据集和基准测试均已开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 51** | Genoma Labs' open 14B agentic coding model trained on Kraken
> **标题**：Genoma Labs' open 14B agentic coding model trained on Kraken
> **原文链接**：🔗 [打开原文](https://huggingface.co/GenomaLabs-com/KALYPSO-v1.1L)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：3 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 51** | How I use premortems with Claude and Codex
> **标题**：How I use premortems with Claude and Codex
> **原文链接**：🔗 [打开原文](https://dev.to/pablonax/how-i-use-premortems-with-claude-and-codex-46mm)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: codex, llm; high-value terms: codex
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：I started using premortems for a boring reason: I did not trust the default review question. When I...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 50** | 用SGLang-JAX在TPU上优化Ling-2.6-1T：一个Pallas核将MoE数据移动隐藏在计算中
> **标题**：用SGLang-JAX在TPU上优化Ling-2.6-1T：一个Pallas核将MoE数据移动隐藏在计算中
> **原文链接**：🔗 [打开原文](https://www.lmsys.org/blog/2026-06-17-ling-2-6-tpu)
> **source**：AI HOT / LMSYS：Blog（Chatbot Arena 团队）
> **kind**：`paper`
> **reason**：AI HOT selected item
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：SGLang-JAX现已支持inclusionAI的Ling-2.6-1T（1T稀疏MoE，63B激活参数，256路由专家，top-8路由加共享专家）在TPU v7x上高效推理。团队开发了Fused MoE V2--一个融合scatter、专家FFN和gather的Pallas核，通过将MoE数据移动隐藏在计算中，使MoE预填充延迟从5.16ms降至2.42ms（降幅53%），解码核延迟从0.249ms降至0.211ms（降幅约15%）。仅替换MoE核即提升预填充吞吐量24.8%，解码吞吐量18.5%-35.3%。在SGLang解码基准测试中，16块TPU v7x芯片输出吞吐量达16块H200 GPU的1.29倍（mc=128）至1.77倍（mc=512）。完整上线还包含混合KV/循环内存池、GLA线性注意力和单控制器数据并行支持。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 50** | Google 医学推理 AI 系统 AMIE 新研究：从诊断迈向长期疾病管理
> **标题**：Google 医学推理 AI 系统 AMIE 新研究：从诊断迈向长期疾病管理
> **原文链接**：🔗 [打开原文](https://blog.google/innovation-and-ai/models-and-research/google-research/amie-for-disease-management-in-nature)
> **source**：AI HOT / Google Blog：AI（RSS）
> **kind**：`paper`
> **reason**：AI HOT selected item
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：今日发表在《自然》杂志上的研究展示了 Google 的医学推理 AI 系统 AMIE（Articulate Medical Intelligence Explorer）从单次诊断对话演进到长期疾病管理的能力。AMIE 利用 Gemini 模型的长上下文能力，整合共情对话智能体和深度思考管理推理智能体，可交叉引用数百页临床指南。在盲测中，AMIE 与 21 名初级保健医生相比，在整体管理推理上匹配临床医生，在计划精确性和指南一致性上得分显著更高。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 50** | iammonth1997/paperwiki-research-compiler
> **标题**：iammonth1997/paperwiki-research-compiler
> **原文链接**：🔗 [打开原文](https://github.com/iammonth1997/paperwiki-research-compiler)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: obsidian, research
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：AI Research Wiki 2026: Auto-Building Knowledge Base with Deep Citation Syntheses
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 只归档

> [!info]+ **只归档 / 49** | tfatykhov/nous
> **标题**：tfatykhov/nous
> **原文链接**：🔗 [打开原文](https://github.com/tfatykhov/nous)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：AI Agent with decisions memory at its core
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 49** | silkweave/silkweave
> **标题**：silkweave/silkweave
> **原文链接**：🔗 [打开原文](https://github.com/silkweave/silkweave)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: mcp; high-value terms: mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：Silkweave Monorepo
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 49** | JRs-Master/firebat
> **标题**：JRs-Master/firebat
> **原文链接**：🔗 [打开原文](https://github.com/JRs-Master/firebat)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: mcp; high-value terms: mcp
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：firebat
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | GitHub 发布 CC0-1.0 开源多语言仓库级数据集，覆盖 README、Issue 和 PR
> **标题**：GitHub 发布 CC0-1.0 开源多语言仓库级数据集，覆盖 README、Issue 和 PR
> **原文链接**：🔗 [打开原文](https://github.blog/ai-and-ml/github-copilot/getting-more-from-each-token-how-copilot-improves-context-handling-and-model-routing)
> **source**：AI HOT / GitHub Blog
> **kind**：`product`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：GitHub 推出一个新的仓库级数据集，采用 CC0-1.0 许可证，旨在帮助研究人员和开发者发现跨 README、Issue 和 Pull Request 的多语言开发者内容，加速多语言 AI 开发。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Claude Design与Replit联动，设计变应用
> **标题**：Claude Design与Replit联动，设计变应用
> **原文链接**：🔗 [打开原文](https://x.com/Replit/status/2067328501003497684)
> **source**：AI HOT / X：Replit (@Replit)
> **kind**：`product`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：在Claude中设计。在Replit中构建。 你现在可以将Claude Design中的设计发送到Replit，将其变成一个可工作的应用。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Google发布99美元Gemini智能音箱
> **标题**：Google发布99美元Gemini智能音箱
> **原文链接**：🔗 [打开原文](https://techcrunch.com/2026/06/17/google-bets-on-gemini-to-reinvent-the-smart-home-speaker)
> **source**：AI HOT / TechCrunch：AI（RSS）
> **kind**：`product`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Google推出首款专为Gemini打造的智能音箱Google Home Speaker，售价99.99美元。支持自然语言请求和多步指令，可在说话中途纠正，并具备连续对话功能。内置10种新声音。高级AI功能需订阅Google Home Premium（月费10美元或年费100美元），包括Gemini Live自由对话、Nest摄像头活动摘要等。即日起预售，本月发货。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Cloudflare 发布 Cloudflare One stack：智能体驱动的部署工具集
> **标题**：Cloudflare 发布 Cloudflare One stack：智能体驱动的部署工具集
> **原文链接**：🔗 [打开原文](https://blog.cloudflare.com/cloudflare-one-stack)
> **source**：AI HOT / Cloudflare Blog
> **kind**：`product`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：6月17日，Cloudflare 推出 Cloudflare One stack，一组可直接赋予 AI 智能体的技能文件，用于自动配置、部署和管理 Zero Trust 环境。工具集包含两个轻量级 skill：`cloudflare-one` 负责通用产品指导（VPN 替换、网络连接、安全策略等），`cloudflare-one-migration` 提供从 Zscaler、Palo Alto Networks 等厂商迁移的明确引导。技能内置决策树与结构化知识，智能体可自动执行云环境评估、网络拓扑生成及 Digital Experience Monitoring 排障。该 stack 基于 Cloudflare 员工数万小时客户经验提炼，降低学习与迁移门槛。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | 阿里云发布HappyOyster 1.0：一句话生成可实时交互的数字世界
> **标题**：阿里云发布HappyOyster 1.0：一句话生成可实时交互的数字世界
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/652.htm)
> **source**：AI HOT / IT之家（RSS）
> **kind**：`product`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：6月17日，阿里云发布开放式世界模型HappyOyster 1.0（快乐生蚝）。该产品基于原生多模态架构，支持多模态输入与音视频联合生成，可在生成过程中持续接收用户指令并实时响应画面。它深度学习物理世界状态转移规律，保持人物和环境长程一致性。官网开放"实时导演"与"世界探索"两种玩法：前者可随时叫停改写故事、与虚拟男友实时互动等；后者支持自由漫游、滑板冲刺、翼装滑翔、骑马奔驰、攻击打怪等交互。该产品已于今年4月16日开放内测，即日起至7月17日官网不定期掉落体验积分。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Wolfram 语言和 Mathematica 15 版发布：内置 AI 助手、符号音乐等新功能
> **标题**：Wolfram 语言和 Mathematica 15 版发布：内置 AI 助手、符号音乐等新功能
> **原文链接**：🔗 [打开原文](https://writings.stephenwolfram.com/2026/06/launching-version-15-of-wolfram-language-mathematica-built-in-useful-ai-lots-of-new-core-functionality)
> **source**：AI HOT / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`product`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：在 Mathematica 诞生近 38 年后，Wolfram 语言与 Mathematica 发布 Version 15。每个笔记本内置 AI 助手，支持从 AI 环境中直接调用 Wolfram 技术。新增符号音乐系统、大规模时间序列与事件序列处理、分类数据计算、模型拟合超函数 ModelFit。笔记本支持千兆字节级大小与实时查找，首次引入侧边栏、视觉主题及弃用功能样式。强化了表格连接、多点可视化、图形刻度绘制与轨道运行计算等功能。DSolve 拐角处获得 AI 方法辅助，支持偏微分方程曲线坐标求解。扩充了矩阵分解、多元 zeta 函数与调和数、流线型部分分式分解。强化了 WebSocket 实时连接、Python 交互改进，支持 CUDA 内核作为外部函数，Wolfram Compute Services 新增 GPU 支持。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Sixty percent of US consumers say 'AI' in brand messaging is a turnoff
> **标题**：Sixty percent of US consumers say 'AI' in brand messaging is a turnoff
> **原文链接**：🔗 [打开原文](https://wpvip.com/future-of-the-web-2026/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：998 points | 527 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Only 16 Percent of Americans Think AI Will Have a Positive Impact on Society
> **标题**：Only 16 Percent of Americans Think AI Will Have a Positive Impact on Society
> **原文链接**：🔗 [打开原文](https://techcrunch.com/2026/06/17/only-16-percent-of-americans-think-ai-will-have-a-positive-impact-on-society-a-new-study-shows/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：365 points | 441 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | AI demands more engineering discipline. Not less
> **标题**：AI demands more engineering discipline. Not less
> **原文链接**：🔗 [打开原文](https://charitydotwtf.substack.com/p/ai-demands-more-engineering-discipline)
> **source**：Hacker News
> **kind**：`community`
> **reason**：strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：341 points | 165 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | The founder's playbook: Building an AI-native startup
> **标题**：The founder's playbook: Building an AI-native startup
> **原文链接**：🔗 [打开原文](https://claude.com/blog/the-founders-playbook)
> **source**：Hacker News
> **kind**：`community`
> **reason**：strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：212 points | 152 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Launch HN: Adam (YC W25) – Open-Source AI CAD
> **标题**：Launch HN: Adam (YC W25) – Open-Source AI CAD
> **原文链接**：🔗 [打开原文](https://github.com/Adam-CAD/CADAM)
> **source**：Hacker News
> **kind**：`community`
> **reason**：strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：151 points | 79 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | The Competitive Moat That AI Can't Replicate
> **标题**：The Competitive Moat That AI Can't Replicate
> **原文链接**：🔗 [打开原文](https://ghostinthedata.info/posts/2026/2026-06-13-human-connection-moat/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：strong public engagement
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：116 points | 96 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Anthropic Public Record
> **标题**：Anthropic Public Record
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/anthropic-public-record)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Claude Corps
> **标题**：Claude Corps
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/claude-corps)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Claude Fable 5 Mythos 5
> **标题**：Claude Fable 5 Mythos 5
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/claude-fable-5-mythos-5)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Core Views On Ai Safety
> **标题**：Core Views On Ai Safety
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/core-views-on-ai-safety)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Developing Nuclear Safeguards For Ai Through Public Private Partnership
> **标题**：Developing Nuclear Safeguards For Ai Through Public Private Partnership
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/developing-nuclear-safeguards-for-ai-through-public-private-partnership)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Dxc Anthropic Alliance
> **标题**：Dxc Anthropic Alliance
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/dxc-anthropic-alliance)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Fable Mythos Access
> **标题**：Fable Mythos Access
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/fable-mythos-access)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Seoul Office Partnerships Korean Ai Ecosystem
> **标题**：Seoul Office Partnerships Korean Ai Ecosystem
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/seoul-office-partnerships-korean-ai-ecosystem)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Tcs Anthropic Partnership
> **标题**：Tcs Anthropic Partnership
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/tcs-anthropic-partnership)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Attack Navigator
> **标题**：Attack Navigator
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/research/attack-navigator)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Biorisk
> **标题**：Biorisk
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/research/biorisk)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 48** | Building Ai Cyber Defenders
> **标题**：Building Ai Cyber Defenders
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/research/building-ai-cyber-defenders)
> **source**：Anthropic
> **kind**：`article`
> **reason**：matches topics: anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | Beyond Parallel Sampling: Diverse Query Initialization for Agentic Search
> **标题**：Beyond Parallel Sampling: Diverse Query Initialization for Agentic Search
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17209)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17209v1 Announce Type: new Abstract: Test-time scaling for agentic search typically increases depth (i.e., more turns and tokens per trajectory) or breadth (i.e., more parallel rollouts). Here we focus on breadth scaling, showing that standard parallel sampling yields diminishing returns...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | SkillChain-Gym: A Benchmark for Reskilling-Aware Production-Inventory Control under Disruptions
> **标题**：SkillChain-Gym: A Benchmark for Reskilling-Aware Production-Inventory Control under Disruptions
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17266)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: benchmark; high-value terms: benchmark
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17266v1 Announce Type: new Abstract: Production planning increasingly has to treat workforce capability as a decision variable: certifications lapse when skills are not maintained, new products require skills the current workforce does not hold, and reskilling competes for the same worke...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | SpeechDx: A Multi-Task Benchmark for Clinical Speech AI
> **标题**：SpeechDx: A Multi-Task Benchmark for Clinical Speech AI
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17339)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: benchmark; high-value terms: benchmark
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17339v1 Announce Type: new Abstract: Speech offers a uniquely informative window into health by simultaneously engaging neurological, motor, respiratory, and vocal systems. Current clinical speech AI methods have largely progressed through isolated condition-specific studies, making resu...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | Dissecting model behavior through agent trajectories
> **标题**：Dissecting model behavior through agent trajectories
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17454)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17454v1 Announce Type: new Abstract: AI agent performance is not just a modeling problem, it is fundamentally a systems problem. The advanced capabilities of models are realized through agent harnesses. Therefore, a gap between model assumptions and harness behavior can easily prevent th...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | PromptMN: Pseudo Prompting Language
> **标题**：PromptMN: Pseudo Prompting Language
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17164)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17164v1 Announce Type: new Abstract: Prompting has become the primary interface between humans and generative AI, yet many natural language prompts remain fragile: roles, goals, constraints, and expected outputs are often buried in prose or left implicit. In agentic and software developm...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | From Parasocial Scripts to Dyadic Persistence in Autonomous AI-Agent Communities
> **标题**：From Parasocial Scripts to Dyadic Persistence in Autonomous AI-Agent Communities
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17174)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17174v1 Announce Type: new Abstract: While parasocial interactions (PSIs) and parasocial relationships (PSRs) have been studied in conventional media settings, we investigate whether PSI- (colloquial) relational cues also exist in online communities where both sides are autonomous AI age...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | Speaking in Self-Assessing Tongues: On the Verbalized Confidence of LLMs in Machine Translation
> **标题**：Speaking in Self-Assessing Tongues: On the Verbalized Confidence of LLMs in Machine Translation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17234)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm; high-value terms: api
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17234v1 Announce Type: new Abstract: The rapid rise in popularity of large language models (LLMs) for translation calls for a thorough study of the reliability of their confidence in their own outputs. Unlike many generation tasks, translation errors and confidence levels can be useful a...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | MLLP-VRAIN UPV system for the IWSLT 2026 Simultaneous Speech Translation task
> **标题**：MLLP-VRAIN UPV system for the IWSLT 2026 Simultaneous Speech Translation task
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17255)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: research; high-value terms: release
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17255v1 Announce Type: new Abstract: This work describes the participation of the MLLP-VRAIN research group in the shared task of the IWSLT 2026 Simultaneous Speech Translation track. Our submission utilizes the recently released Parakeet and Qwen 3.5 models to create a robust, cascaded...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | Translating the Untranslatable: An Operationalizable Ontology for Untranslatability
> **标题**：Translating the Untranslatable: An Operationalizable Ontology for Untranslatability
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17354)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: benchmark; high-value terms: benchmark
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17354v1 Announce Type: new Abstract: Untranslatability, cases where meaning cannot be directly preserved across languages, is well-studied in linguistics but underexplored in NLP. As machine translation (MT) systems improve on standard benchmarks, their limitations increasingly concentra...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | AIPatient Arena: EHR-grounded evaluation of large language models in end-to-end clinical consultation workflows
> **标题**：AIPatient Arena: EHR-grounded evaluation of large language models in end-to-end clinical consultation workflows
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17474)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm; high-value terms: eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17474v1 Announce Type: new Abstract: Large language models (LLMs) are increasingly considered for use in clinical consultation tasks, yet most medical evaluations remain static, single-turn, or narrowly outcome-based, limiting their ability to reflect the sequential, uncertain, and inter...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | Decoding Hidden Deception in Reasoning LLMs: Activation Explainers for Deception Auditing
> **标题**：Decoding Hidden Deception in Reasoning LLMs: Activation Explainers for Deception Auditing
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17478)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm; high-value terms: reasoning
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17478v1 Announce Type: new Abstract: As LLMs acquire stronger reasoning capabilities, deceptive behavior becomes an increasingly serious safety concern. Existing deception monitors either score visible transcripts or derive scalar probe scores from representation vectors, leaving little...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 47** | Evaluating Second-Order Bias of LLMs Through Epistemic Entitlement
> **标题**：Evaluating Second-Order Bias of LLMs Through Epistemic Entitlement
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17506)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm; high-value terms: eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17506v1 Announce Type: new Abstract: Evaluations of social bias in LLMs largely focus on whether models generate or imply biased content. However, as LLMs are increasingly used as judges of bias, they may exhibit social biases in subtler ways in how they evaluate biased content, which cu...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | 上交所发布指引：AI大模型企业可适用科创板第五套上市标准
> **标题**：上交所发布指引：AI大模型企业可适用科创板第五套上市标准
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/735.htm)
> **source**：AI HOT / IT之家（RSS）
> **kind**：`article`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：上海证券交易所6月17日发布指引，支持尚未形成稳定收入的优质人工智能大模型企业通过科创板第五套上市标准发行上市。申报企业需在行业地位、产业链优势、目标市场需求、研发进度及关键指标方面具备突出竞争力。指引明确，申报时至少有一个大模型产品已完成上线发布并实现规模化应用，以验证商业模式可行性。下一步，上交所将在中国证监会指导下推进符合标准的企业上市。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | 库克：AI 浪潮引发存储芯片价格暴涨，iPhone 等苹果产品涨价已"不可避免"
> **标题**：库克：AI 浪潮引发存储芯片价格暴涨，iPhone 等苹果产品涨价已"不可避免"
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/694.htm)
> **source**：AI HOT / IT之家（RSS）
> **kind**：`article`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：苹果CEO库克确认，AI热潮导致存储芯片严重短缺和价格暴涨，苹果产品涨价已"不可避免"。库克未透露涨价具体细节。华尔街日报指出，全球AI巨头大幅增加资本开支，高带宽内存需求激增，挤压消费电子芯片供应。自2024年以来内存和存储芯片价格已翻四倍，涨势预计延续至2027年。研究机构估算，下一代iPhone 18 Pro售价或需增加约270美元。苹果已在上月提高Mac Mini起售价。摩根士丹利预测，今年美国智能手机和PC价格将上涨15%。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | Matt Pocock 开源 skills v1：将技能描述 Token 成本降低 63%
> **标题**：Matt Pocock 开源 skills v1：将技能描述 Token 成本降低 63%
> **原文链接**：🔗 [打开原文](https://x.com/AYi_AInotes/status/2067327021005656135)
> **source**：AI HOT / X：阿易 AI Notes (@AYi_AInotes)
> **kind**：`article`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Matt Pocock（Total TypeScript 作者）开源了 skills v1，将技能描述的 Token 成本降低 63%。该工具包将技能分为模型可调用和用户可调用，新增 /codebase-design、/domain-modeling、/grilling 三项技能；重写 /writing-great-skills；将 /diagnose 更新为 /diagnosing-bugs 并改为模型可调用；新增 /ask-matt 路由技能，帮助 AI 自动判断时机触发合适工程流程。主推文评价其将 prompt 从咒语拆解为纪律性流程。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | 中国加紧筹建世界人工智能合作组织
> **标题**：中国加紧筹建世界人工智能合作组织
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/965/248.htm)
> **source**：AI HOT / IT之家（RSS）
> **kind**：`article`
> **reason**：AI HOT selected item
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：中国正加紧筹建世界人工智能合作组织，欢迎各方加入。2025年7月26日，中国政府倡议成立该组织，作为践行多边主义、推动共商共建共享全球治理的举措，旨在弥合数字和智能鸿沟、促进人工智能向善普惠发展。初步考虑总部设在上海。同日，2025世界人工智能大会发表《人工智能全球治理行动计划》，呼吁各方遵循向善为民、尊重主权、发展导向、安全可控、公平普惠、开放合作的原则，协力推进全球人工智能发展与治理。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | n0-computer/iroh
> **标题**：n0-computer/iroh
> **原文链接**：🔗 [打开原文](https://github.com/n0-computer/iroh)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | meshery/meshery
> **标题**：meshery/meshery
> **原文链接**：🔗 [打开原文](https://github.com/meshery/meshery)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | obra/superpowers
> **标题**：obra/superpowers
> **原文链接**：🔗 [打开原文](https://github.com/obra/superpowers)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | RocketChat/Rocket.Chat
> **标题**：RocketChat/Rocket.Chat
> **原文链接**：🔗 [打开原文](https://github.com/RocketChat/Rocket.Chat)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | continuedev/continue
> **标题**：continuedev/continue
> **原文链接**：🔗 [打开原文](https://github.com/continuedev/continue)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | calesthio/OpenMontage
> **标题**：calesthio/OpenMontage
> **原文链接**：🔗 [打开原文](https://github.com/calesthio/OpenMontage)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | alexzhang13/rlm
> **标题**：alexzhang13/rlm
> **原文链接**：🔗 [打开原文](https://github.com/alexzhang13/rlm)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | OpenBMB/VoxCPM
> **标题**：OpenBMB/VoxCPM
> **原文链接**：🔗 [打开原文](https://github.com/OpenBMB/VoxCPM)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | music-assistant/server
> **标题**：music-assistant/server
> **原文链接**：🔗 [打开原文](https://github.com/music-assistant/server)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | PaddlePaddle/PaddleOCR
> **标题**：PaddlePaddle/PaddleOCR
> **原文链接**：🔗 [打开原文](https://github.com/PaddlePaddle/PaddleOCR)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | roboflow/rf-detr
> **标题**：roboflow/rf-detr
> **原文链接**：🔗 [打开原文](https://github.com/roboflow/rf-detr)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | freeCodeCamp/freeCodeCamp
> **标题**：freeCodeCamp/freeCodeCamp
> **原文链接**：🔗 [打开原文](https://github.com/freeCodeCamp/freeCodeCamp)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | bytedance/UI-TARS-desktop
> **标题**：bytedance/UI-TARS-desktop
> **原文链接**：🔗 [打开原文](https://github.com/bytedance/UI-TARS-desktop)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | makeplane/plane
> **标题**：makeplane/plane
> **原文链接**：🔗 [打开原文](https://github.com/makeplane/plane)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | fluxerapp/fluxer
> **标题**：fluxerapp/fluxer
> **原文链接**：🔗 [打开原文](https://github.com/fluxerapp/fluxer)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 46** | earendil-works/pi
> **标题**：earendil-works/pi
> **原文链接**：🔗 [打开原文](https://github.com/earendil-works/pi)
> **source**：GitHub Trending
> **kind**：`github_repo`
> **reason**：baseline source relevance
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 45** | MCP 2000
> **标题**：MCP 2000
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/mcp-2000)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：matches topics: mcp; high-value terms: mcp
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | The hacker sent by Anthropic to calm the government's nerves about AI safety
> **标题**：The hacker sent by Anthropic to calm the government's nerves about AI safety
> **原文链接**：🔗 [打开原文](https://www.wsj.com/tech/ai/anthropic-mythos-safety-nicholas-carlini-20bceaa3)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：70 points | 77 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Show HN: ML condenses billions of logs into a tiny snapshot your LLM can debug
> **标题**：Show HN: ML condenses billions of logs into a tiny snapshot your LLM can debug
> **原文链接**：🔗 [打开原文](https://github.com/Rocketgraph/rocketgraph)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：8 points | 2 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Jagged Intelligence: The Dangerous Unknowns at the Heart of LLMs
> **标题**：Jagged Intelligence: The Dangerous Unknowns at the Heart of LLMs
> **原文链接**：🔗 [打开原文](https://yalereview.org/article/melanie-mitchell-jagged-intelligence)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：4 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Show HN: Models Pie – LLMs ranked by fast / cheap / good tradeoffs
> **标题**：Show HN: Models Pie – LLMs ranked by fast / cheap / good tradeoffs
> **原文链接**：🔗 [打开原文](https://modelspie.com/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：4 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | France's OVHcloud plans frontier AI models to become Europe's second LLM player
> **标题**：France's OVHcloud plans frontier AI models to become Europe's second LLM player
> **原文链接**：🔗 [打开原文](https://www.reuters.com/world/asia-pacific/frances-ovhcloud-plans-frontier-ai-models-become-europes-second-llm-player-2026-06-17/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：3 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Ask HN: How are you managing context loss when switching LLMs to save tokens?
> **标题**：Ask HN: How are you managing context loss when switching LLMs to save tokens?
> **原文链接**：🔗 [打开原文](https://news.ycombinator.com/item?id=48574135)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：3 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | LLM Chess – Leaderboard
> **标题**：LLM Chess – Leaderboard
> **原文链接**：🔗 [打开原文](https://maxim-saplin.github.io/llm_chess/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：3 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Noam Shazeer is joining OpenAI
> **标题**：Noam Shazeer is joining OpenAI
> **原文链接**：🔗 [打开原文](https://www.reuters.com/technology/googles-gemini-co-lead-noam-shazeer-join-openai-2026-06-18/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: openai
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：13 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | OpenAI's financials have leaked, showing $21B in losses against $13B in revenue
> **标题**：OpenAI's financials have leaked, showing $21B in losses against $13B in revenue
> **原文链接**：🔗 [打开原文](https://fortune.com/2026/06/16/openai-financials-leaked-losses-revenue-profit/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: openai
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：12 points | 5 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Noam Shazeer Joins OpenAI
> **标题**：Noam Shazeer Joins OpenAI
> **原文链接**：🔗 [打开原文](https://twitter.com/NoamShazeer/status/2067400851438932297)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: openai
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：8 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | OpenAI's leaked financials reveal soaring losses as it prepares to go public
> **标题**：OpenAI's leaked financials reveal soaring losses as it prepares to go public
> **原文链接**：🔗 [打开原文](https://groups.google.com/a/netflix.com/g/ios-ui-kickoffs/c/772e4-hycBE)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: openai
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 points | 2 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | OpenAI spending hit $34B last year ahead of planned IPO, $21B losses
> **标题**：OpenAI spending hit $34B last year ahead of planned IPO, $21B losses
> **原文链接**：🔗 [打开原文](https://www.ft.com/content/e15b0d7e-ff6b-4f16-ba7a-4068feddb828)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: openai
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | The Reason Anthropic's Models Are Offline: A Six-Year-Old Trump Grudge
> **标题**：The Reason Anthropic's Models Are Offline: A Six-Year-Old Trump Grudge
> **原文链接**：🔗 [打开原文](https://www.techdirt.com/2026/06/16/apparently-the-real-reason-anthropics-models-are-offline-a-six-year-old-trump-grudge/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：9 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | The White House Wants Anthropic to Block All Jailbreaks. It May Not Be Possible
> **标题**：The White House Wants Anthropic to Block All Jailbreaks. It May Not Be Possible
> **原文链接**：🔗 [打开原文](https://www.wired.com/story/the-white-house-wants-anthropic-to-block-all-jailbreaks-that-may-not-be-possible/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：7 points | 2 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Read the Lutnick Letter That Led Anthropic to Disable Mythos
> **标题**：Read the Lutnick Letter That Led Anthropic to Disable Mythos
> **原文链接**：🔗 [打开原文](https://www.bloomberg.com/news/articles/2026-06-16/read-the-lutnick-letter-that-led-anthropic-to-disable-mythos)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：6 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Anthropic lost the White House's trust – and then its flagship product
> **标题**：Anthropic lost the White House's trust – and then its flagship product
> **原文链接**：🔗 [打开原文](https://www.washingtonpost.com/technology/2026/06/15/how-anthropic-lost-white-houses-trust-then-its-flagship-product/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 points | 2 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Anthropic's latest feud with the admin may help it, sales data suggests
> **标题**：Anthropic's latest feud with the admin may help it, sales data suggests
> **原文链接**：🔗 [打开原文](https://techcrunch.com/2026/06/16/anthropics-latest-feud-with-the-trump-admin-may-actually-help-it-sales-data-suggests/)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | The Mind of Anthropic CEO Dario Amodei [Extended Interview] [video]
> **标题**：The Mind of Anthropic CEO Dario Amodei [Extended Interview] [video]
> **原文链接**：🔗 [打开原文](https://www.youtube.com/watch?v=x2VHFgyawPE)
> **source**：Hacker News
> **kind**：`community`
> **reason**：matches topics: anthropic
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：3 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 44** | Claude AI by Anthropic: Key Features That Set This Model Apart in 2024 [EN]
> **标题**：Claude AI by Anthropic: Key Features That Set This Model Apart in 2024 [EN]
> **原文链接**：🔗 [打开原文](https://dev.to/andr_diasmoreiraprol_b/claude-ai-by-anthropic-key-features-that-set-this-model-apart-in-2024-en-4ac0)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: anthropic, llm
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：When I first started integrating large language models into enterprise workflows over a decade into...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 43** | Gotta Earn 'Em All: The Gym Badges of Agentic Engineering (Part 1)
> **标题**：Gotta Earn 'Em All: The Gym Badges of Agentic Engineering (Part 1)
> **原文链接**：🔗 [打开原文](https://dev.to/kaleman15/gotta-earn-em-all-the-gym-badges-of-agentic-engineering-part-1-5bff)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：There's a guy who stands at the entrance to the Indigo Plateau and will not let you through. Level 80...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 43** | Building a Hermes Memory Plugin for a Voice-Powered Conference Agent with Weaviate Engram🧠
> **标题**：Building a Hermes Memory Plugin for a Voice-Powered Conference Agent with Weaviate Engram🧠
> **原文链接**：🔗 [打开原文](https://dev.to/astrodevil/building-a-hermes-memory-plugin-for-a-voice-powered-conference-agent-with-weaviate-engram-39jj)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Introduction Recently, I have been attending a lot of conferences where different booths...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 43** | How to build a coding agent — Part 1.1
> **标题**：How to build a coding agent — Part 1.1
> **原文链接**：🔗 [打开原文](https://dev.to/ramunarasinga-11/how-to-build-a-coding-agent-part-11-4mel)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：In this tutorial series, we build a coding agent that you can assign tasks on GitHub. In this part...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 42** | lbliii/bengal
> **标题**：lbliii/bengal
> **原文链接**：🔗 [打开原文](https://github.com/lbliii/bengal)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: llm
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：ᓚᘏᗢ Bengal — High-performance static site generator for Python 3.14+ with parallel builds, autodoc, and content validation
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 42** | stavarengo/folia-kanban
> **标题**：stavarengo/folia-kanban
> **原文链接**：🔗 [打开原文](https://github.com/stavarengo/folia-kanban)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: obsidian
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A Kanban board for Obsidian where every card is a plain Markdown file.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 42** | zihui7896/AI-learning-notes
> **标题**：zihui7896/AI-learning-notes
> **原文链接**：🔗 [打开原文](https://github.com/zihui7896/AI-learning-notes)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: obsidian
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：用 AI 学习并用 Obsidian 记录，最后整理用 hugo 构建
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 42** | bluejfox/feishu-obsidian-sync
> **标题**：bluejfox/feishu-obsidian-sync
> **原文链接**：🔗 [打开原文](https://github.com/bluejfox/feishu-obsidian-sync)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：matches topics: obsidian
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | Incumbent Advantage: Brand Bias and Cognitive Manipulation Dynamics in LLM Recommendation Systems
> **标题**：Incumbent Advantage: Brand Bias and Cognitive Manipulation Dynamics in LLM Recommendation Systems
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17443)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17443v1 Announce Type: new Abstract: Large language models (LLMs) are becoming a major way for consumers to find products, but we do not yet understand how brands compete in this new channel. We study brand dynamics in LLM recommendations using skincare products -- a category where consu...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | LLM-as-Judge in Education: A Curriculum-Grounded Marking Pipeline
> **标题**：LLM-as-Judge in Education: A Curriculum-Grounded Marking Pipeline
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17507)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17507v1 Announce Type: new Abstract: Generative AI and large language models (LLMs) are increasingly applied to question generation and automated assessment. However, deploying LLMs in preparation for high-stakes exams requires more than prompt engineering; it demands software pipelines...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | RepSelect: Robust LLM Unlearning via Representation Selectivity
> **标题**：RepSelect: Robust LLM Unlearning via Representation Selectivity
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17168)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17168v1 Announce Type: new Abstract: Making large language models (LLMs) deeply forget specific knowledge and values without sacrificing general capabilities remains a central challenge in unlearning. However, current methods are easily reversed by fine-tuning or few-shot prompting, sugg...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | Revisiting LLM Adaptation for 3D CT Report Generation: A Study of Scaling and Diagnostic Priors
> **标题**：Revisiting LLM Adaptation for 3D CT Report Generation: A Study of Scaling and Diagnostic Priors
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17213)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17213v1 Announce Type: new Abstract: Recent advances in multimodal learning, including large language models (LLMs) and vision-language models (VLMs), have demonstrated strong adaptability to natural images. However, extending their use to the medical domain, particularly for volumetric...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | Are you speaking my languages? On spoken language adherence in multimodal LLMs
> **标题**：Are you speaking my languages? On spoken language adherence in multimodal LLMs
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17281)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17281v1 Announce Type: new Abstract: While Large Language Model (LLM) based Automatic Speech Recognition (ASR) enables seamless multilingual use, models often misidentify the output language, compromising transcription fidelity and downstream application quality. To preserve flexibility...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | Do Large Language Models Always Tell The Same Stories?
> **标题**：Do Large Language Models Always Tell The Same Stories?
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17350)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17350v1 Announce Type: new Abstract: Recent advances in large language models (LLMs) have enabled the generation of high-quality prose, yet the question of whether these models are capable of generating diverse outputs remains contested. In this work, we investigate the diversity of LLM-...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | Correct When Paired, Wrong When Split: Decoupling and Editing Modality-Specific Neurons in MLLMs
> **标题**：Correct When Paired, Wrong When Split: Decoupling and Editing Modality-Specific Neurons in MLLMs
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17057)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17057v1 Announce Type: new Abstract: Although Knowledge Editing provides an efficient mechanism for updating the knowledge of Multimodal Large Language Models (MLLMs), we find that current paradigms still suffer from an important yet remain underexplored issue : editing decoupling failur...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 40** | MODE: Modality-Decomposed Expert-Level Mixed-Precision Quantization for MoE Multimodal LLMs
> **标题**：MODE: Modality-Decomposed Expert-Level Mixed-Precision Quantization for MoE Multimodal LLMs
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17118)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：matches topics: llm
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17118v1 Announce Type: new Abstract: Mixture-of-Experts Multimodal Large Language Models (MoE-MLLMs) offer remarkable performance but incur prohibitive GPU memory costs, making compression essential. Among PTQ methods, expert-level mixed-precision quantization has proven effective for Mo...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 39** | DeepInsight: A Unified Evaluation Infrastructure Across the Physical AI Stack
> **标题**：DeepInsight: A Unified Evaluation Infrastructure Across the Physical AI Stack
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17574)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：high-value terms: eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17574v1 Announce Type: new Abstract: Evaluating a Physical AI stack spans operators that differ by more than three orders of magnitude -- from a single foundation-model decoding step to thousands of physics ticks of whole-body control -- varying orthogonally in modality, reward semantics...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 39** | Self-Generated Error Training for Token Editing in Diffusion Language Models
> **标题**：Self-Generated Error Training for Token Editing in Diffusion Language Models
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17175)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：high-value terms: release
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17175v1 Announce Type: new Abstract: Token-to-token (T2T) editing lets LLaDA2.1 revise committed tokens during block-diffusion decoding. The released recipe trains this editor on random vocabulary corruptions, but at inference the editor sees the model's own fluent, high-confidence draft...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 39** | MODE-RAG: Manifold Outlier Diagnosis and Energy-based Retrieval-Augmented Generation Evaluation
> **标题**：MODE-RAG: Manifold Outlier Diagnosis and Energy-based Retrieval-Augmented Generation Evaluation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17449)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：high-value terms: eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17449v1 Announce Type: new Abstract: While Multimodal Retrieval-Augmented Generation (M-RAG) enhances Large Vision-Language Models, it remains highly susceptible to cross-modal hallucinations, causal fabrications, and sycophancy. Furthermore, existing mitigation pipelines often face an i...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 39** | Probing, Fusion, and Trustworthiness: A Systematic Evaluation of Foundation Model Representations for Multimodal Cancer Analysis
> **标题**：Probing, Fusion, and Trustworthiness: A Systematic Evaluation of Foundation Model Representations for Multimodal Cancer Analysis
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17115)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：high-value terms: eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17115v1 Announce Type: new Abstract: Foundation models (FMs) have emerged as powerful representation extractors for medical data, yet their generalizability to datasets under distribution shift remains underexplored. This work systematically evaluates FM-based representations on a suite...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 39** | Rift: A Conflict Signature for Deception in Language Models
> **标题**：Rift: A Conflict Signature for Deception in Language Models
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17229)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：high-value terms: eval
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17229v1 Announce Type: new Abstract: A model that lies while knowing the truth is the central case ELK cannot handle with behavioral evaluation alone. We ask whether such deception leaves an internal signature distinguishing it from honest error. Our key move is a control for wrongness:...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Show HN: I built 184 free browser tools – PDF, image, dev, AI tasks, no upload
> **标题**：Show HN: I built 184 free browser tools – PDF, image, dev, AI tasks, no upload
> **原文链接**：🔗 [打开原文](https://brevio.pro)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 83 points, 28 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：83 points | 28 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Compiles any HuggingFace model into a single persistent megakernel
> **标题**：Compiles any HuggingFace model into a single persistent megakernel
> **原文链接**：🔗 [打开原文](https://twitter.com/Akashi203/status/2067379010762338681)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 2 points, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：2 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Show HN: Selora – local model for Home Assistant
> **标题**：Show HN: Selora – local model for Home Assistant
> **原文链接**：🔗 [打开原文](https://github.com/SeloraHomes/ha-selora-ai)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 6 points, 4 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：6 points | 4 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Gemma 4 E2B running in-browser at 255 tok/s
> **标题**：Gemma 4 E2B running in-browser at 255 tok/s
> **原文链接**：🔗 [打开原文](https://huggingface.co/spaces/webml-community/gemma-4-webgpu-kernels)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 11 points, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：11 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | GLM-5.2: Built for Long-Horizon Tasks
> **标题**：GLM-5.2: Built for Long-Horizon Tasks
> **原文链接**：🔗 [打开原文](https://huggingface.co/blog/zai-org/glm-52-blog)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 9 points, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：9 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | GLM-5.2
> **标题**：GLM-5.2
> **原文链接**：🔗 [打开原文](https://huggingface.co/zai-org/GLM-5.2)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 8 points, 1 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：8 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | PP-OCRv6
> **标题**：PP-OCRv6
> **原文链接**：🔗 [打开原文](https://huggingface.co/collections/PaddlePaddle/pp-ocrv6)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 5 points, 1 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 points | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Model Card: unsloth/GLM-5.2-GGUF
> **标题**：Model Card: unsloth/GLM-5.2-GGUF
> **原文链接**：🔗 [打开原文](https://huggingface.co/unsloth/GLM-5.2-GGUF)
> **source**：Hacker News
> **kind**：`community`
> **reason**：HN engagement: 2 points, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：2 points | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | How much VRAM do you actually need to run Llama 3 or Gemma locally?
> **标题**：How much VRAM do you actually need to run Llama 3 or Gemma locally?
> **原文链接**：🔗 [打开原文](https://dev.to/sathvic_kollu/how-much-vram-do-you-actually-need-to-run-llama-3-or-gemma-locally-3heg)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: llm
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Every few days someone in a local LLM thread asks the same question: "will this run on my 3060?" And...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Stateful provider fallback for LLM pipelines: an FSM pattern
> **标题**：Stateful provider fallback for LLM pipelines: an FSM pattern
> **原文链接**：🔗 [打开原文](https://dev.to/ale007xd/stateful-provider-fallback-for-llm-pipelines-an-fsm-pattern-48ak)
> **source**：Dev.to
> **kind**：`article`
> **reason**：matches topics: llm
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Gateway-level LLM fallback (LiteLLM, Bifrost, Kong AI Gateway) operates on individual HTTP requests....
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Why adding ontologies to LLMs won't yield machine intelligence
> **标题**：Why adding ontologies to LLMs won't yield machine intelligence
> **原文链接**：🔗 [打开原文](https://youtu.be/Ce-cN5Llaz4?t=93)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：1 score | 3 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Building llm-driven “ai” still requires domain knowledge
> **标题**：Building llm-driven “ai” still requires domain knowledge
> **原文链接**：🔗 [打开原文](https://lobste.rs/s/q9sd1m/building_llm_driven_ai_still_requires)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：0 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 36** | Language integrated LLMs as an OCaml function
> **标题**：Language integrated LLMs as an OCaml function
> **原文链接**：🔗 [打开原文](https://anil.recoil.org/notes/language-integrated-llms)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：matches topics: llm
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：4 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 35** | AI Built My UI in 2 Hours. Then I Spent 3 Weeks Fixing It.
> **标题**：AI Built My UI in 2 Hours. Then I Spent 3 Weeks Fixing It.
> **原文链接**：🔗 [打开原文](https://dev.to/xu_xu_b2179aa8fc958d531d1/ai-built-my-ui-in-2-hours-then-i-spent-3-weeks-fixing-it-4n5f)
> **source**：Dev.to
> **kind**：`article`
> **reason**：high-value terms: api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：The PR had 47 changed files. Three new React components, two API routes, a context provider, and what...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 35** | r4b1t_h0l3
> **标题**：r4b1t_h0l3
> **原文链接**：🔗 [打开原文](https://dev.to/gnomeman4201/r4b1th0l3-5aa3)
> **source**：Dev.to
> **kind**：`article`
> **reason**：high-value terms: security
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：→ Try it: gnomeman4201.github.io/r4b1t It's a curated random link generator for security and OSINT...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 35** | June Framework Memory and storage pricing updates
> **标题**：June Framework Memory and storage pricing updates
> **原文链接**：🔗 [打开原文](https://frame.work/ca/en/blog/updates-on-memory-pricing-and-navigating-the-volatile-memory-market)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：high-value terms: pricing
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 34** | sh1nj1/plan42
> **标题**：sh1nj1/plan42
> **原文链接**：🔗 [打开原文](https://github.com/sh1nj1/plan42)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：10 stars | pushed 2026-06-18
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：A tracker of your creatives. Your creativeness is coming!
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 34** | lllyys/vreader
> **标题**：lllyys/vreader
> **原文链接**：🔗 [打开原文](https://github.com/lllyys/vreader)
> **source**：GitHub Search
> **kind**：`github_repo`
> **reason**：17 stars | pushed 2026-06-18
> **follow_up**：查看 README、最近 release 和 issue，判断是否加入工具评估清单。
> **summary**：iOS reader app for EPUB, PDF, TXT, and Markdown — with AI assistant, annotations, and full-text search. Built with Swift 6, SwiftUI, and SwiftData.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Skill-Constrained Model Predictive Control for Resilient Manufacturing Supply Chains
> **标题**：Skill-Constrained Model Predictive Control for Resilient Manufacturing Supply Chains
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17269)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17269v1 Announce Type: new Abstract: In skill-constrained production-inventory systems, the qualified human capacity available tomorrow depends on training decisions made today: production requires certified workers, certifications decay unless maintained, and training consumes the same...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Nothing from Something: Can a Language Model Discover 0?
> **标题**：Nothing from Something: Can a Language Model Discover 0?
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17289)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17289v1 Announce Type: new Abstract: AI systems based on artificial neural networks are being developed with aspirations of pushing the boundary of human mathematical knowledge. A key question for these systems is how much they can reach beyond their training data. Mathematical discovery...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Treatment Response Optimized Clinical Decision Support AI System via Digital Twin Simulation
> **标题**：Treatment Response Optimized Clinical Decision Support AI System via Digital Twin Simulation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17405)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17405v1 Announce Type: new Abstract: Clinical decision support AI systems (CDSASs) must adapt to evolving patient conditions in real-time while adhering to strict safety constraints. We present an online adaptive framework that integrates Treatment Effect (TE) estimation to quantify clin...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | A Machine-Learned Comorbidity Index
> **标题**：A Machine-Learned Comorbidity Index
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17450)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17450v1 Announce Type: new Abstract: Traditional comorbidity scores (e.g., Charlson and Elixhauser) are widely used for risk adjustment and patient stratification, but they have two key limitations: (i) they are largely mortality-centric and do not align well with other clinical outcomes...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Surrogate Assisted Pedestrian Protection Design via a Foundation Model Orchestrated Workflow
> **标题**：Surrogate Assisted Pedestrian Protection Design via a Foundation Model Orchestrated Workflow
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17577)
> **source**：cs.AI updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17577v1 Announce Type: new Abstract: AI-driven engineering workflows face particular challenges in crash safety design: unlike aerodynamics, crash events involve highly nonlinear contact dynamics, material nonlinearity, and discrete state transitions that are difficult to capture with da...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Examining the Limits of Word2Vec with Toki Pona
> **标题**：Examining the Limits of Word2Vec with Toki Pona
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17299)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17299v1 Announce Type: new Abstract: Word2Vec's effectiveness at generating semantic embeddings has been widely validated, yet it has been tested almost exclusively on languages with large vocabulary inventories. This study examines whether Word2Vec can successfully capture semantic rela...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Implicit vs. Explicit Prompting Strategies for LVLMs in Referential Communication
> **标题**：Implicit vs. Explicit Prompting Strategies for LVLMs in Referential Communication
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17372)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17372v1 Announce Type: new Abstract: Two recent studies (Jones et al. (2026); Zeng et al. (2026)) reach apparently contradictory conclusions about whether LVLMs can coordinate on efficient referring expressions. We control for task differences between the studies while directly comparing...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | An expressivity analysis of hierarchical modelling in deep transformers via bounded-depth grammars
> **标题**：An expressivity analysis of hierarchical modelling in deep transformers via bounded-depth grammars
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17522)
> **source**：cs.CL updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17522v1 Announce Type: new Abstract: Deep neural networks are widely believed to derive their expressive power from their ability to form \textbf{hierarchical representations}, capturing progressively more abstract and compositional features across layers. In language modeling, \textbf{t...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Diagnosing and Repairing Shape-Prior Shortcuts in Long-Range Single-Shot Fringe Projection Profilometry
> **标题**：Diagnosing and Repairing Shape-Prior Shortcuts in Long-Range Single-Shot Fringe Projection Profilometry
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17093)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17093v1 Announce Type: new Abstract: Learning-based single-shot fringe projection profilometry (FPP) has been studied mostly at close range. The long-range regime (standoff beyond 1 m) remains largely unaddressed: inverse-square intensity falloff lowers fringe signal-to-noise ratio and d...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Informative Missingness to Generate Irregular Clinical Time Series
> **标题**：Informative Missingness to Generate Irregular Clinical Time Series
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17106)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17106v1 Announce Type: new Abstract: Laboratory tests in electronic health records are collected irregularly, and the absence of a test order can be as informative as the measurement itself. Such missingness reflects clinicians' decisions and patient physiology, making it important to mo...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Models Take Notes at Prefill: KV Cache Can Be Editable and Composable
> **标题**：Models Take Notes at Prefill: KV Cache Can Be Editable and Composable
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17107)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17107v1 Announce Type: new Abstract: Prefix caching reuses prefill only across an exactly shared prefix, so one changed field invalidates the entire downstream cache. Yet overwriting the field's own key/value vectors and reusing the rest leaves the model acting on the old value. The reas...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | The Critical Role of Model Selection in Causal Inference: A Comparative Analysis of Classification Models within the InferBERT Framework for Pharmacovigilance
> **标题**：The Critical Role of Model Selection in Causal Inference: A Comparative Analysis of Classification Models within the InferBERT Framework for Pharmacovigilance
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17113)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17113v1 Announce Type: new Abstract: Distinguishing causal adverse drug events (ADEs) from spurious correlations remains a central challenge in pharmacovigilance. The InferBERT framework integrates transformer models with Do-calculus, but its success hinges on the underlying classificati...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Noise-Driven Escape from Metastable Phases explains Grokking in Deep Neural Networks
> **标题**：Noise-Driven Escape from Metastable Phases explains Grokking in Deep Neural Networks
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17120)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17120v1 Announce Type: new Abstract: Deep neural networks (DNNs) exhibit first order phase transitions under variations of the L2 regularization strength, with each transition marking the onset of a new learnable feature. Below a critical regularization strength, all features are in prin...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Towards Fast GNN Surrogates for CO2 Migration in Complex Geological Formations
> **标题**：Towards Fast GNN Surrogates for CO2 Migration in Complex Geological Formations
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17180)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17180v1 Announce Type: new Abstract: This chapter discusses how a data-driven machine learning approach can reproduce key aspects of the physical behavior of multiphase flows in complex geological formations. We propose an end-to-end graph neural surrogate tailored to CO$_2$ plume migrat...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Finsler Geometry, Graph Neural Networks, and You
> **标题**：Finsler Geometry, Graph Neural Networks, and You
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17185)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17185v1 Announce Type: new Abstract: Graph neural network architectures based on the graph Laplacian approximate the Laplace-Beltrami operator, thus limiting their application to isotropic operators. As a nonlinear alternative to the Laplace-Beltrami operator, we consider estimates of th...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Constrained Diffusion Models with Primal-Dual Inference
> **标题**：Constrained Diffusion Models with Primal-Dual Inference
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17192)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17192v1 Announce Type: new Abstract: This paper develops constrained diffusion models with primal-dual inference (PDI) to sample from optimal distributions of entropy-regularized optimization problems with \emph{average} constraints. We formalize constrained sampling in the Lagrangian du...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | PowerOPD: Stabilizing On-Policy Distillation with Bounded Power Transformation
> **标题**：PowerOPD: Stabilizing On-Policy Distillation with Bounded Power Transformation
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17199)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17199v1 Announce Type: new Abstract: Standard on-policy distillation (OPD) for large language models estimates the reverse-KL objective using student-sampled tokens, yielding an unbiased single-sample Monte Carlo estimator that avoids vocabulary-wide computation. However, we show that th...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Sum-of-Squares Degree Barriers for the Reweighted-Hinge Method in Robust Halfspace Learning: A Christoffel-Function Characterization
> **标题**：Sum-of-Squares Degree Barriers for the Reweighted-Hinge Method in Robust Halfspace Learning: A Christoffel-Function Characterization
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17215)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17215v1 Announce Type: new Abstract: A certificate that removes outliers sees the data only through its low-degree moments, and an adversary exploits exactly this, hiding corruption where the clean data already looks typical, in the blind spot no bounded-degree test resolves. That blind...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Uncertainty Quantification of Engineering Structures by Polynomial Chaos Expansion and Multivariate Active Learning
> **标题**：Uncertainty Quantification of Engineering Structures by Polynomial Chaos Expansion and Multivariate Active Learning
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17233)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17233v1 Announce Type: new Abstract: In many engineering applications, a single high-fidelity model produces multiple quantities of interest (QoIs) under the same input parameters, e.g. finite element models of complex physical systems. To alleviate the high computational cost of direct...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Rethinking Groups in Critic-Free RLVR
> **标题**：Rethinking Groups in Critic-Free RLVR
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17250)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17250v1 Announce Type: new Abstract: Reinforcement learning (RL) has become a central paradigm for post-training large language models. Existing critic-free RL methods typically generate a group of rollouts for the same question to estimate value baselines for advantage computation. Howe...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Decision-Driven Geosteering Under Uncertainty: A Unified Framework for Sequential Decision Optimization
> **标题**：Decision-Driven Geosteering Under Uncertainty: A Unified Framework for Sequential Decision Optimization
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17331)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17331v1 Announce Type: new Abstract: Geosteering requires navigating a well trajectory through an unknown geological configuration, while sequentially updating decisions based on indirect measurements acquired during drilling. This work presents an uncertainty-aware geosteering framework...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 32** | Counterfactual Optimization of Baseball Pitch Sequences and Estimation of Its Impact on Season-Level Statistics
> **标题**：Counterfactual Optimization of Baseball Pitch Sequences and Estimation of Its Impact on Season-Level Statistics
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2606.17345)
> **source**：cs.LG updates on arXiv.org
> **kind**：`paper`
> **reason**：baseline source relevance
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：arXiv:2606.17345v1 Announce Type: new Abstract: Although pitch sequencing is a central topic in baseball analytics, previous studies have primarily focused on optimizing the final pitch within a single plate appearance, leaving the role of preceding setup pitches and their impact on long-term seaso...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | SuperGoal
> **标题**：SuperGoal
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/supergoal)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Wolfram Language 15
> **标题**：Wolfram Language 15
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/wolfram-mathematica)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Locus Founder
> **标题**：Locus Founder
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/locus-founder)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Mirlo
> **标题**：Mirlo
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/mirlo)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Tyto by ai-coustics
> **标题**：Tyto by ai-coustics
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/tyto)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Framer 3.0
> **标题**：Framer 3.0
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/framer)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Deep Work Plan
> **标题**：Deep Work Plan
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/deep-work-plan)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Dualora
> **标题**：Dualora
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/dualora-dual-size-recorder)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | ClipDone
> **标题**：ClipDone
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/clipdone)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Restaurant Menu Visualizer
> **标题**：Restaurant Menu Visualizer
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/a-bit-differently)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | idmly
> **标题**：idmly
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/idmly)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | NotchSpace
> **标题**：NotchSpace
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/notchspace-2)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Henji
> **标题**：Henji
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/henji)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | AudienceCue
> **标题**：AudienceCue
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/audiencecue)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Vitrine
> **标题**：Vitrine
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/vitrine)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | PaneFlow
> **标题**：PaneFlow
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/paneflow)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 30** | Snapchat SPECS
> **标题**：Snapchat SPECS
> **原文链接**：🔗 [打开原文](https://www.producthunt.com/products/snapchat)
> **source**：Product Hunt — The best new products, every day
> **kind**：`product`
> **reason**：baseline source relevance
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：No summary.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | I scaled a pure Spiking Neural Network (SNN) to 1.088B parameters from scratch. Ran out of budget, but here is what I found
> **标题**：I scaled a pure Spiking Neural Network (SNN) to 1.088B parameters from scratch. Ran out of budget, but here is what I found
> **原文链接**：🔗 [打开原文](https://dev.to/gtausa197svg/i-scaled-a-pure-spiking-neural-network-snn-to-1088b-parameters-from-scratch-ran-out-of-budget-3pg7)
> **source**：Dev.to
> **kind**：`article`
> **reason**：3 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Hey everyone. I’m an 18yo indie dev, and I’ve been experimenting with Spiking Neural Networks (SNNs)...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Most Engineers Use AI. Few Engineer With It.
> **标题**：Most Engineers Use AI. Few Engineer With It.
> **原文链接**：🔗 [打开原文](https://dev.to/jeelvankhede/most-engineers-use-ai-few-engineer-with-it-3pd)
> **source**：Dev.to
> **kind**：`article`
> **reason**：1 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Most software engineers I know use AI in some form now. Maybe it is for debugging, boilerplate,...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | I Ran DeepSeek vs GLM-4 Plus for 30 Days: Here's What I Saved
> **标题**：I Ran DeepSeek vs GLM-4 Plus for 30 Days: Here's What I Saved
> **原文链接**：🔗 [打开原文](https://dev.to/rarenode/i-ran-deepseek-vs-glm-4-plus-for-30-days-heres-what-i-saved-2pa7)
> **source**：Dev.to
> **kind**：`article`
> **reason**：0 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：I Ran DeepSeek vs GLM-4 Plus for 30 Days: Here's What I Saved Look, I'll be straight with you. When...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | I Ran 10 AI Models Through 5 Coding Tasks — Here's the Full Data
> **标题**：I Ran 10 AI Models Through 5 Coding Tasks — Here's the Full Data
> **原文链接**：🔗 [打开原文](https://dev.to/rarenode/i-ran-10-ai-models-through-5-coding-tasks-heres-the-full-data-4ie6)
> **source**：Dev.to
> **kind**：`article`
> **reason**：0 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：I Ran 10 AI Models Through 5 Coding Tasks — Here's the Full Data Last weekend I cleared my kitchen...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | 8 Python Patterns a DevOps Engineer Actually Needs for ML
> **标题**：8 Python Patterns a DevOps Engineer Actually Needs for ML
> **原文链接**：🔗 [打开原文](https://dev.to/shivam_singh_8a9ada1e8b88/8-python-patterns-a-devops-engineer-actually-needs-for-ml-39c6)
> **source**：Dev.to
> **kind**：`article`
> **reason**：0 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：I'm a DevOps engineer moving into MLOps. When I started, I assumed my Python scripting background...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Making my TypeScript types 15.7x faster
> **标题**：Making my TypeScript types 15.7x faster
> **原文链接**：🔗 [打开原文](https://dev.to/dzakh/making-my-typescript-types-157x-faster-4gcg)
> **source**：Dev.to
> **kind**：`article`
> **reason**：5 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：What slows large TypeScript types down, how to measure it with @ark/attest, and the one-field match that made my schema library 15.7x cheaper to type-check.
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | macOS Still Has No Volume Mixer, So I Built One
> **标题**：macOS Still Has No Volume Mixer, So I Built One
> **原文链接**：🔗 [打开原文](https://dev.to/thalesbmc/macos-still-has-no-volume-mixer-so-i-built-one-53lp)
> **source**：Dev.to
> **kind**：`article`
> **reason**：0 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：macOS has never had a proper per-app volume mixer. On Windows, this is basic. You can lower Spotify,...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | The DX we wanted for tapflow setup: a host-ready Mac in one command
> **标题**：The DX we wanted for tapflow setup: a host-ready Mac in one command
> **原文链接**：🔗 [打开原文](https://dev.to/joduchan/the-dx-we-wanted-for-tapflow-setup-a-host-ready-mac-in-one-command-3cb0)
> **source**：Dev.to
> **kind**：`article`
> **reason**：5 reactions
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：tapflow streams iOS simulators and Android emulators into a browser, so a whole team can test an app...
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Can gzip be a language model?
> **标题**：Can gzip be a language model?
> **原文链接**：🔗 [打开原文](https://nathan.rs/posts/gzip-lm/)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：54 score, 5 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：54 score | 5 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | CrankGPT — Local Human-powered AI
> **标题**：CrankGPT — Local Human-powered AI
> **原文链接**：🔗 [打开原文](https://crankgpt.com)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：10 score, 2 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：10 score | 2 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | AI, Gods and Selves: Incredibly Effective Illusions
> **标题**：AI, Gods and Selves: Incredibly Effective Illusions
> **原文链接**：🔗 [打开原文](https://www.youtube.com/watch?v=9X1CQlrwgDI)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：2 score, 1 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：2 score | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | The future of Siri, or: why private inference isn’t private enough
> **标题**：The future of Siri, or: why private inference isn’t private enough
> **原文链接**：🔗 [打开原文](https://blog.cryptographyengineering.com/2026/06/09/apples-siri-ai-or-more-shouting-into-the-void-about-private-agents/)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：37 score, 17 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：37 score | 17 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | The Curse of Depth in Large Language Models
> **标题**：The Curse of Depth in Large Language Models
> **原文链接**：🔗 [打开原文](https://arxiv.org/pdf/2502.05795)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：3 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：3 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Debootstrapping without Archeology: Stacked Implementations in Camlboot
> **标题**：Debootstrapping without Archeology: Stacked Implementations in Camlboot
> **原文链接**：🔗 [打开原文](https://arxiv.org/abs/2202.09231)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：2 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：2 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | A line-by-line translation of the OCaml runtime from C to Rust
> **标题**：A line-by-line translation of the OCaml runtime from C to Rust
> **原文链接**：🔗 [打开原文](https://discuss.ocaml.org/t/a-line-by-line-translation-of-the-ocaml-runtime-from-c-to-rust/18247)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：30 score, 3 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：30 score | 3 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Self-hosting email the hard way from your own routable IPv4 block up
> **标题**：Self-hosting email the hard way from your own routable IPv4 block up
> **原文链接**：🔗 [打开原文](https://anil.recoil.org/notes/recoil-self-hosting-2026)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：57 score, 20 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：57 score | 20 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Announcing Pyro Caml: The First Continuous Profiler for OCaml
> **标题**：Announcing Pyro Caml: The First Continuous Profiler for OCaml
> **原文链接**：🔗 [打开原文](https://semgrep.dev/blog/2026/announcing-pyro-caml-continuous-profiler-ocaml)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：5 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | strace-ui, Bonsai_term, and the TUI renaissance
> **标题**：strace-ui, Bonsai_term, and the TUI renaissance
> **原文链接**：🔗 [打开原文](https://blog.janestreet.com/strace-ui-bonsai-term-and-the-tui-renaissance/)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：32 score, 1 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：32 score | 1 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | OCaml Infrastructure: How the opam-repository Works
> **标题**：OCaml Infrastructure: How the opam-repository Works
> **原文链接**：🔗 [打开原文](https://ocaml.org/backstage/2025-11-05-how-the-opam-repository-works)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：5 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：5 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Introducing Incremental (2015)
> **标题**：Introducing Incremental (2015)
> **原文链接**：🔗 [打开原文](https://blog.janestreet.com/introducing-incremental/)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：12 score, 4 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：12 score | 4 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | FMAG: A single-instruction GPU virtual machine and toolchain
> **标题**：FMAG: A single-instruction GPU virtual machine and toolchain
> **原文链接**：🔗 [打开原文](https://github.com/jangafx/FMAG)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：7 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：7 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Pull Requests are Free Puppies
> **标题**：Pull Requests are Free Puppies
> **原文链接**：🔗 [打开原文](https://www.youtube.com/watch?v=x8_ZZhRL3YU&t=1733s)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：37 score, 5 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：37 score | 5 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | R Core team wins Rousseeuw Prize for Statistics 2026
> **标题**：R Core team wins Rousseeuw Prize for Statistics 2026
> **原文链接**：🔗 [打开原文](https://rousseeuwprize.org/2026)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：13 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：13 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Game Engine White Papers: Commander Keen
> **标题**：Game Engine White Papers: Commander Keen
> **原文链接**：🔗 [打开原文](https://forgottenbytes.net/commander_keen.html)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：17 score, 5 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：17 score | 5 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Getting Creative with Perlin Noise Fields
> **标题**：Getting Creative with Perlin Noise Fields
> **原文链接**：🔗 [打开原文](https://sighack.com/post/getting-creative-with-perlin-noise-fields)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：3 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：3 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Demystifying Noise Contrastive Estimation
> **标题**：Demystifying Noise Contrastive Estimation
> **原文链接**：🔗 [打开原文](http://jxmo.io/posts/nce)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：2 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：2 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | But yak shaving is fun
> **标题**：But yak shaving is fun
> **原文链接**：🔗 [打开原文](https://parksb.github.io/en/article/32.html)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：62 score, 26 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：62 score | 26 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 28** | Computerizing Hieroglyphic Scripts
> **标题**：Computerizing Hieroglyphic Scripts
> **原文链接**：🔗 [打开原文](https://www.youtube.com/watch?v=Vhx-hRyh6BM)
> **source**：Lobste.rs
> **kind**：`community`
> **reason**：1 score, 0 comments
> **follow_up**：阅读讨论区，提炼争议点和实践经验。
> **summary**：1 score | 0 comments
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **只归档 / 0** | Hugging Face fetch failed
> **标题**：Hugging Face fetch failed
> **原文链接**：🔗 [打开原文](https://huggingface.co/api/models?sort=trending&direction=-1&limit=20)
> **source**：System
> **kind**：`failure`
> **reason**：source failure logged
> **follow_up**：检查数据源是否限流或地址失效。
> **summary**：HTTP Error 400: Bad Request
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

## 运行信息

- 生成方式：Research Radar daily_digest
