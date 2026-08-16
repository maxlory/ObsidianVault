Develop的总框架：
- 主 Agent：总包负责人。决定范围、方案、<font color="#2DC26B">Ticket</font>、验收和 commit。
- <font color="#2DC26B">develop-nextctl</font>：门禁和档案系统。记录状态、核验证据、防止跳步。
- supporting <font color="#2DC26B">Skills</font>：不同工种的操作手册，例如 Diagnose、Spec、TDD、Review。
- Ticket：一张能单独验收的施工单。
- <font color="#2DC26B">Implementer</font>：真正写测试和产品代码的施工员。
- <font color="#2DC26B">Reviewer</font>：不写代码的验收员。
- <font color="#2DC26B">Gate</font>：必须拿出证据才能通过的关卡。
- <font color="#2DC26B">fingerprint</font>：当前代码现场的“封条编号”。代码一变，旧 Review/Verification 就不能冒充当前证据。
- <font color="#2DC26B">checkpoint</font>：交班摘要，记录做到哪里、什么有效、下一步唯一是什么。


<mark style="background:#d3f8b6">Ticket</mark>是什么
定义：可以独立完成和验收的开发任务单
将模糊的需求落地成可显示测试得到验证的测试
包括：允许改什么内容、不允许做什么内容、验收条件是什么
特点：Red/Green设计的意义是什么？
将模糊的、尚未实现的需求当成一个bug解决。red表明这个bug没有修复（需求没有实现），green表明这个bug修复了（需求已经实现）
我的问题：既然已经知道red没有实现，跑一遍red是否是浪费时间、token？
解答：
- Red：这个测试真的能捕捉当前缺失的行为。（解决的方向是对的）
- Green：实现后，这个测试证明目标行为已经成立。（真的解决了这个问题，而不是测试方法出错导致假阴性）
Spec和Ticket之间的关系：
- Spec 是完整施工图。
- Ticket 是从施工图中拆出的单张施工单。
每张 Ticket 只负责 Spec 中一个可以独立实现、测试和验收的行为。
具体关系：
1. 每个 Ticket 都必须来自 Spec
2. Spec 的每条 Acceptance Criterion 都必须有 Ticket 承接
3. 多张 Ticket 合起来应完整覆盖 Spec
4. Ticket 可以决定实施顺序
5. Spec 改变，Ticket 也得跟着变
重点：Ticket 不是把 Spec 按文件拆开，而是按用户可观察的行为拆开。
因为要求每张 Ticket 都能独立回答：用户获得了什么新行为，以及我们如何证明它已经实现。

<mark style="background:#d3f8b6">Implementer</mark>是什么
定义：负责一张 Ticket 的写代码角色。默认运行在<u>独立子会话</u>中，也可以在能力不足时由 <u>fresh subagent</u> 承载
问题：什么时候会由subagent实现？
解答：如果平台无法创建 Local Project task，可以启动一个新的 same-session subagent，让它扮演 Implementer。
追问：什么叫‘平台无法创建 Local Project task’（主会话无法分派任务到子会话中）？
回答：“无法创建 Local Project task”通常有以下几种情况：
1. 当前运行环境不支持独立任务。例如在 <u>CLI</u>、精简版运行环境或未提供任务管理能力的宿主中运行
2. 项目没有正确注册。工作区虽然是一个本地目录，但<u>没有被 Codex 保存为 Project</u>
3. Git 项目识别异常。没有明确的项目根目录，例如项目路径失效、仓库未识别
4. 当前会话没有创建新任务的授权。<u>按照目前 Codex 的规则，创建新的独立任务通常需要用户明确要求</u>
5. 模型或执行环境不可用。例如 Harness 要求 DeepSeek implementer，但当前宿主没有该模型
<font color="#2DC26B">发现的设计问题</font>：流程把“独立 Local Project task”定义为默认实现容器，但 Codex 当前规则把用户可见的新任务视为需要用户明确授权的操作。
解决方法：在整个 planning package 获批时，同时取得“后续可为每张 Ticket 创建独立实现任务”的一次性授权
衍生出的新问题：一个 Ticket 的实现很小。是否需要专门分派一个子会话实现一个 Ticket ？这应该要结合具体实际操作中对 Ticket 的实现来思考

