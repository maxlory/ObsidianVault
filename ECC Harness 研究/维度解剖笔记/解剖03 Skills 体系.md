# ECC Skills 体系解剖（维度 03）

> 研究对象：`/Users/aa00158/harness-research/ECC`（只读浅克隆，VERSION = `2.1.0`，HEAD = `e4e4163`）
> 目录：`ECC/skills/`（281 个）与 `ECC/.agents/skills/`（39 个）
> 术语先打底：**skill**（技能）= 一个文件夹，里面必须有一份 `SKILL.md`。`SKILL.md` 开头有一段 YAML「名片」（frontmatter），写 `name` 和 `description`；Claude Code 启动时只把这张名片读进上下文，正文要等它判断"这次任务用得上"才真正打开。这套机制官方叫 **progressive disclosure**（渐进式披露）——先给目录，用到才翻页。

---

## 0. 一句话结论

ECC 的 281 个 skill 本质是**一座按官方 frontmatter 规范写的 Markdown 知识库**：93% 的 skill 是纯一份 `SKILL.md`（无脚本、无子文件），中位 189 行，形态更接近"塞进 agent 的技术备忘录"而非"可执行程序"；它自创了 `metadata.origin` 做出处溯源（258 个 skill 在用），并在 `manifests/install-modules.json` 里把 281 个 skill 切成 19 个安装模块，<mark style="background:#d3f8b6">其中只有 `workflow-quality`（43 个）标了 defaultInstall: true</mark>。对本机用户而言：目录名硬撞车只有 **1 个**（`deep-research`，且是无备份硬覆盖），但 `--profile full` 会往 `~/.claude/skills/` 平铺 280 个目录，把目录数从 79 推到 358，常驻上下文的 name+description 从 2.46 万字符（≈6.2k token）涨到 9.32 万字符（≈23.3k token，占 200K 窗口的 11.6%）。

---

## 1. 两个 skills 目录的关系

### 1.1 `skills/` 是唯一权威源

- 281 个子目录，每个目录**必有且仅一份** `SKILL.md`（`find skills -name SKILL.md | wc -l` = 281，`find skills -type f | wc -l` = 452）。
- 全部 281 份 frontmatter 都能被正常解析（无一缺 `---` 包裹）——见下文 §4 的校验脚本结果。

### 1.2 `.agents/skills/` 是 39 个 skill 的「跨 harness 镜像」

`.agents/` 是 ECC 为**非 Claude 的 agent 运行器**（OpenAI Codex 一类）准备的平行副本：

- 只有 39 个 skill，全是 `skills/` 里挑出来的子集（`ls /Users/aa00158/harness-research/ECC/.agents/skills` = 39 项）。
- 内容几乎一致，但**去掉了 ECC 自创的 `metadata:` 块**。实测 `skills/api-design/SKILL.md` 是 524 行、frontmatter 里有 `metadata: origin: ECC`；`.agents/skills/api-design/SKILL.md` 是 522 行、frontmatter 只剩 `name` + `description`（两处文件对比，行数差 2 正好是被删的 2 行 metadata）。
- 多出一个 `agents/openai.yaml`，装 OpenAI 侧的展示与调用策略。例：`/Users/aa00158/harness-research/ECC/.agents/skills/api-design/agents/openai.yaml`

```yaml
interface:
  display_name: "API Design"
  short_description: "REST API design patterns and best practices"
  brand_color: "#F97316"
  default_prompt: "Use $api-design to design production REST API resources and responses."
policy:
  allow_implicit_invocation: true
```

> 白话：同一份技能文档在 Claude 世界叫 `SKILL.md` 靠 description 自动触发；到了 OpenAI 世界要额外声明"显示名叫什么、卡片什么颜色、默认怎么调、允不允许自动触发"，这就是 `openai.yaml` 干的事。`allow_implicit_invocation: true` 等价于 Claude 的"模型自己决定要不要用"。

- 38 个 skill 有 `agents/openai.yaml`（`find .agents/skills -name openai.yaml | wc -l`；总文件数 88 = 39 SKILL.md + 38 openai.yaml + 1 额外 `frontend-slides/STYLE_PRESETS.md`）。

### 1.3 结论

`.agents/skills/` **不是新内容**，是发行渠道差异。做体系分析只需看 `skills/`；做"多 harness 兼容"分析才需要看 `.agents/`。

---

## 2. 分类学：281 个 skill 按功能归类

### 2.1 用仓库自己的分类，不用我猜的

ECC 已经在 `manifests/install-modules.json` 里给出了官方分组（`kind: "skills"` 的 19 个 module），这是比关键词猜测更硬的证据。下表就是这份官方分组，我只补了中文名和代表 skill。

| #   | 模块 ID（`manifests/install-modules.json`） | 中文归类                | 数量      | `defaultInstall` | `cost`    | 代表 skill                                                                                  |
| --- | --------------------------------------- | ------------------- | ------- | ---------------- | --------- | ----------------------------------------------------------------------------------------- |
| 1   | `framework-language`                    | 语言/框架专项             | **69**  | false            | medium    | `react-patterns` `django-tdd` `kotlin-ktor-patterns` `angular-developer`                  |
| 2   | `workflow-quality`                      | 开发流程与质量（**唯一默认安装**） | **43**  | **true**         | medium    | `tdd-workflow` `verification-loop` `code-tour` `delivery-gate` `git-workflow`             |
| 3   | `agentic-patterns`                      | 元技能 / agent 自身工程    | **35**  | false            | medium    | `agent-harness-construction` `token-budget-advisor` `orch-*` 六件套 `prompt-optimizer`       |
| 4   | `security`                              | 安全与合规               | **19**  | false            | medium    | `security-review` `security-scan` `hipaa-compliance` `defi-amm-security`                  |
| 5   | `operator-workflows`                    | 连接外部 SaaS 的操作流      | **19**  | false            | medium    | `github-ops` `jira-integration` `email-ops` `google-workspace-ops`                        |
| 6   | `devops-infra`                          | 运维 / 网络 / 基础设施      | **16**  | false            | medium    | `docker-patterns` `kubernetes-patterns` `homelab-wireguard-vpn` `network-bgp-diagnostics` |
| 7   | `business-content`                      | 商业内容与市场             | **14**  | false            | **heavy** | `brand-discovery` `investor-materials` `market-research` `seo`                            |
| 8   | `research-apis`                         | 研究与学术数据源            | **9**   | false            | medium    | `deep-research` `exa-search` `scientific-db-pubmed-database`                              |
| 9   | `optimization-workflows`                | 性能/成本优化循环           | **8**   | false            | medium    | `benchmark-optimization-loop` `latency-critical-systems` `parallel-execution-optimizer`   |
| 10  | `media-generation`                      | 音视频/动画生成            | **8**   | false            | **heavy** | `remotion-video-creation` `manim-video` `videodb` `taste`                                 |
| 11  | `supply-chain-domain`                   | 供应链/制造业垂直域          | **8**   | false            | **heavy** | `inventory-demand-planning` `customs-trade-compliance` `production-scheduling`            |
| 12  | `database`                              | 数据库                 | **7**   | false            | medium    | `postgres-patterns` `redis-patterns` `prisma-patterns`                                    |
| 13  | `swift-apple`                           | Apple 生态            | **7**   | false            | medium    | `swiftui-patterns` `swift-concurrency-6-2` `liquid-glass-design`                          |
| 14  | `prediction-market-skills`              | 预测市场 / Itô 交易研究     | **6**   | false            | medium    | `ito-trade-planner` `prediction-market-risk-review`                                       |
| 15  | `machine-learning`                      | ML 工程               | **4**   | false            | medium    | `mle-workflow` `pytorch-patterns` `recsys-pipeline-architect`                             |
| 16  | `social-distribution`                   | 社交分发                | **3**   | false            | medium    | `crosspost` `x-api` `social-publisher`                                                    |
| 17  | `document-processing`                   | 文档处理                | **2**   | false            | medium    | `nutrient-document-processing` `visa-doc-translate`                                       |
| 18  | `skill-unified-memory`                  | 单件（跨 harness 记忆）    | **1**   | false            | light     | `unified-memory`                                                                          |
| 19  | `ito-compute`                           | 单件（Itô GPU 算力，需鉴权）  | **1**   | false            | light     | `ito-compute`                                                                             |
| —   | **未被任何模块引用**                            | 孤儿                  | **1**   | —                | —         | `skill-comply`                                                                            |
|     | **合计**                                  |                     | **281** |                  |           |                                                                                           |

