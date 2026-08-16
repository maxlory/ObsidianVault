# 05 — Reddit / X 田野调查（ECC: affaan-m/everything-claude-code）

> 渠道：Reddit（r/ClaudeAI、r/ClaudeCode、r/AIAgentsInAction 等）+ X/Twitter（作者推文回复区、第三方讨论）
> 调查日期：2026-08-01　|　调查者：field-research subagent（渠道 = Reddit + X）
> 状态：进行中

---

## 0. 方法与可达性

| 路径 | 结果 |
|---|---|
| `curl reddit.com/search.json` | **403**（Reddit 已封匿名 JSON，`old.reddit.com` 同样 403） |
| `agent-reach doctor` reddit / twitter 后端 | 两者均 **未安装**（reddit: `status: off`；twitter: `status: warn`） |
| Chrome MCP → `www.reddit.com/search` | ✅ 过 JS challenge 后可读 DOM |
| Chrome MCP → `old.reddit.com` + 页内 `fetch('.../.json')` | ✅ **最高效**：同源 fetch 拿到完整评论树（含折叠/低分评论），一次调用取多帖 |
| Chrome MCP → x.com（已登录态） | 见 §2 |

> 🔑 关键技巧存档：在 old.reddit.com 页面里用 `evaluate_script` 跑 `fetch('https://old.reddit.com/r/X/comments/<id>/.json?sort=top&limit=200')`，同源不受 CORS 限制，且拿到的是完整 JSON 评论树，比滚动 DOM 快一个数量级。

---

## 1. Reddit：搜到了什么

### 1.1 命中清单（有实质内容的帖子）