同时得出结论：为什么在实际开发中，发现主会话很忙，实现了很多内容。主要原因有两点：
1. 主会话承担了非常重的验证、真实验收工作。这是设计原因，主会话不能盲目相信子会话返回的结果，需要二次检查
2. Harness 状态摩擦太多，主会话花了大量时间手工协调和修复状态。Develop 流程还是不够完善，需要优化




<mark style="background:#d3f8b6">Reviewer</mark>
定义：由主会话派发的subagent实现代码验收
问题：什么时候 Review？是 Ticket 完成还是 Spce 完成
回答：默认在整个 Spec 实现完成后统一 Review。
单个 Ticket 做完后，先通过自己的 Green 测试和局部检查。等整个 Spec 的所有 Ticket 都 Green 后，再统一 Review 一次。这次的验收不止是单个 Ticket 的验收，还要检查 Ticket 之间拼起来是否正确，例如接口衔接、重复实现、依赖方向和整个 Spec 是否完整落地。
问题：Review 什么？验收的是用户行为还是代码
回答：既检查用户行为是否符合 Spec，也检查代码本身是否可靠、可维护。以 Spec 描述的用户行为为标准，对行为正确性、测试可信度和代码质量进行独立审查。

Gate
定义：是某个节点进入下一流程的通行证。不是只检验代码，而是对诸如Spec、Tickets、Implementation 开发的代码、Review 审核的代码等都进行检验。根据一个或多个证据，判断是否有资格离开当前阶段。
代码是实现 Gate 的手段；JSON 是 Gate 的记录；测试报告是 Gate 的证据；“能否继续前进”才是 Gate 真正控制的东西。
不同阶段有不同的 Gate

|Gate|检验对象|核心问题|
|---|---|---|
|Spec Gate|Spec|需求是否已经变成可观察、可测试的规格？|
|Tickets Gate|Spec + Tickets|Spec 是否被完整拆解，每张 Ticket 是否有范围、依赖、Red 条件和验收方式，并得到用户批准？|
|Implementation Gate|代码 + 测试 + Ticket 执行证据|所有应完成的行为是否已经达到 Green？|
|Review Gate|当前代码 diff + Spec + Tickets|实现是否符合规格，是否存在阻塞问题？|
|Verification Gate|当前工作区 + 验证命令|当前版本是否通过 build、lint、test 等正式检查？|
|Completion Gate|前述全部证据 + 变更范围|是否真的可以提交和关闭任务？|
<font color="#2DC26B">发现的设计问题</font>：


<mark style="background:#d3f8b6">fingerprint</mark>
定义：本质是一段哈希值。根据当前关键文件和配置生成的内容身份标识，让Review、测试和 Verification 能与具体版本绑定。这样是为了保证正确的版本不会被偷偷改了，但是agent不知道

<mark style="background:#d3f8b6">checkpoint</mark>
定义：表现为一段 JSON。用于解决：当现在对话中断、上下文被压缩，下一位 Agent 应该从哪里继续（承接任务内容）
在当前的设计中，Checkpoint包含：任务目标、已完成内容、任务进度、修改了哪些文件、用户做了哪些决定、阻塞点、下一步的动作、fingerprint
如何实现的：系统里原来分散保存着这些信息，然后代码直接从工作区中几个路径和结构都已知的数据源中读取的（通过固定格式检索得到的，类似于数据库查询和报表生成）——Checkpoint 只能汇总已经结构化保存的事实，不能保存聊天中未保存的信息
读取 state→ 得到目标、等级、执行者
读取 Gate→ 找出仍然有效且 passed 的 Gate
读取 Ticket→ 找出已完成 Ticket 和当前 Ticket 阶段
读取 Git→ 得到实际修改文件和当前 fingerprint
读取用户决定与 blocker→ 得到已批准选择和阻塞问题
重新计算 route→ 得到下一步动作
程序依次读取这些记录，然后按固定模板摘取字段，将这些内容登记到 Checkpoint 中
由于 Agent 可能产生幻觉，所以程序会检查当前 Git 和 fingerprin，保证没有疏漏
生成完整交班单后，程序再对它生成一段哈希值（digest）
在什么时候生成：
1. 手动执行
2. L1 从主 Agent升级到 delegated writer 前
3. PreCompact 上下文压缩前（由 PreCompact hook触发）