校验：把 19 个模块 `paths` 里所有 `skills/*` 取并集 = 279 个（`skill-comply` 缺席，`skills/continuous-learning` 与 `unified-memory` 分散在别的 module 里），加上 `skill-comply` 与去重差异后与磁盘上 281 个目录对齐；脚本比对结果 "in skills/ but not in any module paths: ['skill-comply']"、"in modules but missing on disk: []"。

### 2.2 我自己的粗粒度归类（跨模块视角）

官方模块偏"安装打包"，不完全等于"功能"。按功能重新切：

| 功能大类 | 大致数量 | 说明 | 代表 |
|---|---|---|---|
| **语言/框架/数据库技术手册** | ~83 | `framework-language` 69 + `database` 7 + `swift-apple` 7，本质是"某技术栈的写法约定" | `postgres-patterns` `springboot-tdd` |
| **开发流程与质量门禁** | ~43 | TDD、验证循环、代码巡览、交付门禁 | `tdd-workflow` `delivery-gate` |
| **元技能（agent 造 agent）** | ~35 | 这是 ECC 最特别的一块：教 agent 怎么设计 agent、怎么省 token、怎么编排子 agent | `agent-harness-construction` `token-budget-advisor` |
| **安全与合规** | ~19 | 通用安全审查 + 行业合规（HIPAA / 医疗 PHI）+ 链上安全 | `security-review` `defi-amm-security` |
| **外部系统操作（Ops）** | ~19 | 需要真实凭证去操作 GitHub / Jira / Gmail 的流程 | `github-ops` `email-ops` |
| **基础设施与网络** | ~16 | 含一整套 homelab（家用实验室）网络技能 | `homelab-vlan-segmentation` |
| **研究与学术** | ~9 | 含 5 个 `scientific-*` 系列（PubMed / USPTO / gget 生信包） | `scientific-db-pubmed-database` |
| **商业/内容/营销** | ~17 | `business-content` 14 + `social-distribution` 3 | `brand-discovery` `crosspost` |
| **垂直行业域** | ~14 | 供应链 8 + 预测市场 6，明显是外部贡献者带进来的行业知识 | `energy-procurement` `ito-basket-compare` |
| **媒体生成** | ~8 | Remotion / Manim / Blender / VideoDB | `remotion-video-creation` |
| **性能与成本** | ~8 | benchmark 循环、并行执行、延迟敏感系统 | `benchmark-optimization-loop` |
| **ECC 自我管理** | ~7 | 装/配/审计 ECC 自己的技能 | `configure-ecc` `ecc-guide` `skill-stocktake` `skill-scout` `skill-comply` `config-gc` `ecc-tools-cost-audit` |
| **其他/长尾** | ~3 | ML 4、文档 2、单件 2 | `mle-workflow` `visa-doc-translate` |

> 🔑 值得注意的一条：`skills/` 里有 **7 个 skill 是用来管理 skill 本身的**（`skill-scout` 找技能、`skill-stocktake` 盘点、`skill-comply` 跑合规评测、`config-gc` 垃圾回收、`rules-distill` 从规则里蒸馏、`hookify-rules` 把规则变 hook、`ecc-tools-cost-audit` 审计工具成本）。这说明 ECC 已经大到"需要工具来管工具"的规模。

### 2.3 出处（`metadata.origin`）分布

frontmatter 里 ECC 自创的 `metadata.origin` 字段记录 skill 来源：

| origin 值 | 数量 |
|---|---|
| `ECC` | 207 |
| `community` | 42 |
| （无 origin 字段） | 16 |
| `ECC direct-port adaptation` | 8 |
| `Health1 Super Speciality Hospitals — contributed by Dr. Keyur Patel` | 4 |
| `oh-my-agent-check` / `Flox` / `ECC-community` / `"Ronald Skelton - Founder, RapportScore.ai"` | 各 1 |

（统计脚本对 281 份 frontmatter 逐个正则匹配 `origin:`）

> 白话：约 74% 是 ECC 自己写的，15% 来自社区 PR，还有 4 个医疗类 skill 直接标了投稿医生的名字。这也解释了为什么会有"能源采购""海关合规"这种和 coding 八竿子打不着的技能——它们是行业从业者贡献的。

---

## 3. 深读：9 个代表性 / 最"重"的 skill

挑选原则：覆盖"最短 / 最长 / 最工程化 / 最标准 / 最越界"四个极端。

### 3.1 `orch-pipeline` + `orch-*` 六件套 —— 「引擎 + 薄壳」编排模式

**文件**：`skills/orch-pipeline/SKILL.md`（122 行）、`skills/orch-add-feature/SKILL.md`（46 行）等 5 个薄壳。

这是 ECC 里唯一一个**显式的 skill-to-skill 继承结构**。玩法：

- `orch-pipeline` 是共享引擎，它的 description 直接写明「Not usually invoked directly」（`skills/orch-pipeline/SKILL.md:3`），并在正文再强调一次「Invoke an operation skill … rather than this engine directly」（同文件 :14-15）。
- 5 个操作 skill（`orch-add-feature` / `orch-change-feature` / `orch-fix-defect` / `orch-refine-code` / `orch-build-mvp`）各自只有 44-50 行，正文明确说自己是 "Thin wrapper over the shared engine in [`orch-pipeline`](../orch-pipeline/SKILL.md)"（`skills/orch-add-feature/SKILL.md:10-11`）。

引擎里的核心机制有三个，都值得单独看：

1. **Size classifier（体量分级）**——先给任务打 trivial / small / standard / large 四档，按"碰几个文件 / 有没有新依赖 / 设计有没有歧义"三条信号取最高档，然后**用档位决定跑哪几个阶段**（`skills/orch-pipeline/SKILL.md:39-54`）。trivial 直接跳到 4→5→6（实现→评审→提交），large 才走完整 1→2→3→4→5→6。
   > 白话：不是所有任务都值得先做调研再写计划。这张表就是"要不要走完整流程"的判据，避免改个错别字也让 agent 写 PRD。

2. **两道人工闸门（Gate）**——GATE 1 在出计划之后（未批准不许写实现代码），GATE 2 在提交之前（未确认不许 commit）。原文一句关键定性："This family is **gated, not autonomous**"（`skills/orch-pipeline/SKILL.md:78`）。
3. **Agent / command map**——每个阶段委派给谁写死在一张表里：Plan→`planner` agent、Implement→`tdd-guide` agent 或 `tdd-workflow` skill、Review→`code-reviewer` agent 或 `/code-review` 命令、Security→`security-reviewer`（`skills/orch-pipeline/SKILL.md:87-98`）。

**它没有做的事**：整个流程没有任何代码，没有状态文件。原文自己承认 "The pipeline carries no hidden state — the planning docs *are* the handoff"（:108）。也就是说这套编排完全靠模型读到这段文字后自觉执行，**没有任何机制保证它真的按顺序跑**。

### 3.2 `skill-comply` —— 全仓最工程化的一个，而且它是孤儿

**文件**：`skills/skill-comply/SKILL.md`（60 行）+ `scripts/`（10 个 Python 文件，1090 行含 prompts）+ `tests/`（3 个 pytest）+ `prompts/`（3 个 LLM 提示词）+ `fixtures/`（3 个测试夹具）+ `pyproject.toml`。

它解决的问题很尖锐：**skill 写了，agent 到底听不听？** 流程（`skills/skill-comply/SKILL.md:11-17`）：

1. 拿任意一份 `.md`（skill / rule / agent 定义）→ 用 LLM 反推出「本应发生的行为序列」（spec）——代码在 `scripts/spec_generator.py`（72 行）。
2. 自动生成 3 档"提示词严格度"的场景：supportive（明说要用）→ neutral（不提）→ competing（反向诱导）——`scripts/scenario_generator.py`（70 行）。
3. 起 `claude -p` 子进程真跑，用 `stream-json` 抓工具调用轨迹——`scripts/runner.py`（194 行）。
4. **用 LLM 而不是正则**把每次工具调用归类到 spec 的哪一步——`scripts/classifier.py`（85 行）。
5. 用确定性代码检查时序顺序、算合规率——`scripts/grader.py`（124 行）。
6. 出自带 spec + prompt + 时间线的报告——`scripts/report.py`（170 行）。

关键概念作者叫 **Prompt Independence**：「衡量的是即使提示词没有明说，skill/rule 会不会仍被遵守」（`skills/skill-comply/SKILL.md:45-47`）。

