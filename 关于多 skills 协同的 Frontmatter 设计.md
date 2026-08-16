
为什么需要多 skills 协同：agent可以编排，skill当然也可以。而且skill的编排没有langgraph这样的调度模板，只能通过agent自主调度，这样就会产生出错的可能。
发现：Claude Code **没有**原生的 skill 流水线注册机制。
重点：Skill 之间的链接通过三个通道实现—— 
==**description**（决定何时触发 / 何时不触发）
**body 正文**（指导如何消费上游 / 产出什么给下游）
**artifact 产物文件**（实际在 skill 之间传递的数据合同）==

### 一、 Skill 发现与调用的底层机制
重点： Claude 靠 description 语义匹配决定是否调用
详情：所有已安装 skill 的 `name` + `description` 在 session 启动时被注入 context（作为 system-reminder 里的 skill 列表）。Claude 对用户请求做**语义匹配**——不是正则、不是关键词精确命中，而是 LLM 推理判断"这个请求跟哪个 skill 的 description 最相关"。
注意：description 的上限约 1,536 字符（context 预算的 1%）。超长会被截断，截断后的内容 Claude 看不到。

Agent调用skill的机制对多skills协同的影响：
1. Skill 正文只在被调用后才加载——平时 Claude 只看 description，不看正文。
	1. **description 里的守卫条件**（"先走阶段 1"）在skill调用前就能生效
	2. **正文里的链接指令**（"完成后调用 Skill('eval-design')"）只在 skill 被加载后才生效
	3. 正文加载后不会重新读文件——同 session 内改 SKILL.md 不生效
2. Skill 的多种调用方式
	![[Pasted image 20260707155229.png]]
	可以通过不同的调用方法，强制控制skill的调用

#### 二、调研得到的多种链接模式
从弱到强分别是：
1. Description 里写前置条件
2. Description 里声明上下游关系
3. 编号命名 + 层级标记
4. 正文中显式 Skill() 调用
5. 产物驱动交接（Artifact-driven handoff）
