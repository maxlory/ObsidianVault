---
title: Paper Daily 2026-08-01
date: 2026-08-01
tags:
  - paper-radar
  - daily
---

# 2026-08-01 Paper Daily

## 今日概览

- 种子推荐：10 篇（候选池 60 篇，去重掉 0 篇看过的）
- 人工精选补充：4 篇（HF Daily Papers 当期 50 篇里挑）
- 种子来自你方法论里的 **26 篇 arXiv 引用**（改方法论加引用，这里自动跟上）
- 与方法论的关系：支持 3 篇　补充 5 篇　无关 6 篇
- **可升级技巧库：2 条**　→ 见 [[技巧库待升级]]
- AI 解读：正常（14 篇全部有解读）
- 数据源失败：0 项
- 分组说明：**按「跟你方法论的关系」分组呈现，不是打分排序**。`可验证结论` 一栏没有数字的，说明摘要里就没有，需要点开原文——这一栏不允许编数字。

## 🟢 给你现有主张补上了实验证据（3 篇）

这几篇能把技巧库里的条目从「我知道」升级成「可验证」。

> [!info]+ **被引 1** | SLMJury: Can Small Language Models Judge as Well as Large Ones?
> **发现了什么**：多数小模型法官用 10-token 快速判断更好，过度思考效应因领域而异。
> **挂到**：`§7 够用就好的验证纪律`　**关系**：`支持`
> **可验证结论**：✅ 在 16 个 SLM judge、N=64,824 judgments/config 条件下，多数 judge 的 10-token 快速判断更好（overthinking 效应领域依赖）。
> **原文**：🔗 [2606.07810](https://arxiv.org/abs/2606.07810)
> 发表 2026-06-05　作者 Anish Laddha、Nitesh Pradhan、Gaurav Srivastava

> [!info]+ **被引 1** | Can LLMs Judge Better Than They Generate? Evaluating Task Asymmetry, Mechanistic Interpretability and Transferability for In-Context QA
> **发现了什么**：生成准确率在 4 个基准中有 3 个超过自评，自评时模型对上下文注意力少 3-5 倍。
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`支持`
> **可验证结论**：✅ 在 in-context QA 四个基准中，生成准确率在 3/4 任务上超过自评；自评时对上下文注意力低 3-5 倍，且几乎不读候选答案。
> **原文**：🔗 [2606.28050](https://arxiv.org/abs/2606.28050)
> 发表 2026-06-26　作者 Sambaran Bandyopadhyay

> [!info]+ **被引 0** | Summarization is Not Dead Yet
> **发现了什么**：人类参考摘要仍比LLM生成更信息丰富、更忠实；LLM只在表面流畅度上被偏好
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`支持`
> **可验证结论**：在5个数据集、5个LLM上结合人工评估+LLM-as-Judge+事实性核验，人类参考摘要的信息量/忠实度优于LLM，LLM仅流畅性占优；摘要无具体数字，需读全文
> **原文**：🔗 [2606.08000](https://arxiv.org/abs/2606.08000)
> 发表 2026-06-06　作者 Dongqi Liu、Chenxi Whitehouse、Zheng Zhao、Zhuchen Cao

## 🔵 同一节里的新角度（5 篇）

挂得上章节，但讲的是你现在没写的东西。

> [!info]+ **被引 1** | Ask, Don't Judge: Binary Questions for Interpretable LLM Evaluation and Self-Improvement
> **发现了什么**：把评估标准拆成原子化二元问题，再逐题聚合，能提升评估的可解释性和可调试性。
> **挂到**：`§4.5 路径 B：拆成 YES-NO 清单`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文
> **原文**：🔗 [2606.27226](https://arxiv.org/abs/2606.27226)
> 发表 2026-06-25　作者 Sangwoo Cho、Kushal Chawla、Pengshan Cai、Zefang Liu

> [!info]+ **被引 1** | When Does Intrinsic Self-Correction Help? A Task-Sensitive Analysis
> **发现了什么**：内在自校正的效果因任务结构而异，只在支持验证/复检/竞争策略的任务中稳定提升。
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文
> **原文**：🔗 [2606.23196](https://arxiv.org/abs/2606.23196)
> 发表 2026-06-22　作者 Elroy Stav、Dvir Berlowitz、Maayan Orner、Sarit Kraus

> [!info]+ **被引 0** | On the Limited Effect of Role Prompting in LLM-based Code Vulnerability Detection
> **发现了什么**：角色提示在漏洞检测任务上效果有限，六个模型七种角色设置未见提升。
> **挂到**：`§4.1 基点 1 · 输入侧`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文；实验涉及6个LLM、7种角色设置、Big-Vul三CWE类别。
> 发表 2026-06-01　作者 Jinyang Guan、Haoyu Wang、Ying Yin、Yuhai Zhao

> [!info]+ **被引 0** | Soft-Prompt Tuning for Fair and Efficient LLM Benchmark Evaluation
> **发现了什么**：基准分数会因格式遵循能力失真，仅调10个soft-prompt向量即可消除该偏差。
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`补充`
> **可验证结论**：仅优化10个soft-prompt向量（约0.0006%参数）即可让模型适应基准格式，但摘要未给消除偏差后的效果数字。
> **原文**：🔗 [2606.12117](https://arxiv.org/abs/2606.12117)
> 发表 2026-06-10　作者 Selen Erkan、Bastian Boll、Kristian Kersting、Bjorn Deiseroth

> [!info]+ **社区 289 赞 · 配套仓库 7⭐** | AskChem: Claim-Centered Infrastructure for Chemistry Literature Synthesis
> **发现了什么**：检索单位从论文改为带DOI和原文引用的原子化声明，便于跨论文综合答案。
> **挂到**：`§4.3 基点 1 · 检索与生命周期`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文。
> **原文**：🔗 [2607.28618](https://arxiv.org/abs/2607.28618)
> 发表 2026-07-30　配套仓库 https://github.com/bingyan4science/askchem

## ⚪️ 挂不上方法论（只列标题）（6 篇）

如实归档，不硬凑关联。

- [NuclearQAv2: A Structured Benchmark for Evaluating Domain-Science Competence in Large Language Models](https://arxiv.org/abs/2606.27047)　`被引 0`
- [Does Reasoning Preserve Alignment? On the Trustworthiness of Large Reasoning Models](https://arxiv.org/abs/2606.11046)　`被引 0`
- [More Yap Less Meaning: Uncovering Self-Improvement Behavior in SLMs](https://arxiv.org/abs/2606.08471)　`被引 0`
- [Qwen-UI-Agent Technical Report: Toward Next-Generation Real-World Centric Foundation GUI Agents](https://arxiv.org/abs/2607.28227)　`社区 277 赞`
- [Metis: Memory Foundation Model](https://arxiv.org/abs/2607.26760)　`社区 252 赞 · 配套仓库 51⭐`
- [Frontis-MA1: Training an AI4AI Model towards Recursive Self-Improvement in Machine Learning Engineering](https://arxiv.org/abs/2607.28568)　`社区 161 赞 · 配套仓库 91⭐`

---

信源：[Semantic Scholar 推荐 API](https://api.semanticscholar.org/)（种子取自 `~/.claude/methodology/方法论体系.md`）+ [HuggingFace Daily Papers](https://huggingface.co/papers)。想精读某篇，用 [karpathy/nanochat](https://github.com/karpathy/nanochat) 的 `read-arxiv-paper` skill。