由于 Checkpoint 完整内容可能比较长，将完整 Checkpoint 内容保存到 Ledger 中，将最新 Checkpoint 的 digest保存到 state 中，为了：1. 辨识哪一份是当前 Checkpoint；2. 避免重复记录


<mark style="background:#d3f8b6">Ledger</mark>
定义：保存整个任务历史记录的日志文件。
Ledger
├── 任务初始化记录
├── Gate 变化记录
├── Ticket 变化记录
├── Checkpoint A
├── Review 记录
└── Checkpoint B

<mark style="background:#d3f8b6">provenance</mark>

现在 develop 流程有一些比较突出的问题：
1. 单 write 执行设计不合理，导致开发效率较低
2. 在记录 Ledger、ADR、SPEC、Ticket 等文档的时候，没有将为什么这种较为抽象的内容记录下来，导致这类内容在频繁的上下文压缩中可能丢失


完整流程梳理：
一、判断任务类型和规模
主 Agent先判断任务类型和规模，是 feature、bug、refactor 还是 architecture
|等级|判断方式|默认实现者|
|---|---|---|
|L0|很小、明确、低风险，一个局部改动|主 Agent|
|L1|一个有边界的行为结果|主 Agent|
|L2|多个可独立测试的行为或多个阶段|delegated writer|
|L3|连问题地图都不清楚的大任务|先 Wayfinder，随后通常收敛为 L2|
|Bug|根因尚未证实|先 Diagnoser|

二、摸清任务具体情况
对如果存在会改变产品行为、架构、安全或范围的重大歧义，就使用 Grill 澄清
对项目具体信息，例如使用框架、技术方案等问题，通过读代码或官方资料查清
遇到不同问题，采用不同解决方式：
- 术语冲突：Domain Modeling
- 外部 API、框架行为未知：Research
- 只有运行起来才能知道：Prototype
- 模块边界有两种实质方案：Codebase Design
- 普通局部实现已有明显模式：不调用这些流程

三、写 Spec
主 Agent加载 Spec skill，把已确认的信息整理成可测试规格，重点包括：
- 问题是什么；
- 用户能观察到什么；
- 每个 Acceptance Criterion 如何证明；
- 已决定的实现边界；
- 测试选择；  
    -明确不做什么；
- 仍有哪些会阻止实施的问题。
重点：Spec 本身不单独要求用户审批；审批被合并到下一步 planning package

四、将写好的 Spec 按行为拆 Ticket
每个 Ticket 都是一个可以独立验证的用户行为
每张 Ticket 都包含：
- 一个明确 Outcome；
- 允许修改的文件；
- 禁止扩大的范围；
- 依赖哪些前序 Ticket；
- Acceptance Criteria；
- Red 应该怎样失败；
- Green 后执行哪些检查。
然后把 Spec、关键假设、Ticket 切片和依赖边一次性展示给用户批准。不是每张 Ticket 都重新问一次。
注意：分为两种模式——Lean+Strict
Lean 模式只做本地规划质量检查；Strict 模式才会额外派发一次 `plan_reviewer`。

五、subagent与派发子任务
A：只读 subagent
reviewer 是通过 subagent 实现
B：delegated writer
1. Implementer 通过独立 writer 实现
2. Review 后需要 delegated review-fix

注意：分发 writer 期间
1. 主 Agent停止写测试和产品代码
2. 同一时间只能有一个 writer——多个 writer 会让 diff、fingerprint 和责任边界难以可信归因

