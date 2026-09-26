# 源自 Vercel 的设计系统

## 1. 视觉主题与氛围

Vercel 的网站是"开发者基础设施隐形化"的视觉论述——一套克制到近乎哲学的设计系统。页面 overwhelmingly（压倒性地）是白色（`#ffffff`）配近黑（`#171717`）文本，营造画廊般的空旷，每个元素都配得上它的像素。这不是装饰性的极简，而是作为工程原则的极简。Geist 设计系统把界面当作编译器对待代码——剥掉每个不必要的令牌，直到只剩结构。

定制 Geist 字体族是皇冠上的明珠。Geist Sans 在展示尺寸用激进的负字间距（-2.4px 到 -2.88px），标题感觉压缩、紧迫、工程化——像为生产环境压缩过的代码。正文尺寸字距放宽，但几何精度不减。Geist Mono 作为等宽伴侣，用于代码、终端输出和技术标签。两种字体全局启用 OpenType `"liga"`（连字），加了一层值得细读的排印精致感。

Vercel 与其他单色设计系统的区别在于阴影即边框哲学。Vercel 不用传统 CSS 边框，而用 `box-shadow: 0px 0px 0px 1px rgba(0,0,0,0.08)`——零偏移、零模糊、1px 扩散的阴影，创造无盒模型影响的边框线。这项技术让边框存在于阴影层，实现更平滑的过渡、不裁剪的圆角，以及比传统边框更微妙的视觉分量。整个深度系统建立在分层、多值阴影栈上，每层有明确用途：一层做边框，一层做柔和抬升，一层做环境深度。

**关键特征：**
- Geist Sans 配极端负字间距（展示尺寸 -2.4px 到 -2.88px）——文本即压缩的基础设施
- Geist Mono 用于代码和技术标签，全局 OpenType `"liga"`
- 阴影即边框技法：`box-shadow 0px 0px 0px 1px` 全系统替代传统边框
- 多层阴影栈实现细腻深度（单声明中边框 + 抬升 + 环境）
- 近纯白画布配 `#171717` 文本——不是纯黑，营造微对比的柔和
- 工作流专属点缀色：Ship Red（`#ff5b4f`）、Preview Pink（`#de1d8d`）、Develop Blue（`#0a72ef`）
- 焦点环系统用 `hsla(212, 100%, 48%, 1)`——饱和蓝保障无障碍
- 胶囊徽章（9999px）带 tint 背景，作状态指示

## 2. 色彩体系与角色

### 主色
- **Vercel Black**（`#171717`）：主文本、标题、深色表面背景。不是纯黑——微暖防止生硬。
- **Pure White**（`#ffffff`）：页面背景、卡片表面、深色上的按钮文本。
- **True Black**（`#000000`）：次级用途，`--geist-console-text-color-default`，用于特定控制台/代码语境。

### 工作流点缀色
- **Ship Red**（`#ff5b4f`）：`--ship-text`，"上线生产"工作流步骤——暖、紧迫的珊瑚红。
- **Preview Pink**（`#de1d8d`）：`--preview-text`，预览部署工作流——鲜明的品红粉。
- **Develop Blue**（`#0a72ef`）：`--develop-text`，开发工作流——亮、聚焦的蓝。

### 控制台/代码色
- **Console Blue**（`#0070f3`）：`--geist-console-text-color-blue`，语法高亮蓝。
- **Console Purple**（`#7928ca`）：`--geist-console-text-color-purple`，语法高亮紫。
- **Console Pink**（`#eb367f`）：`--geist-console-text-color-pink`，语法高亮粉。

### 交互
- **Link Blue**（`#0072f5`）：主链接色，带下划线装饰。
- **Focus Blue**（`hsla(212, 100%, 48%, 1)`）：`--ds-focus-color`，交互元素的焦点环。
- **Ring Blue**（`rgba(147, 197, 253, 0.5)`）：`--tw-ring-color`，Tailwind ring 工具类。

### 中性阶梯
- **Gray 900**（`#171717`）：主文本、标题、导航文本。
- **Gray 600**（`#4d4d4d`）：次级文本、描述文案。
- **Gray 500**（`#666666`）：三级文本、弱化链接。
- **Gray 400**（`#808080`）：占位文本、禁用态。
- **Gray 100**（`#ebebeb`）：边框、卡片轮廓、分隔线。
- **Gray 50**（`#fafafa`）：微妙表面 tint、内阴影高光。

