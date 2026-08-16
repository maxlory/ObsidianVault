# ECC 使用经验调研（第一批：仓库内部信号 + GitHub Issues）

> 调研日期：2026-08-01　|　对象：affaan-m/ECC v2.1.0（236k star）
> 数据全部本机实测或来自 GitHub 原始 issue，每条附出处
> ⚠️ 本批只覆盖 2 个渠道（仓库信号 + GitHub issues）。Exa/HN/Reddit/X/中文社区/YouTube/fork 生态 6 个渠道因 session 配额耗尽未完成

---

## 一、最硬的一条数据：常驻 catalog 成本 18.9k token/轮

**白话解释**：Claude Code 启动时会把每个 skill 的"名片"（name + description）全部读进上下文，好让模型知道有哪些能力可用。skill 越多，这张名片墙越大，**每一轮对话都要重付这笔钱**。

本机实测 v2.1.0（脚本逐个解析 281 份 SKILL.md + 67 份 agent .md 的 frontmatter）：

| 项目 | 数量 | 字符 | ≈token |
|---|---|---|---|
| skills | 281 | 60,771 | **15,193** |
| agents | 67 | 14,904 | **3,726** |
| **合计** | — | 75,675 | **18,919 token/轮** |
| 占 200k 窗口 | | | **9.5%** |

### 🔑 反直觉的地方：这个问题不仅没解决，还恶化了 48%

