# ECC 维度 05：Memory 与 Instincts（学习到的直觉）

> 研究对象：`/Users/aa00158/harness-research/ECC`（浅克隆，只读）
> 提交：`e4e4163 fix(docs): restore main CI (#2623)`
> 研究日期：2026-08-01
> 证据纪律：每条结论标 `路径:行号`。无法从代码确认的标「未验证」。README 自夸话术标「仓库声称」。

---

## 0. 结论速览（TL;DR）

**一句话**：ECC 的「记忆」是四套互不相通的子系统；最独特的 instinct（学习到的直觉）本质是「让一个 Haiku 子进程读最近 500 行工具日志、按提示词模板自己写 markdown 文件」，confidence 分数由 LLM 自己填写、**代码里没有任何打分或衰减实现**，注入时也不做相关性匹配、只是把置信度最高的 6 条无条件塞进 SessionStart。

### 8 条核心结论

1. **instinct 是 markdown 文件，不是数据库记录。** 存在 `~/.local/share/ecc-homunculus/`（或 `$XDG_DATA_HOME`）下的目录树里，按项目哈希分目录。`.yaml`/`.yml`/`.md` 三种后缀都认。
2. **「学习」= 一条 shell 命令。** `claude --model haiku --print --allowedTools "Read,Write" -p "<40行提示词>"`（`observer-loop.sh:258-260`）。没有算法，全靠模型即兴发挥。
3. **confidence 没有打分代码。** 「3-5次=0.5, 6-10=0.7, 11+=0.85」是提示词文本（`observer-loop.sh:174`）。「+0.05确认/-0.1矛盾/-0.02每周衰减」（`observer.md:136-139`）**在代码里零实现**。
4. **注入不看 trigger。** `summarizeActiveInstincts()`（`session-start.js:406-460`）按 confidence 排序取前 6 条，只输出 `## Action` 第一行。README 说的 "Recalled when relevant" 属于仓库声称、代码不支持。
5. **「聚类」是删 6 个停用词后的字符串精确匹配**（`instinct-cli.py:1177-1185`）。`"new functions"` 和 `"a new function"` 聚不到一起（已实测验证）。
6. **默认关闭。** `config.json` 里 `observer.enabled: false`，绝大多数用户从没跑过这套系统。
7. **Vault 是另一套东西**，跟 instinct 零代码耦合。它是 `.ecc/memory/` 下的 markdown 文件柜，create-only、永远 `trust: unreviewed`、搜索是词频打分（不是语义检索）、**不会自动注入 context**（得 agent 主动调 `memory_search`）。跨 harness 靠共享文件系统 + `target_harnesses` 字段路由。
8. **SQLite（sql.js）跟记忆无关。** 数据库在 `.claude/ecc/state.db`，7 张表存 sessions / skill_runs / decisions / work_items 等运维数据。

### 一图看懂 instinct 全链路

```
用户在 Claude Code 里干活
   │
   ├─[PreToolUse/PostToolUse hook]→ observe-runner.js → observe.sh
   │        └→ 追加一行 JSON 到 projects/<hash>/observations.jsonl（截断5000字符+脱敏密钥）
   │        └→ 每 20 次发一个 SIGUSR1
   │
   ├─[后台守护进程 observer-loop.sh 收到 SIGUSR1]
   │        └→ 冷却60秒检查 → tail -500 行 → 起 Haiku 子进程
   │             └→ Haiku 读日志 → 用 Write 工具写 <id>.md 到 instincts/personal/
   │                  （confidence 由 Haiku 按提示词自己填）
   │
   └─[下次 SessionStart hook] session-start-bootstrap.js
            → run-with-flags.js → session-start.js
                 └→ 扫 4 个目录 → 滤 confidence<0.7 → 按 confidence 排序 → 取前 6
                      └→ 输出 {"hookSpecificOutput":{"additionalContext":"Active instincts:\n- [project 85%] ..."}}
                           └→ Claude Code 拼进会话开头
```

hook 注册链路已验证：`hooks/hooks.json` 只写了 `session-start-bootstrap.js`（`scripts/hooks/session-start-bootstrap.js:46-48` 再 spawn `run-with-flags.js session:start scripts/hooks/session-start.js minimal,standard,strict`）。`isHookEnabled()` 的逻辑是「当前 profile 是否在允许列表里」（`scripts/lib/hook-flags.js:57-69`），列表含全部三档，所以——**`ECC_HOOK_PROFILE=minimal` 会关掉观察（`observe.sh:163` 直接 exit）但不会关掉 instinct 注入**。要彻底关掉注入得用 `ECC_DISABLED_HOOKS="session:start"` 或 `ECC_SESSION_START_CONTEXT=off`。

---

## 1. 地图：记忆相关的文件都在哪

ECC 里叫「记忆」的东西其实是**四套彼此独立、几乎不互通的子系统**，先分清楚否则会读晕：

| 子系统 | 核心代码 | 存储位置 | 干什么 |
|---|---|---|---|
| **A. Instincts（continuous-learning-v2）** | `skills/continuous-learning-v2/` | `~/.local/share/ecc-homunculus/` | 观察 session → 后台 LLM 提炼「直觉」→ SessionStart 注入 |
| **B. Memory Vault（跨 harness 记忆）** | `scripts/memory.js`、`scripts/memory-mcp.mjs` | `.ecc/memory/*.json`（项目内） | 手工 remember/recall/handoff，跨工具共享 |
| **C. Session 持久化（WORKING-CONTEXT.md）** | `scripts/hooks/session-*.js` | 项目根 `WORKING-CONTEXT.md` | 会话开始/结束时读写一份人类可读的工作状态文件 |
| **D. contexts/ 预设上下文** | `contexts/*.md` | 仓库内静态文件 | 三个手写的模式切换文件，跟「学习」无关 |

关键文件清单：

- `skills/continuous-learning-v2/SKILL.md`（362 行）—— instinct 系统的说明书
- `skills/continuous-learning-v2/hooks/observe.sh`（586 行）—— PreToolUse/PostToolUse 观察钩子
- `skills/continuous-learning-v2/agents/observer.md`（190 行）—— 后台 observer agent 的**提示词**
- `skills/continuous-learning-v2/agents/observer-loop.sh`（366 行）—— 后台守护进程，真正调 LLM 的地方
- `skills/continuous-learning-v2/scripts/instinct-cli.py`（2073 行）—— instinct 的增删改查 CLI
- `scripts/hooks/session-start.js`（772 行）—— 把 instinct 注入 context 的地方
- `scripts/memory.js`（504 行）—— Memory Vault 的 CLI
- `scripts/memory-mcp.mjs`（649 行）—— Memory Vault 的 MCP 服务器包装
- `docs/design/ecc-memory-vault.md`（222 行）—— Vault 的设计文档
- `.claude/homunculus/instincts/inherited/everything-claude-code-instincts.yaml` —— 仓库里唯一一份**真实的 instinct 样本**（8 条，全是人工写的）

---

## 2. instinct 是什么：数据结构与存储位置

### 2.1 数据结构

instinct 就是一个**带 YAML frontmatter 的 markdown 文件**，一条一个文件。标准形态见 `skills/continuous-learning-v2/SKILL.md:52-72`：

```yaml
---
id: prefer-functional-style          # kebab-case 唯一标识
trigger: "when writing new functions" # 什么时候该想起它（自然语言）
confidence: 0.7                       # 0.3~0.9 的置信度
domain: "code-style"                  # 分类标签
source: "session-observation"         # 来源：观察 / 导入 / 仓库分析
scope: project                        # project 还是 global
project_id: "a1b2c3d4e5f6"
project_name: "my-react-app"
---

# Prefer Functional Style

## Action
Use functional patterns over classes when appropriate.

## Evidence
- Observed 5 instances of functional pattern preference
- User corrected class-based approach to functional on 2025-01-15
```

白话：**一条 instinct = 「什么时候（trigger）→ 该怎么做（Action）」+ 一个我有多确信的数字（confidence）+ 为什么这么说（Evidence）**。它不是嵌入向量、不是知识图谱，就是一段 markdown。

