---
title: AI HOT Daily 2026-06-03
date: 2026-06-03
tags:
  - aihot
  - daily
  - research-radar
---

# 2026-06-03 AI HOT Daily

## AI HOT 官方日报

### matches topics: openai

> [!info]+ **今日必须看 / 79** | 微软首款高级推理AI模型MAI-Thinking-1发布
> **标题**：微软首款高级推理AI模型MAI-Thinking-1发布
> **原文链接**：🔗 [打开原文](https://www.theverge.com/tech/941664/microsoft-ai-model-reasoning-mai-thinking-1-build-2026)
> **source**：AI HOT Daily / The Verge：AI（RSS）
> **kind**：`model`
> **reason**：matches topics: openai
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：微软在Build 2026上发布了其首款高级推理AI模型MAI-Thinking-1。该模型被定位为“中等规模”，能在“关键”软件工程基准测试中达到领先模型的水平。微软称其完全从头使用干净数据进行训练，未涉及从第三方模型进行知识蒸馏。这标志着微软在自研AI模型上迈出重要一步，此前其主要依赖OpenAI。近期两家公司已重新协商合作协议，关系有所松绑。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 72** | OpenAI呼吁通过全球领导力推进青年AI安全与机遇
> **标题**：OpenAI呼吁通过全球领导力推进青年AI安全与机遇
> **原文链接**：🔗 [打开原文](https://openai.com/index/advancing-youth-safety-and-opportunity-through-global-leadership)
> **source**：AI HOT Daily / OpenAI：官网动态（RSS · 排除企业/客户案例）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：OpenAI呼吁通过设立专门的AI安全研究所，在全球范围内采取行动，以保障青少年在使用AI时的安全，并创造更多发展机遇。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 72** | Travelers借助OpenAI在全国部署AI理赔助手
> **标题**：Travelers借助OpenAI在全国部署AI理赔助手
> **原文链接**：🔗 [打开原文](https://openai.com/index/travelers)
> **source**：AI HOT Daily / OpenAI：官网动态（RSS · 排除企业/客户案例）
> **kind**：`article`
> **reason**：matches topics: openai
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：美国保险公司Travelers与OpenAI合作，构建了一款AI驱动的Claim Assistant。该工具旨在引导客户完成理赔流程，并提供全天候支持，以在业务高峰期扩展运营规模。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 模型发布/更新

> [!info]+ **可延后 / 71** | Holo3.1：快速本地计算机使用智能体
> **标题**：Holo3.1：快速本地计算机使用智能体
> **原文链接**：🔗 [打开原文](https://huggingface.co/blog/Hcompany/holo31)
> **source**：AI HOT Daily / Hugging Face：Blog（RSS）
> **kind**：`model`
> **reason**：AI HOT official daily section: 模型发布/更新
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：Holo3.1 是基于 Qwen 模型家族的计算机使用智能体系列，旨在提升在桌面、网页和移动环境中的鲁棒性。新模型提供 0.8B、4B、9B 和 35B-A3B 四种尺寸，并首次发布量化检查点，包括 FP8、Q4 GGUF 和 NVFP4，以优化本地推理。在 AndroidWorld 基准测试中，35B-A3B 模型得分从 67% 提升至 79.3%。在 DGX Spark 上，NVFP4 量化相比 BF16 实现 1.74 倍 token 吞吐量提升，并将平均步骤时间从 6.8 秒缩短至 3.3 秒。模型支持函数调用协议，可在第三方智能体框架中部署。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 71** | 阶跃星辰Step 3.7 Flash发布，专为高效推理设计
> **标题**：阶跃星辰Step 3.7 Flash发布，专为高效推理设计
> **原文链接**：🔗 [打开原文](https://x.com/StepFun_ai/status/2061655529731342402)
> **source**：AI HOT Daily / X：阶跃星辰 StepFun (@StepFun_ai)
> **kind**：`model`
> **reason**：AI HOT official daily section: 模型发布/更新
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：阶跃星辰发布其推理优化型模型Step 3.7 Flash。该模型为196B MoE架构，从设计之初就专注于推理效率。其采用多矩阵分解注意力机制，使KV-cache成本仅为DeepSeek模型的约22%；同时通过注意力与FFN解耦技术，实现了硬件优化的高效服务。该模型已通过Fireworks AI提供，采用Apache 2.0许可，并可用于构建智能体应用。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: claude code; high-value terms: claude code

> [!info]+ **今日必须看 / 81** | Claude Code 新增动态工作流功能
> **标题**：Claude Code 新增动态工作流功能
> **原文链接**：🔗 [打开原文](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code)
> **source**：AI HOT Daily / Claude：Blog（网页）
> **kind**：`product`
> **reason**：matches topics: claude code; high-value terms: claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Claude Code 新增动态工作流功能，允许模型在运行时即兴创建和协调多智能体框架来处理复杂任务。该功能通过执行特定的 JavaScript 文件来生成和协调拥有独立上下文窗口的子代理，可解决单一上下文窗口中长时间执行任务可能出现的智能惰性等问题。工作流适用于研究、安全分析、代码审查等场景，通常消耗更多 token，更适合高价值复杂任务，其最佳实践仍在发展中。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 79** | Claude Code团队实践：智能体编程如何重塑工程组织与流程
> **标题**：Claude Code团队实践：智能体编程如何重塑工程组织与流程
> **原文链接**：🔗 [打开原文](https://claude.com/blog/running-an-ai-native-engineering-org)
> **source**：AI HOT Daily / Claude：Blog（网页）
> **kind**：`article`
> **reason**：matches topics: claude code; high-value terms: claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：在Code w/ Claude SF 2026活动上，Claude Code工程团队分享了将智能体编程设为默认工作方式后带来的流程与结构变革。核心变化包括：规划转向即时（JIT）模式，强调快速原型与反馈；上下文收集变为“先问Claude”；代码审查中Claude处理风格与测试，人工专注于法律、安全等专业判断。新范式下，工程瓶颈从编写代码转向验证、审查与安全维护。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 79** | Claude Code 自我检查与反馈闭环技巧
> **标题**：Claude Code 自我检查与反馈闭环技巧
> **原文链接**：🔗 [打开原文](https://x.com/ClaudeDevs/status/2061900434722496604)
> **source**：AI HOT Daily / X：Claude Devs (@ClaudeDevs)
> **kind**：`article`
> **reason**：matches topics: claude code; high-value terms: claude code
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：如何让 Claude Code 在交回工作前检查自己的成果？ 看看如何编码你的手动检查，让 Claude 自己关闭反馈循环：
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: claude code; high-value terms: claude code, api

> [!info]+ **今日必须看 / 88** | Claude Platform 新增 CLI 工具
> **标题**：Claude Platform 新增 CLI 工具
> **原文链接**：🔗 [打开原文](https://x.com/ClaudeDevs/status/2061877343078244459)
> **source**：AI HOT Daily / X：Claude Devs (@ClaudeDevs)
> **kind**：`product`
> **reason**：matches topics: claude code; high-value terms: claude code, api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：我们为 Claude Platform 添加了一个 CLI，使每个 API 端点都可以从你的终端运行。 调用 Messages API，启动 Claude 托管智能体，并将结果直接管道传输到你的 shell。 ant CLI 被使用 claude-api 技能的编码智能体（Claude Code）很好地理解。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: agent, codex, openai; high-value terms: agent, codex

> [!info]+ **今日必须看 / 100** | OpenAI Codex 发布 Python SDK，可直接嵌入应用
> **标题**：OpenAI Codex 发布 Python SDK，可直接嵌入应用
> **原文链接**：🔗 [打开原文](https://x.com/vista8/status/2061846741885018296)
> **source**：AI HOT Daily / X：Vista (@vista8)
> **kind**：`product`
> **reason**：matches topics: agent, codex, openai; high-value terms: agent, codex
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：这个有点厉害，Codex 出 Python SDK了。 安装指令：pip install openai-codex 整合到自己的代码中，相当于直接内置了顶级编程和生图Agent？ 最关键的是，可以复用 Codex 登录态。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: deepmind

> [!info]+ **可延后 / 74** | Google DeepMind 开源科学智能体工具包
> **标题**：Google DeepMind 开源科学智能体工具包
> **原文链接**：🔗 [打开原文](https://x.com/googleaidevs/status/2061924472245153863)
> **source**：AI HOT Daily / X：Google AI for Developers (@googleaidevs)
> **kind**：`product`
> **reason**：matches topics: deepmind
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：构建用于科学发现的自主智能体？🧬🤖 @GoogleDeepMind Science Skills 现已在 GitHub 上发布。我们已开源这个专用工具包，以科学基础和更高的 token 效率加速您的智能体工作流。 立即下载 ↓ https://github.com/google-deepmind/science-skills
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 72** | Gemini Spark：最令人印象深刻也最可怕的AI体验
> **标题**：Gemini Spark：最令人印象深刻也最可怕的AI体验
> **原文链接**：🔗 [打开原文](https://www.theverge.com/ai-artificial-intelligence/941388/gemini-spark-ai-agent-trip-planning)
> **source**：AI HOT Daily / The Verge：订阅版科技（RSS）
> **kind**：`article`
> **reason**：matches topics: deepmind
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Google DeepMind的AI模型Gemini Spark提供了一次极为深刻但同时令人感到不安的用户体验。该模型展现的强大能力令人印象深刻，但其带来的影响和潜力也引发了深刻的恐惧感。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### high-value terms: eval

> [!info]+ **可延后 / 73** | 微软发布开源框架 Adaptive Spec-driven Scoring：支持用文本描述创建 AI 评估测试
> **标题**：微软发布开源框架 Adaptive Spec-driven Scoring：支持用文本描述创建 AI 评估测试
> **原文链接**：🔗 [打开原文](https://techcrunch.com/2026/06/02/new-microsoft-tool-lets-devs-spin-up-ai-behavior-tests-using-text-descriptions)
> **source**：AI HOT Daily / TechCrunch：AI（RSS）
> **kind**：`product`
> **reason**：high-value terms: eval
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：微软开源发布 Adaptive Spec-driven Scoring for Evaluation and Regression Testing 框架。开发者可通过文本描述快速生成 AI 行为测试，用于模型评估与回归测试。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### high-value terms: api

> [!info]+ **可延后 / 73** | Runway API 推出 Aleph 2.0 视频编辑功能
> **标题**：Runway API 推出 Aleph 2.0 视频编辑功能
> **原文链接**：🔗 [打开原文](https://x.com/runwayml/status/2061895998545244342)
> **source**：AI HOT Daily / X：Runway (@runwayml)
> **kind**：`product`
> **reason**：high-value terms: api
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Aleph 2.0 现已通过 Runway API 提供。将精准视频编辑直接集成到您的应用、产品和平台中。支持在多镜头序列中编辑最长 30 秒、1080p 分辨率的视频，仅修改您想要的部分。 请通过以下链接开始使用。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 产品发布/更新

> [!info]+ **可延后 / 66** | GitHub Copilot应用：智能体原生的桌面体验
> **标题**：GitHub Copilot应用：智能体原生的桌面体验
> **原文链接**：🔗 [打开原文](https://github.blog/news-insights/product-news/github-copilot-app-the-agent-native-desktop-experience)
> **source**：AI HOT Daily / GitHub Blog
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：在微软 Build 2026 大会上，GitHub 发布了新的工具和更新，并将 Copilot 应用定位为“智能体原生的桌面体验”。其核心目标是让 AI 智能体能够以用户已经习惯的方式进行工作。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 66** | Replit 与微软合作发布 Fabric 集成
> **标题**：Replit 与微软合作发布 Fabric 集成
> **原文链接**：🔗 [打开原文](https://x.com/Replit/status/2061892255028486435)
> **source**：AI HOT Daily / X：Replit (@Replit)
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：宣布与 @Microsoft 的新合作 组织现在可以在 Replit 中构建内部工具、工作流或数据仪表板，并直接发布到 Microsoft Fabric，内置安全、身份验证和治理功能。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: openai, anthropic

> [!info]+ **今日必须看 / 80** | Alphabet拟融资800亿美元 Anthropic提交IPO申请
> **标题**：Alphabet拟融资800亿美元 Anthropic提交IPO申请
> **原文链接**：🔗 [打开原文](https://www.bloomberg.com/news/videos/2026-06-02/bloomberg-tech-6-2-2026-video)
> **source**：AI HOT Daily / Bloomberg：Technology（RSS）
> **kind**：`article`
> **reason**：matches topics: openai, anthropic
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Alphabet宣布拟通过股权融资800亿美元，用于扩展AI基础设施。Anthropic已秘密提交IPO申请，在上市竞赛中领先于竞争对手OpenAI。此外，SpaceX正与华尔街机构协商其IPO的承销费用，HPE则因AI基础设施需求旺盛，年度销售预期超出市场估计。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 行业动态

> [!info]+ **可延后 / 64** | Nathan Lambert离开Ai2，结束2.5年OLMO等项目工作
> **标题**：Nathan Lambert离开Ai2，结束2.5年OLMO等项目工作
> **原文链接**：🔗 [打开原文](https://x.com/natolambert/status/2061813361848029631)
> **source**：AI HOT Daily / X：Nathan Lambert (@natolambert)
> **kind**：`article`
> **reason**：AI HOT official daily section: 行业动态
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Ai2（Allen Institute for AI）研究员Nathan Lambert宣布离职。他在Ai2工作超过2.5年，期间主导或参与了OLMO和Tulu等开源模型项目，称其为职业生涯的巅峰。他表示将暂时休息，未来仍会继续深耕开源模型与开放科学领域。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | SK会长崔泰源：SK海力士计划未来五年内晶圆产能翻倍
> **标题**：SK会长崔泰源：SK海力士计划未来五年内晶圆产能翻倍
> **原文链接**：🔗 [打开原文](https://www.ithome.com/0/958/810.htm)
> **source**：AI HOT Daily / IT之家（RSS）
> **kind**：`article`
> **reason**：AI HOT official daily section: 行业动态
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：SK海力士会长崔泰源宣布，计划在未来五年内将整体晶圆产能提高一倍，以应对AI普及带来的持续存储供应短缺。他预测AI数据中心和AI PC的普及将持续拉动存储需求，供需紧张局面可能延续至2030年。SK海力士将投入大规模资金用于设备、建设等扩张，尽管面临前置时间长（新建晶圆厂至少三年）和资源成本上涨等挑战。目前，SK海力士市值已首次突破1万亿美元。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: anthropic; high-value terms: security

> [!info]+ **今日必须看 / 79** | Anthropic扩展Project Glasswing计划
> **标题**：Anthropic扩展Project Glasswing计划
> **原文链接**：🔗 [打开原文](https://www.anthropic.com/news/expanding-project-glasswing)
> **source**：AI HOT Daily / Anthropic：Newsroom（网页）
> **kind**：`article`
> **reason**：matches topics: anthropic; high-value terms: security
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Anthropic正将其Project Glasswing计划扩展至约150个新组织，此前首批约50个合作伙伴。新伙伴分布于十五个多国家，覆盖电力、水务、医疗、通信和硬件等关键基础设施行业。这些合作伙伴的共同点在于，其代码库若遭成功攻击，后果可能极其严重，影响或超1亿人。项目旨在利用Claude Mythos Preview等前沿模型扫描漏洞并协助修复，以应对AI驱动的网络安全挑战。同时，Anthropic推出了基于Claude Opus 4.8等公开模型的Claude Security产品，用于扫描代码并建议补丁。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 79** | Anthropic支持美国AI行政令实施
> **标题**：Anthropic支持美国AI行政令实施
> **原文链接**：🔗 [打开原文](https://x.com/AnthropicAI/status/2061924580222968183)
> **source**：AI HOT Daily / X：Anthropic (@AnthropicAI)
> **kind**：`article`
> **reason**：matches topics: anthropic; high-value terms: security
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：这项行政令是加强美国AI领导地位的重要一步。 我们期待与白宫合作，支持其实施。 https://www.whitehouse.gov/presidential-actions/2026/06/promoting-advanced-artificial-intelligence-innovation-and-security/
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 论文研究

> [!info]+ **可延后 / 68** | 微软研究：Aurora天气预报速度超传统超算数千倍
> **标题**：微软研究：Aurora天气预报速度超传统超算数千倍
> **原文链接**：🔗 [打开原文](https://x.com/MSFTResearch/status/2061927189977727450)
> **source**：AI HOT Daily / X：Microsoft Research (@MSFTResearch)
> **kind**：`paper`
> **reason**：AI HOT official daily section: 论文研究
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：天气预报速度比传统超级计算机快数千倍。听听Kenji Takeda在#MSBuild微软研究实验室关于Aurora的分享。了解更多：https://msft.it/6018vjGUA
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: anthropic

> [!info]+ **今日必须看 / 76** | Anthropic可解释性研究：区分因果效应相似的特征
> **标题**：Anthropic可解释性研究：区分因果效应相似的特征
> **原文链接**：🔗 [打开原文](https://transformer-circuits.pub/2026/may-update/index.html)
> **source**：AI HOT Daily / Anthropic：Transformer Circuits（可解释性研究）
> **kind**：`paper`
> **reason**：matches topics: anthropic
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：Anthropic可解释性团队介绍了其Circuits研究的新进展。为区分那些激活模式相似但因果效应不同的模型特征，团队提出一种新方法。该方法通过分析特征的下游连接来预测其实际影响，并使用基于共激活统计的TWERA（虚拟权重）对连接进行加权排序。实验表明，借助下游连接信息能更准确地判断哪个特征会引导特定输出。此方法为识别模型内部真正的因果组件提供了新途径。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: codex; high-value terms: codex

> [!info]+ **今日必须看 / 83** | Codex正在成为每个人的生产力工具
> **标题**：Codex正在成为每个人的生产力工具
> **原文链接**：🔗 [打开原文](https://openai.com/index/codex-for-knowledge-work)
> **source**：AI HOT Daily / OpenAI：官网动态（RSS · 排除企业/客户案例）
> **kind**：`paper`
> **reason**：matches topics: codex; high-value terms: codex
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：The Next Era of Knowledge Work报告指出，Codex正通过AI增强的研究、数据分析、工作流自动化与内容创作，变革知识工作的生产力。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **今日必须看 / 79** | 为了不花那120刀，我把电脑清理软件做成了开源skill
> **标题**：为了不花那120刀，我把电脑清理软件做成了开源skill
> **原文链接**：🔗 [打开原文](https://x.com/Khazix0918/status/2061669881725309048)
> **source**：AI HOT Daily / X：卡兹克 (@Khazix0918)
> **kind**：`article`
> **reason**：matches topics: codex; high-value terms: codex
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：作者受一条推文启发，使用Codex对自己的MacBook进行了只读存储分析，发现了B站缓存视频等大量可清理空间（激进方案超140G）。为替代收费软件CleanMyMac，作者制作并开源了一个跨平台（支持Mac/Windows）的AI清理skill。该skill会扫描文件并生成可交互的HTML报告，通过三色分级（绿灯可放心清理、黄灯需人工判断、红灯禁止动）直观展示，并提供安全执行按钮。实测清理后释放了近120G空间，相比CleanMyMac仅扫描出的15.8G，其信息更透明、建议更详细。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: agent; high-value terms: agent

> [!info]+ **今日必须看 / 79** | 商汤开源SenseNova-Skills AI办公技能套件
> **标题**：商汤开源SenseNova-Skills AI办公技能套件
> **原文链接**：🔗 [打开原文](https://x.com/SenseTime_AI/status/2061822148076093625)
> **source**：AI HOT Daily / X：商汤 SenseTime (@SenseTime_AI)
> **kind**：`article`
> **reason**：matches topics: agent; high-value terms: agent
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：商汤开源了AI办公技能套件SenseNova-Skills。这是一个为任何技能兼容智能体（如OpenClaw与HermesAgent）设计的开源技能集合，提供四大核心功能：图像信息图表生成（可镜像参考风格）、数据分析（支持多表解析、清洗与可视化）、PPT创建（生成大纲内容并智能排版，输出可编辑文件）以及深度研究（跨学术、技术、社交等多源搜索并生成报告）。该技能套件现已完全开源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 技巧与观点

> [!info]+ **可延后 / 64** | Karpathy 分享学习方法论
> **标题**：Karpathy 分享学习方法论
> **原文链接**：🔗 [打开原文](https://x.com/rohanpaul_ai/status/2061601689841648120)
> **source**：AI HOT Daily / X：Rohan Paul (@rohanpaul_ai)
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：🎯 Andrej Karpathy 谈如何学习。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | 开放模型的繁荣生态
> **标题**：开放模型的繁荣生态
> **原文链接**：🔗 [打开原文](https://www.tomtunguz.com/the-thriving-ecosystem-of-open-models)
> **source**：AI HOT Daily / Tomer Tunguz 博客（VC 分析）
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：根据OpenRouter平台数据，自2025年以来，开放模型使用量显著增长。最新数据显示，开放权重模型产生了69.1%的token使用量，闭源模型为30.9%。新模型的发布会吸引开发者测试，推动token使用量达到新的平台期。开放模型市场内部竞争激烈，领导地位频繁更迭，如DeepSeek的早期优势在2025年末至2026年初被MiniMax与Kimi模型取代，随后MiMo、Qwen、腾讯Hy3、阿里巴巴及Arcee等模型的发布再次改变了份额格局。尽管开放模型目前仍只占推理总量的一小部分，但激烈的竞争与增长表明，开发者正越来越愿意将生产流量路由至开放模型。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 64** | Gary Marcus：为什么事情终将崩塌
> **标题**：Gary Marcus：为什么事情终将崩塌
> **原文链接**：🔗 [打开原文](https://garymarcus.substack.com/p/why-things-will-eventually-fall-apart)
> **source**：AI HOT Daily / Gary Marcus：The Road to AI We Can Trust（RSS）
> **kind**：`article`
> **reason**：AI HOT official daily section: 技巧与观点
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：知名人工智能批评者Gary Marcus在其关于可信赖AI的专栏中，探讨了人工智能发展面临的根本性挑战。文章开篇即指向问题的核心，指出相关数学理论的局限性与人类心理的复杂性，是导致AI系统最终可能出现问题的根源。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
