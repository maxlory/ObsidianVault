---
title: Paper Daily 2026-08-02
date: 2026-08-02
tags:
  - paper-radar
  - daily
---

# 2026-08-02 Paper Daily

## 今日概览

- 种子推荐：10 篇（候选池 60 篇，去重掉 10 篇看过的）
- 人工精选补充：4 篇（HF Daily Papers 当期 50 篇里挑）
- 种子来自你方法论里的 **26 篇 arXiv 引用**（改方法论加引用，这里自动跟上）
- 与方法论的关系：推翻 1 篇　支持 4 篇　补充 4 篇　无关 5 篇
- **可升级技巧库：3 条**　→ 见 [[技巧库待升级]]
- AI 解读：正常（14 篇全部有解读）
- 数据源失败：0 项
- 分组说明：**按「跟你方法论的关系」分组呈现，不是打分排序**。`可验证结论` 一栏没有数字的，说明摘要里就没有，需要点开原文——这一栏不允许编数字。

## 🔴 推翻或收窄了你现有的主张（1 篇）

这几篇最该先看——它们说的跟你方法论里写的不一样，或者划出了你没注意到的边界。

> [!info]+ **被引 0** | The Consistency Dilemma in LLMs: Generator-Evaluator Agreement and Vulnerability to Mistakes
> **发现了什么**：生成-评估自我一致性越高，模型在临床错误场景中反而越脆弱。
> **挂到**：`§8 AI 审核的真实机制（实测 + 文献）`　**关系**：`推翻`
> **可验证结论**：✅ 在 10 个前沿模型、491 个概念上，生成器-评估器自一致性越高，模型在医生确认错误上的脆弱性越高（摘要无效应量）。
> **原文**：🔗 [2606.30653](https://arxiv.org/abs/2606.30653)
> 发表 2026-06-16　作者 Marina Mancoridis、Zoë Hitzig

## 🟢 给你现有主张补上了实验证据（4 篇）

这几篇能把技巧库里的条目从「我知道」升级成「可验证」。

> [!info]+ **被引 1** | When LLMs Read Tables Carelessly: Measuring and Reducing Data Referencing Errors
> **发现了什么**：表格任务上所有模型都会错引/漏引数值；critic 过滤+拒绝采样最高提准确率12%。
> **挂到**：`§4.2 基点 1 · 输出侧：溯源契约`　**关系**：`支持`
> **可验证结论**：✅ 在表格数据引用任务上，所有测试模型（1.7B–20B 参数）都会出现数据引用错误；使用 critic 过滤和拒绝采样后，答案准确率最高提升 12.0%。
> **原文**：🔗 [2606.32029](https://arxiv.org/abs/2606.32029)
> 发表 2026-06-30　作者 Yuqing Yang、Qi Zhu、Zhen Han、Boran Han

> [!info]+ **被引 0** | An Empirical Study of Security Calibration in Large Language Models for Code
> **发现了什么**：LLM 对生成代码的安全性普遍过度自信，功能正确性校准比安全校准更差。
> **挂到**：`§9 反谄媚与不确定性表达`　**关系**：`支持`
> **可验证结论**：摘要无具体数字，需读全文
> **原文**：🔗 [2606.31159](https://arxiv.org/abs/2606.31159)
> 发表 2026-06-30　作者 Mohammed Latif Siddiq、Md Nafiu Rahman、Joanna C. S. Santos

> [!info]+ **被引 0** | Are We Evaluating Knowledge or Phrasing? Mitigating MCQA Sensitivity with ParaEval
> **发现了什么**：MCQA 标准评分受措辞影响，会虚假报告性能差距；多释义评分可缓解。
> **挂到**：`§1.1 四象限：锚点挂在"可验证"上，不挂在"我知道"上`　**关系**：`支持`
> **可验证结论**：✅ 在相同知识的 1B-8B 模型上，标准 MCQA 指标会虚假报告超过 2 点的性能差距
> **原文**：🔗 [2606.10657](https://arxiv.org/abs/2606.10657)
> 发表 2026-06-09　作者 João Janeiro、Mathurin Videau、Andrea Caciolai、Benjamin Piwowarski

> [!info]+ **被引 0** | Mitigating LLM Sycophancy in Code Smell Detection Using Evidence-Guided Reasoning Prompts
> **发现了什么**：LLM 做代码气味检测时会被用户预设带偏，证据引导推理可缓解
> **挂到**：`§9 反谄媚与不确定性表达`　**关系**：`支持`
> **可验证结论**：摘要无具体数字，需读全文
> **原文**：🔗 [2607.10411](https://arxiv.org/abs/2607.10411)
> 发表 2026-07-11　作者 Istiaq Ahmed Fahad、K. Asif、Mohammad A. Tawhid

## 🔵 同一节里的新角度（4 篇）

挂得上章节，但讲的是你现在没写的东西。

> [!info]+ **被引 0** | ConfidenceBench: Evaluating Confidence Calibration in Large Language Models
> **发现了什么**：口头置信度校准可用 Brier score 检验，且不需要 logits；基准覆盖 15 模型/200 题。
> **挂到**：`§9 反谄媚与不确定性表达`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文；实验条件为 15 个模型、200 道四类私有题、3 次独立运行。
> **原文**：🔗 [2607.20526](https://arxiv.org/abs/2607.20526)
> 发表 2026-07-10　作者 M. ffrench-Constant、Daniel Yang、Xinmeng Huang、Sanyam Kapoor

> [!info]+ **被引 1** | Steering LLM Viewpoints through Fabricated Evidence Injection
> **发现了什么**：伪造证据可操纵 LLM 观点，即使有安全分类器也只能降低、无法消除。
> **挂到**：`§4.3 基点 1 · 检索与生命周期`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文
> **原文**：🔗 [2606.06244](https://arxiv.org/abs/2606.06244)
> 发表 2026-06-04　作者 Xi Yang、Chang Liu、Zhenglin Huang、Haoran Li

> [!info]+ **被引 1** | Validating LLM-Generated SQL Queries through Metamorphic Prompting
> **发现了什么**：语法正确可执行的 SQL 也可能语义不符，需用输入变换+多次执行一致性来暴露
> **挂到**：`§3 ⚡ 验证阶梯`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文
> 发表 2026-06-30　作者 Li Lin、Qinglin Zhu、Jintai Hong、Chong Wang

> [!info]+ **被引 0** | When Models Hesitate: Answer Instability as a Label-Free Uncertainty Signal for LLMs
> **发现了什么**：重复生成同提示时，最终答案的变异性可作为无需内部信息的黑盒不确定性信号，性能接近语义熵。
> **挂到**：`§9 反谄媚与不确定性表达`　**关系**：`补充`
> **可验证结论**：摘要无具体数字，需读全文。
> 作者 J. M. Araña、Kristine Ann M. Carandang、E. R. Casin、Christian Alis

## ⚪️ 挂不上方法论（只列标题）（5 篇）

如实归档，不硬凑关联。

- The Inner Monologue of Language Models: When Reasoning Traces Reveal More Than They Hide　`被引 0`
- [PhiZero: A World Model Built Around Physical Language](https://arxiv.org/abs/2607.28624)　`社区 156 赞 · 配套仓库 37⭐`
- [DistillAlign: Coordinating Mode Covering and Mode Seeking in Autoregressive Video Distillation](https://arxiv.org/abs/2607.26811)　`社区 88 赞 · 配套仓库 89⭐`
- [VideoCoCo: Code-as-CoT for Physically-Consistent Video Generation via an Agentic Dual-Engine System](https://arxiv.org/abs/2607.27380)　`社区 64 赞 · 配套仓库 65⭐`
- [Memory Decoder at Scale: A Pretrained, Parametric Long-Term Memory](https://arxiv.org/abs/2607.27919)　`社区 48 赞 · 配套仓库 7⭐`

---

信源：[Semantic Scholar 推荐 API](https://api.semanticscholar.org/)（种子取自 `~/.claude/methodology/方法论体系.md`）+ [HuggingFace Daily Papers](https://huggingface.co/papers)。想精读某篇，用 [karpathy/nanochat](https://github.com/karpathy/nanochat) 的 `read-arxiv-paper` skill。
