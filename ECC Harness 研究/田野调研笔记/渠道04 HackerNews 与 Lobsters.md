# HN / Lobsters 田野调研：ECC 与「harness 之上再加 harness」的第三方声音

> 调研日期：2026-08-01
> 渠道：Hacker News（Algolia API，story + comment 全量检索）、Lobsters（search + .json API）
> 调研对象：affaan-m/ECC（everything-claude-code），及同类 skill/agent 框架的社区批判性讨论
> 调研员纪律：每条都带 URL + 发言人 + 日期；作者方 / 第三方分开标注；无出处的一律丢弃

---

## 0. 一句话结论

**HN 上直接点名 ECC 的实质性内容只有 2 条**（1 条第三方拆解分析 + 1 条实际用户的取舍表态），但 HN 对**同一类产品（Superpowers）**有一场 254 条评论的高质量论战，几乎逐条回答了你想问的问题——因为社区把 ECC / Superpowers / Get Shit Done / Compound Engineering 当作同一个物种在讨论：**「harness 的 harness」**。

那 1 条 ECC 实际用户的话是全场最值钱的：

> **"ECC 里我唯一真正觉得有用的 skill 是 `/strategic-compact`……而且这个效果你用一句话 + `/compact <message>` 就能复现。除此之外，就是裸奔（barebone）。ECC 大概带了 300 个 skill，我可能该直接删了它！"**
> — HN 用户 Otterly99，2026-07-03，https://news.ycombinator.com/item?id=48774063

**留 1 个（还可被原生功能替代）/ 删 300 个**——这就是 HN 上唯一一条能追溯到人的 ECC 实战取舍记录。

---

## 1. HN 上直接点名 ECC 的全部内容（穷尽式，共 6 条）

检索 `everything-claude-code` / `affaan` / `affaan-m` / `ecc-universal` / `agentshield` / `strategic-compact`，全站（story + comment）命中去重后**只有 6 条**，其中只有 2 条有信息量。