### 表面与叠加
- **Overlay Backdrop**（`hsla(0, 0%, 98%, 1)`）：`--ds-overlay-backdrop-color`，弹窗/对话框背景。
- **Selection Text**（`hsla(0, 0%, 95%, 1)`）：`--geist-selection-text-color`，文本选中高亮。
- **Badge Blue Bg**（`#ebf5ff`）：胶囊徽章背景，tint 蓝表面。
- **Badge Blue Text**（`#0068d6`）：胶囊徽章文本，更深的蓝保证可读。

### 阴影与深度
- **Border Shadow**（`rgba(0, 0, 0, 0.08) 0px 0px 0px 1px`）：标志性——替代传统边框。
- **Subtle Elevation**（`rgba(0, 0, 0, 0.04) 0px 2px 2px`）：卡片极简抬升。
- **Card Stack**（`rgba(0,0,0,0.08) 0px 0px 0px 1px, rgba(0,0,0,0.04) 0px 2px 2px, rgba(0,0,0,0.04) 0px 8px 8px -8px, #fafafa 0px 0px 0px 1px`）：完整多层卡片阴影。
- **Ring Border**（`rgb(235, 235, 235) 0px 0px 0px 1px`）：选项卡和图片的浅灰环边框。

## 3. 排印规则

### 字体族
- **主要**：`Geist`，回退：`Arial, Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol`
- **等宽**：`Geist Mono`，回退：`ui-monospace, SFMono-Regular, Roboto Mono, Menlo, Monaco, Liberation Mono, DejaVu Sans Mono, Courier New`
- **OpenType 特性**：所有 Geist 文本全局启用 `"liga"`；特定说明文字用 `"tnum"` 做表格式数字。

### 层级

| 角色 | 字体 | 字号 | 字重 | 行高 | 字间距 | 备注 |
|------|------|------|--------|-------------|----------|-------|
| 展示首屏 | Geist | 48px (3.00rem) | 600 | 1.00–1.17（紧） | -2.4px 到 -2.88px | 最大压缩、广告牌冲击力 |
| 章节标题 | Geist | 40px (2.50rem) | 600 | 1.20（紧） | -2.4px | 功能区块标题 |
| 大副标题 | Geist | 32px (2.00rem) | 600 | 1.25（紧） | -1.28px | 卡片标题、子区块 |
| 副标题 | Geist | 32px (2.00rem) | 400 | 1.50 | -1.28px | 较轻的副标题 |
| 卡片标题 | Geist | 24px (1.50rem) | 600 | 1.33 | -0.96px | 功能卡片 |
| 卡片标题浅 | Geist | 24px (1.50rem) | 500 | 1.33 | -0.96px | 次级卡片标题 |
| 大正文 | Geist | 20px (1.25rem) | 400 | 1.80（放松） | normal | 引言、功能描述 |
| 正文 | Geist | 18px (1.13rem) | 400 | 1.56 | normal | 标准阅读文本 |
| 小正文 | Geist | 16px (1.00rem) | 400 | 1.50 | normal | 标准 UI 文本 |
| 正文适中 | Geist | 16px (1.00rem) | 500 | 1.50 | normal | 导航、强调文本 |
| 正文半粗 | Geist | 16px (1.00rem) | 600 | 1.50 | -0.32px | 强标签、激活态 |
| 按钮/链接 | Geist | 14px (0.88rem) | 500 | 1.43 | normal | 按钮、链接、说明 |
| 小按钮 | Geist | 14px (0.88rem) | 400 | 1.00（紧） | normal | 紧凑按钮 |
| 说明文字 | Geist | 12px (0.75rem) | 400–500 | 1.33 | normal | 元数据、标签 |
| 等宽正文 | Geist Mono | 16px (1.00rem) | 400 | 1.50 | normal | 代码块 |
| 等宽说明 | Geist Mono | 13px (0.81rem) | 500 | 1.54 | normal | 代码标签 |
| 等宽小字 | Geist Mono | 12px (0.75rem) | 500 | 1.00（紧） | normal | `text-transform: uppercase`，技术标签 |
| 微徽章 | Geist | 7px (0.44rem) | 700 | 1.00（紧） | normal | `text-transform: uppercase`，微小徽章 |

