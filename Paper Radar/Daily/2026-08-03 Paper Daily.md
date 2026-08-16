---
title: Paper Daily 2026-08-03
date: 2026-08-03
tags:
  - paper-radar
  - daily
---

# 2026-08-03 Paper Daily

## 今日概览

- 种子推荐：10 篇（候选池 60 篇，去重掉 20 篇看过的）
- 人工精选补充：4 篇（HF Daily Papers 当期 50 篇里挑）
- 种子来自你方法论里的 **26 篇 arXiv 引用**（改方法论加引用，这里自动跟上）
- 与方法论的关系：支持 2 篇　补充 4 篇　无关 8 篇
- **可升级技巧库：0 条**（今天没有够硬的）
- AI 解读：正常（14 篇全部有解读）
- 数据源失败：0 项
- 分组说明：**按「跟你方法论的关系」分组呈现，不是打分排序**。`可验证结论` 一栏没有数字的，说明摘要里就没有，需要点开原文——这一栏不允许编数字。

## 🟢 给你现有主张补上了实验证据（2 篇）

这几篇能把技巧库里的条目从「我知道」升级成「可验证」。

> [!info]+ **被引 1** | LLM-Based Scientific Peer Review: Methods, Benchmarks, and Reliability Challenges
> **发现了什么**：LLM 能写流利评审、分数接近人类，但作为决策支持的可靠性/鲁棒性/安全性仍不够清楚。
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`支持`
> **可验证结论**：摘要无具体数字，需读全文；综述显示 LLM 评审可生成流利批评、分数接近人类，但可靠/鲁棒/安全仍不够清楚。
> **原文**：🔗 [2606.25057](https://arxiv.org/abs/2606.25057)
> 发表 2026-06-23　作者 Thi Huyen Nguyen、Zahra Ahmadi

> [!info]+ **被引 0** | LLM Judges Can Be Too Generous When There Is No Reference Answer
> **发现了什么**：无参考答案时，LLM评审会过度给错误答案打高分；加入参考答案会翻转判断。
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`支持`
> **可验证结论**：跨三种语言，无参考答案时LLM judges过度给错误答案高分，添加参考答案后判断翻转（摘要无具体数字，需读全文）。
> **原文**：🔗 [2607.12885](https://arxiv.org/abs/2607.12885)
> 发表 2026-07-14　作者 Chalamalasetti Kranti、Sowmya Vajjala

## 🔵 同一节里的新角度（4 篇）

挂得上章节，但讲的是你现在没写的东西。

> [!info]+ **被引 0** | Can LLMs Be Constrained to the Past? Improving Knowledge Cutoff through Recall-Based Prompting
> **发现了什么**：让模型先复述截止约束或回忆截止前有效信息，比直接回答/逐步推理更守截止时间，反事实题提升最大。
> **挂到**：`§4.3 基点 1 · 检索与生命周期`　**关系**：`补充`
> **可验证结论**：摘要只给 3 个 benchmark，无效果数字，需读全文；SR/QR 均优于直接回答和逐步推理，反事实题提升最强。
> **原文**：🔗 [2606.05804](https://arxiv.org/abs/2606.05804)
> 发表 2026-06-04　作者 Michiro Asai、Ailiang Lin、Yuma Kishimoto、Takao Obi

> [!info]+ **被引 0** | CAVEWOMAN: How Large Language Models Behave Under Linguistic Input and Output Compression
> **发现了什么**：输出压缩多数模型省成本 1.4-2.4 倍（最高 3 倍），输入压缩反而严格双输。
> **挂到**：`§4 技巧库`　**关系**：`补充`
> **可验证结论**：在 8 模型×5 数据集×5 压缩级别下，输出压缩对多数 API 模型省成本 1.4-2.4 倍（最高 3 倍），四个开放权重模型也省成本；输入压缩为严格双输（摘要未给具体数值）
> **原文**：🔗 [2606.24083](https://arxiv.org/abs/2606.24083)
> 发表 2026-06-23　作者 Morayo Danielle Adeyemi、Ryan A. Rossi、Franck Dernoncourt

> [!info]+ **被引 1** | Operadic consistency: a label-free signal for compositional reasoning failures in LLMs
> **发现了什么**：OC信号无需标签即可通过组合一致性检测推理失败，与准确率强相关
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`补充`
> **可验证结论**：在12个4B-671B指令微调LLM、4个多跳QA数据集上，OC与准确率相关系数r∈[0.86,0.94]
> **原文**：🔗 [2606.13649](https://arxiv.org/abs/2606.13649)
> 发表 2026-06-11　作者 Nathaniel Bottman、Yinhong Liu、Kyle Richardson

> [!info]+ **社区 46 赞** | BM25 Wins at Scale: A Scaling Study of Retrieval-Augmented Generation Paradigms
> **发现了什么**：RAG范式无普适最优，存在规模依赖交叉；File-System Agent最小规模领先但查询token高39倍
> **挂到**：`§4.3 基点 1 · 检索与生命周期`　**关系**：`补充`
> **可验证结论**：在28个严格嵌套语料规模（跨度约450倍）的对比中，File-System Agent在最小共享规模领先，但其顺序探索的查询token消耗高39倍（原文句子截断，需核对具体规模条件）
> **原文**：🔗 [2607.26497](https://arxiv.org/abs/2607.26497)
> 发表 2026-07-30

## ⚪️ 挂不上方法论（只列标题）（8 篇）

如实归档，不硬凑关联。

- [SPARK: Security Knowledge Priming and Representation-Guided Knowledge Activation for LLM-based Secure Code Generation](https://arxiv.org/abs/2606.16244)　`被引 0`
- [FormalRx: Rectify and eXamine Semantic Failures in Autoformalization](https://arxiv.org/abs/2607.04655)　`被引 0`
- [LexRubric: A Rubric-Guided Diagnostic Benchmark for Open-Ended Legal Tasks](https://arxiv.org/abs/2606.09389)　`被引 0`
- AbstractReasoner at SemEval-2026 Task 11: Reducing Content Effects via Knowledge Distillation and Structured Reasoning Prompts　`被引 0`
- [Re-Centering Humans in LLM Personalization](https://arxiv.org/abs/2606.06614)　`被引 1`
- [Beacon: Knowing When and How to Perform Agentic Visual Reasoning](https://arxiv.org/abs/2607.28595)　`社区 48 赞 · 配套仓库 2⭐`
- [CLBench-V: Evaluating Multimodal Context Learning from Grounding to Knowledge Acquisition](https://arxiv.org/abs/2607.25294)　`社区 44 赞 · 配套仓库 3⭐`
- [Flux-OPD: On-Policy Distillation with Evolving Contexts](https://arxiv.org/abs/2607.28022)　`社区 40 赞`

---

信源：[Semantic Scholar 推荐 API](https://api.semanticscholar.org/)（种子取自 `~/.claude/methodology/方法论体系.md`）+ [HuggingFace Daily Papers](https://huggingface.co/papers)。想精读某篇，用 [karpathy/nanochat](https://github.com/karpathy/nanochat) 的 `read-arxiv-paper` skill。