字段的权威定义分散在三处，且**格式其实有两个版本**（后面 §13 会说破这个坑）：
- `.md` 单文件版：`skills/continuous-learning-v2/agents/observer-loop.sh:171-190`（LLM 被要求写成 `<id>.md`）
- `.yaml` 多文档版：`.claude/homunculus/instincts/inherited/everything-claude-code-instincts.yaml:4-11`（一个文件里用 `---` 分隔多条）

### 2.2 存储位置：不是数据库，是文件目录树

目录布局见 `skills/continuous-learning-v2/SKILL.md:243-269`：

```
${XDG_DATA_HOME:-~/.local/share}/ecc-homunculus/
├── identity.json           # 用户画像
├── projects.json           # 项目哈希 → 名字/路径/remote 的注册表
├── observations.jsonl      # 全局观察流水（兜底）
├── instincts/
│   ├── personal/           # 全局自动学到的
│   └── inherited/          # 全局导入的
├── evolved/{agents,skills,commands}/
└── projects/
    └── a1b2c3d4e5f6/       # 12 位项目哈希
        ├── project.json
        ├── observations.jsonl
        ├── observations.archive/
        ├── instincts/{personal,inherited}/
        └── evolved/{skills,commands,agents}/
```

三条判定证据：
- 路径解析逻辑在 `skills/continuous-learning-v2/scripts/lib/homunculus-dir.sh`，优先级 `CLV2_HOMUNCULUS_DIR` > `$XDG_DATA_HOME/ecc-homunculus` > `$HOME/.local/share/ecc-homunculus`（`SKILL.md:138-145` 有对应说明）。
- 刻意搬出 `~/.claude` 的原因写在 `SKILL.md:140`：Claude Code 的敏感路径保护会挡住后台进程往 `~/.claude` 写文件。
- 项目哈希来自 `git remote get-url origin`，取不到就退到 repo 路径，再取不到就落全局（`SKILL.md:129-136`）。

> 🔑 **重要**：instinct **不在 SQLite 里**。仓库依赖里的 `sql.js` 是给别的东西用的（见 §10）。instinct 全程是纯文件 + 目录扫描。

---

## 3. 谁写入 instinct：产生路径与触发时机

一共有 **4 条写入路径**，其中只有第 1 条算「自动学习」：

### 3.1 路径 A：后台 observer（唯一的自动学习通道）

数据流（每一步都能指到代码）：

**第 1 步 — 钩子采集。** 每次工具调用前后，`observe.sh` 被 PreToolUse / PostToolUse 触发（`skills/continuous-learning-v2/hooks/observe.sh:15-28` 用参数或 `CLAUDE_HOOK_EVENT_NAME` 判断是 pre 还是 post）。它把这次调用序列化成一行 JSON 追加到 `observations.jsonl`：

```
{"timestamp":..., "event":"tool_start|tool_complete", "tool":"Edit",
 "session":"abc", "project_id":"a1b2c3", "project_name":"my-app",
 "input":"<截断到 5000 字符>", "output":"<截断到 5000 字符>"}
```
写入代码：`observe.sh:316-362`；截断在 `observe.sh:237-245`；密钥脱敏正则在 `observe.sh:344-354`。

值得注意的工程细节（这些是这套系统里质量最高的部分）：
- **5 层「不要观察自己」过滤**（`observe.sh:148-182`）：入口类型必须是 cli/sdk-ts/claude-desktop/claude-vscode；`ECC_HOOK_PROFILE=minimal` 跳过；`ECC_SKIP_OBSERVE=1` 跳过；有 `agent_id`（子 agent 会话）跳过；cwd 命中 `observer-sessions,.claude-mem` 跳过。防的是「ECC 观察自己的 observer 会话」造成自循环。
- **密钥脱敏**用的是一条有界量词正则，注释明确说是为了修 catastrophic backtracking 把 CPU 打满的 bug（`observe.sh:273-279`，引用 issue #2278）。
- **SIGALRM 8 秒自杀**（`observe.sh:281-289`）：赶在异步 hook 10 秒超时前自己退出，避免留下孤儿进程。
- **文件超 10 MB 自动归档**（`observe.sh:299-306`），观察文件超 30 天自动删（`observe.sh:203-208`）。

**第 2 步 — 节流通知。** 每 20 次观察（`ECC_OBSERVER_SIGNAL_EVERY_N` 默认 20，`observe.sh:480`）才给 observer 进程发一次 `SIGUSR1`（`observe.sh:561-583`）。计数器的读-改-写用 flock 或 mkdir 原子锁保护（`observe.sh:494-558`）。

**第 3 步 — 后台守护进程被唤醒。** `observer-loop.sh` 收到 USR1（`observer-loop.sh:293-320`），检查冷却时间（默认 60 秒，`observer-loop.sh:17`）和重入锁，然后调 `analyze_observations`。

**第 4 步 — 真正的「学习」：拉起一个 Haiku 子进程。** 这是全系统最关键的 10 行：

```bash
ECC_SKIP_OBSERVE=1 ECC_HOOK_PROFILE=minimal claude --model "${ECC_OBSERVER_MODEL:-haiku}" \
  --max-turns "$max_turns" --print \
  --allowedTools "Read,Write" \
  -p "$prompt_content" < /dev/null >> "$LOG_FILE" 2>&1 &
```
`skills/continuous-learning-v2/agents/observer-loop.sh:258-260`

也就是说：**「学习」= 起一个受限的 Claude CLI 子进程（只给 Read/Write 两个工具），让它读最近 500 行观察日志，自己判断有没有模式，自己把 instinct 文件写出来。** 提示词全文在 `observer-loop.sh:162-202`。

配套的工程约束：
- 只喂最后 500 行（`ECC_OBSERVER_MAX_ANALYSIS_LINES` 默认 500，`observer-loop.sh:145`），防止把几 MB 日志塞给 LLM。
- `max_turns` 按 `analysis_count / 10` 自动缩放，下限 20 上限 100（`observer-loop.sh:224-226`）。
- 120 秒看门狗超时杀进程（`observer-loop.sh:216, 263-270`）。
- **只有分析成功（exit 0）才归档观察文件**，失败保留待重试（`observer-loop.sh:277-290`，注释引用 issue #2370）。
- Windows 上默认直接跳过分析（`observer-loop.sh:127-130`，issue #295 非交互挂起）。
- 模型可通过 `ECC_OBSERVER_MODEL` 换成 opus（`observer-loop.sh:258` 及上方注释）。

**默认关闭。** `skills/continuous-learning-v2/config.json` 里 `observer.enabled` 默认 `false`（`SKILL.md:222-237` 给出同样的默认值）。所以开箱即用的 ECC 是**不会自动学任何东西的**，需要用户手动打开。

### 3.2 路径 B：`/instinct-import` 手工导入

从别人导出的文件里读 instinct，写进本地目录。命令定义在 `.opencode/commands/instinct-import.md` 等各 harness 目录下，实现在 `instinct-cli.py`。

### 3.3 路径 C：仓库策展（repo-curation）

`.claude/homunculus/instincts/inherited/everything-claude-code-instincts.yaml` 就是这类——8 条 instinct **全部是人工手写的**，`source: repo-curation`，confidence 是拍脑袋填的 0.8/0.84/0.85/0.86/0.88/0.9（该文件 `:7, :27, :47, :67, :87, :107, :127, :147`）。

### 3.4 路径 D：ECC-Tools GitHub App（外部服务）

`SKILL.md:355` 提到「Generate instincts from repo history」的 GitHub App。**这是仓库外的闭源服务，代码不在本仓库里，无法核实其提取质量**（未验证）。生成的 instinct 带 `source: "repo-analysis"` + `source_repo`，`observer.md:184-189` 说这类应该给 0.7+ 的初始置信度。

`.claude/rules/everything-claude-code-guardrails.md` 就是这个 App 的产物（文件头写着 "Generated by ECC Tools from repository history. Review before treating it as a hard policy file."）。看它输出的内容——「Prefer `conventional` commit messaging」「Use `camelCase` file naming」「Prefer `relative` imports and `mixed` exports」——明显是**对仓库做静态统计后填模板**得到的，不是深度理解。

---

## 4. confidence 分数怎么算：打分代码解剖