### 原则
- **压缩即身份**：展示尺寸的 Geist Sans 用 -2.4px 到 -2.88px 字间距——所有主流设计系统中最激进的负字距。这让文本感觉 _minified_（被压缩过），像为生产优化的代码。字距随字号减小逐步放宽：32px 时 -1.28px，24px 时 -0.96px，16px 时 -0.32px，14px 时 normal。
- **处处连字**：每个 Geist 文本元素都启用 OpenType `"liga"`。连字不是装饰——它是结构性的，创造更紧凑、更高效的字形组合。
- **三字重、严格角色**：400（正文/阅读）、500（UI/交互）、600（标题/强调）。除微小徽章外不用粗体（700）。这个窄字重范围通过字号和字距而非字重创造层级。
- **等宽即身份**：大写加 `"tnum"` 或 `"liga"` 的 Geist Mono 是"开发者控制台"语调——紧凑的技术标签，连接营销站与产品。

## 4. 组件样式

### 按钮

**主白按钮（阴影边框）**
- 背景：`#ffffff`
- 文本：`#171717`
- 内边距：0px 6px（极简——内容驱动宽度）
- 圆角：6px（微妙圆润）
- 阴影：`rgb(235, 235, 235) 0px 0px 0px 1px`（环边框）
- 悬停：背景变为 `var(--ds-gray-1000)`（深色）
- 焦点：`2px solid var(--ds-focus-color)` 轮廓 + `var(--ds-focus-ring)` 阴影
- 用途：标准次级按钮

**主深色按钮（从 Geist 系统推断）**
- 背景：`#171717`
- 文本：`#ffffff`
- 内边距：8px 16px
- 圆角：6px
- 用途：主 CTA（"Start Deploying"、"Get Started"）

**胶囊按钮/徽章**
- 背景：`#ebf5ff`（tint 蓝）
- 文本：`#0068d6`
- 内边距：0px 10px
- 圆角：9999px（全胶囊）
- 字体：12px 字重 500
- 用途：状态徽章、标签、功能标签

**大胶囊（导航）**
- 背景：透明或 `#171717`
- 圆角：64px–100px
- 用途：选项卡导航、区块选择器

### 卡片与容器
- 背景：`#ffffff`
- 边框：通过阴影——`rgba(0, 0, 0, 0.08) 0px 0px 0px 1px`
- 圆角：8px（标准）、12px（精选/图片卡片）
- 阴影栈：`rgba(0,0,0,0.08) 0px 0px 0px 1px, rgba(0,0,0,0.04) 0px 2px 2px, #fafafa 0px 0px 0px 1px`
- 图片卡片：`1px solid #ebebeb`，顶部 12px 圆角
- 悬停：阴影微妙增强

### 输入框与表单
- 单选：标准样式，焦点背景 `var(--ds-gray-200)`
- 焦点阴影：`1px 0 0 0 var(--ds-gray-alpha-600)`
- 焦点轮廓：`2px solid var(--ds-focus-color)`——一致的蓝色焦点环
- 边框：用阴影技法，而非传统边框

### 导航
- 白色上简洁横向导航，粘性
- Vercel 文字标志左对齐，262x52px
- 链接：Geist 14px 字重 500，`#171717` 文本
- 激活：字重 600 或下划线
- CTA：深色胶囊按钮（"Start Deploying"、"Contact Sales"）
- 移动端：汉堡菜单折叠
- 产品下拉，多级菜单

### 图片处理
- 产品截图用 `1px solid #ebebeb` 边框
- 顶部圆角图片：`12px 12px 0px 0px` 圆角
- 仪表盘/代码预览截图主导功能区块
- 首屏图片背后柔和渐变背景（粉彩多色）

### 特色组件

**工作流流水线**
- 三步横向流水线：Develop → Preview → Ship
- 每步有自己的点缀色：蓝 → 粉 → 红
- 用线/箭头连接
- Vercel 核心价值主张的视觉隐喻

**信任栏/标志网格**
- 公司标志（Perplexity、ChatGPT、Cursor 等）灰度
- 横向滚动或网格布局
- 微妙的 `#ebebeb` 边框分隔

**指标卡片**
- 大数字展示（如 "10x faster"）
- 指标用 Geist 48px 字重 600
- 下方灰色正文描述
- 阴影边框卡片容器

## 5. 布局原则

