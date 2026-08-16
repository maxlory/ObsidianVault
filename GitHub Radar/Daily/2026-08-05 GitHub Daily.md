---
title: GitHub Daily 2026-08-05
date: 2026-08-05
tags:
  - github-radar
  - daily
---

# 2026-08-05 GitHub Daily

## 今日概览

- GitHub 全站 TOP10：10 条　`isboyjc/github-trending-api (JSON)`
- GitHub AI 领域 TOP10：10 条　`GitHub Search API（5 个 topic，近 90 天新仓库，池子 109 个）`
- Claude skills TOP10（按本周装机量）：10 条　`skills.sh（解析 600 条，去重键=仓库/skillId）`
- 本周冒头最快（按环比增速，不计入 TOP10）：4 条　`skills.sh（解析 600 条，去重键=仓库/skillId）`
- AI 解读：正常（34 条全部有解读）
- 数据源失败：1 项
- **排序原则：只看热度硬数字**（今日新增 star / 本周装机量）。AI 只做解读，不参与排序、不打分、不过滤。
- 名次徽标：🆕 首次上榜　↑/↓ 相比上次运行的名次变化　— 名次未变

## GitHub 全站 TOP10

> [!info]+ **#1 · 今日 +2,524 star** · 🆕 | firecrawl/pdf-inspector
> **是什么**：快速识别PDF是扫描件还是文本件，做分类和文本提取，便于路由处理。
> **跟你的关系**：🟡 `可借鉴` — PDF识别分类思路可借鉴，用于财务/文档解析agent的文档预处理。
> **链接**：🔗 [打开仓库](https://github.com/firecrawl/pdf-inspector)
> **语言**：Rust　**总 star**：9,803　**fork**：646
> **原始描述**：Fast Rust library for PDF inspection, classification, and text extraction. Intelligently detects scanned vs text-based PDFs to enable smart routing decisions.

> [!info]+ **#2 · 今日 +2,310 star** · ↑4 | zhaoxuya520/reverse-skill
> **是什么**：给AI编程客户端用的安全技能路由包，自动装配渗透工具链和知识库。
> **跟你的关系**：🟡 `可借鉴` — skill路由/自举设计可借鉴，用于Claude Code skill工程化流水线。
> **链接**：🔗 [打开仓库](https://github.com/zhaoxuya520/reverse-skill)
> **语言**：PowerShell　**总 star**：17,710　**fork**：2,440
> **原始描述**：Reverse Engineering / Authorized Penetration Testing / Security Research Skill Router Pack AI-powered routing + On-demand toolchain bootstrapping + Self-evolving knowledge base Supports Claude Code, Kiro, Cursor, Cline, and other AI coding clients 逆向/渗透/安全技能路由包 - AI 自动路由 + 按需自举工具链 + 自动进化经验库 | 支持 Cla

> [!info]+ **#3 · 今日 +1,716 star** · 🆕 | lyogavin/airllm
> **是什么**：让70B大模型在单张4GB显卡上跑推理的库。
> **跟你的关系**：⚫️ `无关` — 模型推理优化，不在他关注方向内，与当前项目无直接关联。
> **链接**：🔗 [打开仓库](https://github.com/lyogavin/airllm)
> **语言**：Jupyter Notebook　**总 star**：28,236　**fork**：3,046
> **原始描述**：AirLLM 70B inference with single 4GB GPU

> [!info]+ **#4 · 今日 +1,138 star** · 🆕 | TencentCloud/TencentDB-Agent-Memory
> **是什么**：给Agent团队用的记忆中枢，把对话文档代码沉淀成可复用资产。
> **跟你的关系**：🟡 `可借鉴` — Agent记忆复用设计可借鉴，用于AgentDaniel的知识沉淀或知识库管道。
> **链接**：🔗 [打开仓库](https://github.com/TencentCloud/TencentDB-Agent-Memory)
> **语言**：TypeScript　**总 star**：13,367　**fork**：1,260
> **原始描述**：TencentDB Agent Memory is a team-level memory hub for AI Agents — turning conversations, docs, and code into four reusable memory assets (Chat Memory, Skill, LLM-Wiki, Code-Graph) that are governed, shared, and equipped across agents and frameworks.

> [!info]+ **#5 · 今日 +924 star** · 🆕 | esengine/DeepSeek-Reasonix
> **是什么**：基于DeepSeek的终端AI编程助手，利用前缀缓存保持稳定。
> **跟你的关系**：🟡 `可借鉴` — 终端编程Agent的前缀缓存设计可借鉴，用于Claude Code skill工程化。
> **链接**：🔗 [打开仓库](https://github.com/esengine/DeepSeek-Reasonix)
> **语言**：Go　**总 star**：30,718　**fork**：1,980
> **原始描述**：DeepSeek-native AI coding agent for your terminal. Engineered around prefix-cache stability — leave it running.

> [!info]+ **#6 · 今日 +784 star** · 🆕 | microsoft/generative-ai-for-beginners
> **是什么**：微软出的21课生成式AI入门教程。
> **跟你的关系**：⚫️ `无关` — AI入门教程，与他已有实践和关注方向无关。
> **链接**：🔗 [打开仓库](https://github.com/microsoft/generative-ai-for-beginners)
> **语言**：Jupyter Notebook　**总 star**：116,197　**fork**：61,541
> **原始描述**：21 Lessons, Get Started Building with Generative AI

> [!info]+ **#7 · 今日 +617 star** · 🆕 | obra/superpowers
> **是什么**：一套Agent技能框架和软件开发方法论。
> **跟你的关系**：🟡 `可借鉴` — 技能框架与方法论可借鉴，直接对应Claude Code skill工程化。
> **链接**：🔗 [打开仓库](https://github.com/obra/superpowers)
> **语言**：Shell　**总 star**：266,372　**fork**：23,818
> **原始描述**：An agentic skills framework & software development methodology that works.

> [!info]+ **#8 · 今日 +565 star** · — | usekaneo/kaneo
> **是什么**：开源项目管理工具，功能精简，强调为使用者服务。
> **跟你的关系**：⚫️ `无关` — 项目管理工具不在关注方向，与AI自动化/知识管理无直接关联。
> **链接**：🔗 [打开仓库](https://github.com/usekaneo/kaneo)
> **语言**：TypeScript　**总 star**：7,229　**fork**：579
> **原始描述**：🎯 All you need. Nothing you don't. Open source project management that works for you, not against you.

> [!info]+ **#9 · 今日 +432 star** · 🆕 | livekit/agents
> **是什么**：实时语音AI Agent开发框架，集成音视频与AI能力，用于构建能听会说的对话代理。
> **跟你的关系**：⚪️ `了解即可` — 实时语音Agent框架，属于Agent方向但暂无语音场景可落地。
> **链接**：🔗 [打开仓库](https://github.com/livekit/agents)
> **语言**：Python　**总 star**：12,334　**fork**：3,470
> **原始描述**：A framework for building realtime voice AI agents 🤖🎙️📹

> [!info]+ **#10 · 今日 +306 star** · 🆕 | browser-use/video-use
> **是什么**：让编码Agent直接编辑视频，用自然语言指令完成剪辑/修改。
> **跟你的关系**：⚫️ `无关` — 视频编辑工具，与自动化测试/金融/文档解析等方向无关。
> **链接**：🔗 [打开仓库](https://github.com/browser-use/video-use)
> **语言**：Python　**总 star**：19,213　**fork**：2,400
> **原始描述**：Edit videos with coding agents

## GitHub AI 领域 TOP10

> [!info]+ **#1 · 较上次 +2,561 star** · — | DietrichGebert/ponytail
> **是什么**：引导AI Agent像偷懒的高级工程师一样，尽量少写或改写代码。
> **跟你的关系**：🟡 `可借鉴` — 少写代码原则可借鉴到Claude Code skill设计，减少无效生成。
> **链接**：🔗 [打开仓库](https://github.com/DietrichGebert/ponytail)
> **语言**：JavaScript　**总 star**：95,855　**fork**：5,268
> **原始描述**：Makes your AI agent think like the laziest senior dev in the room. The best code is the code you never wrote.

> [!info]+ **#2 · 较上次 +1,525 star** · ↑1 | alibaba/open-code-review
> **是什么**：阿里开源的混合架构代码审查工具，结合规则与LLM，定位到行注释。
> **跟你的关系**：🟡 `可借鉴` — 确定性管道+LLM混合架构可借鉴到内部AI自动化测试平台。
> **链接**：🔗 [打开仓库](https://github.com/alibaba/open-code-review)
> **语言**：Go　**总 star**：18,785　**fork**：1,267
> **原始描述**：Fast, efficient, battle-tested at Alibaba's scale. Hybrid architecture code review tool: deterministic pipelines + LLM Agent, precise line-level comments, built-in multi-language ruleset (NPE, thread-safety, XSS, SQL injection), OpenAI & Anthropic compatible.

> [!info]+ **#3 · 较上次 +739 star** · ↑2 | img2threejs/img2threejs
> **是什么**：将参考图重建为可直接用的Three.js 3D模型，用代码描述几何体。
> **跟你的关系**：⚫️ `无关` — 图像转3D建模，不在其财务/Agent/量化等关注方向内。
> **链接**：🔗 [打开仓库](https://github.com/img2threejs/img2threejs)
> **语言**：Python　**总 star**：9,607　**fork**：720
> **原始描述**：Rebuild the object in a reference image as a code-only, procedural, quality-gated, animation-ready Three.js model. Token-efficient image-to-3D.

> [!info]+ **#4 · 较上次 +354 star** · ↑2 | StarTrail-org/PixelRAG
> **是什么**：用像素级方式做网页/文档搜索，跳过传统解析，直接识别屏幕内容。
> **跟你的关系**：🟡 `可借鉴` — 像素级解析思路可借鉴到AgentDaniel，处理扫描件/图片型财报。
> **链接**：🔗 [打开仓库](https://github.com/StarTrail-org/PixelRAG)
> **语言**：Python　**总 star**：9,096　**fork**：777
> **原始描述**：The end of web parsing. The beginning of scalable pixel-native search. link: https://pixelrag.ai/

> [!info]+ **#5 · 较上次 +185 star** · ↓3 | open-gsd/gsd-core
> **是什么**：描述信息不足，需点开原链接
> **跟你的关系**：⚫️ `无关` — 描述过于简略，无法确认与其关注方向相关。
> **链接**：🔗 [打开仓库](https://github.com/open-gsd/gsd-core)
> **语言**：JavaScript　**总 star**：7,714　**fork**：535
> **原始描述**：Git. Ship. Done - Core

> [!info]+ **#6 · 较上次 +142 star** · ↑1 | simonlin1212/a-stock-data
> **是什么**：A股数据一体化工具包，覆盖行情、研报、资金、公告等43个端点。
> **跟你的关系**：🟢 `可直接用` — A股行情/研报/资金面数据可直接接入量化研究做数据源。
> **链接**：🔗 [打开仓库](https://github.com/simonlin1212/a-stock-data)
> **语言**：—　**总 star**：8,354　**fork**：1,531
> **原始描述**：A股全栈数据工具包 · 10层架构 · 43端点(含3官方备胎) · 15数据源 · 行情/研报/资金面/筹码/公告/打板/ETF期权/舆情互动全覆盖+备用源降级 | China A-Share full-stack data toolkit (43 endpoints)

> [!info]+ **#7 · 较上次 +139 star** · ↓3 | cobusgreyling/loop-engineering
> **是什么**：给 AI 编程智能体做循环设计用的模式、脚手架和命令行工具
> **跟你的关系**：🟡 `可借鉴` — 他的 Claude Code skill 流水线可借鉴 loop 模式与成本审计工具
> **链接**：🔗 [打开仓库](https://github.com/cobusgreyling/loop-engineering)
> **语言**：JavaScript　**总 star**：9,854　**fork**：1,336
> **原始描述**：Practical patterns, starters & CLI tools for loop engineering with AI coding agents. Design systems that prompt and orchestrate agents (inspired by Addy Osmani and Boris Cherny). Includes loop-audit, loop-init, loop-cost.

> [!info]+ **#8 · 较上次 +120 star** · ↑2 | omnigent-ai/omnigent
> **是什么**：开源 AI Agent 编排框架，可统一调度 Claude Code、Codex 等多个智能体
> **跟你的关系**：🟡 `可借鉴` — 与 Agent 框架编排相关，可借鉴其多 harness 调度设计到内部自动化测试平台
> **链接**：🔗 [打开仓库](https://github.com/omnigent-ai/omnigent)
> **语言**：Python　**总 star**：8,118　**fork**：1,216
> **原始描述**：Omnigent is an open-source AI agent framework and meta-harness: orchestrate Claude Code, Codex, Cursor, Pi, and custom agents — swap harnesses without rewriting, enforce policies and sandboxing, and collaborate in real time from any device.

> [!info]+ **#9 · 较上次 +56 star** · ↓1 | XiaomiMiMo/MiMo-Code
> **是什么**：描述信息不足，需点开原链接
> **跟你的关系**：⚪️ `了解即可` — 属 Agent 方向，但描述不足且不在当前项目链路
> **链接**：🔗 [打开仓库](https://github.com/XiaomiMiMo/MiMo-Code)
> **语言**：TypeScript　**总 star**：12,641　**fork**：1,290
> **原始描述**：MiMo Code: Where Models and Agents Co-Evolve

> [!info]+ **#10 · 较上次 +52 star** · ↓1 | nexu-io/html-anything
> **是什么**：让 AI 在本地直接写 HTML 并一键发布到多平台的编辑器，含 75 个技能模板
> **跟你的关系**：🟡 `可借鉴` — 他的 Claude Code skill 工程化可参考其按场景拆分技能的组织方式
> **链接**：🔗 [打开仓库](https://github.com/nexu-io/html-anything)
> **语言**：HTML　**总 star**：8,085　**fork**：789
> **原始描述**：✨ The agentic HTML editor — your local AI agent writes the HTML, you ship it. 🚀 75 Skills × 9 Surfaces (magazine · deck · poster · XHS / tweet · prototype · data report · Hyperframes) 🛡️ Sandboxed preview · 📤 1-click to WeChat / X / Zhihu / HTML / PNG 🔑 Zero API key — Claude Code / Cursor / Codex / 

## Claude skills TOP10（按本周装机量）

> [!info]+ **#1 · 本周 132,099 装** · 🆕 | reddit-automation
> **是什么**：通过 MCP 让 AI 自动搜索、发帖和管理 Reddit 评论的 Claude skill
> **跟你的关系**：⚪️ `了解即可` — 属于 skill 工程化关注范围，可作为 MCP 集成示例，但当前无落地场景
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/reddit-automation)
> **来源仓库**：[doany-skills/skills](https://github.com/doany-skills/skills)
> **累计装机**：266,350　**官方**：否
> **原始描述**：Automate Reddit tasks via Rube MCP (Composio): search subreddits, create posts, manage comments, and browse top content. Always search tools first for current schemas.

> [!info]+ **#2 · 本周 123,057 装** · 🆕 | coll_ai-video-generation
> **是什么**：描述信息不足，需点开原链接
> **跟你的关系**：⚫️ `无关` — AI 视频生成与他的项目及关注方向无关
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/coll_ai-video-generation)
> **来源仓库**：[skills-collective/skills](https://github.com/skills-collective/skills)
> **累计装机**：164,352　**官方**：否
> **同仓库另有 4 个也在榜**：`coll_ai-music`(#3)、`coll_ai-image-generation`(#4)、`coll_image-to-video`(#5)、`coll_video-edit`(#6)

> [!info]+ **#7 · 本周 101,120 装** · ↓1 | find-skills
> **是什么**：自动发现并安装社区 Claude skills 的工具
> **跟你的关系**：🟢 `可直接用` — 可接入其 Claude Code skill 工程化流水线，自动搜索安装所需技能。
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/find-skills)
> **来源仓库**：[vercel-labs/skills](https://github.com/vercel-labs/skills)
> **累计装机**：2,817,008　**官方**：是
> **原始描述**：Discovers and installs community skills from the public registry. Use when the user mentions a technology, framework, or task that could benefit from specialized skills not yet installed, asks 'how do I do X', 'find a skill for X', or starts work in a new technology area. Proactively suggest when th

> [!info]+ **#8 · 本周 99,136 装** · 🆕 | sleek-design-mobile-apps
> **是什么**：用自然语言创建移动应用界面并管理 Sleek 项目
> **跟你的关系**：⚫️ `无关` — 移动端UI设计，不在其七个关注方向内。
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/sleek-design-mobile-apps)
> **来源仓库**：[design-layers/agent-skills](https://github.com/design-layers/agent-skills)
> **累计装机**：140,724　**官方**：否
> **原始描述**：Use when the user wants to design a mobile app, create screens, build UI, or interact with their Sleek projects. Covers high-level requests ("design an app that does X") and specific ones ("list my projects", "create a new project", "screenshot that screen").

> [!info]+ **#9 · 本周 86,056 装** · 🆕 | anti-ui-slop
> **是什么**：避免 AI 生成千篇一律的俗套 UI 的 Claude skill
> **跟你的关系**：⚫️ `无关` — 侧重UI设计质量，与其财务/agent/量化等关注方向无关。
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/anti-ui-slop)
> **累计装机**：133,097　**官方**：否

> [!info]+ **#10 · 本周 64,738 装** · 🆕 | grill-me
> **是什么**：让 AI 不断追问，帮你压力测试计划或设计
> **跟你的关系**：🟢 `可直接用` — 可接入其 Claude Code skill 工程化流水线，做计划/设计压力测试。
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/grill-me)
> **来源仓库**：[mattpocock/skills](https://github.com/mattpocock/skills)
> **累计装机**：752,348　**官方**：否
> **原始描述**：Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".

> [!note] 上榜名次未做任何调整——同一来源仓库的多个 skill 只是合并到首条卡片里展示，共折叠 4 条。

## 本周冒头最快（按环比增速，不计入 TOP10）

> [!info]+ **#1 · 本周 54,816 装 · 16.6x** · 🆕 | ui-radar
> **是什么**：描述信息不足，需点开原链接
> **跟你的关系**：⚫️ `无关` — 从名称看偏UI方向，与其关注方向无明确关联。
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/ui-radar)
> **累计装机**：73,202　**官方**：否
> **同仓库另有 1 个也在榜**：`anti-ui-slop`(#2)

> [!info]+ **#3 · 本周 50,220 装 · 2.1x** · 🆕 | ai-video-generation
> **是什么**：通过命令行调用 RunComfy 生成 AI 视频，支持多种视频模型。
> **跟你的关系**：⚫️ `无关` — 视频生成不在其关注方向内，与财务、Agent、测试等均无关联。
> **链接**：🔗 [skills.sh](https://www.skills.sh/s/ai-video-generation)
> **来源仓库**：[magentosh/skills](https://github.com/magentosh/skills)
> **累计装机**：88,742　**官方**：否
> **同仓库另有 1 个也在榜**：`remotion-render`(#4)
> **原始描述**：Generate AI videos on RunComfy via the `runcomfy` CLI — a smart router across the full video-model catalog: HappyHorse 1.0 (Arena #1, native in-pass audio), Wan-AI Wan 2-7 (open weights, audio-driven lip-sync), ByteDance Seedance v2 / 1-5 / 1-0 (multi-modal cinematic), Kling 3.0 / 2-6, Google Veo 3-

> [!note] 上榜名次未做任何调整——同一来源仓库的多个 skill 只是合并到首条卡片里展示，共折叠 2 条。

> [!note] 另有 6 个 skill 上周装机量为 0、本周才有数据——除以 0 算不出环比，所以没进上面的增速排名，列在这里不丢信息：
> [coll_ai-video-generation](https://www.skills.sh/s/coll_ai-video-generation)（skills-collective/skills，本周 123,057 装）；[coll_ai-music](https://www.skills.sh/s/coll_ai-music)（skills-collective/skills，本周 122,903 装）；[coll_ai-image-generation](https://www.skills.sh/s/coll_ai-image-generation)（skills-collective/skills，本周 122,324 装）；[coll_image-to-video](https://www.skills.sh/s/coll_image-to-video)（skills-collective/skills，本周 112,062 装）；[coll_video-edit](https://www.skills.sh/s/coll_video-edit)（skills-collective/skills，本周 111,541 装）；[sleek-design-mobile-apps](https://www.skills.sh/s/sleek-design-mobile-apps)（design-layers/agent-skills，本周 99,136 装）

## 数据源失败

> [!warning] skill 描述补全（claudeskills.info + SKILL.md 两路）
> **报错**：8 条没拿到描述：skills-collective/skills/coll_ai-video-generation、skills-collective/skills/coll_ai-music、skills-collective/skills/coll_ai-image-generation、skills-collective/skills/coll_image-to-video、skills-collective/skills/coll_video-edit、uizze.com/anti-ui-slop
> **影响**：这些条目的 AI 解读会写「描述信息不足」，榜单与排名本身完整
> **地址**：`https://claudeskills.info/api/v1/search`

---

数据来源致谢：GitHub 全站榜取自 [isboyjc/github-trending-api](https://github.com/isboyjc/github-trending-api)（MIT）的 Actions 产物；skills 装机量取自 [skills.sh](https://skills.sh/)；skill 描述取自 [claudeskills.info](https://claudeskills.info/) 公开 API。
