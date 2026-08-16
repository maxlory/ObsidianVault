
### 对于ECC
#### 一、安装什么工具
1. 使用skill-stocktake<u>给 skill 库做体检</u>，判断哪些该删/该改/该留
2. 将skill-scout纳入程序构建前的<mark style="background:#d3f8b6">搜索</mark>
3. agent-self-evaluation：让 AI 完成任务后按 5 个维度给自己<mark style="background:#d3f8b6">打分</mark>
4. agent-introspection-debugging：AI 干活失败之后的<mark style="background:#d3f8b6">复盘</mark>流程
5. hookify-rules：<u>用自然语言写 hook 规则</u>
6. strategic-compact：<font color="#92d050">在逻辑节点主动压缩上下文</font>，而不是让自动压缩在任务中途乱切。与 token 优化模式联系起来
7. intent-driven-development：<u>把模糊需求逼成可验证的验收标准</u>。收集相关方向确定的skills，构建产品、需求体系
8. council：为<u>模糊决策</u>开一个"四声议会"
9. santa-method：两个独立<mark style="background:#d3f8b6">审查</mark> agent 都通过才放行
10. architecture-decision-records：把会话里做出的<u>架构决策记成标准格式的 ADR</u>
11. product-lens：<mark style="background:#d3f8b6">产品思想</mark>构建，但是比较单薄
12. loop-design-check：<mark style="background:#d3f8b6">评估</mark>自动化 agent 流程
13. plan-canvas：把<mark style="background:#d3f8b6">计划</mark>开在本地浏览器里，你在页面上批注、对话、批准或要求改。使用hook强制调用
14. tdd-workflow：<u>强制先写测试再写代码</u>。可能偏向TS/Next.js，需要考察其他tdd类skills
15. git-workflow：分支策略 + 提交规范 + merge/rebase 决策 + 冲突解决。适用于公司中的多人协作，但是其中的避免冲突方法也许可以借鉴
16. browser-qa：部署后用<mark style="background:#d3f8b6">浏览器自动化</mark>做四轮验证。验证中极其重要的一个环节
17. delivery-gate：<mark style="background:#d3f8b6">质检</mark>不过就不让 AI 收工的 Stop hook。本体不用，但是4 条正则直接抄进你自己的 Stop hook
18. iterative-retrieval：解决"派出去的 subagent 返回的摘要不够用"这个问题。弄清楚是在subagent中调用还是直接调用（应该不用）
19. codebase-onboarding：<u>分析陌生代码库</u>，产出结构化上手指南 + 一份起步 CLAUDE.md。这个skill可能主要用于多人共同开发中接手其他人的仓库，但是我可以将其用于对陌生仓库的调研，作为调研流程的一部分
20. code-tour：和 `codebase-onboarding` 配对使用——后者产出文字指南，前者产出**可点击跳转的导览**。作为<mark style="background:#d3f8b6">调研流程</mark>的一部分
21. verification-loop：六阶段顺序验证。作为中央枢纽调用其他skills。我的想法是可能不用，即使用也要对其中的内容进行优化
22. eval-harness：评估“AI 做得好不好”。非常重要的一个skills
23. e2e-testing：<u>Playwright 端到端测试</u>的工程实践。程序自动化测试
24. ai-regression-testing：专门针对"AI 写的代码"的<mark style="background:#d3f8b6">回归测试</mark>策略。技术栈是 Next.js 专属
25. production-audit：在正式上线前，检查它是否已经<u>达到生产环境要求</u>。
26. context-budget：审计上下文窗口被谁吃了。不可靠，斟酌使用
27. growth-log：教写"<mark style="background:#d3f8b6">成长日志</mark>"。自学习的一部分，模式可提取。可以与hermess程序进行对比
28. ck：<mark style="background:#d3f8b6">每项目持久记忆</mark>，会话启动自动加载。对长期记忆的管理，很重要
29. rules-distill：skills内容审查与治理
30. config-gc：`~/.claude` 的垃圾回收
31. repo-scan：不用
#### 二、安装什么程序
1. 安装一个skills管理仓库，最好能够给出每个skills的大体能力和使用节点，这样就能用将大部分skills改为AI不可直接调用，节省上下文
2. TDD工作流
3. 调用工作流
4. 自学习工作流
5. 需求确定工作流