### 4.1 🔑 核心发现：初始 confidence 没有打分代码，是写在提示词里让 LLM 自己填的

这是本维度最重要的结论。搜遍全仓库，**找不到任何计算初始 confidence 的函数**。真正决定分数的是这行提示词：

```
confidence: <0.3-0.85 based on frequency: 3-5 times=0.5, 6-10=0.7, 11+=0.85>
```
`skills/continuous-learning-v2/agents/observer-loop.sh:174`

以及 observer agent 提示词里的同一张表：

```
- 1-2 observations: 0.3 (tentative)
- 3-5 observations: 0.5 (moderate)
- 6-10 observations: 0.7 (strong)
- 11+ observations: 0.85 (very strong)
```
`skills/continuous-learning-v2/agents/observer.md:130-134`

也就是说：**「观察了几次」这个计数是 Haiku 自己数的，映射到分数也是 Haiku 自己做的，没有任何代码去校验或复算。** 一个 Haiku 模型读 500 行 JSONL，声称「我看到这个模式 7 次」，于是写下 `confidence: 0.7`——这个 7 没有任何程序验证。

### 4.2 confidence 的「演化」规则同样只是文字

`observer.md:136-139` 写着：

```
- +0.05 for each confirming observation
- -0.1 for each contradicting observation
- -0.02 per week without observation (decay)
```

`SKILL.md:318-326` 也有一段散文版（"Confidence increases when... Confidence decreases when..."）。

我在 `instinct-cli.py`(2073 行)里搜索这些常数的实现情况见 §4.3。

### 4.3 instinct-cli.py（2073 行）里到底有没有 confidence 数学：没有

我把 `instinct-cli.py` 里所有出现 `confidence` 的行都过了一遍（共 40+ 处），**全部是「读出来、比大小、算平均、打印百分比」，没有一处修改或重算 confidence**：

- 解析：`instinct-cli.py:552-556` —— 从 frontmatter 读 float，解析失败退回 0.5
- 显示：`:101-110` 画一个 10 格进度条；`:863-865` 按 confidence 降序排列
- 过滤：`:964`（import 的 `--min-confidence`）、`:1098`（export）、`:1174`（>=0.8 算高置信）、`:1220`（>=0.7 的 workflow 算命令候选）
- 求平均：`:1191`、`:1307`、`:1509` —— 用于集群和跨项目提升
- 去重取大：`:941`、`:955`、`:1548` —— 同 id 保留 confidence 高的那份

搜 `decay`、`0.05`、`0.02`、`-0.1`、`contradict` 关键字：**零命中**。

> 🔑 **结论**：`observer.md:136-139` 承诺的「每次确认 +0.05 / 每次矛盾 -0.1 / 每周不出现衰减 -0.02」**在整个仓库里没有任何实现**。它是写给 LLM 看的提示词文本，不是程序。confidence 一旦被 Haiku 写进文件，除非再次被 Haiku 覆写，否则永远不变。「时间衰减」根本不存在。

### 4.4 真正在代码里的常数只有三个门槛

```python
PROMOTE_CONFIDENCE_THRESHOLD = 0.8   # 提升到全局需要的平均置信度
PROMOTE_MIN_PROJECTS = 2             # 需要在几个项目里出现过
PENDING_TTL_DAYS = 30                # pending instinct 的过期天数
```
`skills/continuous-learning-v2/scripts/instinct-cli.py:128-133`

用在 `:1308` 和 `:1510`（`_promote_auto`：跨项目出现 >= 2 次且平均置信 >= 0.8 就提升到全局）。

### 4.5 ⚠️ 一个明显的死代码：pending/ 目录没人写

`cmd_prune`（`:1942-1986`）负责删 `instincts/pending/` 里超过 30 天的文件，`observer-loop.sh:336` 每次启动都会调它。但**全仓库没有任何代码往 `pending/` 目录写文件**——observer 的写入目标是 `INSTINCTS_DIR="${PROJECT_DIR}/instincts/personal"`（`agents/start-observer.sh:52`），import 写 `personal/` 或 `inherited/`。

我 grep 了 `pending` 的所有出现（`instinct-cli.py:15,132,791-807,1851-1935`，`observer-loop.sh:334`），全部是**读取和删除**，没有写入。所以 `/instinct-status` 里那句「N pending instincts awaiting review」永远是 0，这套「人工审核队列」是**设计了但没接上的功能**。

### 4.6 ⚠️ 另一个 bug：status 命令数不到 instinct

`start-observer.sh:160` 统计 instinct 数量用的是：

```bash
instinct_count=$(find "$INSTINCTS_DIR" -name "*.yaml" 2>/dev/null | wc -l)
```

但 observer 的提示词让 Claude 写的是 `.md` 文件（`observer-loop.sh:166`：`write an instinct file directly to ${INSTINCTS_DIR}/<id>.md`）。所以 `start-observer.sh status` 报告的 instinct 数会一直是 0。（`instinct-cli.py:130` 的 `ALLOWED_INSTINCT_EXTENSIONS` 三种后缀都认，所以 `/instinct-status` 是对的——只有这个 shell 脚本的统计错了。）

---

## 5. instinct 怎么被注入进 context：SessionStart 注入链路

### 5.1 注入代码的位置

全部在 `scripts/hooks/session-start.js`，核心函数 `summarizeActiveInstincts()`（`:406-460`）。它在 `main()` 里是**第一个**被加进上下文的部分（`:617-621`）：

```js
if (shouldInjectContext) {
  const instinctSummary = summarizeActiveInstincts(observerContext);
  if (instinctSummary) {
    additionalContextParts.push(instinctSummary);
  }
  ...
}
```

### 5.2 完整算法（7 步）

1. **扫 4 个目录**（`:407-420`）：项目级 `personal/` + `inherited/`，全局 `personal/` + `inherited/`。项目级排在前面。若当前不在已识别项目里（`isGlobal`），跳过项目级两个目录。
2. **解析文件**（`:368-393`）：只认 `.yaml/.yml/.md` 后缀，按文件名字典序读。解析器 `parseInstinctFile()`（`:319-366`）是**手写的行扫描器**——遇到 `---` 切换 frontmatter 状态，用 `line.indexOf(':')` 切 key/value，`confidence` 转 float 失败退回 0.5。**不是真 YAML 解析器**。
3. **按 confidence 过滤**（`:427`）：`instinct.confidence < confidenceThreshold` 就丢掉，默认阈值 0.7。
4. **按 id 去重**（`:428-431`）：同 id 时项目级覆盖全局级。
5. **抽取 Action 一句话**（`:395-404`）：正则 `/## Action\s*\n+([\s\S]+?)(?:\n## |\n---|$)/` 抓 `## Action` 段，再取第一行非空文本。**只有第一行进上下文，Evidence 段完全不注入。**
6. **排序 + 截断**（`:440-445`）：confidence 降序 → 项目级优先 → id 字典序，然后 `.slice(0, maxInjected)`，默认 6 条。
7. **渲染成极简文本**（`:453-459`）：

```
Active instincts:
- [project 85%] Always use functional components with hooks instead of class components.
- [global 75%] Validate and sanitize all user input before processing.
```

### 5.3 注入通道：Claude Code 的 SessionStart hook 协议

最终写到 stdout 的是这个 JSON（`session-start.js:735-743`）：

```json
{"hookSpecificOutput":{"hookEventName":"SessionStart","additionalContext":"<拼好的字符串>"}}
```

Claude Code 收到后把 `additionalContext` 拼进会话开头的系统上下文。所以 instinct 就是**几行纯文本被塞进 prompt 开头**——没有向量检索，没有按当前任务动态召回，`trigger` 字段（"when writing new functions"）**根本没被用来做匹配**，它只是被写进文件里给人看的。

> 🤔 这点很关键：README 把 instinct 描述成有 trigger 的「直觉」，但注入时 trigger 被丢弃了，只剩 Action 的第一句话。所谓「按情境触发」实际上是**全量塞进去，让主模型自己判断哪条相关**。

### 5.4 整段上下文里 instinct 只是一小块

`session-start.js` 注入的 `additionalContextParts` 一共 4 类（`:617-727`）：