六、Writer 具体执行 Ticket

七、多 Ticket 推进
单个 Ticket Red → Green走完一次。所有 Ticket 完成后进行一次综合 Review。
检查：各 Ticket 单独能工作，组合起来是否仍然正确；Ticket 之间的接口、数据流和错误处理是否一致；是否出现重复实现、依赖方向错误或集成回归等问题
而不会每张 Ticket 都跑一次完整 Review 和 full suite
Strict 模式需要另外审核

八、Review 节点
Review 是独立只读验收，不负责顺手改代码，由subagent实现（是以什么模型驱动的subagent）
检查：漏需求、边界错误、安全隐患、错误测试、设计问题——代码是不是做对了
- 是否满足整份 Spec；
- 错误和边界路径；
- 安全、数据完整性、回归；
- 跨 Ticket 接口；
- 是否出现重复设计或错误依赖方向；
- 测试是否真的证明行为。
当 Review 出现阻塞：
reviewer subagent 发现 blocker
→ 主会话判断 finding 是否成立、是否在当前范围——这部分任务也由主会话承担
→ 主会话把 Ticket 置为 review-fix
→ 派发 writer 修复——优化后的 develop 流程：L0/L1 是主会话自己改；已升级的 L1、所有 L2/L3由分发的子会话实现（注意：此时如果进行修改需要 writer 实现，仍然只允许一个 writer 实现——所有流程都要停下来等它一个子会话）
→ 再 Review
→ 再 Verification

九、Verification
在当前工作区执行项目配置的 build、typecheck、lint、test、security 等命令，并保存可复查的命令结果。——和 Review 检查的方向不同
详细来说：编译错误、类型错误、测试失败、格式违规——代码有没有坏
- `npm run build`：项目能不能打包；
- `npm run typecheck`：TypeScript 类型有没有错误；
- `npm run lint`：有没有违反项目规定的代码写法；
- `npm test`：现有自动化测试是否通过；
- 安全扫描：是否出现已知依赖漏洞或危险配置。
如果 Verification 出现阻塞：
|Verification 失败原因|下一步|
|---|---|
|test/typecheck/build 发现产品缺陷|回到 implementation 或 review-fix，由当前代码所有者修|
|lint/配置错误|回到对应 Ticket 修复；仍视为代码/配置变动|
|Review 后代码被改，证据已 stale|重做必要 Review，再 Verification|
|验证命令缺失或配置不可信|回到 bootstrap/项目配置确认|
|外部服务不可用、环境损坏|标为 blocked 或 insufficient-evidence|
|非关键缺陷且用户明确接受风险|走 `defer-ticket`，记录 owner、恢复条件和风险；不能伪装成 passed|
任一失败，主会话都负责把任务送回正确的修复阶段——依旧是主会话的任务

十、Checkpoint 和恢复
Checkpoint 的具体内容包括：在阶段切换、L1 升级和 PreCompact（hook） 时生成
- 当前目标；
- 任务等级和实现者；
- 已通过 Gate；
- 已完成 Ticket；
- 当前 Ticket 阶段；
- 有效测试证据；
- 改动文件；
- 用户已确认决定；
- blocker；
- 唯一下一步；
- 当前 fingerprint

当出现开发中断，需要恢复时遵循以下的顺序：
当前文件和 Git
> 与当前 fingerprint 匹配的证据
> Spec / ADR
> Ticket / Ledger
> state 标签
> 聊天摘要或 Agent 自述

十一、最终交付
需要检查以下的内容：
- 再检查所有 Acceptance Criteria；
- Review 没有 blocker；
- Verification 属于当前 fingerprint；
- 没有 unexpected files；
- 没有活跃 writer；
- completion summary 已生成；
- 固化文件集合、binary diff、权限和 symlink 快照。

