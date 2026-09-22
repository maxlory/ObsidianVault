---
title: AI HOT Daily 2026-09-22
date: 2026-09-22
tags:
  - aihot
  - daily
  - research-radar
---

# 2026-09-22 AI HOT Daily

## AI HOT 官方日报

### 模型发布/更新

> [!info]+ **可延后 / 71** | Qwen 开源 Qwen-Image-2.1：7B 统一生成与编辑并原生支持透明图像
> **标题**：Qwen 开源 Qwen-Image-2.1：7B 统一生成与编辑并原生支持透明图像
> **原文链接**：🔗 [打开原文](https://qwen.ai/blog?id=qwen-image-2.1)
> **source**：AI HOT Daily / Qwen：Blog Retrieval（API）
> **kind**：`model`
> **reason**：AI HOT official daily section: 模型发布/更新
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：Qwen 团队开源 Qwen-Image-2.1，将文生图与图像编辑统一到一个模型中，视觉生成组件仅 7B 参数，并原生支持生成和编辑透明图像。模型支持最多 10 张参考图、圆形/涂鸦/独立蒙版指定局部编辑，通过混合粒度注意力架构和 KV cache 复用提升推理效率，同时改进文字渲染、人像光照与人物产品保真度，并覆盖全景图、信息图和分镜等任务。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

> [!info]+ **可延后 / 71** | 阶跃星辰发布旗舰模型 Step 5 Preview，10 月 15 日开源权重
> **标题**：阶跃星辰发布旗舰模型 Step 5 Preview，10 月 15 日开源权重
> **原文链接**：🔗 [打开原文](https://mp.weixin.qq.com/s?__biz=MzkyNTYxNzg5Mg%3D%3D&mid=2247488120&idx=1&sn=8ba9ac7f0b36682d6262290677c665da)
> **source**：AI HOT Daily / 公众号：阶跃星辰（Step）
> **kind**：`model`
> **reason**：AI HOT official daily section: 模型发布/更新
> **follow_up**：检查模型卡、license、benchmark 和可部署性。
> **summary**：阶跃星辰发布旗舰基座模型 Step 5 Preview，采用稀疏 MoE 架构，总参数量 600B、激活 27B，支持 100 万 Token 上下文和文本与视觉输入，在 Artificial Analysis Intelligence Index 得 44 分，居全球开源模型前三，单任务成本为 Claude Opus 5 的 1/8。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### 产品发布/更新

> [!info]+ **可延后 / 66** | Qwen-Image-2.1 已支持 ComfyUI，开源权重开放下载
> **标题**：Qwen-Image-2.1 已支持 ComfyUI，开源权重开放下载
> **原文链接**：🔗 [打开原文](https://x.com/Alibaba_Qwen/status/2101670814953455780)
> **source**：AI HOT Daily / X：通义千问 / Qwen (@Alibaba_Qwen)
> **kind**：`product`
> **reason**：AI HOT official daily section: 产品发布/更新
> **follow_up**：判断是否需要沉淀为长期主题笔记。
> **summary**：Qwen 宣布 Qwen-Image-2.1 现已支持 ComfyUI，权重开放。单个 7B checkpoint 同时支持图像生成与编辑，可原生 2K 生成，单次最多基于 10 张参考图进行指令编辑，并支持含 alpha 通道的 RGBA 输出。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略

### matches topics: openai

> [!info]+ **今日必须看 / 76** | 独立调查：ChatGPT 的 __obi 跨站 Cookie 可将站外浏览行为关联到 ChatGPT 账号
> **标题**：独立调查：ChatGPT 的 __obi 跨站 Cookie 可将站外浏览行为关联到 ChatGPT 账号
> **原文链接**：🔗 [打开原文](https://www.buchodi.com/chatgpt-now-knows-what-you-do-on-other-websites-via-ad-collector)
> **source**：AI HOT Daily / Hacker News 热门（buzzing.cc 中文翻译）
> **kind**：`paper`
> **reason**：matches topics: openai
> **follow_up**：阅读摘要和方法，判断是否需要建立永久论文笔记。
> **summary**：作者通过自己手机上的流量捕获复现了 OpenAI 广告收集器机制：bzr.openai.com 在 .openai.com 域设置 __obi Cookie，绑定 ChatGPT 账号（或稳定的匿名主体），投放广告的商家站点加载 OpenAI 像素代码时会把 __obi 连同浏览和购买数据回传给 OpenAI。
>
> **人工选择**：
> - [ ] 纳入长期知识库
> - [ ] 稍后复盘
> - [ ] 忽略