### 间距系统
- 基础单位：8px
- 阶梯：1px、2px、3px、4px、5px、6px、8px、10px、12px、14px、16px、32px、36px、40px
- 显著缺口：从 16px 跳到 32px——主阶梯中没有 20px 或 24px

### 网格与容器
- 最大内容宽：约 1200px
- 首屏：居中单列，顶部内边距慷慨
- 功能区块：卡片用 2–3 列网格
- 全宽分隔线用 `border-bottom: 1px solid #171717`
- 代码/仪表盘截图全宽或收纳加边框

### 留白（Whitespace）哲学
- **画廊式空旷**：区块间巨大的垂直内边距（80px–120px+）。留白本身就是设计——它传达 Vercel 无需证明、无需隐藏。
- **文本压缩、空间扩张**：标题激进的负字间距被周围慷慨的留白平衡。文本密集；周围空间辽阔。
- **区块节奏**：白区块与白区块交替——区块间没有色彩变化。分隔只靠边框（阴影边框）和间距。

### 圆角阶梯
- 微（2px）：行内代码片段、小 span
- 微妙（4px）：小容器
- 标准（6px）：按钮、链接、功能元素
- 舒适（8px）：卡片、列表项
- 图片（12px）：精选卡片、图片容器（顶部圆角）
- 大（64px）：选项卡导航胶囊
- XL（100px）：大导航链接
- 全胶囊（9999px）：徽章、状态胶囊、标签
- 圆形（50%）：菜单开关、头像容器

## 6. 深度与层级

| 层级 | 处理 | 用途 |
|-------|-----------|-----|
| 扁平（0 级） | 无阴影 | 页面背景、文本块 |
| 环（1 级） | `rgba(0,0,0,0.08) 0px 0px 0px 1px` | 大多数元素的阴影即边框 |
| 浅环（1b 级） | `rgb(235,235,235) 0px 0px 0px 1px` | 选项卡、图片的浅环 |
| 微妙卡片（2 级） | 环 + `rgba(0,0,0,0.04) 0px 2px 2px` | 标准卡片，极简抬升 |
| 完整卡片（3 级） | 环 + 微妙 + `rgba(0,0,0,0.04) 0px 8px 8px -8px` + 内 `#fafafa` 环 | 精选卡片、高亮面板 |
| 焦点（无障碍） | `2px solid hsla(212, 100%, 48%, 1)` 轮廓 | 所有交互元素的键盘焦点 |

**阴影哲学**：Vercel 可以说拥有现代网页设计中最精密的阴影系统。Vercel 不用传统 Material Design 意义上的阴影做抬升，而是用多值阴影栈，每层有不同的建筑用途：一层做"边框"（0px 偏移、1px），一层加环境柔和（2px 模糊），一层处理远处深度（8px 模糊加负扩散），内环（`#fafafa`）创造让卡片从内"发光"的微妙高光。这种分层手法让卡片感觉是被建造出来的，而非漂浮的。

### 装饰性深度
- 首屏渐变：首屏内容背后柔和的粉彩多色渐变渲染（几乎看不见，氛围感）
- 区块边框：主要区块间 `1px solid #171717`（全深色线）
- 无背景色变化——深度完全来自阴影分层和边框对比

## 7. 应该做与不应该做

### 应该做
- 展示尺寸用 Geist Sans 加激进负字间距（48px 时 -2.4px 到 -2.88px）
- 用阴影即边框（`0px 0px 0px 1px rgba(0,0,0,0.08)`）替代传统 CSS 边框
- 所有 Geist 文本启用 `"liga"`——连字是结构性的，不是可选的
- 用三字重系统：400（正文）、500（UI）、600（标题）
- 工作流点缀色（红/粉/蓝）只用在各自的工作流语境
- 卡片用多层阴影栈（边框 + 抬升 + 环境 + 内高光）
- 色板保持无彩——`#171717` 到 `#ffffff` 的灰就是系统
- 主文本用 `#171717` 而非 `#000000`——微暖很重要

### 不应该做
- Geist Sans 不用正字间距——永远负或零
- 正文不用字重 700（粗体）——600 是上限，只用于标题
- 卡片不用传统 CSS `border`——用阴影边框技法
- UI 镀铬不引入暖色（橙、黄、绿）
- 工作流点缀色（Ship Red、Preview Pink、Develop Blue）不作装饰用
- 不用厚重阴影（> 0.1 不透明度）——阴影系统是耳语级的
- 正文不加宽字间距——Geist 天生紧凑
- 主操作按钮不用胶囊圆角（9999px）——胶囊只用于徽章/标签
- 卡片阴影不跳过内 `#fafafa` 环——它是让系统成立的内光