| 帖子 | 版块 | 日期 | 分数/评论 | 价值 |
|---|---|---|---|---|
| [This guy Won the Anthropic Hackathon Solo…](https://old.reddit.com/r/AIAgentsInAction/comments/1t84rlc/) | r/AIAgentsInAction | 2026-05-09 | 1512↑ / 83 | ⭐⭐⭐ 批评最集中，含署名权争议 |
| [What do guys think of this? Anyone has tried it?](https://old.reddit.com/r/ClaudeAI/comments/1sttolx/) | r/ClaudeAI | 2026-04-23 | 0↑ / 19 | ⭐⭐⭐ 全是实战吐槽 + 精简做法 |
| [What is your go-to plugin that really improves your productivity?](https://old.reddit.com/r/ClaudeAI/comments/1qkm7gs/) | r/ClaudeAI | 2026-01-23 | 41↑ / 23 | ⭐⭐⭐ "看了但没找到理由用" |
| [These 10 GitHub repos completely changed how I use Claude Code](https://old.reddit.com/r/ClaudeAI/comments/1sapnyb/) | r/ClaudeAI | 2026-04-02 | 371↑ / 36 | ⭐⭐ 缺 CC 原生优化的批评 |
| [Is there any agent harness similar to ECC for Copilot CLI](https://old.reddit.com/r/GithubCopilot/comments/1s6npll/) | r/GithubCopilot | 2026-03-29 | 2↑ / 7 | ⭐⭐ 跨 harness 安装踩坑（微软员工回复） |
| [If you're using Claude Code, someone just dropped this repo](https://old.reddit.com/r/ClaudeCode/comments/1r193as/) | r/ClaudeCode | 2026-02-10 | 1↑ / 26 | ⭐⭐ OpenCode 安装路径全坏 |
| [everything-claude-code vs superpowers](https://old.reddit.com/r/ClaudeCode/comments/1s6ahmq/) | r/ClaudeCode | 2026-03-28 | 0↑ / 6 | ⭐ 泛泛质疑 |
| [Superpowers vs ECC](https://old.reddit.com/r/opencodeCLI/comments/1s42nrn/) | r/opencodeCLI | 2026-03-26 | — / 2 | ⭐ "star 这么多却没人讨论" |
| [Superpowers vs. Everything Claude Code (ECC)](https://old.reddit.com/r/ClaudeAI/comments/1un4puf/) | r/ClaudeAI | 2026-07-04 | 7↑ / 4 | ⭐ 无人用过 ECC |
| [Is everything-claude-code really that good?](https://old.reddit.com/r/ClaudeCode/comments/1slk08g/) | r/ClaudeCode | 2026-04-14 | 2↑ / 2 | ⭐ 加载机制答疑 |
| ["Skills" packs are dominating GitHub trending…](https://old.reddit.com/r/PromptEngineering/comments/1tvoie8/) | r/PromptEngineering | 2026-06-03 | — / 2 | ⭐⭐ 唯一一篇拆机制的分析 |
| [My Claude Code setup burned 70,000 tokens before I typed a single word](https://old.reddit.com/r/SideProject/comments/1va0h8z/) | r/SideProject | 2026-07-29 | — / 8 | ⭐⭐⭐ 唯一实测 token 数字（非 ECC 专属） |

---

### 1.2 上下文膨胀：真实数字与实测

#### （A）唯一一条 ECC 专属的实测体感 —— 新会话开局就 20–25%

> **mtyroot**（2026-05-13，r/AIAgentsInAction）：
> "I've tried it but it eats up the context window, I would start a new session and all this would have the context window around 20-25% you just enable a lot of unnecessary skills and would run out of context right away"
> https://old.reddit.com/r/AIAgentsInAction/comments/1t84rlc/

- 可信度：**中**。第三方、明确说"我试过"，但没给测量方法，20–25% 是体感读数（Claude Code 状态栏百分比）。
- 换算：200K 窗口的 20–25% ≈ **40–50K token 开局开销**。

#### （B）"100k tokens on load"

> **TheCannings**（2026-04-23，r/ClaudeAI，+5）："100k tokens on load lol"
> https://old.reddit.com/r/ClaudeAI/comments/1sttolx/

- 可信度：**低-中**。一句话调侃，没有测量过程。作为"社区体感上限"记录，不作为数字依据。

#### （C）最严谨的 token 测量（不是 ECC，但方法可复用）—— 70K 开局 / 每个 subagent 再付 68K

> **u/Intelligent_Mine2502**（2026-07-29，r/SideProject）
> 仓库：https://github.com/basementdante/token-ledger
> https://old.reddit.com/r/SideProject/comments/1va0h8z/

实测结论（185 个会话、3400 次去重 API 调用、单机 n=1）：
1. **一个会话在你打第一个字之前就烧掉 ~70,000 token**（26 个会话中位数 69,979；两个冷缓存会话独立复核为 70,346 / 71,263）。构成 = 系统提示 + 工具定义 + CLAUDE.md + 15 个 rule 文件 + plugin 列表 + agent 目录。
2. **两周前同一台机器只要 12,416 token** → 他自己加配置带来 **5.6 倍**的会话启动成本膨胀（作者诚实标注：同期 Claude Code 也升级过版本，无法完全归因）。
3. **transcript 里 98.7% 的堆积来自 tool result**，用户 prompt 只占 0.7%，助手文字占 0.6%。→ 意味着「少读整文件、多 grep」比精简 prompt 有效得多。
4. **每个 subagent 都要重新付一次 ~68,000 token 的启动成本**（331 次 subagent 调用的中位数）。→ 直接反驳"多 agent = 便宜的委派"。

他后来的精简结果（回帖，2026-07-31）：
> "down to 4 [rule files] that load every session… the other 11 moved into on-demand skill files… the always-on part went from ~9.7k to ~2.5k tokens. **the embarrassing part is nothing broke, half of those rules had never visibly changed an output.**"

- 可信度：**中-高**。有开源可复跑脚本 + 自曝方法论弱点（无 tokenizer、n=1、一个数字是外推）。扣分项：作者承认正文是 AI 起草，且卖配套付费指南（被 u/MaybeARunnerTomorrow 当场指出 "AI slop"）。
- ⚠️ **与 ECC 无直接关系**，是"重配置 Claude Code"的通用成本画像。用它当 ECC 的量级参照，不当 ECC 的实测。

#### （D）一条重要的机制纠正（防止误判）

> **MediumChemical4292**：「136 个 skill 你日常能用上 1/3 吗？so much context bloat」
> **ChocomelP**（回复）："That's not how skills work. That's how MCP servers work."
> https://old.reddit.com/r/ClaudeAI/comments/1sapnyb/

同源解释（**iEatedCoookies**，2026-04-14）：
> "the skills and tools will get their descriptions loaded into context. If Claude determines one of the tools may be of value, it can then load the whole skill. The token usage of the descriptions are minimal and will be cached anyway so you don't need to worry too much **as long as you don't have hundreds of skills**."
> https://old.reddit.com/r/ClaudeCode/comments/1slk08g/

- 🔑 结论：skill 只有 **description 常驻**，正文按需加载 —— 所以"281 个 skill"不等于"281 个 skill 的 token"。但 ECC 恰好落在那句限定语的另一边（*hundreds of skills*）。真正常驻的膨胀来源是 **rule 文件 / CLAUDE.md / agent 目录 / MCP 工具 schema**，不是 skill 正文。

---

### 1.3 与 Claude Code 原生能力重叠 → 冗余

**① 最具体的一条：ECC 为了跨 harness 兼容，放弃了 Claude Code 原生优化**

> **Obvious_Equivalent_1**（2026-04-03，r/ClaudeAI，+12）：
> "The only problem with it: it's setup in a way to support a broad set of AI tools (Claude Code, Codex, OpenCode, Gemini CLI) **but it is lacking any of the optimizations for Claude Code**. Fortunately Anthropic allows for extending plugins from marketplace so you can use skills like Superpowers with native functionality from Claude Code. This makes it possible to leverage Claude's native support for like `TaskCreate`, `TaskList`, `TaskUpdate`."
> https://old.reddit.com/r/ClaudeAI/comments/1sapnyb/　参考实现：https://github.com/pcvelz/superpowers

- 可信度：**中-高**。点名到具体原生 API（TaskCreate/TaskList/TaskUpdate），可自行核实 ECC 是否用了。
- 👉 这条正好对上你笔记里的观察：ECC 把编排搬到模型外面（tmux + git worktree + 文件），而 CC 原生已有 Task 系列。

**② "CC 自己一直在变好，不想装装了下版本就被取代的东西"**

> **u/WashTop956**（2026-01-23，r/ClaudeAI，主帖 41↑）：
> "Anthropic's defaults are already solid, and claude-plugins-official covers most bases… I checked out everything-claude-code recently, **It's really well organized, tons of options but I still couldn't find a reason to use most of it. CC keeps getting better on its own, so I don't want to add anything that'll just get replaced next update.**"
> https://old.reddit.com/r/ClaudeAI/comments/1qkm7gs/

**③ 更直接的两条**

> **magic_claw**（2026-05-14）："It already is [irrelevant]. **Lot of this is built into Claude Code already.** This is quite old."
> **crusoe**（2026-05-14）："Or you can just ask Claude to do it without skills. **A lot of this stuff is actually harmful now.**"
> **Foreign-Upstairs-237**（2026-05-23）："Using opus 4.7, I would default and build using none of these skills. Too much tech debt"
> 均在 https://old.reddit.com/r/AIAgentsInAction/comments/1t84rlc/

**④ 结构性质疑（漂移风险）**

> **dashingsauce**（2026-05-09，+19）："this entire combobulation will become irrelevant and incompatible with Claude itself in like 6 months. if you need 156 skills and 38 agents and 17 layers of review and containment, **the complexity of rapid drift at the frontier will just swallow you whole**"

---

### 1.4 踩坑记录（安装 / 兼容 / 维护）

| 坑 | 出处 | 日期 |
|---|---|---|
| **OpenCode 安装路径全坏**："I've bookmarked it - hoped to get it into opencode but **the install routes are totally broken, instructions wrong, and npm module doesn't yet exist**." — u/gwawr | [r/ClaudeCode 1r193as](https://old.reddit.com/r/ClaudeCode/comments/1r193as/) | 2026-02-11 |
| **Copilot CLI 装完 `/skills` 列出 200 条、终端里翻都翻不动**："the /skills command just return a list of 200 skills, I cannot even scroll this list in Copilot CLI terminal." — u/thinkriver | [r/GithubCopilot 1s6npll](https://old.reddit.com/r/GithubCopilot/comments/1s6npll/) | 2026-03-30 |
| **hook 无法随 plugin 分发**（微软 Copilot CLI 团队成员证词）："fun fact - CLI will install hooks if they are in the `plugin.json`, but they aren't since **Claude doesn't support hooks-via-plugins**." — u/aaronpowell_msft | 同上 | 2026-03-30 |
| **装完还是往 `~/.claude/` 写**（跨 harness 时的污染）："I use configure-ecc skill in Copilot CLI, it still install/copy/setup skill, rules in ~/.claude folder." — u/thinkriver | 同上 | 2026-03-29 |
| **更新频率低导致弃用**："I tried this and then switched to Superpowers, because **the Everything project's update frequency was slow or not-at-all**." — u/miles-of-fun | [r/AIAgentsInAction 1t84rlc](https://old.reddit.com/r/AIAgentsInAction/comments/1t84rlc/) | 2026-05-10 |
| **误触发**（Superpowers 同类问题，可类比）："OC decides itself that it should call the brainstorming skill. Then sometimes I have to explicitly say not to use that. Its sometimes annoying." — u/mdrahiem | [r/opencodeCLI 1s42nrn](https://old.reddit.com/r/opencodeCLI/comments/1s42nrn/) | 2026-03-26 |
| **不适合有历史包袱的代码库**："This is great if you want to build something from scratch, if you work in corporate dealing with messy legacy code, not so much (I speak from experience)." — u/IntelligentLeading11 | [r/AIAgentsInAction 1t84rlc](https://old.reddit.com/r/AIAgentsInAction/comments/1t84rlc/) | 2026-05-25 |

---

### 1.5 精简配置方案（有人公开过怎么裁）

社区没人贴出成型的「ECC 最小可用集清单」。找到的全是**方法**，不是名单：

| 做法 | 原话 | 出处 |
|---|---|---|
| **克隆 → 读 → 挑几个搬走 → 走人** | "I cloned it I studied and pulled out what may be or seem of benefit to me. **Moved on.** No way anyone and everyone will just up and need the amount of stuff here" — u/rmartinez2 | [r/ClaudeAI 1sttolx](https://old.reddit.com/r/ClaudeAI/comments/1sttolx/) 2026-04-23 |
| **把有用的几个塞进一个 profile，按任务开** | "I liked a few on there but **it costs a lot loading them all together. I put the useful ones in to a profile and run them when my work requires it**" — u/Academic-Job39 | 同上 |
| **用 `--with` / `--without` 选择性安装**（ECC 官方 CLI 能力） | "they've got `--with` and `--without` flags now so you can just grab the agents that matter for your stack… you tell it which agents matter for your stack upfront so they're the only ones that ever touch your context window." — u/theLonesmith | 同上（⚠️ 该账号在同帖两次为 ECC 辩护，疑似推广，见 §4） |
| **让 Claude 自己筛** | "You can even ask Claude Code which skills it thinks are useful, which aren't, based on your project." — u/iEatedCoookies | [r/ClaudeCode 1slk08g](https://old.reddit.com/r/ClaudeCode/comments/1slk08g/) 2026-04-14 |
| **rule 文件从 15 减到 4，其余改成按需 skill**（通用方法，非 ECC） | 常驻从 9.7k → 2.5k token，"nothing broke" | [r/SideProject 1va0h8z](https://old.reddit.com/r/SideProject/comments/1va0h8z/) 2026-07-31 |

📌 **被点名"值得单独抄走"的 ECC 组件**（第三方，非作者）：
- `skills/continuous-learning-v2/SKILL.md` — u/LordLederhosen："This right here is the type of thing that I need to learn more about"（[r/ClaudeCode 1r193as](https://old.reddit.com/r/ClaudeCode/comments/1r193as/), 2026-02-10，附直链）
- **hook 用法示例** — u/evandroferreiras："One thing interesting on this approach is how to actually use hooks. We have some good examples here"（同帖，2026-03-29）
- **instinct 文件（置信度评分）+ 会话自动萃取新 skill 的 hook** — u/gvij 的机制拆解认为这是 ECC 相对 mattpocock/skills 真正多出来的东西（[r/PromptEngineering 1tvoie8](https://old.reddit.com/r/PromptEngineering/comments/1tvoie8/), 2026-06-03）

---

### 1.6 反面意见（本节最稀缺，逐条列全）

**① 署名权/抄袭指控（最硬的一条）**

> **u/YUYbox**（2026-05-10，r/AIAgentsInAction）：
> "**The 1282 security tests are InsAIts tests FROM MY fork into ECC.** He didn't want to recognize the InsAIts value and the fact that improve Claude work and make the session longer and instead **he renamed my fork and took the credit** for what InsAIts do. Below my discussion with Affan."
> 附证据截图（Reddit 图片）+ 仓库 https://github.com/Nomadu27/InsAIts-public
> https://old.reddit.com/r/AIAgentsInAction/comments/1t84rlc/

- 可信度：**中**。单方面指控 + 一张截图，但**点名到具体可核实的数字（1,282 security tests）和具体仓库**，可以自己去 diff 两个仓库的测试文件比对。
- 👉 你可以验证：`git log`/`git blame` ECC 的 AgentShield 测试目录，看是否有 InsAIts 来源痕迹；对照 https://github.com/Nomadu27/InsAIts-public。

**② "star 多得反常，讨论却少得反常"**

> **u/mdrahiem**（2026-03-26，r/opencodeCLI）："First thing I wondered is, **it has lot of Github stars and it feels a bit weird that people are not talking much about this?**"
> https://old.reddit.com/r/opencodeCLI/comments/1s42nrn/

同向证据 —— **star 数在半年里翻了 5 倍以上，且各处引用互相打架**：

| 日期 | 声称的 star 数 | 出处 |
|---|---|---|
| 2026-02-10 | 43.5k | u/wifestalksthisuser, r/ClaudeCode |
| 2026-04-02 | 128k | u/virtualunc, r/ClaudeAI |
| 2026-04-24 | 单日 +1.1k | u/gvij 的 trending 追踪, r/AI_Agents |
| 2026-05-09 | 153k+ | r/AIAgentsInAction 主帖 |
| 2026-05-10 | 177k | u/asenna987 |
| 2026-06-03 | ~175k | u/gvij, r/PromptEngineering |
| 2026-08（当前） | 185.9k（SkillsLLM）/ 236k（本次调研前提） | — |

- 🔑 这个曲线本身不能证明刷 star，但配合"讨论量与 star 严重不匹配"是值得警惕的信号。**同期 obra/superpowers 单日 +2.9k star**（比 ECC 还快），说明这一波是整个 skill-pack 赛道的 hype，不是 ECC 独有。

**③ 组件数量在各处宣传里对不上（版本漂移 or 数字随口编）**

| 日期 | 宣传口径 | 出处 |
|---|---|---|
| 2026-04-23 | 27 agents / 64 skills / 33 commands | r/ClaudeAI 1sttolx 转贴的推广文案 |
| 2026-04-02 | 30 agents / 136 skills / 60 commands | u/virtualunc |
| 2026-05-09 | 38 agents / 156 skills / 72 commands | r/AIAgentsInAction 主帖 |
| 2026-06-03 | 48 agents / 182 skills | u/gvij 实读仓库 |
| 2026-08-01（v2.1.0） | 67 agents / 281 skills / 94 commands | 本地仓库实测 |

数量是**单调增长**的，所以这更像真实的版本演进而非造假。但注意：**同一段推广文案里的"60% documented cost reduction"从没被任何第三方复现过**（Reddit 全站零复现报告）。

**④ "AI slop / 卖课的做的" 类批评**

> **Special-Economist-64**（2026-04-23，+12）："as soon as i see these many agents i immediately smell AI slop there. just skip away and save your time"
> **versaceblues**（2026-04-24）："looks like it was created by someone that sells courses and doesn't actually code"
> **Awkward-Suggestion90**（2026-05-13）："It's just ai slop advertisment."
> **smoke-bubble**（2026-05-13）："Does it do anything useful? It does not look like it would. **Just a bunch of random text files about everything and nothing.**"
> **scotty_ea**（2026-05-10）："Theres a few decent one off items but **overall it's just someone else's harness setup. A lot of overlap.**"
> 均在 https://old.reddit.com/r/AIAgentsInAction/comments/1t84rlc/

**⑤ 屯积心理（针对整个 skill-pack 品类）**

> **Adventurous_Ad_9658**（2026-01-23，+6）："The problem with all these plugins is there's so much bloat they become convoluted, over engineered, etc. **It's almost like the psychology of hoarding but for Claude Code capabilities and you end up worse off than where you started.**"
> https://old.reddit.com/r/ClaudeAI/comments/1qkm7gs/

**⑥ 没人说得清它到底干什么用**

那条 1512 赞的帖子下面，`"What does it do?"` / `"But WHAT DOES IT DOOOOOOO"` / `"what product did he build?"` 被至少 6 个不同账号问过，**没有一个得到实质回答**。u/edward_jazzhands（2026-05-24）："Not a single person anywhere has an answer to this question."

**⑦ 关于 hackathon 光环的降温**

- u/nerdswithattitude："To be fair the prize was paid out in Anthropic credits"（$15,000 是额度不是现金）
- 主帖 OP（u/Best_Volume_3126）**在正文里根本没放仓库链接**，最高赞评论是 "Do you think it would be useful to link the github repo?"（63↑）。典型的流量农场转贴。

---

## 2. X/Twitter

（待补）

## 3. 汇总：点名到具体组件的取舍表

（待补）

## 4. 可信度评级说明

（待补）

## 5. 本渠道未能回答的问题（gaps）

（待补）

## 6. 试过但没结果的关键词清单

（待补）