issue [#2482](https://github.com/affaan-m/ECC/issues/2482)（2026-07-09，**已 CLOSED**）由第三方用户实测报告：

> Enabling the plugin injects **all 182 skill descriptions (~10.3k tokens) + 48 agent descriptions (~2.5k tokens) ≈ 12.8k tokens into every single turn**... For users who rely on only ~15–20 skills, ~90% of that per-turn cost is unused.

那是 2.0.0-rc.1 时代的 12.8k。到 v2.1.0 skill 从 182 涨到 281，成本变成 **18.9k**。issue 关了，账单涨了 48%。

该 issue 还指出两个**结构性死结**：

1. **无法只关 skill 保留其它**：同一个 plugin 同时提供 MCP/agents/hooks，想减 skill 描述就只能整个禁用 plugin，连 MCP 和 agent 一起丢——作者本人称之为 "an unacceptable trade"。
2. **Claude Code 没有 `disabledSkills` 设置**。原文：`Claude Code has no disabledSkills setting, so anything an enabled plugin advertises gets loaded.`（[#1579](https://github.com/affaan-m/ECC/issues/1579)）

### 由此得到的第一条取舍结论

| 安装方式 | 常驻成本 | 倍数 |
|---|---|---|
| `/plugin install ecc@ecc`（广告全部 281+67） | **18.9k token/轮** | 7× |
| 只装默认 module `workflow-quality`（43 个 skill） | **2.7k token/轮** | 1× |

👉 **差 7 倍。这就是"老手不全装"的量化理由。** 走 selective install（`--profile` / `--modules`），不要走 plugin 全量。

### 顺带：description 最长的几个 skill（最吃名片墙）

`loop-design-check` 996 字符（249 token，单个就占了近 1/60 的 skill 预算）、`flox-environments` 638、`taste` 613、`intent-driven-development` 590、`videodb` 590、`ecc-recipes` 577。

---

## 二、作者自己的取舍表态（比任何第三方推荐都权威）

读 `manifests/install-modules.json` 得到的硬事实——**34 个 module 里只有 6 个标了 `defaultInstall: true`**：

| 默认装的 6 个 | kind | cost |
|---|---|---|
| `rules-core` | rules | light |
| `agents-core` | agents | light |
| `commands-core` | commands | medium |
| `hooks-runtime` | hooks | medium |
| `platform-configs` | platform | light |
| `workflow-quality` | skills | medium |

**注意：281 个 skill 里，作者默认只推 `workflow-quality` 这 43 个。** 其余 238 个全部需要 opt-in。

### 另外两个信号

**7 个 module 至今是 beta**：`optimization-workflows`、`operator-workflows`、`prediction-market-skills`、`ito-compute`、`media-generation`、`orchestration`、`machine-learning`。作者自己没把它们标 stable。

**12 个 `cost: heavy` 的 module 里有 8 个是翻译文档**（`docs-ja-jp` / `docs-zh-cn` / `docs-ko-kr` / `docs-pt-br` / `docs-ru` / `docs-tr` / `docs-vi-vn` / `docs-zh-tw` / `docs-de-de`）。剩下 3 个真·heavy 的是 `business-content`、`media-generation`、`supply-chain-domain`。

### 作者本人的实际配置（出自他自己的两篇指南）

- 插件：装了 14 个，**"我通常同一时间只启用其中 4-5 个"**
- MCP：配了 14 个，**"每个项目只启用大约 5-6 个"**，并明确经验法则「配置里放 20-30 个，但保持启用数 <10 个、活跃工具数 <80 个」
- 默认 MCP 从 6 个砍到 1 个（只留 `chrome-devtools`），砍掉的 `github`/`context7`/`exa`/`memory`/`playwright`/`sequential-thinking` 理由是：能被"包装 CLI 的 skill"或 harness 原生功能取代（`CHANGELOG.md:7`）
- 终端数：**"连我自己也总共只用 4 个左右的终端"**，明确反对 Boris 建议的"本地 5 个 + 上游 5 个"

👉 **作者自己都不全启用。** 这是最有力的"别全装"证据。

---

## 三、GitHub Issues 里的真实抱怨（300 条里筛出 45 条相关）

### 已修的（v2.1.0 本机核实过）

| issue | 问题 | 核实结果 |
|---|---|---|
| [#1579](https://github.com/affaan-m/ECC/issues/1579) 第 1 项 | 12 个 legacy shim skill 每个都自述 "Prefer the X skill"，却默认加载吃 catalog | ✅ 已修：`grep -rl "Legacy slash-entry shim" skills/` = **0 命中** |
| [#1579](https://github.com/affaan-m/ECC/issues/1579) 第 2 项 | 重复打包 Anthropic 官方 skill 的删节版（`claude-api` 官方 33KB vs ECC 5KB 且无 LICENSE；`frontend-design` 有 3 份 ECC 拷贝；`skill-creator`），会**静默遮蔽**更完整的官方版 | ✅ 已修：三者在 v2.1.0 的 `skills/` 里均**已不存在** |
| [#1557](https://github.com/affaan-m/ECC/issues/1557) | 「用户即使照着 README 做也还是被烧到」，要求明确的低上下文/无 hook 安装路径 | ✅ 已修：催生了 `--profile minimal` 和 `--without baseline:hooks` |
| [#2120](https://github.com/affaan-m/ECC/issues/2120) | context monitor 误报 "stuck loop"，同一条警告一轮内重复约 20 次 | ✅ 已修：改为按消息内容去重而非按调用计数 |
| [#1382](https://github.com/affaan-m/ECC/issues/1382) | hook 被自动注入 `~/.claude/settings.json`，但 `${CLAUDE_PLUGIN_ROOT}` 未解析导致全挂 | ✅ 已改为不碰 settings.json |

### 未修 / 仍开放的（截至 2026-08-01，117 个 open issue）

| issue | 问题 | 对你的影响 |
|---|---|---|
| [#2600](https://github.com/affaan-m/ECC/issues/2600) | hook wrapper 在每次无输出的 hook 运行时把**整个 stdin 负载回显到 stdout** | 噪音 + 潜在 token 浪费 |
| [#2608](https://github.com/affaan-m/ECC/issues/2608) | `gateguard` 对每个新文件的首次 Edit/Write 都拦，应该按会话 N 次拒绝后停止 | GateGuard 过度拦截，会持续打断你 |
| [#2636](https://github.com/affaan-m/ECC/issues/2636) | Stop hook 把一次性会话和 summarizer 子 agent 会话也持久化，破坏 `/resume-session` 选择 | 会话恢复功能受损 |
| [#2622](https://github.com/affaan-m/ECC/issues/2622) | `harness-optimizer` agent 里有 null skill 引用 | 该 agent 可能直接报错 |
| [#2630](https://github.com/affaan-m/ECC/issues/2630) | **22 个翻译版 SKILL.md 的 frontmatter 没有任何 YAML 解析器能接受** | 别装 `docs-zh-cn` 等翻译 module |
| [#2626](https://github.com/affaan-m/ECC/issues/2626) | memory-vault 的 `sameFileIdentity` 比较 stat dev，Windows 上恒为 0 | 仅 Windows |
| [#2619](https://github.com/affaan-m/ECC/issues/2619) | Plugin doesn't install | 插件路径本身有装不上的报告 |

### 被点名批评的具体组件

- **`code-reviewer` agent** —— [#1486](https://github.com/affaan-m/ECC/issues/1486) "Too many false positives"（误报太多）
- **`gateguard`** —— [#2608](https://github.com/affaan-m/ECC/issues/2608) 过度拦截；[#1406](https://github.com/affaan-m/ECC/issues/1406) 还报过 gateguard 性能问题
- **instincts** —— [#1231](https://github.com/affaan-m/ECC/issues/1231) "Instincts are stored but never proactively injected into session context"（存了但从不主动注入）。**这条独立印证了我们解剖笔记 05 的发现**：注入逻辑只按 confidence 排序取前 6 条，不做相关性匹配
- **`suggest-compact`** —— [#2461](https://github.com/affaan-m/ECC/issues/2461) / [#2290](https://github.com/affaan-m/ECC/issues/2290) 在大窗口模型上把 context 百分比算错（Opus 4.x 400k 窗口上翻倍，fable-5 上高估）
- **整体吃 token** —— [#1575](https://github.com/affaan-m/ECC/issues/1575) 标题直接就是 "This plugin make token limit so fast"
- **卸载** —— [#1425](https://github.com/affaan-m/ECC/issues/1425) "how uninstall ecc"（有人装了就想卸，且当时找不到方法）
- **覆盖用户文件** —— [#1505](https://github.com/affaan-m/ECC/issues/1505) "Install rules/skills into dedicated subdirectories to avoid overwriting user-custom files"。**这正是我们发现的 `commands/learn.md` 会被无条件覆盖那个问题**，用户早在 4 月就提过，至今 skills 之外的目录仍无保护
- **Windows 全线崩** —— [#2368](https://github.com/affaan-m/ECC/issues/2368) / [#2043](https://github.com/affaan-m/ECC/issues/2043) / [#1985](https://github.com/affaan-m/ECC/issues/1985) / [#1484](https://github.com/affaan-m/ECC/issues/1484) / [#1454](https://github.com/affaan-m/ECC/issues/1454) 五条独立报告，内联 `node -e` 的引号处理在 Git Bash 下路径变成 `C:\c\Users\`

---

## ⚠️ 四、一个关于可信度的观察

拉取的 300 条 issue 里，**评论数最多的一批看起来不是真实用户反馈**：

- `#2648 [مؤقت] إضافة أسرار Actions لتشغيل Terraform (staging)` —— 阿拉伯语的 Terraform 部署请求，与 ECC 无关
- `#2645 Zayidni scaffold added — request ECC to implement models & bid endpoint` —— 像是拿 ECC 仓库当自己项目的任务板
- `#2644 B-05:` / `#2643 B-03:` / `#2642 H-03:` —— 带编号前缀的批量安全 bug 报告，格式高度一致，疑似自动化工具产出
- `#1571 Spam — posted by automated tool in error, please disregard` —— 作者自己标记的机器人误发

**判断（置信度中，约 65%）**：这个仓库的 issue 区混有相当比例的自动化/无关内容，说明 236k star 的声量里有非有机成分。但**也确实存在高质量的第三方实战反馈**——#2482、#1579、#1557 三条的技术深度和实测数据不可能是刷的，作者也真的照着改了。所以结论是：**社区有真实使用者，但规模远小于 star 数暗示的量级。**

---

## 五、本批可执行结论

### ✅ 建议这样装

1. **绝不用 `/plugin install ecc@ecc`**——它广告全部 281+67，18.9k token/轮，且无法只关 skill。
2. 走 selective install，起步只要作者自己都认的默认集：`--profile core`（6 个 module / 626 文件 / skill 部分仅 2.7k token）。
3. 想更省就 `--profile minimal`（不含 hooks-runtime，460 文件）。
4. **装之前先备份 `~/.claude/commands/learn.md`**——[#1505](https://github.com/affaan-m/ECC/issues/1505) 至今未修，非 skills 目录无覆盖保护。

### ❌ 建议不装

- 任何 `docs-*` 翻译 module（8 个，全部 `cost: heavy`，且 [#2630](https://github.com/affaan-m/ECC/issues/2630) 报 22 个翻译 SKILL.md 的 frontmatter 无法被 YAML 解析器接受）
- 7 个 beta module，除非你确实要那个具体能力
- `gateguard`（[#2608](https://github.com/affaan-m/ECC/issues/2608) 过度拦截未修）——虽然它是 ECC 理论上最有意思的机制
- 指望 instincts 能用（[#1231](https://github.com/affaan-m/ECC/issues/1231) + 我们的代码核实：confidence 由 LLM 自填、衰减逻辑零实现、默认关闭）

### 🕳️ 本批未能回答的问题

- 有没有人公开过自己的「ECC 最小可用集」配置文件？（需 fork 生态 + Exa 渠道）
- HN/Reddit 上有没有系统性的批评长文？（需那两个渠道）
- 中文社区到底有没有人在真用？（需中文渠道）
- 有没有人实测过 hook 对响应速度的实际影响（毫秒级数据）？