在遇到不同层次的任务时，采取不同的开发路径，从而实现开发效率的提升、避免不必要的浪费
L0/L1：收敛需求——>主 Agent 直接进行开发（通常不建 Ticket）——>主 Agent 做 Review——>Verification——>commit
但是在开发过程中发现以下情况，会自动升级为L2路径：
- 预计还要连续处理超过 30 分钟；
- 跨两个以上相对独立模块；
- 同时涉及前后端、迁移、外部服务或真实浏览器；
- 首轮实现失败，需要重新诊断或大幅调整；
- PreCompact 发生上下文压缩。

在 Develop 流程中，哪些是硬约束，哪些是 Agent 行为约束
硬约束：
- L2/L3 默认 delegated；
- delegated write stage 要求 provenance 和 report；
- 单 writer reservation；
- 文件授权和真实 diff 对账；
- fingerprint 失效；
- Review report 结构；
- Verification 只能走可信入口；
- completion 和 commit 范围一致；
- CLI 侧原版/新版单活跃保护。
主要依赖主 Agent遵守：
- 一开始把任务正确分成 L0/L1/L2/L3；——没有问题，可以加一层人工确认约束
- 正确识别“实质歧义”并调用 Grill；——问题比较大，‘实质歧义’比较抽象，无法保证哪些确实是需要人工确认，哪些不需要人工确认
- 正确决定是否需要 Research、Prototype、Design；——问题不大，在这些节点都可以向用户提供自己的意见，让人工决定是否进行这些操作
- 真正创建 fresh Local Project task；——没有歧义且比较重要。希望能使用较强的工程约束保证实现
- 主 Agent在 delegated writer 期间不并发写代码；——问题较大，希望能突破单 write 的限制
- Ticket 是否按行为而不是技术层拆分。——重要，但是人工作用不大，主要还是依靠 Agent 实现，对主 Agent 要求较高


需求
→ Ticket：写成可执行的新行为契约
→ Red：当前代码不满足契约
→ 实现
→ Green：当前代码满足契约
→ Review：实现方式和边界是否正确
→ Verification：整个项目在当前代码上是否仍然健康

Red / Green
    验收一个行为切片是否实现

Review
    检查实现方式、边界、回归风险和代码质量

Verification
    检查当前整个项目是否仍然健康

Completion
    确认所有 Ticket 和交付条件都满足

- Green 是 <mark style="background:#d3f8b6">Ticket 级</mark>的局部行为验收；
- Review 是<mark style="background:#d3f8b6">独立</mark>正确性和质量验收；
- Verification 是<mark style="background:#d3f8b6">项目级</mark>技术验收；
- Completion 是<mark style="background:#d3f8b6">任务级</mark>最终验收。

| 阶段                 | 核心问题                       |
| ------------------ | -------------------------- |
| Ticket Green       | 这个局部需求在测试中实现了吗？            |
| 综合 Reviewer        | 整个 Spec 是否正确实现，而且代码质量是否合格？ |
| Fresh Verification | 当前工作区中的最终版本，实际执行检查还能通过吗？   |
| 用户验收               | 最终产品是不是用户真正想要的？            |

| Agent 角色           | 模型                  | Reasoning | 权限                         |
| ------------------ | ------------------- | --------- | -------------------------- |
| Ticket Coordinator | `gpt-5.6-luna`      | `max`     | 只管理 Ticket 和 Controller 状态 |
| Implementer        | `deepseek-v4-flash` | `high`    | 唯一 product writer          |
| Standards Reviewer | `gpt-5.6-luna`      | `max`     | 只读                         |
| Spec Reviewer      | `gpt-5.6-luna`      | `max`     | 只读                         |
| Verifier           | `gpt-5.6-luna`      | `max`     | 可运行测试，但不能改源码               |
| Explorer           | `gpt-5.6-luna`      | `max`     | 只读探索                       |
| Diagnoser          | `gpt-5.6-luna`      | `max`     | 只读诊断                       |
| Plan Reviewer      | `gpt-5.6-luna`      | `max`     | 只读审阅计划                     |
实际体验：
通过 Grill 确定需求，一次性生成 Spce、ADR等内容
	根据 Spec，按实施顺序拆分 tickets