> 🔑 反讽点：这个唯一带完整测试套件、能量化"skill 有没有生效"的 skill，**没有被任何安装模块引用**（`manifests/install-modules.json` 里 281 个 skill 中唯一缺席的就是它）。装 ECC 装不到它。

### 3.3 `continuous-learning-v2` —— skill 外壳包着一整套后台学习系统

**文件**：`skills/continuous-learning-v2/SKILL.md`（362 行）+ `hooks/observe.sh` + `agents/`（observer.md + 3 个 shell）+ `scripts/`（instinct-cli.py 等 5 个）+ `config.json`。

机制本体（`skills/continuous-learning-v2/SKILL.md:83-125` 那张 ASCII 流程图）：

```
会话活动 → PreToolUse/PostToolUse hook 抓 prompt + 工具调用
        → 写入 projects/<项目哈希>/observations.jsonl
        → 后台 observer agent（跑 Haiku 模型）定期读取
        → 提炼出「instinct（本能）」：一条 trigger + 一条 action + 一个 0.3-0.9 置信度
        → /evolve 把相关 instinct 聚类，升格成真正的 skill / command / agent
```

三个值得记的设计：

- **项目隔离**：用 `git remote get-url origin` 哈希成 12 位项目 ID，React 项目学到的东西不会污染 Python 项目；同一条 instinct 在 2+ 项目出现且平均置信度 ≥0.8 才升格为全局（`SKILL.md:128-136`、`:288-290`）。
- **数据不放在 `~/.claude`**：明确说是为了绕开 Claude Code 的敏感路径保护，默认落在 `$XDG_DATA_HOME/ecc-homunculus`（`SKILL.md:138-145`）。
- **作者自己承认 skill 不可靠**，这是全仓最诚实的一句话（`SKILL.md:328-334`）：

> "v1 relied on skills to observe. Skills are probabilistic -- they fire ~50-80% of the time based on Claude's judgment." / "Hooks fire **100% of the time**, deterministically."

> 🔑 这句话是理解整个 ECC skills 体系的钥匙：**作者知道 skill 的触发率只有 50-80%**，所以凡是必须发生的事（观测、门禁、安全拦截）他都改用 hook，skill 只承担"知识供给"。

### 3.4 `angular-developer` —— progressive disclosure 的教科书样本

**文件**：`skills/angular-developer/SKILL.md`（155 行）+ `references/`（**35 个 .md**）。

SKILL.md 本体只干两件事：(1) 写死几条硬规则（"生成完代码必须跑 `ng build`，报错必须修完再往下"，`SKILL.md:25`；`ng new` 的三步版本判定逻辑，`:35-49`）；(2) 剩下全是**索引**——

```
- **Inputs**: Signal-based inputs, transforms, and model inputs. Read [inputs.md](references/inputs.md)
```

35 个 reference 覆盖 signals / forms / DI / routing / SSR / a11y / animations / testing / CLI。任务碰到哪块才读哪个文件。

> 白话：这就是官方 progressive disclosure （渐进式披露）的标准用法——门口挂一张 35 条的目录（155 行进上下文），真正的 3 万字内容躺在硬盘上等召唤。ECC 281 个 skill 里，**只有 8 个这么做**（见 §8.1）。

### 3.5 `remotion-video-creation` —— 44 行的纯路由器（但目录名不合规）

**文件**：`skills/remotion-video-creation/SKILL.md`（44 行）+ `rules/`（28 个 .md + 3 个 .tsx）。

正文除了 6 行说明，剩下 28 行全是链接列表（`SKILL.md:16-43`）。它和 angular-developer 是同一个模式，唯一差别是**参考资料目录叫 `rules/` 不叫 `references/`**——这在 Anthropic 官方惯例里是非标准命名（官方示例用 `references/`）。功能不受影响（相对链接照样能读），但工具化扫描时会漏掉。

它也是全仓 frontmatter 最不规范的之一：`metadata:` 下只有 `tags:`，没有 `origin`（`SKILL.md:4-6`）。

### 3.6 `laravel-security` —— 949 行，全仓最长，纯静态百科

**文件**：`skills/laravel-security/SKILL.md`（949 行，约 30 KB）。

结构是 12 个大节：Production Configuration → Authentication（Sanctum/密码/会话）→ Authorization（Gates/Policies/Middleware）→ Eloquent Security（批量赋值 / SQL 注入 / 属性转换）→ CSRF → XSS → Input Validation → API Security（限流/CORS）→ File Upload → Dependencies and Secrets（`grep -n '^#'` 结果）。

每节都是"可直接抄的 PHP 代码块 + 一句为什么"。没有任何脚本、没有 references、没有引用其他 skill。

> 这类"语言/框架百科"是 ECC 体量的主要来源：`framework-language` 模块 69 个 skill，长度榜前 12 里有 9 个属于这一类。一旦装上，它们不进上下文（只有 description 进），但一旦被触发，单个就要吃掉约 7500 token。

### 3.7 `delivery-gate` —— skill 里塞了一个 Stop hook

**文件**：`skills/delivery-gate/SKILL.md`（127 行）+ `hooks/quality-gate.py`。

它本身不是"知识"，是**一段 Python 门禁程序的说明书**。检查三件事（`SKILL.md:21-27`）：

| 检查 | 机制 | 命中后 |
|---|---|---|
| 合理化话术（"先跳过测试""这是历史 bug"） | 对 transcript 尾部跑正则 | 只警告，永不阻断 |
| 学习日志过期 | 5 个路径的文件 mtime | ≥3 个过期 或 growth-log 过期 → 阻断 |
| 磁盘 < 50GB | `shutil.disk_usage` | 警告 |
| 磁盘 < 15GB | 同上 | 阻断（exit 2） |

作者把它和另外两件明确分工（`SKILL.md:122-127`）：`delivery-gate` 管机器可验证的事实、`self-audit` 管推理质量、`verification-loop` 管 build/type/lint/test、`gateguard` 管 PreToolUse 安全。

它的自我限制写得很坦白（`SKILL.md:108-110`）："The hook enforces the **habit** of touching learning libraries, not the **quality** of what was recorded."

> 🔑 注意安装方式：SKILL.md 里让用户手动 `cp quality-gate.py ~/.claude/scripts/` 并自己编辑 `~/.claude/settings.json`（`SKILL.md:36-55`）。也就是说**装 skill 本身不会让门禁生效**，还得手工接线。

### 3.8 `ck`（Context Keeper）—— skill 里塞了一个 Node CLI

**文件**：`skills/ck/SKILL.md`（148 行）+ `commands/`（8 个 `.mjs`）+ `hooks/session-start.mjs`。

它是 `origin: community` 的外部项目（frontmatter 里带 `author: sreedhargs89`、`repo: https://github.com/sreedhargs89/context-keeper`，`skills/ck/SKILL.md:4-8`）。

工作方式很直白（`SKILL.md:12-15`）："When the user invokes any `/ck:*` command, run the corresponding Node.js script and present its stdout to the user verbatim."

description 里那句设计理由值得单独摘出来：

> "Commands run deterministic Node.js scripts — behavior is consistent across model versions."（`skills/ck/SKILL.md:3`）

只有 `/ck:save` 需要 LLM 参与（要总结会话），其余 7 个命令纯执行脚本（`SKILL.md:51-53`）。

> 这是 skill 的第三种形态：**不是知识、不是流程，而是一层"把 CLI 包装成自然语言接口"的适配器**。

### 3.9 `token-budget-advisor` —— 触发词工程做到极致的样本

**文件**：`skills/token-budget-advisor/SKILL.md`，description 长 **840 字符**（全仓第二长）。

它的 description 不是描述，是一份**触发规则表**（`skills/token-budget-advisor/SKILL.md:3-16`）：

```yaml
description: >-
  ...
  TRIGGER when: "token budget", "token count", "token usage", "token limit",
  "response length", ... "respuesta corta vs larga", "cuántos tokens",
  "ahorrar tokens", "responde al 50%", ...
  DO NOT TRIGGER when: user has already specified a level in the current
  session (maintain it), the request is clearly a one-word answer, or
  "token" refers to auth/session/payment tokens rather than response size.
```

正文的机制也很具体：先用 `words × 1.3`（散文）或 `chars / 4`（代码）估输入 token，再按复杂度乘 3×~40× 估输出窗口，然后给用户 4 档深度选项让他选（25%/50%/75%/100%）。它还显式复用另一个 skill 的口径："Use the same calibration guidance as [context-budget](../context-budget/SKILL.md)"。