### 对于matt
#### 一、安装什么skills

首先，model是什么？和skill有什么区别
	model就是模型调用，user就是必须人工调用
	user-invoked skill ：grill-with-docs → to-spec → to-tickets → implement 这一串skills都是必须人工调用——这些节点都是需要人工排版，不给模型自主权
其次，issue tracker、ticket到底是什么意思
	issue tracker就是任务存放点
	ticket就是任务单
1. grill-with-docs：用逼问的方式把想法磨锐利，同时留下纸面痕迹。让每次 grilling 的产出沉淀进项目的 <mark style="background:#d3f8b6">CONTEXT.md 和 ADR</mark>
	1. 调用了grilling、domain-modeling这两个model自动调用的skills。他的设计很有意思，通过人工调用控制节点，但是人工确认节点后模型就可以自动调用
	2. /grill-with-docs和/grill-me 最大的区别在于前者是根据代码库实现，后者纯想法实现
2. prototype：写一个一次性**原型**来回答一个设计问题。不是很懂这个skill的作用，写原型有什么意义么——非常重要
	1. 原型的意义在于<mark style="background:#d3f8b6">把一个说不清的判断变成可以用手验证的东西</mark>
	2. 将模糊的需求先落地成具体的、简单的实现，看看能否走得通
3. to-spec：把当前对话变成 spec 并发布到 issue tracker。首先，这个skill只负责收拢内容，具体内容由grilling实现；其次，issue tracker是什么东西？
4. to-tickets：不是很懂这个skill具体是做什么的，和to-spec有什么区别
	1. 非常重要的skill，能够<font color="#92d050">将to-spec整理出来的spec划分成tickets去逐步实现</font>
	2. <mark style="background:#d3f8b6">通过闪光弹的形式不是横向而是纵向地实现tickets</mark>——一条**窄但打穿所有层**的路径，做完**能单独演示或验证**
5. implement：按 spec 或 ticket 集合来构建。具体构建什么？
	1. 构建之前to-spec和to-tickets构建出来的任务清单
	2. <mark style="background:#d3f8b6">自动调用tdd和code-review去实现划分好的开发任务</mark>
6. tdd：说先约定seam，但是对于我一个不懂具体开发但是想通过开发实现我的需求的人，我怎么知道什么seam是我想要的？还有3个反模式是什么意思
	1. AI自动帮我提出seam，而且是用业务语言而不是代码语言提出，我只需要确定需求是否和我的想法对得上即可
	2. to-spec中也存在seam内容
	3. 具体如何进行测试的，方法论很重要：
		1. 检查能力怎么实现的，而不是聚焦在实现了什么上
		2. 测试结果如何和被测试结果使用了同一套计算逻辑，那么就无法检查是否出错了
		3. 如果先把测试写完，测试的内容可能是想象中的行为而不是真正实现的行为，自然测不出东西。所以一边写测试一边实现
7. code-review：不仅看代码质量还看是不是做了要求的事。
8. triage：针对于接手别人的项目，对我暂时用不上
9. diagnosing-bugs：测试驱动的 Bug 修复。很重要
10. wayfinder：在遇到一个大到一个会话无法实现的工作，将其在在 issue tracker 上建一张**决策 ticket 的共享地图**，分割任务，逐步实现。很重要
11. improve-codebase-architecture：扫代码库找深化机会，做成可视化 HTML 报告。深化机会具体是什么东西
12. domain-modeling：挑战一个模糊术语、解决一个**一词多义**的词，通过把难以逆转的决策记成 ADR，解决领域语言漂移问题
13. codebase-design：看不懂，不知道什么意思，更不知道怎么用