## 8. 响应式行为

### 断点
| 名称 | 宽度 | 关键变化 |
|------|-------|-------------|
| 小手机 | <400px | 紧凑单列、最小内边距 |
| 手机 | 400–600px | 标准移动端、堆叠布局 |
| 小平板 | 600–768px | 2 列网格开始 |
| 平板 | 768–1024px | 完整卡片网格、扩展内边距 |
| 小桌面 | 1024–1200px | 标准桌面布局 |
| 桌面 | 1200–1400px | 完整布局、最大内容宽 |
| 大桌面 | >1400px | 居中、慷慨边距 |

### 触控目标
- 按钮用舒适的内边距（垂直 8px–16px）
- 导航链接 14px，间距足够
- 胶囊徽章横向 10px 内边距作点击目标
- 移动端菜单开关用 50% 圆角圆形按钮

### 折叠策略
- 首屏：展示 48px → 缩小，保持负字距成比例
- 导航：横向链接 + CTA → 汉堡菜单
- 功能卡片：3 列 → 2 列 → 单列堆叠
- 代码截图：保持宽高比，可横向滚动
- 信任栏标志：网格 → 横向滚动
- 页脚：多列 → 堆叠单列
- 区块间距：80px+ → 移动端 48px

### 图片行为
- 仪表盘截图在所有尺寸保持边框处理
- 移动端首屏渐变柔化/简化
- 产品截图用响应式图片，保持一致圆角
- 全宽区块保持边缘到边缘处理

## 9. 智能体提示指南

### 快速色彩参考
- 主 CTA：Vercel Black（`#171717`）
- 背景：Pure White（`#ffffff`）
- 标题文本：Vercel Black（`#171717`）
- 正文文本：Gray 600（`#4d4d4d`）
- 边框（阴影）：`rgba(0, 0, 0, 0.08) 0px 0px 0px 1px`
- 链接：Link Blue（`#0072f5`）
- 焦点环：Focus Blue（`hsla(212, 100%, 48%, 1)`）

### 组件提示示例
- "Create a hero section on white background. Headline at 48px Geist weight 600, line-height 1.00, letter-spacing -2.4px, color #171717. Subtitle at 20px Geist weight 400, line-height 1.80, color #4d4d4d. Dark CTA button (#171717, 6px radius, 8px 16px padding) and ghost button (white, shadow-border rgba(0,0,0,0.08) 0px 0px 0px 1px, 6px radius)."
- "Design a card: white background, no CSS border. Use shadow stack: rgba(0,0,0,0.08) 0px 0px 0px 1px, rgba(0,0,0,0.04) 0px 2px 2px, #fafafa 0px 0px 0px 1px. Radius 8px. Title at 24px Geist weight 600, letter-spacing -0.96px. Body at 16px weight 400, #4d4d4d."
- "Build a pill badge: #ebf5ff background, #0068d6 text, 9999px radius, 0px 10px padding, 12px Geist weight 500."
- "Create navigation: white sticky header. Geist 14px weight 500 for links, #171717 text. Dark pill CTA 'Start Deploying' right-aligned. Shadow-border on bottom: rgba(0,0,0,0.08) 0px 0px 0px 1px."
- "Design a workflow section showing three steps: Develop (text color #0a72ef), Preview (#de1d8d), Ship (#ff5b4f). Each step: 14px Geist Mono uppercase label + 24px Geist weight 600 title + 16px weight 400 description in #4d4d4d."

### 迭代指南
1. 永远用阴影即边框替代 CSS 边框——`0px 0px 0px 1px rgba(0,0,0,0.08)` 是基础
2. 字间距随字号缩放：48px 时 -2.4px，32px 时 -1.28px，24px 时 -0.96px，14px 时 normal
3. 只有三字重：400（阅读）、500（交互）、600（宣告）
4. 色彩是功能性的，从不装饰——工作流色（红/粉/蓝）只标记流水线阶段
5. 卡片阴影中的内 `#fafafa` 环是 Vercel 卡片微妙内光的来源
6. 技术标签用大写 Geist Mono，其他一切用 Geist Sans