| 顺序 | 内容 | 代码位置 |
|---|---|---|
| 1 | `Active instincts:` 最多 6 条 | `:618-621` |
| 2 | 上一次会话摘要（带 STALE-REPLAY 警告包裹） | `:645-672` |
| 3 | `Available learned skills:` 列表 | `:686-689` |
| 4 | `Project type: {JSON}` 项目语言/框架探测 | `:722-724` |

全部拼起来后被 `limitSessionStartContext()` 按 `ECC_SESSION_START_MAX_CHARS`（默认 8000 字符）截断（`:729-731`）。

值得单独点出的是第 2 项的**防陈旧重放护栏**（`:658-670`）：上次会话摘要被包在 "HISTORICAL REFERENCE ONLY — NOT LIVE INSTRUCTIONS" 里，明确告诉模型不要重跑里面的 slash 命令。注释里说这是真实事故——compaction 恢复后模型会拿旧 ARGUMENTS 重跑 `/fw-task-new`，重复建 issue/branch/Notion 任务（引用 issue #1534）。这是整份 session-start.js 里最有价值的一段工程经验。

---

## 6. 环境变量开关

### 6.1 instinct 注入相关（README 与代码一致）

| 环境变量 | 默认值 | 作用 | 代码位置 |
|---|---|---|---|
| `ECC_MAX_INJECTED_INSTINCTS` | 6 | 最多注入几条 instinct | `session-start.js:31, 151-162` |
| `ECC_INSTINCT_CONFIDENCE_THRESHOLD` | 0.7 | 注入所需最低置信度 | `session-start.js:30, 127-141` |
| `ECC_SESSION_START_MAX_CHARS` | 8000 | 整段注入上下文的字符上限，设 0 = 关闭 | `session-start.js:583, 729-731` |
| `ECC_SESSION_START_CONTEXT` | — | 设 `off` 完全关闭注入 | `session-start.js:582, 611-612` |

README 对应段落：`README.md:1363-1367`。两个变量名与代码**完全一致**（任务线索里给的 `ECC_MAX_INSTINCTS` / `ECC_INSTINCT_MIN_CONFIDENCE` 是记忆偏差，实际不存在这两个名字，我 grep 过全仓库零命中）。

两个 getter 都做了严格校验：非数字、超出 0-1 范围、非正整数都会静默退回默认值（`:135-140`、`:159-162`）。

### 6.2 observer 相关

| 环境变量 | 默认值 | 作用 | 代码位置 |
|---|---|---|---|
| `ECC_OBSERVER_MODEL` | `haiku` | 分析用的模型 | `observer-loop.sh:258` |
| `ECC_OBSERVER_TIMEOUT_SECONDS` | 120 | 单次分析看门狗超时 | `observer-loop.sh:216` |
| `ECC_OBSERVER_MAX_ANALYSIS_LINES` | 500 | 每次喂给 LLM 的观察行数 | `observer-loop.sh:145` |
| `ECC_OBSERVER_MAX_TURNS` | 自动（count/10，20~100） | LLM 最大轮次 | `observer-loop.sh:221-227` |
| `ECC_OBSERVER_ANALYSIS_COOLDOWN` | 60 秒 | 两次分析最小间隔 | `observer-loop.sh:17` |
| `ECC_OBSERVER_IDLE_TIMEOUT_SECONDS` | 1800 | 无活动多久后守护进程自杀 | `observer-loop.sh:18` |
| `ECC_OBSERVER_SIGNAL_EVERY_N` | 20 | 每几次观察发一次 SIGUSR1 | `observe.sh:480` |
| `ECC_OBSERVER_ALLOW_WINDOWS` | false | Windows 上强制开启分析 | `observer-loop.sh:127` |
| `ECC_SKIP_OBSERVE` | 0 | 设 1 跳过观察（观察自己时用） | `observe.sh:166` |
| `ECC_HOOK_PROFILE` | standard | 设 `minimal` 跳过观察 | `observe.sh:163` |
| `ECC_OBSERVE_SKIP_PATHS` | `observer-sessions,.claude-mem` | cwd 命中就跳过 | `observe.sh:173` |
| `CLV2_HOMUNCULUS_DIR` | — | 强制指定数据目录（必须绝对路径） | `instinct-cli.py:54-59` |
| `CLV2_PYTHON_CMD` | — | 指定 python 解释器 | `observe.sh:70-73` |
| `ECC_AGENT_DATA_HOME` | `~/.claude` | 会话/学到的 skill 的存储根（多 harness 隔离用） | `README.md:1386-1398` |

---

## 7. vault 是什么

### 7.1 一句话定义

Memory Vault = **一个放在 `.ecc/memory/` 下的 markdown 文件柜**，每份记忆是一个带严格 YAML frontmatter 的 `.md` 文件，通过 `ecc memory` CLI 或一个可选的 MCP 服务器读写。它跟 instinct 系统**完全没有代码耦合**——是两套独立的东西。

设计文档：`docs/design/ecc-memory-vault.md`（222 行）。核心实现：`scripts/lib/memory-vault.js`（778 行）。CLI：`scripts/memory.js`（504 行）。MCP：`scripts/memory-mcp.mjs`（649 行）。

### 7.2 目录结构与三个 scope

```
<repo>/.ecc/memory/
├── project/   ← 本仓库私有，自动加 fail-closed .gitignore
│   ├── contexts/ decisions/ facts/ handoffs/ lessons/ notes/ preferences/ runbooks/
└── team/      ← 打算给人审核后提交进 git 的
    └── <同样 8 个 kind 目录>

~/.ecc/memory/ ← 跟着人走，跨仓库；召回必须显式 --scope user
└── <同样 8 个 kind 目录>
```
`docs/design/ecc-memory-vault.md:64-89`；建目录代码 `scripts/lib/memory-vault.js:259-274`（`initializeVault`，目录权限 0700）。

`project` scope 会被强制写一个 `.gitignore`，内容必须严格是 `*\n!.gitignore\n`，否则**直接抛错拒绝写入**（`memory-vault.js:229-248`）。这是防止把本地记忆误提交进仓库。

### 7.3 文档格式

```markdown
---
schema: "ecc.memory.v1"
id: "mem_20260726_01k123example"
title: "Authentication migration handoff"
kind: "handoff"
scope: "project"
trust: "unreviewed"
status: "active"
source_harness: "codex"
target_harnesses: ["claude"]
tags: ["auth", "migration"]
links: ["mem_20260725_01kolder"]
created_at: "2026-07-26T20:00:00.000Z"
updated_at: "2026-07-26T20:00:00.000Z"
---

The token rotation tests pass. The remaining task is ...
```
`docs/design/ecc-memory-vault.md:95-113`

ID 生成规则：`mem_<YYYYMMDD>_<20位随机十六进制>`（`memory-vault.js:276-280`）。

### 7.4 三条硬性设计约束（这部分设计得相当克制，值得肯定）

**1. 只能创建，不能覆盖。** `saveMemory` 用 `writeCreateOnlyTextFile`，同 ID 已存在就抛 `Memory <id> already exists; writes are create-only.`（`memory-vault.js:318-327`）。要更新只能新建一条并用 `links` 指向旧的。

**2. 所有条目永远是 `trust: "unreviewed"`。** `normalizeSaveInput` 把 `trust` 硬编码成 `'unreviewed'`，调用方传什么都没用（`memory-vault.js:293`）。设计文档明确说明理由：

> "The runtime exposes no automated promotion transition. This is intentional: a shell-capable agent cannot be treated as an independent human approval boundary."
> `docs/design/ecc-memory-vault.md:136-138`

白话：**一个能跑 shell 的 agent 不能当成「人类批准」这一环**，所以记忆永远是「未审核的参考资料」，绝不会自动变成规则或策略。想让它变成项目真理，得人肉把内容抄进仓库里正式的 rules / ADR / runbook。

**3. 写入前扫密钥。** `saveMemory` 把整份文档 JSON 化后跑 `findPotentialSecrets`，命中就拒写（`memory-vault.js:308-311`）。设计文档诚实标注这是 "best-effort backstop, not a complete secret classifier"（`docs/design/ecc-memory-vault.md:26-27`）。

