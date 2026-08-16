首先，model是什么？和skill有什么区别
其次，issue tracker、ticket到底是什么意思
1. grill-with-docs：用逼问的方式把想法磨锐利，同时留下纸面痕迹。让每次 grilling 的产出沉淀进项目的 <mark style="background:#d3f8b6">CONTEXT.md 和 ADR</mark>
2. prototype：写一个一次性**原型**来回答一个设计问题。不是很懂这个skill的作用，写原型有什么意义么
3. to-spec：把当前对话变成 spec 并发布到 issue tracker。首先，这个skill只负责收拢内容，具体内容由grilling实现；其次，issue tracker是什么东西？
4. to-tickets：不是很懂这个skill具体是做什么的，和to-spec有什么区别
5. implement：按 spec 或 ticket 集合来构建。具体构建什么？
6. tdd：说先约定seam，但是对于我一个不懂具体开发但是想通过开发实现我的需求的人，我怎么知道什么seam是我想要的？还有3个反模式是什么意思
7. code-review：不仅看代码质量还看是不是做了要求的事。
8. triage：针对于接手别人的项目，对我暂时用不上
9. diagnosing-bugs：测试驱动的 Bug 修复。很重要
10. wayfinder：在遇到一个大到一个会话无法实现的工作，将其在在 issue tracker 上建一张**决策 ticket 的共享地图**，分割任务，逐步实现。很重要
11. improve-codebase-architecture：扫代码库找深化机会，做成可视化 HTML 报告。深化机会具体是什么东西
12. domain-modeling：挑战一个模糊术语、解决一个**一词多义**的词，通过把难以逆转的决策记成 ADR，解决领域语言漂移问题
13. codebase-design：看不懂，不知道什么意思，更不知道怎么用