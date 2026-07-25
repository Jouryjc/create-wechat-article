# 蒸馏小余 2.0 配图风格

当用户没有指定其他风格，或明确要求复用“蒸馏小余 / 码农小余知识图解”风格时，默认使用这套视觉语言。

当前默认视觉标尺是蒸馏小余文章《Claude 总跑偏？做个 Plugin 固化工作流》：
`https://mp.weixin.qq.com/s/GaEdNZRgPV4ofNXvJsJQjQ`

默认贴近它的“奶油纸底横版知识卡”口味，而不是明亮蓝橙装饰海报。

## 核心定位

- hand-drawn technical explainer infographic
- sketchnote style
- 面向中文技术公众号的可扫读工程图解
- 不是海报，不是写实插画，不是企业 PPT
- 像把复杂概念画成一张能保存、能转发、能一眼看懂的工程笔记
- 默认吸收蒸馏小余知识卡的视觉语言：暖米白 / 奶油纸底、深海军蓝手绘线、低饱和便签卡片、居中大标题、清晰箭头、底部 takeaway

详细规范见 [deep-research-sketchnote-image-style.md](deep-research-sketchnote-image-style.md)。当需要生成封面、正文信息图、Agent/RAG/Deep Research 架构图时，优先按这份规范执行。

## 视觉关键词

- warm cream paper background
- dark navy hand-drawn lines
- low-saturation pastel sticky-note boxes
- clear arrows
- cute engineer / robot doodles
- formula blocks
- permission boundaries
- citation chains
- takeaway summary

## 视觉特征

- 背景默认使用暖奶油纸张质感，避免高饱和蓝底、蓝橙海报、暗色赛博风和复杂渐变。
- 使用深海军蓝手绘线条，线条要清楚、有轻微手绘感，但不要凌乱。
- 信息模块使用低饱和 pastel 便签式圆角盒子，例如浅蓝、浅绿、浅黄、浅橙、淡粉。
- 箭头必须清晰，表达从输入到处理、从问题到方法、从判断到行动的关系。
- 可以出现可爱的工程师涂鸦、机器人、浏览器窗口、文档、代码块、齿轮、放大镜、终端、数据库、便签纸。
- 允许用公式块、Prompt 块、Checklist 块、Takeaway Summary 块承载关键信息。
- 中文标签必须短、准、可读；不要生成伪代码、乱码英文或错误品牌词。
- 技术文章里的品牌和产品尽量用文字标签或抽象图标表达，避免复制真实 Logo。

## 版式特征

- 优先画流程、框架、对比、迁移清单、架构关系。
- 从左到右或从上到下推进，读者无需解释就能看懂阅读顺序。
- 每个模块只传达一个意思，避免把整篇文章塞进一张图。
- 标题清楚，模块标题短，说明文字少。
- 留白充足但不要太空，正文图保持参考文章那种“信息密度适中偏高”的横版知识卡观感。
- 末尾优先放一个 takeaway summary，总结读者应该带走的判断。

## 封面图要求

- 封面也必须是信息图式封面，不做单纯标题海报。
- 比例默认 2.35:1，同时兼容公众号居中 1:1 裁剪。
- 标题、主体流程、takeaway summary 必须放在居中 1:1 安全区内。
- 标题区可以是手绘大便签或圆角纸片，必须清晰可读。
- 主体区优先使用“问题 -> 方法 -> 结果”或“旧方式 -> Agent -> 新工作台”的小流程。
- 对 Deep Research / RAG / Agent 主题，优先使用“数据边界 -> 检索层 -> 编排层 -> 报告/引用链”的中心结构。
- 不要使用真实公司 Logo、官方商标或不可控品牌视觉。
- 不要做成大面积明亮蓝底 + 暖橙装饰的产品海报，除非用户明确指定这类参考图。

## 正文配图要求

- 每张图只解决一个问题。
- 优先用对比图、流程图、决策表、工作流、架构关系图。
- 图中必须有中文 takeaway summary 或结论条。
- 不要用过密小字；宁可少写，也要保证手机端可读。
- 推荐优先复用这些版式：云服务 vs 自托管、一轮式 Agent 失败、分层能力栈、多阶段流水线、mini-crews 分工、三圆交集总结。

## 禁止风格

- photorealistic
- cyberpunk
- dark sci-fi poster
- 3D glossy render
- glassmorphism dashboard
- corporate marketing poster
- complex gradient background
- dense tiny text