> 这个写法（TRIGGER when / DO NOT TRIGGER when + 多语言触发词）在 281 个 skill 里是少数派：description 中位数只有 199 字符，超过 400 字符的只有 20 个。绝大多数 ECC skill 的 description 是"一句话说明用途"，没有做正/负触发词工程。


## 4. 是否遵循 Anthropic 官方 Agent Skills 规范

### 4.0 先立标尺：官方规范到底要求什么

对照基准取自本机 `claude-code-guide` MCP 的结构化索引 `skill_frontmatter_fields`（快照版本 3.41.1 / 2026-07-08），它把字段分成两组：

**agentskills.io 开放标准字段**
| 字段 | 规范要求 |
|---|---|
| `name` | 小写，1-64 字符，只能用连字符，**必须与目录名一致** |
| `description` | 什么时候该用这个 skill，**最长 1024 字符**，同时决定自动触发 |
| `allowed-tools` | 空格分隔的预授权工具，支持 `Bash(npm run *)` 这类通配 |
| `license` | 许可名或指向捆绑文件 |
| `compatibility` | 环境要求，最长 500 字符 |
| `metadata` | **任意键值对**（author、version 等都往这里塞） |

**Claude Code 私有扩展字段**：`effort`、`model`、`argument-hint`、`disable-model-invocation`、`context: fork`、`hooks`。

### 4.1 逐条判定

| 规范条目 | ECC 表现 | 判定 |
|---|---|---|
| 每个 skill 一个目录 + `SKILL.md` | 281/281 全部符合 | ✅ 完全符合 |
| frontmatter 有 `name` | 281/281 | ✅ |
| frontmatter 有 `description` | 281/281 | ✅ |
| `description` ≤ 1024 字符 | 最长 979（`loop-design-check`），**无一超标** | ✅ |
| `name` 必须等于目录名 | **5 个不符**（见下） | ❌ 局部违规 |
| `description` 用 inline / folded 标量（不能用 `\|` 字面块） | 267 inline + 16 folded（`>-`），**0 个用 `\|`** | ✅（且 CI 有专门校验，见 `scripts/ci/validate-skills.js`） |
| progressive disclosure（正文精简 + `references/` 分层） | **只有 8/281（2.8%）真正做了** | ⚠️ 大面积不遵循 |
| `references/` 子文件 | 8 个（外加 1 个用 `rules/` 目录名的变体） | ⚠️ |

**5 个 `name` ≠ 目录名的 skill**（全部 `origin: community` 的 `scientific-*` 系列）：

| 目录 | frontmatter `name` |
|---|---|
| `skills/scientific-db-pubmed-database/` | `pubmed-database` |
| `skills/scientific-db-uspto-database/` | `uspto-database` |
| `skills/scientific-pkg-gget/` | `gget` |
| `skills/scientific-thinking-literature-review/` | `literature-review` |
| `skills/scientific-thinking-scholar-evaluation/` | `scholar-evaluation` |

> 🔑 这不是纯洁癖问题。Claude Code 用 `name` 做 `/skill-name` 调用键，用目录名做文件定位。名字对不上，用户在斜杠菜单里看到的是 `literature-review`，但装到 `~/.claude/skills/` 里的目录是 `scientific-thinking-literature-review`——排障时对不上号。另外这 5 个短名字（`gget` / `literature-review` / `scholar-evaluation`）**撞车概率远高于带前缀的目录名**，见 §9。

### 4.2 CI 的实际卡点比规范松

`scripts/ci/validate-skills.js` 只强制两件事（第 120-150 行区间）：

1. 每个子目录必须有非空 `SKILL.md`（缺失/为空 = 硬错误 exit 1）；
2. frontmatter 若存在，必须有 `name`；`description` 不能用 `|` 字面块标量。