### 7.5 「搜索」是词频打分，不是语义检索

`scoreMemory()`（`memory-vault.js:529-552`）是纯词法打分：

```js
const phraseScore = title.includes(normalizedQuery) ? 20
                  : body.includes(normalizedQuery) ? 5 : 0;
return tokens.reduce((score, token) =>
    score
  + (title.includes(token)    ? 8 : 0)
  + (tags.includes(token)     ? 6 : 0)
  + (metadata.includes(token) ? 3 : 0)
  + Math.min(countOccurrences(body, token), 5)
, phraseScore);
```

也就是：**标题整句命中 20 分，标题含词 8 分，标签命中 6 分，元数据 3 分，正文每出现一次 1 分（封顶 5 分）**。没有 embedding、没有 BM25、没有 TF-IDF 归一化。

这点设计文档自己写清楚了（`:29-30`：「Search is bounded lexical retrieval in the first release」），**没有夸大**，README 也没吹成语义检索。这里给个正面评价：宣传与实现一致。

资源边界也硬编码了：最多扫 5000 个文件、16 MB、query 最长 500 字符、返回最多 100 条（`memory-vault.js:32-36`）。

### 7.6 🔑 Vault 不会被自动注入 context

我 grep 了 `scripts/hooks/` 和 `hooks/` 目录里所有对 `memory-vault` / `searchMemories` 的引用：**零命中**。全仓库只有 `scripts/memory.js`、`scripts/memory-mcp.mjs` 和 4 个测试文件 require 它。

设计文档的 Open Questions 也确认了这一点：

> "Whether SessionStart should inject links to governed project references or keep all recall explicitly task-scoped. **The first release keeps recall explicit.**"
> `docs/design/ecc-memory-vault.md:210-212`

**所以 Vault 是「agent 得自己想起来去查」的东西**，跟 instinct（SessionStart 自动注入）完全不同的机制。实际使用里这意味着：如果模型不主动调 `memory_search`，Vault 里的内容等于不存在。

---

## 8. 跨 harness 记忆：handoff / recall 到底怎么传

### 8.1 传递机制：就是共享文件系统 + 一个路由字段

没有网络、没有数据库、没有同步服务。**跨 harness 传递 = 大家读同一个目录**。

路由靠两个 frontmatter 字段：
- `source_harness`：谁写的（`"codex"`）
- `target_harnesses`：给谁看的（`["claude"]` 或 `["all"]`）

典型流程（`README.md:838-856` 的实例）：

```bash
# 在 Hermes 里干完一半活，写一份交接
ecc memory handoff \
  --from hermes --target codex \
  --title "Continue authentication migration" \
  --body-file ./handoff.md

# 换到 Codex，把它捞出来
ecc memory search "authentication migration" --target-harness codex
ecc memory read <memory-id>
```

过滤代码在 `memory-vault.js:605-609`：

```js
.filter(({ memory }) => (
  !targetHarness
  || memory.targetHarnesses.includes('all')
  || memory.targetHarnesses.includes(targetHarness)
))
```

### 8.2 ⚠️ `--target-harness` 不是权限，只是过滤器

设计文档白纸黑字：

> "Its `--target-harness` option is a caller-selected routing filter, **not an authorization boundary**."
> `docs/design/ecc-memory-vault.md:163-164`

CLI 层谁都能传任意 `--target-harness` 值把别人的记忆捞出来。真正的「身份绑定」只在 MCP 层（见 §9）。

### 8.3 另一条独立的跨 harness 路径：ECC_AGENT_DATA_HOME

这跟 Vault 无关，是 **session 持久化**那套的多 harness 隔离开关。默认所有 harness 都往 `~/.claude` 写会话摘要，同机同时用 Claude Code 和 Cursor 会互相覆盖，所以给 Cursor 单独设一个根：

```bash
export ECC_AGENT_DATA_HOME="$HOME/.cursor/ecc"
```

影响的路径：`$ECC_AGENT_DATA_HOME/session-data/`、`skills/learned/`、`session-aliases.json`、`metrics/`（`README.md:1386-1398`，引用 issue #2065）。

注意方向：这是**隔离**开关，不是共享开关——目的是让两个 harness 别互相踩，跟 Vault 的「共享」正好相反。

### 8.4 安装门槛：Vault 默认不在 PATH 上

README 明确警告（`README.md:826-832`）：

> "Skill-only, minimal, manual, and Claude plugin installs do not put the Memory Vault runtime on `PATH`. Install the npm runtime separately before using the CLI or optional MCP server:"
> ```bash
> npm install -g ecc-universal
> ```

`ecc-memory-mcp` 这个 bin 注册在 `package.json:425`（指向 `scripts/memory-mcp.mjs`），只有走 npm 全局安装才拿得到。**用 Claude 插件方式装 ECC 的人默认用不了 Vault。**

---

## 9. memory-mcp：MCP 服务器形态的记忆接口

### 9.1 它是什么

`scripts/memory-mcp.mjs`（649 行）是一个**手写的 stdio JSON-RPC MCP 服务器**（没有用 `@modelcontextprotocol/sdk`，协议帧、Ajv schema 校验、消息边界全是自己实现的，见 `:20-33` 的协议常量和 `:142-152` 的 Ajv 校验器）。

它只暴露 4 个工具（`memory-mcp.mjs:43-141` 的 `TOOL_DEFINITIONS`）：

```
memory_save
memory_search
memory_read
memory_doctor
```

**注意少了什么**：没有 `memory_promote`、没有 `memory_delete`、没有 `memory_update`。设计文档 `:57-58` 明确写 "It has no review or promotion tool."

### 9.2 身份绑定：客户端不能自称是谁

启动时必须给一个小写的 `ECC_MEMORY_HARNESS` 环境变量。这个身份：
- 决定所有写入的 `source_harness`
- 限制搜索/读取只能看到 targeted 到自己或 `all` 的记忆
- **客户端不能在工具参数里覆盖它**

`docs/design/ecc-memory-vault.md:166-170`。这是 §8.2 里 CLI 层缺的那道边界，在 MCP 层补上了——因为 MCP 场景下调用方是模型，不能信。

### 9.3 user scope 的二次开关

MCP 默认**禁止访问 user scope**，除非操作员额外设 `ECC_MEMORY_ALLOW_USER_SCOPE=1`，而且客户端还得显式请求那个 scope（`docs/design/ecc-memory-vault.md:169-171`；代码里的授权检查在 `memory-mcp.mjs:165-189` 的 `resolveServiceSecurity` / `assertScopesAuthorized`）。

理由很直白：`~/.ecc/memory/` 是跨所有仓库的个人记忆，随便一个项目里的 MCP 客户端不该能读到。

### 9.4 资源边界

```js
const MAX_MESSAGE_BYTES = 1024 * 1024;   // 单条消息 1 MB
const MAX_RESPONSE_BYTES = 1024 * 1024;
const MAX_PENDING_MESSAGES = 64;
const MAX_PENDING_BYTES = 2 * MAX_MESSAGE_BYTES;
```
`scripts/memory-mcp.mjs:29-32`

ID 和 slug 都有正则白名单（`:33-35`）。错误返回不带堆栈、不带密钥值（`docs/design/ecc-memory-vault.md:172`）。

### 9.5 默认关闭

README `:858` 说得清楚：需要手动把 `mcp-configs/mcp-servers.json` 里的 `ecc-memory-vault` 条目加进各个 harness 的配置，才会启用。

---

## 10. SQLite（sql.js）用在哪，数据库文件在哪

### 10.1 答案：跟 instinct 和 Vault 都无关

`package.json:462` 里 `"sql.js": "1.14.1"`。用它的只有两个文件：

- `scripts/lib/state-store/index.js:6`（`const initSqlJs = require('sql.js')`）
- `scripts/lib/control-pane/state.js:7`

**数据库文件路径**：`scripts/lib/state-store/index.js:12`

```js
const DEFAULT_STATE_STORE_RELATIVE_PATH = path.join('.claude', 'ecc', 'state.db');
```

即项目下的 `.claude/ecc/state.db`。

### 10.2 库里存什么：7 张表，全是运维/编排数据

从 `scripts/lib/state-store/migrations.js:4-132`：

