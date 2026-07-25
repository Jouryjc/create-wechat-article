# Deep Research Sketchnote 图片设计规范

这套规范用于中文技术公众号封面图和正文信息图。当前蒸馏小余默认优先贴近这篇参考文章的视觉语言：
`https://mp.weixin.qq.com/s/GaEdNZRgPV4ofNXvJsJQjQ`

默认优先级高于泛泛的“技术插画”，但低于用户明确提供的参考图。

## 核心风格

- **风格名**：蒸馏小余知识卡 / Deep Research Sketchnote / 手绘工程研究图解
- **定位**：把复杂 AI Agent、RAG、工程流程、评估结果画成可保存、可转发、可复用的工程笔记。
- **气质**：聪明、克制、清楚、有一点手绘幽默感；不是炫技海报，不是企业 PPT，不是写实插画。
- **默认用途**：架构图、流程图、对比图、决策图、研究工作流、benchmark 解读、Agent 编排图、数据边界图。

## 视觉令牌

| 项目 | 规范 |
|---|---|
| 背景 | 暖米白、淡奶油纸张感，接近 `#F6EEDB` / `#FAF4E8`，不要默认使用高饱和蓝底 |
| 主线条 | 深海军蓝或近黑蓝，接近 `#0B1538`，圆润手绘线 |
| 强调色 | 低饱和浅蓝、浅绿、浅黄、浅橙、淡粉、淡紫 |
| 阴影 | 轻微偏移的暖橙或浅灰投影，模拟手绘卡片浮起 |
| 字体观感 | 粗一点的手写标题 + 清晰无衬线标签；中文必须可读 |
| 线条 | 2-4px 深色描边，允许轻微不规则，但不能凌乱 |
| 图标 | 文档、文件夹、数据库、锁、放大镜、网页、机器人、齿轮、麦克风、链条、复选框 |

## 构图原则

- 用图解释一个工程问题，而不是装饰文章。
- 所有模块都放进圆角卡片、虚线框、便签或分层容器里。
- 标题默认居中，正文图保持横版知识卡的信息密度，不要做成空旷标题海报。
- 主阅读路径必须清楚，优先从左到右，其次从上到下。
- 箭头表达因果、阶段、流转或权限边界，不能只是装饰。
- 每张图控制在 3-7 个主模块，手机端能扫读。
- 保留大量留白，不把正文塞进图里。
- 可以加入小人、机器人或表情气泡，但只作为解释辅助，不抢主体。

## 常用版式

### 1. 云服务 vs 自托管

适合解释数据主权、权限边界、索引位置、供应商风险。

结构：
- 左侧：Vendor Cloud / 闭源云服务，画云朵、服务器、用户查询离开网络。
- 右侧：Your Infrastructure / 自有基础设施，画虚线边界、锁、数据库、权限同步。
- 底部：一句 takeaway，例如“索引在哪里，信任边界就在哪里”。

### 2. 一轮式 Agent 的失败

适合解释上下文污染、错误传播、幻觉、报告失真。

结构：
- 中间一条横向流程：Gather -> Analyze -> Write。
- 用半透明噪音雾、裂缝、问号表示错误向后传播。
- 底部列出 2-3 个失败模式：矛盾被抹平、重复来源被当证据、多跳连接丢失。

### 3. 分层能力栈

适合解释系统能力优先级。

结构：
- 自下而上的 4-6 层积木。
- 底层必须是基础约束，例如“阶段分离”。
- 顶层才是体验增强，例如“语音层”。
- 右侧用短注释解释每层防止什么问题。

### 4. 多阶段流水线

适合解释 RAG、Deep Research、Agent 编排、评估流程。

结构：
- 5-7 个竖向或横向卡片阶段。
- 每个阶段用一个短标题 + 一个小图标。
- 中间用粗箭头连接。
- 对质量闸门加醒目标注，例如“幻觉闸门”“权限检查”“引用合并”。

### 5. Mini-crews / 多 Agent 分工

适合解释 Agent 边界和工具权限。

结构：
- 多个独立圆角卡片表示 Crew / Agent。
- 每张卡片列出“可用工具”和“不可触碰的上下文”。
- 用箭头只传结构化输出，避免画成共享大脑。
- 明确标注工具权限，例如“Analyst 无工具”“Writer 只读摘要”。

### 6. 三圆交集

适合总结价值主张。

结构：
- 三个低饱和半透明圆。
- 每个圆一个关键词：能力、控制、透明。
- 交集放产品/方案组合。
- 圆外可放一个小角色或气泡表达“以前很难同时做到”。

## 封面规范

- 默认比例 `2.35:1`，同时兼容公众号居中 `1:1` 裁剪。
- 标题、核心图形、关键 takeaway 必须放在中间安全区。
- 封面不是纯标题海报，必须包含一个小型信息结构。
- 推荐结构：大标题 + 1 个中心工程图 + 2-4 个短标签。
- 标题可以使用中英混排，但中文副标题必须清楚。
- 避免真实品牌 logo；如果需要表达品牌，用文字标签或抽象图标。

封面 prompt 必须包含：

```text
蒸馏小余知识卡 / Deep Research Sketchnote style, warm cream paper background, dark navy hand-drawn outlines, pastel sticky-note cards, centered title, compact knowledge-card density, clear engineering flowchart, Chinese labels, mobile-readable, not photorealistic, not corporate PPT, not bright blue/orange poster.
```

## 正文配图规范

- 每张图只讲一个概念或一个流程。
- 图片标题必须是具体判断，不要只写名词。
- 每张图至少包含一个 takeaway 条或底部结论句。
- 中文标签尽量 2-8 个字，必要英文术语可以保留。
- 不要生成大段英文小字；技术名词可以中英混排。
- 如果参考原文图，必须保留其信息结构，但要用中文重画和原创化视觉表达。

## 禁止事项

- 不要写实照片、3D 渲染、赛博朋克、暗黑科幻、玻璃拟态仪表盘。
- 不要企业宣传海报感：大渐变、大 Logo、大人物、空泛口号。
- 不要密集小字、伪代码乱码、错误品牌 logo。
- 不要把所有文章要点塞进一张图。
- 不要让装饰元素比流程关系更醒目。
- 不要默认使用明亮蓝橙大背景、过多云朵 / 齿轮 / 电路线条装饰。

## Prompt 模板

```text
Create a Chinese technical sketchnote infographic in 蒸馏小余知识卡 / Deep Research Sketchnote style.

Topic:
<本图要解释的问题>

Core message:
<一句话结论>

Layout:
<选择一种版式：云服务 vs 自托管 / 一轮式 Agent 失败 / 分层能力栈 / 多阶段流水线 / mini-crews / 三圆交集>

Elements:
- <模块 1>
- <模块 2>
- <模块 3>

Visual style:
- warm cream paper background, not a bright blue/orange poster
- dark navy hand-drawn outlines
- pastel sticky-note rounded cards
- centered title and compact knowledge-card density
- clear arrows and readable Chinese labels
- small friendly engineer/robot doodles only if useful
- mobile-readable, clean spacing

Avoid:
- photorealistic
- 3D glossy render
- dark cyberpunk
- corporate PPT
- dense tiny text
- real brand logos
```
