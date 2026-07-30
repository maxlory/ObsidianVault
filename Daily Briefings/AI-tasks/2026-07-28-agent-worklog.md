---
date: 2026-07-28
type: agent-worklog
generated_at: 2026-07-28T20:01:19+08:00
sources: [claude-code, codex]
session_count: 7
misc_count: 48
coverage: "00:00-20:00"
extractor: claude-code-log
tags: [ai-tasks, agent-worklog]
---

> 关联：[[2026-07-28-plan]] ｜ [[2026-07-28-review]]

# 2026-07-28 Agent 工作日志

## 今日概览

今天核心工作是实习信息的自动化收集与提取，投入超15小时。完成了Wechat2RSS部署、7个公众号配置及168篇全文抓取，基于规则脚本的流水线产出84条高置信岗位和88条待复核记录。同时将微信公众号提取方案从规则切换为LLM，基于n8n改造后生成45个岗位+8条线索报告；Boss直聘爬虫启动64组批量爬取（4城市），完成13/64，处于中期。次要任务包括：自研Agent测试平台与第三方平台对比分析，93个共同题零分歧，生成两份HTML报告；quant-hypothesis-card skill通过评测（delta+0.42）；黄金卡权益PRD等待用户澄清决策点；密码学论文图片字体统一完成。

## 会话明细

### 09:30–17:01 ｜ Codex ｜ ~/Documents/Codex/2026-07-27/ai-1-2-llm-ai-3

**我已经拿到了Wechat2RSS的激活码，请你在这台电脑上开始部署**（done）

部署了Wechat2RSS 1.4.8并激活授权，配置7个公众号订阅，抓取168篇全文。随后设计并实现了基于规则脚本+Codex自动任务的实习信息提取流水线，生成v1.1 Schema，成功运行近一周预览：扫描169篇文章，输出84条高置信岗位和88条待复核记录，Markdown报告已保存至指定路径。

- 改动：`~/.agent-reach/n8n-internship/.env.example`, `~/.agent-reach/n8n-internship/.gitignore`, `~/.agent-reach/n8n-internship/README.md`, `~/.agent-reach/n8n-internship/docker-compose.yml`, `~/.agent-reach/n8n-internship/schemas/internship-extraction-v1.1.schema.json` 等 23 个
- 备注：关键决策：改用Codex自动任务替代n8n调度以减少容器依赖；字段设计采用结构化字段与原文引用分离避免重复。

### 09:44–10:14 ｜ Claude Code ｜ ~

**开始第一个skill的构建**（done）

完成了 quant-hypothesis-card skill 的构建和收口，通过了评测（delta +0.42），并修复了门禁 validator 相对路径 bug；随后用户尝试用该 skill 处理新问题，但因问题不明确未推进。

- 改动：`~/.claude/skills/quant-hypothesis-card/SKILL.md`, `~/.claude/skills/quant-hypothesis-card/evals/evals.json`, `~/xquant-learning/_my/skill提取/01-工序对照矩阵.md`, `~/xquant-learning/_my/skill提取/02-线性推进路线.md`, `~/xquant-learning/_my/skill提取/W1-立研究假设.md` 等 6 个
- 备注：修复了门禁 validator 中 os.path.exists() 相对路径解析的 bug。

### 13:29–16:30 ｜ Claude Code ｜ ~

**对比两个Agent测试平台的评测报告**（done）

完成自研Agent测试平台全量跑与多trial两份报告的对比分析，发现93个共同题的评判结果完全一致（0分歧），并生成两份HTML对比报告（首次报告排版不佳后用户要求重新生成，最终采纳模板风格重排），结果已保存至Downloads目录。

- 备注：两种平台（Langfuse与自研）题库不同无法直接对比，最终仅对比自研内部两版本报告。

### 14:35–19:57 ｜ Claude Code ｜ ~

**研究微信公众号实习信息提取方案**（done）

在 n8n-internship 项目中，完成了微信公众号实习信息提取方案的全面调研与改造：将主力提取从规则切换为 LLM，LLM 调用从 HTTP API key 改为 agent CLI 底座；跑了两天真实数据，生成 45 个岗位 + 8 条线索的报告；根据用户反馈删除了兴趣行、生成 AI 专版文档、过滤广告并保留社招信息。

- 改动：`~/.agent-reach/n8n-internship/README.md`, `~/.agent-reach/n8n-internship/config/interests.md`, `~/.agent-reach/n8n-internship/docker-compose.yml`, `~/.agent-reach/n8n-internship/scripts/run_pipeline.py`, `~/.agent-reach/n8n-internship/scripts/smoke_offline.py` 等 19 个
- 备注：关键决策：将提取主力从规则换成 LLM，消除规则静默漏检；LLM 调用改用 agent 订阅底座（claude CLI），无需 API key，token 开销可控。

### 17:12–19:37 ｜ Claude Code ｜ ~

**Boss直聘爬虫首次设置和实习机会爬取**（partial）

在 boss-zhipin-scraper 项目中完成了工作流沉淀（AGENTS.md），并基于扩展的金融/AI 关键词（40+）启动了 64 组批量爬取（4 城市），同时编写了过滤脚本和 HTML 报表预览页。爬取至 13/64，整体推进至中期，最终提供了原仓库链接。

- 改动：`~/boss-zhipin-scraper/AGENTS.md`, `/private/tmp/claude-501/-Users-aa00158/bd858a78-23a8-44c4-9c9d-ff397a721159/scratchpad/batch_search.py`, `/private/tmp/claude-501/-Users-aa00158/bd858a78-23a8-44c4-9c9d-ff397a721159/scratchpad/batch_search_2.py`, `/private/tmp/claude-501/-Users-aa00158/bd858a78-23a8-44c4-9c9d-ff397a721159/scratchpad/batch_search_v3.py`, `/private/tmp/claude-501/-Users-aa00158/bd858a78-23a8-44c4-9c9d-ff397a721159/scratchpad/batch_v4.py` 等 7 个
- 备注：决策：重点抓 AI+金融交叉岗位，采用分级过滤（A 级=双命中，B 级=单侧+关键词匹配），提升精准度。

### 19:28–20:00 ｜ Claude Code ｜ ~

**优化黄金卡权益产品需求方案**（partial）

尝试优化黄金卡权益产品需求方案，根据用户提供的原有PRD（CC-CHANGE-0722.md）及mentor修改意见，AI阅读了相关材料并识别出4个需要用户确认的决策点，但未生成最终修改计划，等待用户进一步澄清。

- 备注：用户首次请求被中断后重新发起，AI在计划生成前提出了4个待确认决策点。

### 19:32–19:52 ｜ Codex ｜ ~/Documents/Codex/2026-07-28/new-chat

**角色：假如你是一名经验丰富的密码学家**（done）

在密码学论文图片生成任务中，以71.png的字体为模板，将72、73、74三张图片的字体统一为相同的学术无衬线风格，生成了高清版并交付到输出目录。过程中因初次理解顺序错误重做，最终结果符合用户要求。

- 备注：第一次误将74.png作为模板，用户纠正后重新以71.png为基准处理，并修正了73.png中的数值错误。

## 其他

48 个碎会话 等 48 个：生成 AI 产品 PRD 文档；你好；角色：假如你是一名经验丰富的测试人员；标题：某券商研究所TMT组实习生招聘；回答 ok=true；确认 ok=true 响应；回答 ok=true；金融行业实习机会汇总与筛选