| 表 | 存什么 |
|---|---|
| `schema_migrations` | 迁移版本号 |
| `sessions` | 会话记录（状态、起止时间） |
| `skill_runs` | skill 执行记录（session_id、结果、时间） |
| `skill_versions` | skill 版本与 promoted_at |
| `decisions` | 决策记录 |
| `install_state` | 安装状态 |
| `governance_events` | 治理事件（resolved_at） |
| `work_items` | 工作项（对应 GitHub / Linear 条目） |

实体类型定义见 `scripts/lib/state-store/schema.js:9-17`，JSON Schema 在 `schemas/state-store.schema.json`。

> 🔑 **没有一张表跟 instinct 或 memory 有关。** instinct 是 markdown 文件，Vault 也是 markdown 文件。sql.js 服务的是 ECC 的**控制面板 / 状态跟踪**（`scripts/sessions-cli.js`、`scripts/status.js`、`scripts/work-items.js`、`scripts/lib/control-pane/server.js`、`scripts/lib/skill-evolution/tracker.js` 这几个消费者）。

### 10.3 顺带一个实现细节

`sql.js` 是 WebAssembly 版 SQLite，**不落地增量写**——每次事务结束要把整个数据库 export 成 buffer 再 `fs.writeFileSync` 覆盖整个文件（`state-store/index.js:34-43`）。注释里还专门警告 `db.export()` 会隐式结束当前事务（`:31`）。选它而不是 `better-sqlite3` 大概率是为了避免原生编译依赖（跨平台安装友好），代价是数据量大了写入会变慢。（这条是我的推断，标**中等置信 ~60%**，仓库里没有写明选型理由；可自行验证：`grep -rn "better-sqlite3" package.json` 应为空。）

---

## 11. WORKING-CONTEXT.md 与 contexts/ 目录的角色

### 11.1 WORKING-CONTEXT.md（29 KB）不是机制，是 ECC 自己的项目日志

打开看就明白：`WORKING-CONTEXT.md:1-92` 是 ECC 这个仓库**自己维护给自己的工作状态文件**——当前分支、目录数量（47 agents / 79 commands / 181 skills）、活跃队列、PR 分类、Linear 工单号、执行笔记流水账。

**没有任何代码读它。** 我 grep 了全仓库的 js/mjs/py/sh/json，唯一命中是 `.claude/workflows/ecc-pro-security-roadmap.js:127,162` ——那是一个 prompt 字符串里让 agent 去「读 AgentShield 仓库的 WORKING-CONTEXT.md」，指的还是另一个仓库的同名文件。

它自己定义的更新规则（`WORKING-CONTEXT.md:88-90`）：

> "Keep this file detailed for only the current sprint, blockers, and next actions. Summarize completed work into archive or repo docs once it is no longer actively shaping execution."

也就是靠人（或 agent 被指示）手动维护。**它能进 context 只是因为它在项目根目录，Claude 有可能去 Read 它**——不存在自动注入。

**对研究者的意义**：这是 ECC 作者示范的一种「项目级长期记忆」做法——不用任何机制，就在仓库根放一个人类可读的状态文件，让 agent 每次开工去读。朴素但有效，而且零基础设施。

### 11.2 contexts/ 目录：三个手写的系统提示词，靠 shell alias 用

`contexts/` 只有 3 个文件：`dev.md`、`research.md`、`review.md`。内容极简，`contexts/dev.md` 全文就是「Mode: Active development / 写代码优先 / 先跑通再对再干净 / 偏好 Edit,Write,Bash,Grep」这样二十来行。

用法在 `the-longform-guide.md:68-74`：

```bash
alias claude-dev='claude --system-prompt "$(cat ~/.claude/contexts/dev.md)"'
alias claude-review='claude --system-prompt "$(cat ~/.claude/contexts/review.md)"'
alias claude-research='claude --system-prompt "$(cat ~/.claude/contexts/research.md)"'
```