而且第 2 条**默认只是 WARN 不是 ERROR**——脚本注释原话：「Frontmatter findings default to WARN so CI does not break while pre-existing data defects are being cleaned up out of band (see #1663).」（`scripts/ci/validate-skills.js:15-18`）。要变成硬错误得加 `--strict` 或设 `CI_STRICT_SKILLS=1`。

**CI 完全不检查的事**：`description` 是否存在、`name` 是否等于目录名、`description` 是否超 1024、有没有非法字段。这解释了上面那 5 个 name 不匹配为什么能进主干。

### 4.3 progressive disclosure：概念遵循，实践只做了 3%

官方推荐的三层结构是「frontmatter（永远在上下文）→ SKILL.md 正文（触发才读）→ references/ 分文件（用到才读）」。ECC 绝大多数 skill 只做到两层——正文一读就是整块 189-949 行。

具体到数字：**8 个 skill 有 references/（2.8%）**，其中真正做出规模的只有 `angular-developer`（35 个）、`videodb`（10 个）、`brand-discovery`（8 个）、`openclaw-persona-forge`(6)、`tinystruct-patterns`(6)。相比之下，`laravel-security` 把 949 行全塞在一个文件里，`python-testing` 818 行同理。

> 🤔 这算不算问题，取决于用法：如果一次会话只会触发 1-2 个 skill，7500 token 的百科一次性读进来是可以接受的；但如果一个任务同时命中 `python-patterns` + `python-testing` + `security-review` + `verification-loop`，就是 2 万+ token 的正文一次性灌入。ECC 自己也意识到了，所以才有 `context-budget` 和 `token-budget-advisor` 两个 skill 专门管这件事。

---

## 5. 自创扩展字段清单

把 281 份 frontmatter 的键全部统计出来，按"是否在官方规范内"分类：

### 5.1 合规字段

| 字段 | 出现次数 | 说明 |
|---|---|---|
| `name` | 281 | 规范必填 |
| `description` | 281 | 规范必填 |
| `metadata`（顶层对象） | 259 | 规范允许任意键值对 |
| `license` | 18（MIT 10 / Apache-2.0 8） | 规范字段 |
| `argument-hint` | 3（`ecc-recipes` / `tdd-workflow` / `videodb`） | Claude Code 扩展字段 |
| `allowed-tools` | 2（`inherit-legacy-style` / `videodb`） | 规范字段 |

### 5.2 ECC 自创 / 放错位置的字段（**均非官方顶层字段**）

| 字段 | 次数 | 谁在用 | 问题 |
|---|---|---|---|
| `version`（顶层） | **29** | `delivery-gate` `ck` `continuous-learning-v2` `hipaa-compliance` 等 | 规范说 version 应放 `metadata:` 里。ECC 有 1 个 skill 确实放对了（`metadata.version`），其余 29 个放顶层 |
| `tools`（顶层） | **11** | `agent-eval` `eval-harness` `skill-comply` `gan-style-harness` `parallel-execution-optimizer` `latency-critical-systems` `data-throughput-accelerator` `recursive-decision-ledger` `benchmark-optimization-loop` `agent-architecture-audit` `blender-motion-state-inspection` | **拼错了键名**——规范是 `allowed-tools`，`tools` 是 **agent（subagent）** 的字段。这 11 个 skill 的工具限制大概率不生效（未验证：需要实跑 Claude Code 观察是否报警） |
| `homepage` | 8 | 8 个 `supply-chain-domain` skill，全指向 `https://github.com/affaan-m/everything-claude-code` | 非规范字段 |
| `origin`（顶层） | 7 | `agent-self-evaluation` `taste` `vue-patterns` `react-native-patterns` `ml-adoption-playbook` `mailtrap-email-integration` `ecc-recipes` | ECC 自己的约定字段放错层级（其余 258 个放在 `metadata.origin`） |
| `author`（顶层） | 5 | `ck`(sreedhargs89) `ecc-recipes`(KyawZinLatt) `motion-*` 三件套(jeff) | 应放 metadata |
| `tags`（顶层） | 3 | `motion-foundations` `motion-patterns` `motion-advanced` | 非规范 |
| `category` | 3 | 同上三个，值都是 `frontend` | 非规范 |
| `repo` | 2 | `ck`（GitHub URL）、`agent-eval`（值是 `./my-project`，看着像模板没填干净） | 非规范 |

### 5.3 `metadata` 里的 ECC 自定义子键（这些是合法的）

| 子键 | 次数 | 用途 |
|---|---|---|
| `metadata.origin` | **258** | ECC 的核心自创约定：标记 skill 来源（ECC / community / 具体贡献者），驱动 §2.3 那张出处表 |
| `metadata.author` | 9 | 贡献者名 |
| `metadata.clawdbot` | 8 | 给 **Clawdbot**（另一个 harness）用的展示元数据，目前 8 个供应链 skill 里都只有一个空 `emoji: ""`。例：`skills/energy-procurement/SKILL.md:16-17` |
| `metadata.equivalents` / `metadata.version` / `metadata.tags` | 各 1 | 零散 |

> 🔑 一句话总结自创扩展：**ECC 唯一成体系的自创字段是 `metadata.origin`（出处溯源），其余"自创"基本是把 version/author/tags 放错了层级，以及把 `allowed-tools` 写成了 `tools`。** 后者是 11 个 skill 上的实质性 bug，不是风格问题。

---

## 6. skill 之间怎么互相调用 / 编排

### 6.1 硬事实：ECC 的 skill 之间**没有程序化调用机制**

我扫了全部 281 份 SKILL.md：

- `grep -ric 'Skill('` → **0 命中**。没有任何 skill 通过 Claude Code 的 `Skill` 工具显式调用另一个 skill。
- 相对路径链接 `../<name>/SKILL.md` → 只有 **9 个 skill** 用，共指向 11 个目标（最多的是 `orch-pipeline` 5 次、`accessibility` 5 次）。
- 反引号提名（正文里写 `` `security-review` `` 这种）→ 427 条边，**110 个 skill（39%）至少提到 1 个别的 skill**。

所以编排是**三层"软"机制**，从强到弱：

| 层级 | 机制 | 强度 | 例子 |
|---|---|---|---|
| **L1 文件链接** | Markdown 相对路径，模型可以 Read 过去 | 中（模型看得懂，但要不要读全凭自觉） | `skills/orch-add-feature/SKILL.md:11` → `../orch-pipeline/SKILL.md` |
| **L2 提名引用** | 正文里写另一个 skill 的名字，指望模型自己去加载 | 弱 | `skills/orch-pipeline/SKILL.md:93` "delegate to `tdd-guide` (or `tdd-workflow` skill)" |
| **L3 委派给 agent/command** | 让模型去起 subagent 或跑斜杠命令 | 中（Task 工具/命令是真机制） | `skills/orch-pipeline/SKILL.md:87-98` 那张 agent map |

### 6.2 引用图谱（谁是枢纽）

**被引用最多（inbound，即"大家都说要配合我用"）**：

| 次数 | skill |
|---|---|
| 16 | `security-review` |
| 13 | `verification-loop` |
| 12 | `tdd-workflow` / `backend-patterns` / `brand-voice` |
| 11 | `frontend-patterns` |
| 10 | `content-engine` |
| 9 | `x-api` |
| 7 | `knowledge-ops` / `market-research` |

**引用别人最多（outbound，即"路由器型"）**：

| 次数 | skill | 性质 |
|---|---|---|
| **49** | `configure-ecc` | ECC 的配置向导，本质是全站目录 |
| **36** | `mle-workflow` | ML 工作流总纲，把 ML 各环节指给别的 skill |
| 10 | `taste` | 审美判断，引 10 个媒体/前端 skill |
| 7 | `ecc-tools-cost-audit` | 成本审计 |
| 6 | `orch-pipeline` / `prompt-optimizer` / `email-ops` / `codehealth-mcp` / `product-capability` / `automation-audit-ops` | 各自领域的调度中心 |

（数据来自我写的图谱脚本：对每份 SKILL.md 正文提取反引号包裹的 token，与 281 个目录名求交集）

### 6.3 唯一一个"正经的"编排结构：`orch-*` 家族

见 §3.1。核心设计是「一个引擎 SKILL.md + 5 个 44-50 行的薄壳」，薄壳只负责三件事：选体量档位、选阶段掩码（phase mask，例如 `orch-add-feature` 是 `0→1→2→4→5→6`，跳过 Scaffold）、定义"第一步动作"（新功能先写新的失败测试 / 修 bug 先复现成失败测试）。

这套模式值得学的地方：**共享的复杂逻辑只写一遍，变体只写差异**。值得警惕的地方：**它没有任何执行保证**——引擎里写的 "GATE 1"、"phase mask" 全是给模型看的文字约定，没有 hook 拦、没有状态机。

### 6.4 ECC 自己的补救：把"必须发生的事"从 skill 挪到 hook

这是全仓最重要的设计判断，白纸黑字写在 `skills/continuous-learning-v2/SKILL.md:328-334`：

> "v1 relied on skills to observe. Skills are probabilistic -- they fire ~50-80% of the time based on Claude's judgment." → "Hooks fire **100% of the time**, deterministically."

所以在 ECC 里：

- **知识 / 建议 / 流程模板** → skill（可以不触发，损失可接受）
- **观测、门禁、拦截、审计** → hook（`skills/delivery-gate/hooks/quality-gate.py`、`skills/continuous-learning-v2/hooks/observe.sh`、`skills/ck/hooks/session-start.mjs`）

有 3 个 skill 干脆把 hook 脚本打包在自己目录里，安装 skill 之后还要用户手工把脚本 cp 出去 + 改 `settings.json` 才生效（`skills/delivery-gate/SKILL.md:36-55`）。

---

## 7. skills 与 commands / agents / rules 的分工边界

### 7.1 四个抽屉各有多少东西

| 目录 | 数量 | 触发方式 | 内容形态 |
|---|---|---|---|
| `skills/` | **281** | 模型自己判断（靠 description） | Markdown 知识 / 流程 |
| `commands/` | **94** | 用户敲 `/xxx` | Markdown 提示词模板 |
| `agents/` | **67** | 主模型用 Task 工具派发 subagent | Markdown + frontmatter(name/description/tools/model) |
| `rules/` | 23 个语言/主题子目录 | 始终生效（注入 CLAUDE.md 层） | Markdown 硬规则 |
| `hooks/` + `scripts/hooks/` | hooks.json + 一堆 Node 脚本 | 事件触发，100% 确定性 | 可执行代码 |

（数量来自 `ls commands \| wc -l` = 94、`ls agents \| wc -l` = 67、`ls rules`）

### 7.2 官方文档里的边界定义

ECC 自己的 `CLAUDE.md` 给了一句最直接的分工（"Architecture" 段）：

- `agents/` — Specialized subagents for delegation
- `skills/` — Workflow definitions and domain knowledge
- `commands/` — Slash commands invoked by users
- `rules/` — Always-follow guidelines
- `hooks/` — Trigger-based automations

用一句白话翻译这五个抽屉的区别：**rules 是"永远遵守"，hooks 是"到点必须跑"，commands 是"我喊你才来"，skills 是"你自己觉得该用就用"，agents 是"你派个分身去干"。**

### 7.3 但边界在实践中是糊的——三处重叠

**重叠 1：同名的 skill 和 command 并存**
`orch-pipeline` 里明说 orch-* 家族是在 "compose existing ECC commands rather than replace them: `/feature-dev`, `/plan`, `/code-review`, `/build-fix`, `/refactor-clean`, `/gan-build`, plus the `tdd-workflow` skill"（`skills/orch-pipeline/SKILL.md:33-37`）。也就是同一件事（加功能）有两条路：斜杠命令 `/feature-dev`，和 skill `orch-add-feature`。区别只在后者多了体量分级和两道闸门。

**重叠 2：skill 目录里塞 command**
`skills/ck/commands/` 装了 8 个 `.mjs`，通过 `/ck:*` 调用（`skills/ck/SKILL.md:12-15`）。`skills/continuous-learning-v2/` 定义了 `/instinct-status` `/evolve` `/promote` 等 6 个命令（`SKILL.md:207-216`）。

**重叠 3：skill 目录里塞 agent 和 hook**
- `skills/lead-intelligence/agents/` 有 4 个 agent 定义（enrichment-agent.md 等）
- `skills/continuous-learning-v2/agents/observer.md` + 3 个 shell
- `skills/delivery-gate/hooks/quality-gate.py`、`skills/ck/hooks/session-start.mjs`、`skills/continuous-learning-v2/hooks/observe.sh`

> 🔑 结论：ECC 实际上把 skill 目录当成了**"能力包（capability bundle）"**——一个 skill 目录里可以同时装知识文档、参考资料、可执行脚本、subagent 定义和 hook。这超出了 Anthropic 官方对 skill 的设定（官方 skill 是"知识 + 可选脚本"），但因为多出来的部分都要用户手工接线（cp 脚本、改 settings.json），所以不会破坏兼容性——只是装了 skill 不等于那些能力生效。

### 7.4 三种 skill 的"户口"：curated / learned / imported / evolved

`docs/SKILL-PLACEMENT-POLICY.md` 定义了一套溯源制度，这是 ECC 少见的成体系设计：

| 类型 | 位置 | 随仓库发布 | 溯源要求 |
|---|---|---|---|
| Curated（策展） | 仓库 `skills/` | 是 | 不强制，用 frontmatter `origin` 标注 |
| Learned（学来的） | `~/.claude/skills/learned/` | 否 | **必须**有 `.provenance.json` |
| Imported（外部导入） | `~/.claude/skills/imported/` | 否 | **必须**有 `.provenance.json` |
| Evolved（instinct 进化来的） | `~/.local/share/ecc-homunculus/.../evolved/skills/` | 否 | 继承 instinct 出处 |

`.provenance.json` 必填 4 个字段：`source` / `created_at` / `confidence`（0-1）/ `author`，schema 在 `schemas/provenance.schema.json`，校验代码在 `scripts/lib/skill-evolution/provenance.js`。

配套还有一个 skill 健康度子系统 `scripts/lib/skill-evolution/`（6 个文件：dashboard / health / index / provenance / tracker / versioning），入口 `scripts/skills-health.js` 支持 `--dashboard`、按面板看 success-rate / failures / amendments / versions。

> 白话：ECC 不只是堆 skill，它还建了一套"skill 户籍 + 体检"制度——每个非官方 skill 要登记来源和置信度，并且能统计每个 skill 被调用后的成功率。这套东西在 281 个 Markdown 里是含金量最高的一块工程。（未验证：`skills-health.js` 依赖一个 skill run JSONL 日志，我没确认这个日志由谁写入、默认是否开启。）

## 8. 统计：scripts / references / SKILL.md 长度分布

### 8.1 结构统计（硬数据）

| 指标 | 数值 | 佐证 |
|---|---|---|
| skill 总数 | **281** | `ls /Users/aa00158/harness-research/ECC/skills \| wc -l` |
| `SKILL.md` 总数 | 281（每个目录恰好 1 份） | `find skills -name SKILL.md \| wc -l` |
| skills/ 下文件总数 | 452 | `find skills -type f \| wc -l` |
| 带 `scripts/` 的 skill | **8**（2.8%） | 见下表 |
| 带 `references/` 或 `reference/` 的 skill | **8**（2.8%） | 见下表 |
| 带其他子目录的 skill | 9（`hooks/` 3、`agents/` 2、`commands/`/`tests/`/`prompts/`/`fixtures/`/`examples/`/`templates/`/`rules/`/`assets/` 各 1） | `find skills -mindepth 2 -maxdepth 2 -type d` |
| **完全裸的 skill（只有一份 SKILL.md，无任何子文件）** | **≈262（93%）** | 452 总文件 − 281 SKILL.md = 171 个附属文件，集中在 ~19 个 skill 上 |

**带 `scripts/` 的 8 个**（`find skills -mindepth 2 -maxdepth 2 -type d -name scripts`）：
`agent-self-evaluation`、`continuous-learning-v2`、`frontend-slides`、`ios-icon-gen`、`rules-distill`、`skill-comply`、`skill-stocktake`、`videodb`

**带 `references/`（或 `reference/`）的 8 个**：
`agent-self-evaluation`(2)、`angular-developer`(**35**)、`brand-discovery`(8)、`brand-voice`(1)、`openclaw-persona-forge`(6)、`taste`(1)、`tinystruct-patterns`(6)、`videodb`(`reference/` 10)

**用非标准目录名承载"参考资料"的 1 个**：`remotion-video-creation` 用 `rules/`（28 个 md + 3 个 tsx），功能等同 references/，但目录名不符官方惯例（`skills/remotion-video-creation/rules/`）。

### 8.2 SKILL.md 长度分布（行数）

| 统计量 | 值 |
|---|---|
| 最短 | **35 行** — `skills/nanoclaw-repl/SKILL.md` |
| p10 | 77 行 |
| p25 | 126 行 |
| **中位数** | **189 行** |
| 平均 | 261.8 行 |
| p75 | 344 行 |
| p90 | 565 行 |
| 最长 | **949 行** — `skills/laravel-security/SKILL.md` |

直方图（行数区间 / skill 数）：

```
<100      47  ███████████████████████
100-199  101  ██████████████████████████████████████████████████
200-299   47  ███████████████████████
300-399   29  ██████████████
400-499   20  ██████████
500-699   20  ██████████
>=700     17  ████████
```

最长的 12 个：`laravel-security`(949)、`windows-desktop-e2e`(889)、`kotlin-testing`(826)、`generating-python-installer`(821)、`python-testing`(818)、`quarkus-tdd`(813)、`data-scraper-agent`(766)、`kubernetes-patterns`(757)、`python-patterns`(752)、`django-patterns`(736)、`django-tdd`(731)、`cpp-coding-standards`(725)。

最短的 10 个：`nanoclaw-repl`(35)、`orch-change-feature`(44)、`orch-fix-defect`(44)、`remotion-video-creation`(44)、`orch-refine-code`(45)、`orch-add-feature`(46)、`continuous-agent-loop`(47)、`orch-build-mvp`(50)、`enterprise-agent-ops`(52)、`ai-first-engineering`(53)。

> 🔑 一个明显的双峰：**要么是 40-50 行的"路由器"**（`orch-*` 六件套只负责把任务转给别的东西），**要么是 700+ 行的"百科全书"**（语言/框架手册）。中间那一坨 100-200 行才是典型 skill。

### 8.3 体积（字符 / 估算 token）

| 指标 | 值 |
|---|---|
| 281 份 SKILL.md 正文总字符 | **2,476,175**（约 2.4 MB） |
| 单份中位字符 | 6,752 |
| 单份最大字符 | 30,297 |
| **估算总 token（按 4 字符/token 粗算）** | **≈ 62 万 token** |
| 281 份 frontmatter 的 `name`+`description` 总字符 | **68,557** → **≈ 1.7 万 token** |
| description 长度中位 | 199 字符；最长 979 字符（`skills/loop-design-check/SKILL.md`）；最短 63 字符（`skills/verification-loop/SKILL.md`） |

> 白话：正文 62 万 token 是"图书馆总藏书量"，平时不进上下文；**真正每次会话都要付的税是那 1.7 万 token 的名片墙**。这个数字后面 §9 会用到。


## 9. 必答：全装进 `~/.claude/skills/` 对本机意味着什么

### 9.1 先搞清楚"全装"到底装多少

装法有档次之分，这点很关键（数据来自 `node scripts/install-plan.js --profile <p> --target claude --json`，纯只读计划器）：

| profile | 总文件操作数 | 其中往 `~/.claude/skills/` 复制的 skill 目录数 |
|---|---|---|
| `minimal` | 55 | **44** |
| `core` | 58 | 44 |
| `developer`（README 推荐给多数人） | 140 | 约 120 |
| `full` | 301 | **280** |

- 目标根目录写死是 `/Users/aa00158/.claude`，每个 skill 落成 `~/.claude/skills/<skill-name>`——**平铺，没有任何命名空间前缀**（`scripts/install-apply.js:35` 帮助文本原话："Install ECC into ~/.claude/ with managed rules under rules/ecc and **flat skills under skills/**"）。
- 281 里有 1 个（`skill-comply`）不在任何模块，装不到，所以上限是 280。
- 🔑 **哪怕选 `minimal` 也会装 44 个 skill**——`workflow-quality` 模块是 `manifests/install-profiles.json` 里每个 profile 都带的（除 `full` 单列），没有"只装规则不装 skill"的档位。

### 9.2 名称撞车检查（实测）

**方法**：`comm -12 <(ls ECC/skills | sort) <(ls ~/.claude/skills | sort)`，另外再用 frontmatter 的 `name` 字段（即真正的调用键）做第二轮交集。

**结果：目录名交集只有 1 个 —— `deep-research`。**

第二轮，把范围扩大到"本次会话里所有可用 skill 名"（包含 plugin 与内置，共 93 个名字），交集变成 **2 个**：

| 撞名 | 你这边是什么 | ECC 那边是什么 | 后果 |
|---|---|---|---|
| **`deep-research`** | `~/.claude/skills/deep-research/`——你的 13-agent 学术研究流水线，7 种模式（full/quick brief/paper review/lit-review/fact-check/Socratic/systematic review + meta-analysis），中英双语触发词 | `skills/deep-research/SKILL.md`——"Multi-source deep research using firecrawl and exa MCPs" | **同路径 → 直接被覆盖** |
| **`security-review`** | 内置/插件的 `security-review`（不在 `~/.claude/skills/` 里，是 harness 自带的） | `skills/security-review/SKILL.md`——"adding authentication, handling user input, working with secrets…" | 不覆盖文件，但**会话里出现两个同名 skill**，触发时哪个胜出未验证 |

**覆盖是硬覆盖，没有备份。** 安装器最终落盘的那一行是 `fs.copyFileSync(operation.sourcePath, operation.destinationPath)`（`scripts/lib/install/apply.js:235`）；`merge-json` 分支才会先读旧值合并（:202-207），普通文件复制**不检查目标是否存在、不写 `.bak`**。所以：

**哪些 profile 真的会覆盖 `deep-research`？实测逐个跑了一遍**（`node scripts/install-plan.js --profile <p> --target claude --json`，只看计划不写盘）：

| profile | 装的 skill 数 | 覆盖 `deep-research` | 带 `security-review` |
|---|---|---|---|
| `minimal` | 44 | ❌ 否 | ❌ |
| `core` | 44 | ❌ 否 | ❌ |
| `developer` | 121 | ❌ 否 | ❌ |
| **`research`** | 70 | ✅ **是** | ❌ |
| **`full`** | 280 | ✅ **是** | ✅ |

> ⚠️ 也就是说：`full` 和 `research` 这两个 profile 会把你的 `~/.claude/skills/deep-research/SKILL.md` 直接写掉。你那个 13-agent 学术流水线定义**不可恢复**（除非你自己有 git 备份）。`developer` / `core` / `minimal` 不碰它。

**覆盖之后会变成什么样（这点比"被覆盖"更糟）**：

- 你的 `~/.claude/skills/deep-research/` 是一个完整包：`SKILL.md` 488 行 / 25,724 字节 + `agents/`（13 个 agent 定义：research_question_agent、socratic_mentor_agent、meta_analysis_agent、risk_of_bias_agent…）+ `references/`（20 个：APA7 指南、PRISMA 系统综述协议、逻辑谬误、伦理清单…）+ `examples/` + `templates/`。
- ECC 的 `skills/deep-research/SKILL.md` 只有 **160 行，且没有任何子目录**。
- 安装器是**逐文件 copy**（`scripts/lib/install/apply.js:235`），不是"删目录再重建"。所以结果是：**ECC 的 160 行 SKILL.md 覆盖掉你的 488 行，但你的 13 个 agent 和 20 个 reference 文件全部留在原地成为孤儿**——没有任何东西再引用它们，占着磁盘、不进上下文、也修不回来。这是一个"半死不活"的混合体，比整目录被换掉更难排查。

**再看那 5 个 name≠目录名的 skill**（§4.1）：它们的真实调用名分别是 `pubmed-database`、`uspto-database`、`gget`、`literature-review`、`scholar-evaluation`。你没有这些名字，暂时不撞。但 `literature-review` 和 `scholar-evaluation` 这种**通用短名**，和你的 `academic-paper`（含 lit-review 模式）、`academic-paper-reviewer` 是同一战场——见下节。

### 9.3 description 触发冲突：真正的风险不在撞名，在撞语义

我做了 67×281 的 description 关键词 Jaccard 重叠计算，超过 0.10 的只有 17 对，最高 0.161。**字面重叠低**。但 Jaccard 只能抓词面，抓不到"两个 skill 抢同一类任务"。按你的实际工作域逐个对：

| 你的战场 | 你的 skill | ECC 会来抢的 | 冲突性质 |
|---|---|---|---|
| **深度调研** | `deep-research` `github-research` `agent-reach` | `deep-research`(被覆盖) `research-ops` `exa-search` `search-first` `iterative-retrieval` `documentation-lookup` `market-research` | 🔴 **最高**。ECC 的 `search-first` 描述是 "Research-before-coding workflow… Invokes the researcher agent"，会和你的 `github-research`（"新需求先找现成实现"）正面抢 |
| **skill 工程** | `skill-builder` `skill-description-optimizer` `skill-optimization-loop` `name-description-authoring` `engineered-skill-patterns` `skill-curator` | `skill-scout`（"Use when the user wants to **create, build, fork, or find a skill**"）`skill-stocktake`（审计 skill 质量）`prompt-optimizer` `rules-distill` `config-gc` | 🔴 **最高**。`skill-scout` 的触发条件和你的 `skill-builder` 几乎逐字重合，`skill-stocktake` 和你的 `skill-curator` 同理 |
| **竞品分析** | `competitor-profiling` `competitive-brief` | `competitive-platform-analysis` `competitive-report-structure` `market-research` `lead-intelligence` | 🟠 高。实测 Jaccard 0.127（`competitor-profiling` vs `competitive-report-structure`），是全部 17 对里排第 5 的 |
| **需求 → PRD → 任务** | `to-prd` `to-issues` `user-story-writer` `review-requirements` `deliver-acceptance-criteria` `deliver-edge-cases` `project-lifecycle` | `intent-driven-development`（"turn ambiguous changes into scoped, **verifiable acceptance criteria**"）`blueprint` `orch-build-mvp` `product-lens` `contract-first` `plan-orchestrate` `plan-canvas` | 🔴 **最高**。`intent-driven-development` vs 你的 `deliver-acceptance-criteria` 实测 Jaccard 0.105，共享词包括 acceptance/criteria/testable/verifiable/requirements |
| **前端测试** | `frontend-test-pipeline` | `e2e-testing` `browser-qa` `click-path-audit` `windows-desktop-e2e` `ai-regression-testing` | 🟠 高。你的 skill 专走 TestHub + 元素库，ECC 的 `e2e-testing` 是通用 Playwright 模式，模型很可能选错 |
| **原型/设计** | `prototype-designer` | `design-system` `frontend-design-direction` `liquid-glass-design` `make-interfaces-feel-better` `ui-demo` `ui-to-vue` `frontend-slides` `motion-*`(4) | 🟠 高。`frontend-design-direction` 的触发条件是"building or improving websites, dashboards, applications, components, landing pages…"——**范围极宽**，会大面积抢 |
| **学术论文** | `academic-paper` `academic-paper-reviewer` `academic-pipeline` `ml-paper-writing` `systems-paper-writing` | `scientific-thinking-literature-review`（真名 `literature-review`）`scientific-thinking-scholar-evaluation`（真名 `scholar-evaluation`）`scientific-db-pubmed-database` `article-writing` | 🟠 高。`scholar-evaluation` 描述里的 "papers, proposals, literature reviews, methods sections, evidence quality, citation support" 和你的 `academic-paper-reviewer` 高度同域 |
| **演讲/幻灯** | `presenting-conference-talks` | `frontend-slides` | 🟠 实测 Jaccard 0.106，共享 pptx/presentation/slides/talk |
| **量化** | `quant-hypothesis-card` | `ito-*`(5) `prediction-market-*`(2) `llm-trading-agent-security` | 🟡 中。ECC 这批是预测市场/链上，和 A 股短线不同域，但"回测/风险/策略"字眼会撞 |

**冲突量级的定量估计**（置信度 中，~60%）：

- ECC 的 281 个 description 里，包含 `agent` 的 57 个、`skill` 40 个、`test` 41 个、`workflow` 37 个、`review` 32 个、`search` 27 个、`design` 26 个、`plan` 23 个、`security` 22 个、`optimiz` 22 个、`analy` 20 个、`research` 15 个。这些正是你自建 skill 的高频触发词。
- 我的判断：**装 full 之后，你现有 67 个 skill 里大约 20-25 个（30%-37%）会遇到"新增了一个描述相近的竞争者"**。其中 `deep-research` / `skill-builder` / `to-prd`+`deliver-*` / `frontend-test-pipeline` / `prototype-designer` 这 5 条线是重灾区。
- 自己验证的办法：装之前先跑一遍你的 `skill-description-optimizer` 或 `skill-optimization-loop`，用你现有的 benchmark 题集打基线；装完（或在一个隔离的 `.claude` 目录里模拟）再跑一次，看触发准确率掉多少。这是唯一能把"我猜 30%"变成数字的做法。

### 9.4 context 膨胀量级（可精算的部分）

Claude Code 的机制是：**每个 skill 的 `name` + `description` 常驻系统提示词，正文按需加载**。所以膨胀量 = 所有 description 的字符数。

| | skill 数 | name+description 总字符 | 估算 token（÷4） |
|---|---|---|---|
| 你现在的 `~/.claude/skills/`（有 SKILL.md 的） | 67 | 24,603 | **≈ 6,150** |
| ECC 全量 | 281 | 68,557 | **≈ 17,140** |
| **装 full 之后合计** | **≈ 346** | **≈ 93,160** | **≈ 23,290** |
| 倍数 | ×5.2 | ×3.8 | **×3.8** |

（还没算 plugin 与内置那 ~39 个 skill 的 description，那部分你已经在付了。）

**换算成钱和上下文**：

- 常驻 +1.7 万 token，是 200K 上下文窗口的 **8.6%**——每一轮对话都要重复付。
- 按 Opus 输入价粗算（未核实当前价目，仅作量级感），每 100 轮对话多烧 170 万 input token。虽然 prompt caching 能吃掉大部分成本，但 **上下文窗口的占用是省不掉的**。
- 真正贵的是被触发之后：ECC 单个 skill 正文中位 6,752 字符（≈1,700 token），像 `laravel-security` 一个就 30,297 字符（≈7,600 token）。如果一次任务误触发 3 个百科型 skill，直接吃掉 2 万 token 正文。

**注意力稀释（无法量化，但机制清楚）**：从 67 个候选里挑 1 个，和从 346 个候选里挑 1 个，是完全不同难度的分类问题。你自建的 skill 大多做了触发词工程（你的 `agent-reach` / `quant-hypothesis-card` description 都写了大段 MUST USE / Do NOT use），而 ECC 的 description 中位只有 199 字符、绝大多数没有负向触发词——**这意味着 ECC 的 skill 更容易在边界情况被误选**（因为它们的描述宽泛、边界不清）。

### 9.5 还有三个非 skill 的副作用（装了就一起来）

`--profile full --target claude` 的 301 个文件操作里，skill 只占 280。剩下 21 个包括：

- `rules/` → `~/.claude/rules/ecc/`（ECC 的规则包，是"永远生效"级别的注入）
- `.agents/` + `agents/` + `AGENTS.md` → `~/.claude/`（67 个 agent 定义）
- `commands/` → `~/.claude/commands/`（94 个斜杠命令）
- `hooks/` + `scripts/hooks/` + `scripts/lib/` → hook 运行时（`hooks-runtime` 模块）
- `platform-configs` 会走 **`merge-json`** 路径去合并你的配置文件（`scripts/lib/install/apply.js:190-208`）——这是唯一会读旧值再合并的操作，其余全是覆盖。

> ⚠️ 也就是说这不是"装一批 skill"，是**把一整套 harness 配置铺到你的 `~/.claude/` 上**，包括规则和 hook。你自己的 `~/.claude/rules/`（presentation.md / work-principles.md / project-lifecycle.md）不会被覆盖（ECC 的规则去 `rules/ecc/` 子目录），但 `~/.claude/commands/` 和 hook 配置会有交叉。（未验证：我没逐条比对 ECC 的 94 个 command 文件名和你现有 `~/.claude/commands/` 的交集。）

### 9.6 结论与可操作建议

**结论**：名称撞车不是主要风险（只有 1 个硬撞：`deep-research`，且会被无备份覆盖）。真正的代价是 **+1.7 万常驻 token（上下文 +8.6%）** 和 **20-25 个自建 skill 面临语义竞争者**。

**如果一定要用 ECC 的东西，按性价比排序**：

1. **别用 `install.sh`，用 `--skills <id>` 单件装**。安装器支持逐个装：`install.sh --skills continuous-learning-v2`（`scripts/install-apply.js:30,54`）。281 个 skill 每个都有对应的 `skill:<name>` 组件 ID（`node scripts/install-plan.js --list-components` 输出末尾那 200 多条）。
2. **装之前先备份 `~/.claude/skills/deep-research/`**，或者干脆先把它改名。
3. **值得单件借鉴的（我的排序）**：`skill-comply`（唯一能量化"skill 到底有没有生效"的工具，且它没被打包进任何 profile，只能手动 copy）、`orch-pipeline`（体量分级 + 双闸门的编排模板）、`delivery-gate`（Stop hook 门禁思路）、`docs/SKILL-PLACEMENT-POLICY.md` + `scripts/lib/skill-evolution/`（skill 溯源与健康度制度）。
4. **不值得装的**：281 个里约 83 个是语言/框架/数据库百科（`framework-language` + `database` + `swift-apple`），这类知识 Claude 本身就有，装进来纯粹是常驻 token 成本；14 个垂直行业 skill（供应链/预测市场）和你的工作完全无关。

## 10. 仓库声称 vs 代码核实

| README 的说法 | 出处 | 核实结果 |
|---|---|---|
| "281 skills" | `README.md:119,124` | ✅ **属实**。`ls skills \| wc -l` = 281，且每个目录都有非空 SKILL.md |
| "67 agents, 94 legacy command shims" | `README.md:119` | ✅ 属实（`ls agents`=67、`ls commands`=94） |
| "Skills … **Loaded when the task needs them**"（表格里对 skill 的 context behavior 描述） | `README.md:809` | ⚠️ **半真**。正文确实按需加载，但**每个 skill 的 name+description 是无条件常驻的**（281 个合计约 1.7 万 token）。README 这一行没提这笔固定成本 |
| "Rules … Always loaded, **so install them selectively**" | `README.md:811` | ✅ 这句反而是诚实的——它对 rules 提醒了选择性安装，但对 skills 没有给同样的提醒，尽管 skills 的常驻描述成本更大 |
| 未在 README 中找到任何"省 X% token"的量化宣传 | 全文 grep `%` + `token` | — |

---

## 11. 未验证项与开放问题

以下是我**没能**从代码或文档确认的，标注置信度供交叉核对：

1. **11 个用 `tools:` 而非 `allowed-tools:` 的 skill，工具限制是否真的失效？**（置信度 中 ~60% 认为失效）——需要实跑 Claude Code 装一个（比如 `agent-eval`）并观察它能不能调用未列出的工具。相关文件：`skills/agent-eval/SKILL.md` 等 11 个。
2. **同名 skill（`security-review`）同时来自内置和 `~/.claude/skills/` 时谁胜出？** 我只做了名称交集，没找到 Claude Code 的解析优先级文档。需要实测或查官方 precedence 文档。
3. **`scripts/skills-health.js` 依赖的 "skill run JSONL" 由谁写入、默认开不开？** 我读了 CLI 参数（`--runs-file`）但没追到写入方。若默认不开，整套 skill 健康度统计就是空跑。
4. **`.agents/skills/` 的 39 个 skill 是怎么从 `skills/` 同步过来的？** 我没找到同步脚本（可能在 `scripts/harness-adapter-compliance.js` 或 `scripts/gemini-adapt-agents.js` 附近），也就无法判断这份镜像会不会漂移。
5. **`~/.claude/commands/` 的交叉冲突**——我没把 ECC 的 94 个 command 文件名和用户现有的 command 做交集，§9.5 的提醒是基于机制推断，不是实测。
6. **"20-25 个自建 skill 会遇到语义竞争者"这个数字是我的判断，不是测量结果**（置信度 中 ~60%，我已按偏高习惯向下修过一档）。可验证动作见 §9.3 末尾：用 `skill-optimization-loop` 的 benchmark 跑装前/装后对比。
7. **`orch-*` 家族的两道 Gate 实际执行率未知**——SKILL.md 里全是文字约定，没有任何 hook 或状态机保证。讽刺的是仓库里正好有个工具能测这件事（`skill-comply`），但它没被打包进任何安装 profile。

---

## 12. 给读者的三条速记

1. **ECC 的 skill 体系是"广度优先"的**：281 个 Markdown、93% 只有一份 SKILL.md、只有 2.8% 用了官方推荐的 `references/` 分层。它的价值在覆盖面（69 个语言框架 + 35 个 agent 元技能 + 14 个行业域），不在单个 skill 的工程深度。
2. **作者自己知道 skill 不可靠**（`skills/continuous-learning-v2/SKILL.md:330` 那句 "Skills are probabilistic -- they fire ~50-80% of the time"），所以凡是"必须发生"的事都改用 hook。这是这个仓库最值得学的一条设计判断。
3. **对本机用户，装 full 的代价是"+1.7 万常驻 token + 一个不可恢复的 `deep-research` 覆盖 + 20 多个 skill 的触发竞争"**，收益是 281 个你 95% 用不上的文档。正确姿势是 `install.sh --skills <单个 id>` 挑着拿，或者干脆只读源码借鉴模式（`orch-pipeline` 的分级+闸门、`skill-comply` 的合规度量、`docs/SKILL-PLACEMENT-POLICY.md` 的溯源制度）。
