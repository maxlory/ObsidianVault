
### Design reference（设计参照）
——模仿别人设计得优秀的网站

为什么这种方法有效：AI设计前端最大的问题不是乱创造，而是‘看不见’
——AI 写 CSS 的时候看不到渲染出来长什么样
<mark style="background:#d3f8b6">重点出在评判上，而不是生成上</mark>
开发永远是飞轮循环，没有最后的测试，开发无法进行下去

所以，模仿前端的真正意义在于：参照物的价值是给了 AI 一个可对齐的评分函数，而真正让它生效的关键是补上视觉闭环

##### 如何进行
<font color="#92d050">开源模板做骨架（工程质量），你欣赏的商业站做视觉参照（品味）</font>
- 商业站灵感：Godly (https://godly.website/)、Land-book (https://land-book.com/)、siteinspire (https://www.siteinspire.com/)、Minimal Gallery (https://minimal.gallery/)、Awwwards (https://www.awwwards.com/)（Awwwards 上的多半是炫技型，慎选）
- 开源模板：Astro 官方主题库 (https://astro.build/themes/)（个人站/博客主题多，质量高，一堆 MIT）、shadcn/ui (https://ui.shadcn.com/)（组件级，MIT）、Tailwind Plus（原 Tailwind UI，付费但质量顶）

执行路径
关键改进是中间加一步**「设计规格提取」**——不要让 AI 直接看着网址仿写：
1. 选 3–5 个参照物（内容形态跟你的站同类）
2. 让我用 Chrome DevTools MCP 打开它们，提取真实的 computed style：字号阶梯、间距 scale、配色 hex、圆角、阴影、字重、断点，输出成一份 design-spec.md。这一步是关键：把「感觉」变成「数字」（这部分看是否有程序的开源程序能够实现）
3. 你 review 这份 spec（全是数字，不懂 CSS 也看得懂）
4. 你准备内容清单——这一步 AI 代替不了，也是整件事里唯一真正属于你的部分
5. 我按 spec + 你的内容重写（不是复制）
6. 视觉回归：截我的成果 vs 截参照物，并排对比，改。循环 2–3 轮