**没有任何 ECC 代码读 contexts/**（唯一命中是 README 的目录说明和 npm 打包清单 `tests/scripts/npm-publish-surface.test.js:195`）。所以这是纯手动的「模式切换」，跟记忆/学习完全无关。README 把它叫 "Dynamic system prompt injection contexts"（`README.md:1116`）有点抬举了——实际是三个 shell 别名。

### 11.3 hooks/memory-persistence/ 是文档，不是可执行配置

`hooks/memory-persistence/hooks.json` 第一行自己就写了：

> "Reference lifecycle hook definitions for ECC memory persistence. **The production hook graph is hooks/hooks.json.**"

`hooks/memory-persistence/README.md` 同样说明「The installed hook graph is still `hooks/hooks.json`. This directory is the stable, human-readable lifecycle definition surface referenced by the harness audit and longform docs.」

它列出的记忆持久化生命周期契约（`hooks/memory-persistence/README.md` 的表格）：

| 事件 | hook | 干什么 |
|---|---|---|
| SessionStart | `session:start` → `scripts/hooks/session-start-bootstrap.js` | 载入有界的先前上下文 |
| PreCompact | `pre:compact` → `scripts/hooks/pre-compact.js` | 压缩前保存状态 |
| PreToolUse | `pre:observe:continuous-learning` → `scripts/hooks/observe-runner.js` | 记录工具意图 |
| PostToolUse | `post:observe:continuous-learning` → 同上 | 记录工具结果 |
| PostToolUse | `post:session-activity-tracker` → `scripts/hooks/session-activity-tracker.js` | 给 ECC2 记指标 |
| SessionEnd | `session:end` → `scripts/hooks/session-end.js` | 落盘会话摘要 |

注意 `observe-runner.js` 是 ECC 主仓的 Node 包装，最终会去调 continuous-learning-v2 的 `observe.sh`（这一层包装的存在是为了走 `run-with-flags.js` 的 `ECC_HOOK_PROFILE` / `ECC_DISABLED_HOOKS` 门控，见 `.claude/rules/node.md` 的 Hook Development 段）。

---

## 12. 和 Claude Code 原生 CLAUDE.md / memory 的关系：补充还是替代

### 12.1 结论：全部是补充，没有一处替代

分三层看：

| 层 | Claude Code 原生 | ECC 做的事 | 关系 |
|---|---|---|---|
| **静态项目规则** | `CLAUDE.md` / `.claude/rules/` 自动进 context | ECC 也往 `.claude/rules/` 放文件（如 `everything-claude-code-guardrails.md`），并写自己的 `CLAUDE.md` | **直接复用原生机制**，不是另起炉灶 |
| **动态会话上下文** | 无原生跨会话记忆 | SessionStart hook 塞 `additionalContext` | **补空白**，走的是官方 hook 协议 |
| **自动学习** | 无 | instinct 系统 | **纯新增** |

### 12.2 ECC 用的是官方 hook 协议，不是 hack

`scripts/hooks/session-start.js:735-743` 输出的是标准的 Claude Code SessionStart hook 返回格式：

```json
{"hookSpecificOutput":{"hookEventName":"SessionStart","additionalContext":"..."}}
```

也就是 ECC 的所有「记忆注入」都走的是 Anthropic 定义的官方扩展点。这意味着：**它跟 CLAUDE.md 并行生效，两者互不覆盖**——CLAUDE.md 走 Claude Code 自己的加载路径，instinct 走 hook 的 additionalContext。

### 12.3 Vault 明确拒绝当规则

`docs/design/ecc-memory-vault.md:19-21` 讲得最直白：

> "Reviewed project standards **still belong in the repository's canonical rules, decision records, runbooks, or other governed documentation.** The vault may link to those artifacts; it does not replace them."

以及 `:33-34`：

> "Procedural memory remains in rules and instincts, subject to their existing promotion and validation gates."

即：Vault 存「事实、笔记、教训、交接」，**规则仍归 rules/CLAUDE.md 管**。这个边界划得很清楚。

### 12.4 但 ECC 也往 `.claude/rules/` 写生成内容

`.claude/rules/everything-claude-code-guardrails.md` 是 ECC Tools 生成的，文件里自己标注：

> "Generated by ECC Tools from repository history. **Review before treating it as a hard policy file.**"

内容是「用 conventional commit / camelCase 文件名 / relative import」这类从 git 历史统计出来的约定。它会跟着 `.claude/rules/` 一起进 context——所以这是**一条绕过 instinct confidence 门槛、直接进规则层的通道**。写进 rules 的东西没有 confidence、没有 6 条上限、没有 0.7 阈值过滤。使用时要注意这个不对称。

---

## 13. 较真环节：这个「学习」到底有多智能

任务要求特别较真这一节，所以逐条摊开。

### 13.1 「学习」的本体：一个 Haiku 子进程 + 一段提示词

不是模板匹配，也不是正则统计——但**也不是任何形式的算法学习**。真相是：**把最近 500 行工具调用日志喂给一个便宜模型，让它自己看着办**。

```bash
claude --model haiku --max-turns N --print --allowedTools "Read,Write" -p "<提示词>"
```
`skills/continuous-learning-v2/agents/observer-loop.sh:258-260`

提示词全文 40 行（`observer-loop.sh:162-202`），本质是「读这个文件，找 user corrections / error resolutions / repeated workflows / tool preferences 四类模式，出现 3 次以上就写个文件，格式照抄这个模板」。

所以智能程度 = **Haiku 在 500 行截断日志上做零样本模式识别的能力**。既没有可验证的统计，也没有可复现的规则。它比正则统计更灵活，也比正则统计更不可靠——同一批日志跑两次可能得到不同的 instinct，没有任何机制去检测这种不一致。

### 13.2 confidence 是 LLM 填出来的数字，不是算出来的

已在 §4 详述，这里给判决：

| 声称 | 实现 |
|---|---|
| observer.md:130-134 观察次数→分数映射表 | 只是提示词文本，Haiku 自己数次数自己填 |
| observer.md:136-139 `+0.05` 确认 / `-0.1` 矛盾 / `-0.02` 每周衰减 | **代码零实现**。grep `decay`/`0.05`/`0.02`/`-0.1` 全仓库零命中 |
| SKILL.md:308-326「Confidence evolves over time」 | 不会 evolve。写进文件就固定了 |

**打分代码在哪？没有打分代码。** 唯一涉及 confidence 的数值运算是求平均（用于跨项目提升判断）和比大小（用于排序/过滤）。

推论：`ECC_INSTINCT_CONFIDENCE_THRESHOLD=0.7` 这个门槛过滤的，是一个 Haiku 凭感觉写下的数。门槛本身是真的，被过滤的数据不可靠。

### 13.3 「聚类」是去停用词后的字符串精确匹配

`/evolve` 号称把相关 instinct 聚成 skill。实现（`instinct-cli.py:1177-1185`）：

```python
trigger_clusters = defaultdict(list)
for inst in instincts:
    trigger_key = inst.get('trigger', '').lower()
    for keyword in ['when', 'creating', 'writing', 'adding', 'implementing', 'testing']:
        trigger_key = trigger_key.replace(keyword, '').strip()
    trigger_clusters[trigger_key].append(inst)
```

翻译：把 trigger 转小写，删掉 6 个固定单词，剩下的字符串**完全相同**才算一类。

具体后果：
- `"when writing new functions"` → `"new functions"`
- `"when writing a new function"` → `"a new function"`
- 这两条**不会**被聚到一起。

而 `"when creating React components"` 会变成 `"react components"`，`"when adding React components"` 也变成 `"react components"`——恰好能聚。**能不能聚完全取决于 LLM 当时用了哪个动词。**

命令名生成同样粗暴（`:1225-1226`）：`trigger.replace('when ', '').replace('implementing ', '').replace('a ', '')` 然后空格换连字符截 20 字符。注意 `.replace('a ', '')` 会把 `"database migration"` 里的 `"a "` 也删掉（`"datbase migration"`... 实际是 `"dat"+"base migration"` → 不，`'a '` 只匹配 "a"+空格，`database` 里没有 "a "，但 `"add a feature"` → `"add feature"`，而 `"schema and tests"` → `"schemand tests"`）。**这是个真实的字符串处理 bug**，只是影响面小（只影响打印出来的建议名）。

### 13.4 「trigger」字段在注入时被完全丢弃

instinct 的招牌是「有 trigger 的直觉，在相关时被召回」。README:811 的原话：

> "| Instincts | Patterns learned from real sessions with confidence scores | **Recalled when relevant** |"

代码事实（`session-start.js:434-459`）：**排序只看 confidence 和 scope，截取前 6 条，只输出 `## Action` 段的第一行。trigger 字段从头到尾没参与任何匹配。**

所以「Recalled when relevant」属于**仓库声称，未在代码中核实**——实际行为是「按置信度取前 6 条无条件塞进 SessionStart」。相关性判断被隐式甩给主模型（它看到 6 行文本，自己决定用不用）。

### 13.5 另外两条产生 instinct 的路径，智能程度更低

**`/skill-create --instincts`（`commands/skill-create.md:82-105`）**：这是一个**纯 markdown 提示词文件**，让 Claude 跑几条 `git log`，然后按模板写 instinct。模板里 confidence 是**硬编码的 0.8**：

```yaml
id: {repo}-commit-convention
trigger: "when writing a commit message"
confidence: 0.8          ← 写死在模板里
domain: git
```

检测方法表（`commands/skill-create.md:44-50`）诚实地写着 "Regex on commit messages (feat:, fix:, chore:)"、"Files that always change together"——**这一条确实就是正则统计**。

**ECC Tools GitHub App 生成的 rules**：`.claude/rules/everything-claude-code-guardrails.md` 的产出长这样：

```
- Prefer `conventional` commit messaging with prefixes such as fix, test, feat, docs.
- Preserve the current `hybrid` module organization.
- Use `camelCase` file naming.
- Prefer `relative` imports and `mixed` exports.
```

反引号里包着的 `conventional` / `hybrid` / `camelCase` / `relative` / `mixed` 明显是**填进句子模板的枚举值**——这是模板填空 + 静态统计，不是理解。生成代码在仓库外（闭源 GitHub App），**无法核实其提取算法**（未验证）。

而仓库里唯一那份真实 instinct 文件（`.claude/homunculus/instincts/inherited/everything-claude-code-instincts.yaml`）的 8 条，`source: repo-curation`——**是人手写的**。作者自己在其中一条里写道（`:157`）：

> "Prefer a small set of accurate instincts over bulk-generated, duplicated, or contradictory instincts."
>
> Evidence: "Auto-generated instinct dumps can duplicate rules, widen triggers too far, or preserve placeholder detector output."

**作者自己承认自动生成的 instinct 会重复、trigger 过宽、留下占位符输出。** 这是全仓库对这套系统最诚实的评价，而且是作者本人写的。

### 13.6 系统默认是关闭的

`skills/continuous-learning-v2/config.json` 里 `observer.enabled: false`（`SKILL.md:222-237` 记录同样默认值）。`start-observer.sh:176-180` 检查到 false 直接退出并打印「Observer is disabled in config.json」。

也就是说 **99% 的 ECC 用户从没跑过这套学习系统**——他们看到的 `Active instincts:` 只可能来自手动 `/instinct-import` 导入的内容。

### 13.7 那么，值得称赞的是什么

公正地说，这套系统里**工程质量很高的部分是「基础设施」而非「智能」**：

1. **5 层自我观察防护**（`observe.sh:148-182`）——防 agent 观察自己造成无限循环，这是真实踩过的坑。
2. **密钥脱敏 + 有界正则**（`observe.sh:273-279`）——修过 catastrophic backtracking 打满 CPU 的 bug（#2278）。
3. **SIGALRM 自杀 / 看门狗超时 / flock 计数器**（`observe.sh:281-289, 494-558`；`observer-loop.sh:263-270`）——后台进程的生命周期管理相当扎实。
4. **失败不归档**（`observer-loop.sh:277-290`，#2370）——分析失败保留观察数据待重试，这是很多系统会做错的地方。
5. **STALE-REPLAY 护栏**（`session-start.js:658-670`，#1534）——防止上次会话摘要里的 slash 命令被重放，这条是真实事故驱动的设计。
6. **Vault 的 create-only + 永远 unreviewed + 拒绝自动提升**（`memory-vault.js:293, 318-327`；`docs/design/ecc-memory-vault.md:136-138`）——「能跑 shell 的 agent 不能当人类批准环节」这个判断很清醒。

**一句话总结**：这套系统的护栏是工程师写的，智能是提示词许诺的。护栏是真的，智能是 Haiku 的即兴发挥。

### 13.8 给研究者的可迁移结论

| 设计点 | 值不值得抄 |
|---|---|
| 用 hook 而非 skill 做观察（"hooks fire 100%, skills fire 50-80%"，`SKILL.md:328-335`） | ✅ 值得。确定性触发 vs 概率触发的区分是对的 |
| 观察数据落 JSONL + 截断 + 脱敏 + 自动归档 | ✅ 值得。基础但必要 |
| 起独立便宜模型子进程做后台提炼 | ✅ 思路值得，但要加**输出校验**（ECC 没有） |
| 让 LLM 自己填 confidence 然后当门槛用 | ❌ 别抄。数字没有意义，滤不出真信号 |
| 用字符串前缀相等做聚类 | ❌ 别抄。改成 embedding 或让 LLM 做归并 |
| SessionStart 无条件注入 top-N | ⚠️ 可用作起步，但别叫「相关性召回」 |
| Vault 的「永不自动提升为规则」边界 | ✅ 最值得抄的一条 |

---

## 14. 未验证 / 开放问题 / 已知坑清单

### 14.1 明确标注「未验证」的部分

| 内容 | 为什么无法验证 | 你可以怎么自己查 |
|---|---|---|
| ECC-Tools GitHub App 的 instinct 生成质量 | 闭源服务，代码不在本仓库 | 看 `.claude/rules/everything-claude-code-guardrails.md` 的输出形态自行判断；App 地址 https://github.com/apps/ecc-tools |
| Skill Creator GitHub App 同上 | 同上 | https://github.com/apps/skill-creator |
| 实际跑起来 Haiku 提炼 instinct 的质量 | 需要真跑（本次任务禁止安装/运行） | 在一个玩具仓库里开 `observer.enabled: true`，干活 20+ 次工具调用，看 `~/.local/share/ecc-homunculus/projects/*/instincts/personal/` |
| 选 sql.js 而非 better-sqlite3 的动机是「避免原生编译」 | 仓库没写选型理由（我的推断，置信度中 ~60%） | `grep -rn "better-sqlite3" package.json` 应为空 |
| ecc2（Rust）里是否另有一套记忆实现 | 本次未深入 `ecc2/src/session/store.rs` | `ls ecc2/src/session/`，重点看 `store.rs` |

### 14.2 我在代码里发现的实际问题（按严重度排）

1. **confidence 演化规则完全没实现**（§4.2-4.3）。`observer.md:136-139` 承诺的三条规则在 2073 行的 CLI 里一行代码都没有。用户以为分数会随时间调整，实际永远不变。
2. **`instincts/pending/` 目录没有任何写入者**（§4.5）。`cmd_prune`（`instinct-cli.py:1942`）和 `/instinct-status` 的 "N pending awaiting review" 提示对着一个永远为空的目录工作。整个「人工审核队列」功能没接上。
3. **`start-observer.sh status` 数不到 instinct**（§4.6）。`start-observer.sh:160` 用 `find -name "*.yaml"`，但 observer 写的是 `.md`。
4. **`/evolve` 的聚类几乎不可能聚到东西**（§13.3）。字符串精确匹配 + LLM 用词随机 = 大概率每条 instinct 自成一类，永远达不到 `len(cluster) >= 2` 的门槛。
5. **`_frontmatter`/`trigger` 解析器是手写的行扫描，不是 YAML**（`instinct-cli.py:516-567`、`session-start.js:319-366`）。文档里已经警告 instinct 正文不能用 `---` 做水平分割线（`instinct-cli.py:520-521`），否则会被当成 frontmatter 边界。历史上因此丢过内容（`README.md:748` 记录 issue #148/PR #161：`parse_instinct_file()` 曾静默丢弃 frontmatter 之后的全部内容）。
6. **`/evolve` 生成的命令名有字符串 bug**（§13.3）。`.replace('a ', '')` 会把 `"schema and tests"` 变成 `"schemand tests"`（已用 python3 实测确认）。影响仅限打印出来的建议名，不影响数据。
7. **两套 instinct 文件格式并存**：单文档 `.md`（observer 写的）vs 多文档 `.yaml`（策展/导出用的，一个文件里多个 `---` 段）。解析器两种都支持，但 `start-observer.sh` 的统计和 `instinct-cli.py` 的默认导出（`:1040` 写 `.yaml` 风格）之间存在口径不一致。

### 14.3 留给下一轮研究的问题

1. `ecc2/src/session/store.rs`（Rust）是否是第五套记忆系统？设计文档提到 "ECC2 context graph: optional projection populated from the Markdown directory connector"（`docs/design/ecc-memory-vault.md:59-60`），但这个 connector 的代码没找到。
2. `scripts/lib/skill-evolution/tracker.js` 会往 state.db 的 `skill_runs` / `skill_versions` 写数据——这套「skill 演化跟踪」跟 instinct 的 `/evolve` 是什么关系？两者都叫 evolve 但可能完全独立。
3. `skills/unified-memory/SKILL.md`（README:824 提到）本次没读，它是 Vault 的使用工作流描述，可能包含 agent 何时该主动 recall 的引导策略——这是 §7.6「Vault 不自动注入」的补救措施所在。
4. `docs/continuous-learning-v2-spec.md` 只有 14 行，几乎是空的——是否有更完整的规格文档在别处？

---

## 附：关键文件路径速查

```
# instinct 系统
/Users/aa00158/harness-research/ECC/skills/continuous-learning-v2/SKILL.md
/Users/aa00158/harness-research/ECC/skills/continuous-learning-v2/hooks/observe.sh
/Users/aa00158/harness-research/ECC/skills/continuous-learning-v2/agents/observer.md
/Users/aa00158/harness-research/ECC/skills/continuous-learning-v2/agents/observer-loop.sh      ← 真正调 LLM 的地方
/Users/aa00158/harness-research/ECC/skills/continuous-learning-v2/agents/start-observer.sh
/Users/aa00158/harness-research/ECC/skills/continuous-learning-v2/scripts/instinct-cli.py
/Users/aa00158/harness-research/ECC/skills/continuous-learning-v2/config.json                  ← enabled: false
/Users/aa00158/harness-research/ECC/.claude/homunculus/instincts/inherited/everything-claude-code-instincts.yaml  ← 唯一真实样本（人工写的）