| # | 类型 | 出处 | 日期 | 说了什么 |
|---|---|---|---|---|
| 1 | **第三方实战取舍**（高价值） | [Otterly99, id=48774063](https://news.ycombinator.com/item?id=48774063)（Superpowers 6 讨论串下） | 2026-07-03 | 只留 `/strategic-compact`；且指出它可被 `/compact <message>` + 一句话 prompt 平替；"300 个 skill，也许该删了" |
| 2 | **第三方分析拆解**（高价值） | [gauravvij137, id=48383439](https://news.ycombinator.com/item?id=48383439) | 2026-06-03 | 对 3 个 trending skill 仓库的横向解剖，见 §5 |
| 3 | 提问未获回答 | [Relisora, id=47419976](https://news.ycombinator.com/item?id=47419976)（Get Shit Done 473 分主贴下） | 2026-03-17 | "有人拿它和 everything-claude-code (ECC) 比过吗？" —— **247 条评论里零人回答**。这本身是个信号：ECC 在 HN 工程受众里没有渗透 |
| 4 | 商业引用（作者方之外的第三方产品） | [keterslater, id=47390841](https://news.ycombinator.com/item?id=47390841)（Caliber 产品自荐） | 2026-03-15 | 把 ECC 与 skills.sh、Anthropic Skills Guide 并列为 curation 来源 |
| 5 | 疑似推广，无讨论 | [Show HN: One-click fork of "Everything Claude Code" onto an isolated microVM](https://news.ycombinator.com/item?id=48596184)（shving90，jurniti.com） | 2026-06-19 | **2 分 0 评论**。信息量在于：有人专门做了「把 ECC 装进隔离 microVM」的产品——说明**安装污染是被商业化识别过的痛点**，但 HN 没接这个话题 |
| 6 | 疑似作者方/推广投稿，全部沉底 | [46676402](https://news.ycombinator.com/item?id=46676402)（2026-01-19，bzGoRust）、[46768958](https://news.ycombinator.com/item?id=46768958)（2026-01-26，manthangupta109）、[48126691](https://news.ycombinator.com/item?id=48126691)（2026-05-13，doener） | 2026-01 ~ 05 | 三次不同账号投稿 ECC 仓库，全部 **2 分 0 评论**。标题分别是 "Complete Claude Code configuration…"、"Everything Claude Code: performance optimization system…" |

### 🔑 这里最重要的元结论

> **一个号称 23 万 star 的仓库，在 HN 上被投稿 4 次全部 0 评论、被提问 1 次无人回答、实际用户表态仅 1 条且是「只留 1 个」。**

HN 是英语技术圈批判性最强、最不吃营销的场子。**star 数与 HN 讨论量之间这个数量级的背离，本身就是最强的甄别信号。**对比：Superpowers（obra/superpowers）在 HN 有 196 分 / 254 条评论的主贴，Get Shit Done 有 473 分 / 247 条评论，mattpocock/skills 被反复自发提及。ECC 一条都没有。

置信度：**高**（可自行复现：`curl -s "https://hn.algolia.com/api/v1/search?query=everything-claude-code&hitsPerPage=50"`，全站命中 13 条，去重后即上表）。

---

## 2. 第三方对 ECC 的唯一一次系统拆解（gauravvij137）

出处：https://news.ycombinator.com/item?id=48383439 ，2026-06-03，主贴 [48383438](https://news.ycombinator.com/item?id=48383438)（发在 aisignals.heyneo.com，是个 AI newsletter，**注意：作者本人是 newsletter 运营者，动机上属于内容生产而非纯用户，可信度记 medium**）。但内容是**可核实的事实陈述**，不是吹捧，且给出了否定性判断，所以有价值。

关键原文（我的翻译 + 原文）：

**关于规模与身份混乱**：
> "affaan-m/everything-claude-code（~17.5 万 star；另有第二个仓库 affaan-m/ECC ~20.5 万 star，是同一个项目换了标识符）。48 个 agent 定义、182 个 SKILL.md、68 个遗留 slash-command 兼容层、hooks、rules、MCP 配置、npm 包（ecc-universal、ecc-agentshield）、一个 Tkinter 桌面看板、一个安全扫描器（1282 个测试、102 条静态分析规则）。含针对 Claude Code / Codex CLI / Codex macOS / Cursor / OpenCode / Gemini CLI / Antigravity 的逐 harness 适配器。Anthropic hackathon 获奖作品。"

> 🔑 注意：他统计的是 **48 agent / 182 skill / 68 command**，而你手上 v2.1.0 是 **67 agent / 281 skill / 94 command**。三个月涨了 40%~55%。**这个仓库在持续膨胀，不是收敛。**

**关于 star 的含金量（核心批判）**：
> "trending 榜单在用一个标签衡量三种完全不同的东西：小而高杠杆的 CLAUDE.md 改动（karpathy-skills，赢在试错成本低）、精选的个人参考集（mattpocock，赢在人格背书分发）、以及完整框架发行版（ECC，赢在**目录式大而全的营销**）。**Star 并没有告诉我们这里面哪一个是靠真实复用活下来的。**"

**关于护城河（最狠的一句）**：
> "当一个创业公司说'我们的 agent 因为 prompt 和 workflow 更好'，而那个产物是一堆带 YAML frontmatter 的 markdown 文件时，**那是个 Notion 模板，不是护城河**。Karpathy 那四条规则会被下一版 Claude 的默认行为吸收掉。mattpocock 的 TDD skill 很锋利但可抄。**ECC 那 182 个 skill 的目录是很牛的工程，但里面的 prompt 一个下午就能 diff 出来移植走。**"

**他认为 ECC 真正有价值的部分（对你有用的正向筛选）**：
> "读完之后看起来还能留住价值的是：**harness 工程学**（安装路径、hook 管线、跨工具同步脚本、MCP server 生命周期）、**分发**（mattpocock 这个人是护城河，markdown 不是）、以及**围绕 skill 文件的安全工具**（AgentShield，即 ECC 的扫描器，即使被它扫的那些 skill 没价值，它本身是个真产品）。**这些没有一个是 prompt。**"

👉 **这条对你的直接含义**：如果要从 ECC 借鉴，优先看 **hook 管线 / 跨 harness 适配脚本 / AgentShield 扫描器**这三块工程件，而不是 281 个 skill 的 prompt 正文。

---

## 3. Superpowers 6 讨论串：ECC 同类产品的取舍全景（本次调研最富矿脉）

主贴：[Superpowers 6](https://news.ycombinator.com/item?id=48739459)（196 分，2026-06-30，链接 blog.fsck.com）。这是 HN 上关于「装一大坨 skill 框架到底值不值」最完整的一场论战。ECC 那条评论就长在这里。

### 3.1 明确「删了/不用了」的人（点名到组件）

| 发言人 | 日期 | 原话要点 | 链接 |
|---|---|---|---|
| **artisin** | 2026-07-01 | 用 Superpowers 5.x 一周，"除了烧掉蠢量的 token，在我所有个人 benchmark 和日常开发上都**比裸 Codex/Claude 明显更差**。我怀疑这要么是 AI 卡特尔烧 token 的四维阴谋，要么它只给本来就没能力的人赋能。评分：1/5 Pinocchios，不推荐" | [48741672](https://news.ycombinator.com/item?id=48741672) |
| **flashgordon** | 2026-07-01 | "Superpowers 是个巨大的 token 吞噬机。**skill 越通用表现越差。**……我认识的一个团队把它无脑 check 进了所有 repo，**他们的 per-PR 成本是我们组织 60 个工程团队里最高的**" | [48742486](https://news.ycombinator.com/item?id=48742486) |
| **arcticfox** | 2026-07-02 | "我是认证的 Superpowers 黑。现代模型下它**根本没必要，只是往 context window 里塞垃圾**，还平白增加惊人的轮次数。我以前模型指令遵循差的时候写过类似 prompt，那时候塞一堆指令确实有用。**现在我只有几个小 slash command 或者粘贴的 prompt，每次都完美工作**" | [48768689](https://news.ycombinator.com/item?id=48768689) |
| **steve-atx-7600** | 2026-07-03 | "看着不会有坏处，而且 Claude 和 Codex 都默认带了它的 plugin，所以我装了。**但我后来把它拔掉了**——因为我让它把一个 pydantic model 改成 immutable，它给我 TDD 出一堆'测试我配置成 immutable 的 model 确实 immutable'、'测试 foo=bar 之后 app config 里 foo 确实等于 bar'的单测" | [48770693](https://news.ycombinator.com/item?id=48770693) |
| **nullstyle** | 2026-05-04 | "我刚把 Superpowers 从我的配置里**移除了**。鉴于 Claude Code 和 Codex 自带 planning mode 的质量，Superpowers 实际上只是在拖慢速度、比 vanilla 烧更多 token" | [48016488](https://news.ycombinator.com/item?id=48016488) |
| **horsawlarway** | 2026-05-05 | "同一个坑。它烧多得多的 token，产出跟下面这一行字基本一样：『实现前请先做规划，有需要就问我问题。[我的 prompt]』……**我建议大多数情况就用 harness 自带的默认值**，然后把功夫花在把需求说清楚上" | [48017257](https://news.ycombinator.com/item?id=48017257) |
| **fsaintjacques**（Lobsters） | 2026-04-03 | "两个我都用，我发现**近期的 Claude 能力强得多，Superpowers 的 skill 现在是在拖慢我**" | [lobste.rs/c/py8pi2](https://lobste.rs/c/py8pi2) |
| **mattm** | 2026-07-02 | "同感。我试过类似 Superpowers 的东西，**一个小 bugfix 它整个就过火了**——写 TDD、生成一堆产物" | [48768384](https://news.ycombinator.com/item?id=48768384) |
| **yard2010** | 2026-07-03 | "昨天我给了它一个 goal。它启动了一个 workflow，**spawn 了大概 100 个 subagent** 去调研最佳可观测性产品。**在它开始改任何代码之前就把我整个 Max plan 烧光了**——这是我单用 Claude Code 从来做不到的。不适合我" | [48773235](https://news.ycombinator.com/item?id=48773235) |

### 3.2 明确「留下了什么」的人（正向精简配置）

| 发言人 | 日期 | 留了什么 | 链接 |
|---|---|---|---|
| **Otterly99**（ECC 用户） | 2026-07-03 | ECC 里只留 `/strategic-compact`，其余裸奔 | [48774063](https://news.ycombinator.com/item?id=48774063) |
| **losvedir** | 2026-07-02 | "Superpowers 让我想起 20 年前大家分享和争论自己那套精致 .vimrc 的时代……**我尽量用裸的 Claude Code，感觉它一直在变好，所有这些 prompt 花活都不值当**" | [48768715](https://news.ycombinator.com/item?id=48768715) |
| **bashtoni** | 2026-07-03 | "我经常去翻那些 skill，**从里面拿一点东西，做一个只够我用的最小版本，多一点都不要。YAGNI 绝对是这里该遵循的原则。**……我猜 Reddit 上那些开 5 个 Claude Max 20x 还把周限额全打满的人，都装了一堆这种东西" | [48772583](https://news.ycombinator.com/item?id=48772583) |
| **mgambati** | 2026-07-03 | "我基本上就用 Matt 的 `grill-with-docs` + CC/Codex 自带的 plan mode。**够了。**" | [48769855](https://news.ycombinator.com/item?id=48769855) |
| **RugnirViking**（另一串，最具体的最小集） | 2026-06-23 | 只有 **5 个 skill**：① 数据库（schema + 连接方式 + 预写好带 filter 的示例查询，防止 agent 拉一百万行）② 指标/tracing 平台 ③ Jira ④ Confluence（省 token 的检索策略）⑤ code review。"**理想的 skill 基本就是一串 shell 命令，每条旁边一句什么时候用。我对那些有几百个 skill 的人普遍持怀疑态度，尤其是我打开一看里面全是 AI 生成的文字的。skill 应该是命令清单 + 一点人类经验总结的坑，agent 特别不擅长写 prompt**" | [48643351](https://news.ycombinator.com/item?id=48643351) |
| **d4rkp4ttern** | 2026-07-03 | "另外值得注意：**Cherny（Claude Code 创始人）和 Steipete 都在采访里说过他们保持简单，不用这些花活**" | [48773337](https://news.ycombinator.com/item?id=48773337) ⚠️ 二手转述，未给采访链接，可信度 medium |

### 3.3 支持方的声音（为了平衡，不能只挑批评）

- **devnonymous**（2026-07-02，[48768648](https://news.ycombinator.com/item?id=48768648)）："我对这里这么多人说 Superpowers 没用感到意外。对我个人是 game changer，现在跟用 git 一样是工作流的一部分。**那些觉得平淡的人，是不是没有从 brainstorming skill 开始？**"
- **smusamashah**（2026-07-02，[48768393](https://news.ycombinator.com/item?id=48768393)）："我公司有人用 Superpowers 做成了两个 AI 之前一直没人碰的大项目……**但我自己用的时候只觉得它烧太多 token 干太少活。我猜 Superpowers 只在会摆弄它的人手里有用。**"
- **CharlesW**（2026-07-02，[48768514](https://news.ycombinator.com/item?id=48768514)）：长期用户，"Superpowers 是我为数不多在所有软件项目里都用的 skill/agent 套件"。
- **Syntaf**（2026-07-02，[48768209](https://news.ycombinator.com/item?id=48768209)）：先弃后回。"最初那版是'卧槽'时刻，然后光环很快褪去。更高开销、更慢吞吐、平庸结果让我最终放弃回到 plan mode……**6.x 感觉确实不一样，我又上车了**"

> 🔑 **支持方与反对方的分界线很清楚**：支持者都是**长流程、大改造、多会话**场景（跨 repo 迁移、库统一、脚本转 ansible）；反对者都是**日常小改动**场景。ECC 体量比 Superpowers 大一个数量级，这条分界线只会更陡。

### 3.4 元批评（最稀缺的那类）

| 发言人 | 日期 | 原话 | 链接 |
|---|---|---|---|
| **smrtinsert** | 2026-07-03 | "模型 → harness → **harness 的 harness？！** 这结构在我看来完全说不通。就像给方向盘套一个半皮半长毛的套。更别说 harness 本身几乎每天更新好几次。我相信这些框架作者肯定在盯着 harness 每次发版后他们那个 harness-of-harness 的表现呢（反讽）" | [48772004](https://news.ycombinator.com/item?id=48772004) |
| **AIorNot** | 2026-07-02 | "所有这些 prompt 和 skill 的 git 仓库都很可疑……**什么都没 benchmark，全是主观的、未经证明的，而且模型一升级就崩。人人和他叔叔都有个'祖传秘方 skill'——这恰好证明了这件事的主观性**" | [48768604](https://news.ycombinator.com/item?id=48768604) |
| **jannyfer** | 2026-07-03 | "而且这篇文章的故事本质是'我给 Fable 一个 goal 就去睡了，模型搞定了'——**这说明最新模型已经越过了对 Superpowers 的需求……连 Superpowers 的创造者自己都只是在用一个简单的 /goal**" | [48770118](https://news.ycombinator.com/item?id=48770118) |
| **Aurornis**（caveman 串） | 2026-04-30 | "我还是不敢相信有人认真对待 caveman。它是个好笑话，但在编程这种一个 session 动辄几十万 token 的场景里省几百个输出 token 完全可忽略。**你还得算上 skill 本身消耗的 token。**……现在看到它被当成有用操作反复传播，这个领域的 cargo culting（跟风崇拜）有多严重就很明显了" | [48957319 → 47957319](https://news.ycombinator.com/item?id=47957319) |
| **sandrello**（Daily Driver 串） | 2026-05-27 | "这种论调展现的正是整个 genAI 列车里邪教和骗局的那一面……**它本质上是在说：这个产品不过是个空盒子，把它变得有用是你的责任。而且你还得小心挑 skill，别装了 GithubInfluencerA 的二流货，你得用 GithubInfluencerB 的**……最后就是：无论你遇到什么问题，都是你这个用户没配置对" | [48291638](https://news.ycombinator.com/item?id=48291638) |
| **sunaookami** | 2026-05-05 | "MCP 和系统指令也一样，**一大堆人不理解就全装上，把 context 塞满、白白浪费 5 万+ token 在他们根本不需要的工具上**，然后抱怨自己每月得付 100 多刀因为额度用太快" | [48019661](https://news.ycombinator.com/item?id=48019661) |

---

## 4. 上下文/token 膨胀的**实测数字**（你要的第 2 项）

HN 上**没有人**公布过「装完 ECC 后 context 被吃掉 X token」的实测。这是本次调研的明确缺口。能找到的最接近的量化证据：

| 数字 | 来源 | 性质 |
|---|---|---|
| **>5 万 token** 被无谓的 MCP/系统指令吃掉 | sunaookami，2026-05-05，[48019661](https://news.ycombinator.com/item?id=48019661) | 第三方，但是**估计不是实测**，可信度 medium |
| 单个 skill 约 **2000 词**；5 个 skill ≈ 10K token；128K context 下占 ~10%，1M context 下"几乎不显示" | tecoholic，2026-05-05，[48016879](https://news.ycombinator.com/item?id=48016879) | 第三方实数，但他自称"我一个 skill 都没写过"。**按此外推：ECC 281 个 skill 正文 ≈ 56 万 token 量级**（这是我的外推，不是他说的，置信度 中 ~60%，且实际不会全量加载） |
| **skill 的 name + description 清单最终一定会全部进 context** | gwerbin，2026-05-05，[48017124](https://news.ycombinator.com/item?id=48017124) | 🔑 **这是对 ECC 最要命的机制性问题**。原话："Even having too many skills can be an issue because the list of skill names and their descriptions all end up in the context at some point." 281 个 skill × 每条 description 约 30-60 token = **8K~17K token 的常驻开销**，还没开始干活（外推部分置信度 中 ~60%，你可以自己数 ECC 的 frontmatter 验证） |
| LSP plugin：**627 个 session、186 次注入诊断、共 33 条 finding、只有 1 条被采纳**，其余 32 条被判为无关(13)或既存(19) | mil22，2026-05-27，[48298069](https://news.ycombinator.com/item?id=48298069) | ⭐ **全场唯一一份真正的量化审计**。他还说："两个月后我发现 agent 总共只主动调用过 LSP 工具**一次**……我把所有 LSP 卸载了，再没回头。agent 用 ripgrep 加自己跑 cargo clippy / dart analyze / ty check 完全够用" |
| Superpowers 6 官方自称：wall-clock **降 50%**、token **降 60%**（36 小时工作 / 未补贴 $650 token） | 作者 Jesse Vincent 博客，经 sedawkgrep 引用，[48768112](https://news.ycombinator.com/item?id=48768112) | ⚠️ **作者方自述**，且这个数字本身反证「5.x 之前有多浪费」 |
| 100 个 subagent 烧光整个 Max plan | yard2010，2026-07-03，[48773235](https://news.ycombinator.com/item?id=48773235) | 第三方轶事，无精确数字 |

👉 **你的可验证动作**：想拿到 ECC 的真实常驻开销，不要信任何人的估计，直接在隔离环境里跑 `/context`（Claude Code 自带），对比装前装后。这是唯一可信的数字来源。

---

## 5. 与 Claude Code 原生能力重叠 → 冗余（你要的第 3 项，本次收获最好）

这是 HN 讨论最密集、证据最硬的一块。

### 5.1 Anthropic 官方亲口承认要收敛（最强证据）

HN 用户 **mil22**（2026-05-27，[48295884](https://news.ycombinator.com/item?id=48295884)）提出：现在要做 code review 有 **5 条路**——

> "① 写 `.claude/commands/review.md`，简单但已弃用。② 用 `/code-review` skill，装一个或者自己写（反正就是 markdown）。③ 用 `/pr-review` subagent，也是 markdown，但它'后台'、'并行'跑，所以肯定更好吧（反讽）。④ 装 `/code-review` plugin，它其实就是把上面的 skill 和 subagent 装上。⑤ **直接叫 Claude review 一下代码。大多数场景下效果差不多。**
> 它们全都只是'插入一段罐头 prompt'的变体，只在（a）prompt 装在哪、从哪来 和（b）跑在哪个 context 里 这两个维度上有差别。**我个人发现直接叫 Claude review 代码就够好了。**"

**Claude Code 负责人 Boris Cherny 本人下场回复**（[48296387](https://news.ycombinator.com/item?id=48296387)，2026-05-27）：

> "嘿，我是 CC 团队的 Boris。**我同意，我们正在做收敛。今后就只有内置的 `/code-review` skill。**"（后附 `/code-review --fix`、`low/medium/high/xhigh/max`、`/code-review ultra` 用法）

> 🔑 **这对 ECC 的直接含义**：ECC 的 94 个 command 里有 **68 个是「legacy slash-command 兼容层」**（gauravvij137 的统计）。官方正在把 command / skill / subagent / plugin 四路收敛成一路，**ECC 的兼容层是在给一个官方正在拆除的抽象层做适配**。置信度：高。

### 5.2 「原生已经吸收了」的多方观察

| 发言人 | 日期 | 原话 |
|---|---|---|
| **SoMomentary** | 2026-07-01，[48741598](https://news.ycombinator.com/item?id=48741598) | "我一直很爱 Superpowers。**但我觉得它做的很多事现在已经被吸收进 Claude Code 本体了**，所以我很好奇这次发布还能不能改变什么" |
| **Tenoke** | 2026-07-03，[48769694](https://news.ycombinator.com/item?id=48769694) | "我喜欢这个想法，但我宁可这些 plugin 大部分都不用，Superpowers 也包括在内。**Code 本身就在以很快的速度把最好的技巧内置进去**" |
| **sv123** | 2026-07-03，[48769147](https://news.ycombinator.com/item?id=48769147) | （支持派）"提前有个 plan 可以读可以评论很棒，**不过说实话这可能已经内置到主流 harness 里了，只是我不知道**" |
| **nullstyle** | 2026-05-04，[48016488](https://news.ycombinator.com/item?id=48016488) | "**鉴于 Claude Code 和 Codex 自带 planning mode 的质量**，Superpowers 只是在拖慢速度" |

### 5.3 subagent 这一层正在被官方主动压制（针对 ECC 的 67 个 agent）

HN 主贴 [Claude Code has a hardcoded instruction telling Opus 5 not to use subagents](https://news.ycombinator.com/item?id=49056022)（2026-07-26，28 分 13 评论，转自 r/ClaudeCode）。

- **jauntywundrkind**（[49056356](https://news.ycombinator.com/item?id=49056356)）："我一点也不意外。**我觉得 subagent 现在已经不太好用了**——它们跑出去做一堆重复调研，回来交付一份主 agent 本来就知道的信息的廉价仿制品"
- **lwansbrough**（[49057403](https://news.ycombinator.com/item?id=49057403)）："理论上省，**但按我的经验，subagent 实际上增加成本**。dispatch 之后 subagent 的 context 经常重叠……而且随着编排者的 context 增长，它还会丢失 subagent context 里的细微之处。**最新的前沿模型很擅长把一个 session 里学到的东西往后应用。subagent 更适合本身就笨、用不好串行 context 的 agent**"
- **TacticalCoder**（[49056580](https://news.ycombinator.com/item?id=49056580)）："pi.dev 那个极简 harness 的作者也不喜欢 subagent。现在看到 Anthropic 硬编码指令引导远离 subagent 挺有意思的。**还相信 subagent 的人也许该想想为什么**"

⚠️ 反面证据（保持平衡）：**einsteinx2**（[49058705](https://news.ycombinator.com/item?id=49058705)）和 **prtmnth**（[49057340](https://news.ycombinator.com/item?id=49057340)）都说实际观察到 Opus 5 照常用 subagent，怀疑这个说法不准。所以「官方压制 subagent」这条**结论本身置信度只有中 ~60%**，但「subagent 常常增本不减本」这条有多人独立佐证，置信度高。

---

## 6. 踩坑记录（你要的第 4 项）

| 坑 | 出处 | 具体表现 |
|---|---|---|
| **过度触发 / 小任务被重型流程碾压** | steve-atx-7600 [48770693](https://news.ycombinator.com/item?id=48770693)；mattm [48768384](https://news.ycombinator.com/item?id=48768384) | 把 pydantic model 改 immutable → 生成一堆同义反复的单测；小 bugfix → 走完整 TDD + 生成产物 |
| **skill 该触发时不触发**（与 ECC 281 个 skill 的路由直接相关） | calpaterson（Lobsters，[lobste.rs/c/klxsq8](https://lobste.rs/c/klxsq8)，2026-04-03） | "**Claude 触发 skill 的积极性连一半都不到。**极其令人沮丧的是你得不停提醒它'你会搜 reddit 的，我们有自定义 skill，别再用 site:reddit.com 谷歌了'" |
| 同上，且 hook 是唯一可靠解 | jonathannen（Lobsters，[lobste.rs/c/c4qw16](https://lobste.rs/c/c4qw16)，2026-04-03） | "目前还没有稳健的解法。**我把一些东西挪到 hook 里成功了**（我有自定义 lint 之类）。Claude 用 skill 不够多，但我相信他们在改" |
| **prose 规则不是保障，只有 settings.json 的 permissions.deny 是** | hipvlady，2026-06-04，[48398856](https://news.ycombinator.com/item?id=48398856) | "CLAUDE.md 是整个栈里最重要也**最脆弱**的一块。'编辑前永远先重读 X'这种规则写成散文，散文受 context 影响。**一次 compaction 或者一个 subagent 出现，它就悄无声息地消失了，而且没有任何警告。**……唯一能可靠强制规则的是 settings.json 的 permissions.deny，运行时在模型选工具之前就检查，你没法用 cat 或 grep 绕过去。**留在散文里的一切都是强默认值，不是保证**" |
| **团队级成本失控** | flashgordon，2026-07-01，[48742486](https://news.ycombinator.com/item?id=48742486) | 无脑 check 进所有 repo → 60 个工程团队里 per-PR 成本最高 |
| **资源占用**（LSP 类 plugin） | mil22，2026-05-27，[48295884](https://news.ycombinator.com/item?id=48295884) | "**频繁因为 harness 拉起的一堆 rust-analyzer / dart analysis server / Ty LSP 而 OOM**" |
| **供应链/安全**：>1/4 的 agent skill 含至少一个安全漏洞 | 4ppsec，2026-03-16，[47402119](https://news.ycombinator.com/item?id=47402119)（Tego Security 索引库） | ⚠️ **这是安全厂商的自我推广贴**，数字未给论文出处，可信度 low-medium。同串 joe-limia（[47403803](https://news.ycombinator.com/item?id=47403803)）反驳："如果你的工程师在装网上随便找的 skill，那和让工程师往终端粘贴命令是一回事，这（指该数据库）说白了是表面营销" |

---

## 7. HN 上被点名推荐的「精简配置」全清单

按「装什么」而不是「不装什么」整理，供你做减法参照：

**最小主义档（多人独立收敛到这里）**
1. 裸 Claude Code + plan mode（losvedir、Tenoke、horsawlarway、nullstyle）
2. 一句话代替整套框架：`"实现前请先做规划，有需要就问我问题"`（horsawlarway，[48017257](https://news.ycombinator.com/item?id=48017257)）
3. `/compact <message>` + 一句话交接说明，代替 ECC 的 `/strategic-compact`（Otterly99，[48774063](https://news.ycombinator.com/item?id=48774063)）
4. `"be brief."` 两个词，实测在 token 和质量上打平 caveman plugin（max-t-dev 的 24 prompt × 5 arm benchmark，[47954746](https://news.ycombinator.com/item?id=47954746)，仓库 https://github.com/max-taylor/cc-compression-bench —— ⚠️ 作者自承单次运行、方差未控制，[47956306](https://news.ycombinator.com/item?id=47956306)）

**被点名保留的第三方组件（只有 3 个反复出现）**
- `mattpocock/skills` 的 `/grill-me`，升级版 `/grill-with-docs`（AlexErrant [48768350](https://news.ycombinator.com/item?id=48768350)、mgambati [48769855](https://news.ycombinator.com/item?id=48769855)、ElijahLynn [48770626](https://news.ycombinator.com/item?id=48770626)、verdverm [48768890](https://news.ycombinator.com/item?id=48768890)）
- Superpowers 的 `brainstorming` skill（jghn [48770552](https://news.ycombinator.com/item?id=48770552)、sv123 [48769147](https://news.ycombinator.com/item?id=48769147)、devnonymous [48768648](https://news.ycombinator.com/item?id=48768648)）
- Playwright / Puppeteer MCP（Ask HN 那串里唯一被多人点名的 MCP，[46344419](https://news.ycombinator.com/item?id=46344419) / [46347377](https://news.ycombinator.com/item?id=46347377)）

**RugnirViking 的 5-skill 模板（最值得抄的结构）**——[48643351](https://news.ycombinator.com/item?id=48643351)
> 数据库 / 指标-tracing / Jira / Confluence / code review，每个都是「一串 shell 命令 + 每条旁边一句什么时候用」。

> 🔑 **一个反直觉但重要的观察**：2025-12-21 的 [Ask HN: What public Claude Code MCPs, Skills do you have installed and use?](https://news.ycombinator.com/item?id=46343801) —— **只有 7 分 6 条回复，其中第一条回答是 "None."**（danielfalbo，[46343802](https://news.ycombinator.com/item?id=46343802)）。HN 受众对「装公共 skill 包」这件事整体是冷淡的。

---

## 8. Lobsters：基本没有信息（如实报告）

- `everything-claude-code` 搜索：**0 结果**（stories）。全站无 ECC 任何提及。
- 有价值的只有一条 story：[A Rave Review of Superpowers (for Claude Code)](https://lobste.rs/s/veyrwi/rave_review_superpowers_for_claude_code)（emschwartz，2026-04-02，17 分 9 评论）。有用的三条评论已收进 §3.1 和 §6。
- 其余搜索（`agent+skills` / `claude+skills` / `coding+agent+harness`）返回的是模糊全文匹配，命中的是泛 AI 话题文章，与 skill 框架取舍无关。
- 相邻但有参考价值的 Lobsters 文章（本次未深挖）：[Most MCP servers don't need to exist](https://evilmartians.com/chronicles/most-mcp-servers-dont-need-to-exist-your-case-might-be)、[danluu: Agentic coding notes](https://danluu.com/ai-coding/)、[Claude Code: From Agent to Useful Tool](https://serokell.io/blog/claudecode)。

**结论：Lobsters 渠道对 ECC 无有效信息。**

---

## 9. 低可信度内容标记（甄别记录）

按调研纪律，以下内容我**看到了但没有当作证据用**：

1. **ECC 自身的 4 次 HN 投稿**（46676402 / 46768958 / 48126691 / 48596184）：不同账号、不同标题、指向同一仓库，全部 0 评论。**符合推广投稿特征**，不构成社区认可证据。
2. **buildingbetter.tech「我读了 Claude Code 源码」**（[48318174](https://news.ycombinator.com/item?id=48318174)，326 分）：HN 用户 **ricardobeat**（[48325553](https://news.ycombinator.com/item?id=48325553)）逐条给出官方文档链接后判定 **"这篇文章是纯 AI 写的 clickbait，我很惊讶 HN 会捧它"**。另一位 My_Name（[48322812](https://news.ycombinator.com/item?id=48322812)）grep 二进制核实，发现文中两项（`yoloClassifier`、`Magic Docs`）在 2.1.156 里找不到。**——这条对你有元价值：高分 ≠ 可信，HN 自己会打假。**
3. **Tego Security 的「超过 1/4 的 agent skill 有安全漏洞」**：安全厂商自荐贴，无原始研究出处，标记 low。
4. **aisignals.heyneo.com 那条 ECC 拆解**：作者是 AI newsletter 运营者，动机非纯用户。但内容是可核实的事实统计 + 否定性判断，标记 medium 并已注明。

---

## 10. 检索方法与覆盖度（可复现）

**HN（Algolia，免认证）**——已跑过的 query：
```
everything-claude-code / affaan / affaan-m / ecc-universal / agentshield / strategic-compact / 281 skills
claude code skills / claude skills / agent skills / claude code hooks / claude code plugins
subagents / CLAUDE.md / context bloat agent / claude code bloat / skill sprawl / too many skills
skills context window tokens / uninstall claude skills / skills not triggering / skill description routing
search_by_date: ECC claude
```
**已逐条读完全部评论的 story**：48739459(Superpowers 6, 209条) / 48289950(Daily Driver, 245条) / 48383438(ECC 拆解) / 48624327(Using Agent Skills Wrong) / 47040430(SkillsBench, 167条) / 48015397(Agent Skills, 200条) / 47954745(caveman benchmark, 67条) / 48318174 / 49056022 / 47417804(GSD, 247条) / 46343801 / 48160604 / 47144537 / 48936491 / 47982718 / 47402118 / 47866305

**Lobsters**：`/search?q=...&what=stories|comments&order=newest|relevance`（`.json` 端点对 `what`/`order` 参数报 400，须用 HTML 端点解析）；story 详情走 `https://lobste.rs/s/<id>.json`。

**明确没找到的**（空结论也是结论）：
- 没有任何人公布过 ECC 的 context 占用实测数字
- 没有任何人公布过完整的「ECC 最小可用集」清单（只有 Otterly99 的 1 个 skill）
- 没有任何针对 ECC 的 benchmark / 对照实验
- 没有任何关于 ECC 安装冲突、hook 拖慢、卸载残留的具体记录
- 没有任何直接质疑 ECC star 数造假的帖子（最接近的是 gauravvij137 的 "star 不告诉我们哪个是靠真实复用活下来的"）