# 注入
/Users/aa00158/harness-research/ECC/scripts/hooks/session-start.js         ← summarizeActiveInstincts() 在 :406-460
/Users/aa00158/harness-research/ECC/scripts/hooks/session-start-bootstrap.js
/Users/aa00158/harness-research/ECC/scripts/hooks/observe-runner.js
/Users/aa00158/harness-research/ECC/hooks/hooks.json
/Users/aa00158/harness-research/ECC/hooks/memory-persistence/README.md     ← 生命周期契约文档

# Memory Vault
/Users/aa00158/harness-research/ECC/docs/design/ecc-memory-vault.md
/Users/aa00158/harness-research/ECC/scripts/lib/memory-vault.js            ← 核心 778 行
/Users/aa00158/harness-research/ECC/scripts/memory.js                      ← ecc memory CLI
/Users/aa00158/harness-research/ECC/scripts/memory-mcp.mjs                 ← ecc-memory-mcp bin

# SQLite（跟记忆无关）
/Users/aa00158/harness-research/ECC/scripts/lib/state-store/index.js       ← .claude/ecc/state.db
/Users/aa00158/harness-research/ECC/scripts/lib/state-store/migrations.js  ← 7 张表

# 其他
/Users/aa00158/harness-research/ECC/WORKING-CONTEXT.md                     ← 手工维护的项目日志，无代码读它
/Users/aa00158/harness-research/ECC/contexts/{dev,research,review}.md      ← shell alias 用的系统提示词
/Users/aa00158/harness-research/ECC/commands/skill-create.md               ← --instincts 模板里 confidence 写死 0.8
```


