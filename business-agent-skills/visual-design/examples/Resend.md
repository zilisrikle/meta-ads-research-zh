# 源自 Resend 的设计系统

## 1. 视觉主题与氛围

Resend 的网站是一块深色、电影感的画布，把邮件基础设施当作奢侈品来对待。整个页面笼罩在纯黑（`#000000`）中，文本以近白（`#f0f0f0`）发光，营造出剧场般的体验——内容在虚空舞台上表演。这不是典型的开发者工具式深色——而是摄影画廊那种受控的黑暗，每个元素都被精心打光，没有任何东西争抢注意力。

排印（Typography）系统是全场主角。三种精心挑选的字体构成兼具编辑感与技术感的层级：Domaine Display（一款 Klim Type Foundry 的衬线体）以巨大的 96px 出现在首屏标题，行高几乎贴紧（1.00），字距为负（-0.96px），让展示文本像杂志封面。ABC Favorit（Dinamo 出品）负责章节标题，字间距更激进（56px 时 -2.8px），给中层文本一种压缩的、工程化的质感。Inter 接管正文和 UI，提供干净的可读性，让展示字体闪光。Commit Mono 为代码块收尾。

Resend 的独特之处在于冰冷的、蓝调的边框系统。Resend 不用中性灰边框，而是用 `rgba(214, 235, 253, 0.19)`——一条冰霜般的、微蓝的线，19% 不透明度，让每个容器和分隔线在黑色背景上都有冷冽的、晶体的质感。配合胶囊形按钮（9999px 圆角）、多色点缀系统（橙、绿、蓝、黄、红——每种都有自己的 CSS 变量阶梯）和 OpenType 风格集（`"ss01"`、`"ss03"`、`"ss04"`、`"ss11"`），最终呈现出一套高级、精确、安静自信的设计系统。

**关键特征：**
- 纯黑背景配近白（`#f0f0f0`）文本——戏剧般、画廊般的黑暗
- 三字体层级：Domaine Display（衬线首屏）、ABC Favorit（几何章节）、Inter（正文/UI）
- 冰蓝调边框：`rgba(214, 235, 253, 0.19)`——每条边框都有冷冽的晶体微光
- 多色点缀系统：橙、绿、蓝、黄、红——每种带编号的 CSS 变量阶梯
- 胶囊形按钮和标签（9999px 圆角），透明背景
- 展示字体上的 OpenType 风格集（`"ss01"`、`"ss03"`、`"ss04"`、`"ss11"`）
- Commit Mono 用于代码——等宽字体是设计元素，而非事后补救
- 用蓝调环的耳语级阴影：`rgba(176, 199, 217, 0.145) 0px 0px 0px 1px`

## 2. 色彩体系与角色

### 主色
- **Void Black**（`#000000`）：页面背景，定义性的画布色（通过 `--color-black-12` 达 95% 不透明度）
- **Near White**（`#f0f0f0`）：主文本、按钮文本、高对比元素
- **Pure White**（`#ffffff`）：`--color-white`，最大强调文本、链接高亮

### 点缀阶梯——橙色
- **Orange 4**（`#ff5900`）：`--color-orange-4`，22% 不透明度——微妙暖光
- **Orange 10**（`#ff801f`）：`--color-orange-10`，主橙色点缀——暖、 energic（有活力的）
- **Orange 11**（`#ffa057`）：`--color-orange-11`，更浅的橙色，用于次级场景

### 点缀阶梯——绿色
- **Green 3**（`#22ff99`）：`--color-green-3`，12% 不透明度——淡淡的祖母绿渲染
- **Green 4**（`#11ff99`）：`--color-green-4`，18% 不透明度——成功指示光晕

### 点缀阶梯——蓝色
- **Blue 4**（`#0075ff`）：`--color-blue-4`，34% 不透明度——中等蓝色点缀
- **Blue 5**（`#0081fd`）：`--color-blue-5`，42% 不透明度——更强的蓝色
- **Blue 10**（`#3b9eff`）：`--color-blue-10`，亮蓝——链接、交互元素

### 点缀阶梯——其他
- **Yellow 9**（`#ffc53d`）：`--color-yellow-9`，警告或高亮的暖金色
- **Red 5**（`#ff2047`）：`--color-red-5`，34% 不透明度——错误状态、破坏性操作

### 中性阶梯
- **Silver**（`#a1a4a5`）：次级文本、弱化链接、描述
- **Dark Gray**（`#464a4d`）：三级文本、弱化内容
- **Mid Gray**（`#5c5c5c`）：悬停态、微妙强调
- **Medium Gray**（`#494949`）：四级文本
- **Light Gray**（`#f8f8f8`）：浅色模式表面（如适用）
- **Border Gray**（`#eaeaea`）：浅色语境边框
- **Edge Gray**（`#ececec`）：浅色表面上的微妙边框
- **Mist Gray**（`#dedfdf`）：浅色分隔线
- **Soft Gray**（`#e5e6e6`）：备用浅色边框

### 表面与叠加
- **Frost Primary**（`#fcfdff`）：主色令牌（微蓝调，94% 不透明度）
- **White Hover**（`rgba(255, 255, 255, 0.28)`）：深色上的按钮悬停态
- **White 60%**（`oklab(0.999994 ... / 0.577)`）：弱化文本的半透明白
- **White 64%**（`oklab(0.999994 ... / 0.642)`）：稍亮的半透明白

### 边框与阴影
- **Frost Border**（`rgba(214, 235, 253, 0.19)`）：标志性——冰蓝调边框，19% 不透明度
- **Frost Border Alt**（`rgba(217, 237, 254, 0.145)`）：列表项用的稍浅变体
- **Ring Shadow**（`rgba(176, 199, 217, 0.145) 0px 0px 0px 1px`）：蓝调的阴影即边框
- **Focus Ring**（`rgb(0, 0, 0) 0px 0px 0px 8px`）：厚重的黑色焦点环
- **Subtle Shadow**（`rgba(0, 0, 0, 0.1) 0px 1px 3px, rgba(0, 0, 0, 0.1) 0px 1px 2px -1px`）：极简卡片抬升

## 3. 排印规则

### 字体族
- **展示衬线**：`domaine`（Domaine Display，Klim Type Foundry）——首屏标题
- **展示无衬线**：`aBCFavorit`（ABC Favorit，Dinamo），回退：`ui-sans-serif, system-ui`——章节标题
- **正文/UI**：`inter`，回退：`ui-sans-serif, system-ui`——正文文本、按钮、导航
- **等宽**：`commitMono`，回退：`ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas`
- **次级**：`Helvetica`——特定 UI 语境的回退
- **系统**：`-apple-system, system-ui, Segoe UI, Roboto`——嵌入内容

### 层级

| 角色 | 字体 | 字号 | 字重 | 行高 | 字间距 | 备注 |
|------|------|------|--------|-------------|----------|-------|
| 展示首屏 | domaine | 96px (6.00rem) | 400 | 1.00（紧） | -0.96px | `"ss01", "ss04", "ss11"` |
| 展示首屏移动端 | domaine | 76.8px (4.80rem) | 400 | 1.00（紧） | -0.768px | 移动端缩放 |
| 章节标题 | aBCFavorit | 56px (3.50rem) | 400 | 1.20（紧） | -2.8px | `"ss01", "ss04", "ss11"` |
| 副标题 | aBCFavorit | 20px (1.25rem) | 400 | 1.30（紧） | normal | `"ss01", "ss04", "ss11"` |
| 紧凑副标题 | aBCFavorit | 16px (1.00rem) | 400 | 1.50 | -0.8px | `"ss01", "ss04", "ss11"` |
| 功能标题 | inter | 24px (1.50rem) | 500 | 1.50 | normal | 章节副标题 |
| 大正文 | inter | 18px (1.13rem) | 400 | 1.50 | normal | 引言 |
| 正文 | inter | 16px (1.00rem) | 400 | 1.50 | normal | 标准正文文本 |
| 正文半粗 | inter | 16px (1.00rem) | 600 | 1.50 | normal | 强调、激活态 |
| 导航链接 | aBCFavorit | 14px (0.88rem) | 500 | 1.43 | 0.35px | `"ss01", "ss03", "ss04"`——正字距 |
| 按钮/链接 | inter | 14px (0.88rem) | 500–600 | 1.43 | normal | 按钮、导航、CTA |
| 说明文字 | inter | 14px (0.88rem) | 400 | 1.60（放松） | normal | 描述 |
| Helvetica 说明 | Helvetica | 14px (0.88rem) | 400–600 | 1.00–1.71 | normal | UI 元素 |
| 小字 | inter | 12px (0.75rem) | 400–500 | 1.33 | normal | 标签、元数据、细则 |
| 小字大写 | inter | 12px (0.75rem) | 500 | 1.33 | normal | `text-transform: uppercase` |
| 小字首字母大写 | inter | 12px (0.75rem) | 500 | 1.33 | normal | `text-transform: capitalize` |
| 代码正文 | commitMono | 16px (1.00rem) | 400 | 1.50 | normal | 代码块 |
| 代码小字 | commitMono | 14px (0.88rem) | 400 | 1.43 | normal | 行内代码 |
| 代码微字 | commitMono | 12px (0.75rem) | 400 | 1.33 | normal | 小代码标签 |
| 标题（Helvetica） | Helvetica | 24px (1.50rem) | 400 | 1.40 | normal | 备用标题语境 |

### 原则
- **三字体编辑层级**：Domaine Display（衬线，首屏）、ABC Favorit（几何无衬线，章节）、Inter（可读正文）。每种字体职责严格——从不越界。
- **展示字体激进的负字距**：Domaine -0.96px，ABC Favorit -2.8px。展示字体感觉压缩、紧迫、经过设计——像杂志刊头。
- **导航正字距**：ABC Favorit 导航链接用 +0.35px 字间距——系统中唯一的正字距。这营造出通透、疏朗的导航文本，与压缩的标题形成对比。
- **OpenType 即身份**：`"ss01"`、`"ss03"`、`"ss04"`、`"ss11"` 风格集在所有 ABC Favorit 和 Domaine 文本上启用，激活替代字形，赋予 Resend 排印独特个性。
- **Commit Mono 是设计元素**：等宽字体不藏在代码块里——它被显著用于代码示例和技术内容，被当作一等视觉元素对待。

## 4. 组件样式

### 按钮

**主透明胶囊**
- 背景：透明
- 文本：`#f0f0f0`
- 内边距：5px 12px
- 圆角：9999px（全胶囊）
- 边框：`1px solid rgba(214, 235, 253, 0.19)`（冰霜边框）
- 悬停：背景 `rgba(255, 255, 255, 0.28)`（白色玻璃）
- 用途：深色背景上的主 CTA

**白色实心胶囊**
- 背景：`#ffffff`
- 文本：`#000000`
- 内边距：5px 12px
- 圆角：9999px
- 用途：高对比 CTA（"Get started"）

**幽灵按钮**
- 背景：透明
- 文本：`#f0f0f0`
- 圆角：4px
- 无边框
- 悬停：微妙背景 tint
- 用途：次级操作、选项卡项

### 卡片与容器
- 背景：透明或极微妙的深色 tint
- 边框：`1px solid rgba(214, 235, 253, 0.19)`（冰霜边框）
- 圆角：16px（标准卡片）、24px（大区块/面板）
- 阴影：`rgba(176, 199, 217, 0.145) 0px 0px 0px 1px`（环形阴影）
- 深色产品截图和代码演示作为卡片内容
- 无传统盒阴影抬升

### 输入框与表单
- 文本：深色上 `#f0f0f0`，浅色上 `#000000`
- 圆角：4px
- 焦点：基于阴影的环
- 极简样式——继承深色主题

### 导航
- 粘性深色页眉，底部冰霜边框：`1px solid rgba(214, 235, 253, 0.19)`
- "Resend" 文字标志左对齐
- 导航链接用 ABC Favorit 14px 字重 500，+0.35px 字距
- 胶囊 CTA 右对齐
- 移动端：汉堡折叠

### 图片处理
- 产品截图和代码演示主导内容区块
- 深色截图在深色背景上——无缝融合
- 圆角：图片 12px–16px
- 全宽区块带微妙渐变叠加

### 特色组件

**选项卡导航**
- 横向选项卡，带微妙选中指示
- 选项卡项：8px 圆角
- 激活态带微妙背景区分

**代码预览面板**
- 深色代码块，用 Commit Mono
- 冰霜边框（`rgba(214, 235, 253, 0.19)`）
- 用多色点缀令牌（橙、蓝、绿、黄）做语法高亮

**多色点缀徽章**
- 每个产品功能有自己的点缀色，来自 CSS 变量阶梯
- 徽章背景用点缀色低不透明度（12–42%），文本用全不透明度

## 5. 布局原则

### 间距系统
- 基础单位：8px
- 阶梯：1px、2px、4px、5px、6px、7px、8px、10px、12px、16px、20px、24px、30px、32px、40px

### 网格与容器
- 居中内容，最大宽度慷慨
- 全宽黑色区块，内部内容收纳
- 单列首屏，下方展开为功能网格
- 代码预览面板作为全宽或收纳展示

### 留白（Whitespace）哲学
- **电影般的黑空间**：黑色背景本身就是留白。区块间慷慨的垂直间距（80px–120px+）营造穿行黑暗的滚动体验，每个区块像场景一样浮现。
- **内容紧凑，环绕辽阔**：文本块和卡片内部紧凑，但漂浮在浩瀚的黑暗空间中——形成孤立的"内容岛"。
- **排印主导的节奏**：巨大的展示字体（96px）自带垂直节奏——每个标题都是一次锚定周围空间的视觉事件。

### 圆角阶梯
- 锐利（4px）：按钮（幽灵）、输入框、小交互元素
- 微妙（6px）：菜单面板、导航项
- 标准（8px）：选项卡、内容块
- 舒适（10px）：点缀元素
- 卡片（12px）：剪贴板按钮、中型容器
- 大（16px）：功能卡片、图片、主按钮
- 区块（24px）：大面板、区块容器
- 胶囊（9999px）：主 CTA、标签、徽章

## 6. 深度与层级

| 层级 | 处理 | 用途 |
|-------|-----------|-----|
| 扁平（0 级） | 无阴影，透明背景 | 默认——深色虚空上的大多数元素 |
| 环（1 级） | `rgba(176, 199, 217, 0.145) 0px 0px 0px 1px` | 卡片、容器的阴影即边框 |
| 冰霜边框（1b 级） | `1px solid rgba(214, 235, 253, 0.19)` | 显式边框——按钮、分隔线、选项卡 |
| 微妙（2 级） | `rgba(0, 0, 0, 0.1) 0px 1px 3px, rgba(0, 0, 0, 0.1) 0px 1px 2px -1px` | 浅色卡片抬升 |
| 焦点（3 级） | `rgb(0, 0, 0) 0px 0px 0px 8px` | 厚重黑色焦点环——无障碍 |

**阴影哲学**：Resend 几乎不用阴影。在纯黑背景上，传统阴影不可见——你无法向虚空投射阴影。相反，Resend 通过标志性的冰霜边框（`rgba(214, 235, 253, 0.19)`）创造深度——细、冰蓝调的线在黑暗中捕捉光线。这形成"漂浮在太空中的玻璃面板"美学，边框是主要深度机制。

### 装饰性深度
- 首屏内容背后微妙的暖渐变光晕（橙/琥珀 tint）
- 产品截图通过自身内部 UI 创造视觉深度
- 无渐变背景——深度来自边框亮度和内容对比

## 7. 应该做与不应该做

### 应该做
- 页面背景用纯黑（`#000000`）——虚空是画布
- 所有结构线用冰霜边框（`rgba(214, 235, 253, 0.19)`）——它们是蓝调标志
- Domaine Display 只用于首屏标题（96px），ABC Favorit 用于章节标题，Inter 用于其他一切
- Domaine 和 ABC Favorit 文本启用 OpenType `"ss01"`、`"ss04"`、`"ss11"`
- 主 CTA 和标签用胶囊圆角（9999px）
- 用多色点缀阶梯（橙/绿/蓝/黄/红）加不透明度变体，做语境特定的高亮
- 阴影保持在环级别（`0px 0px 0px 1px`）——在黑色上，传统阴影无效
- ABC Favorit 导航链接用 +0.35px 字间距——唯一的正字距

### 不应该做
- 背景不浅于 `#000000`——纯黑虚空不可妥协
- 不用中性灰边框——所有边框必须带冰霜蓝调
- Domaine Display 不用于正文——它是纯展示衬线
- 同一组件不混用点缀色——每个功能一种点缀色
- 深色背景上不用盒阴影做抬升——用冰霜边框代替
- 不跳过 OpenType 风格集——它们定义排印个性
- 导航链接不用负字间距——ABC Favorit 导航用正 +0.35px
- 深色上按钮不做不透明——透明加冰霜边框才是模式

## 8. 响应式行为

### 断点
| 名称 | 宽度 | 关键变化 |
|------|-------|-------------|
| 小手机 | <480px | 单列、紧凑内边距、76.8px 首屏 |
| 手机 | 480–600px | 标准移动端、堆叠布局 |
| 桌面 | >600px | 完整布局、96px 首屏、扩展区块 |

*注：Resend 用极简断点系统——只检测到 480px 和 600px。设计桌面优先，移动端干净折叠。*

### 触控目标
- 胶囊按钮：足够的内边距（最小 5px 12px）
- 选项卡项：8px 圆角，舒适的点击区
- 导航链接用 0.35px 字距做视觉分隔

### 折叠策略
- 首屏：Domaine 96px → 移动端 76.8px
- 导航：横向 → 汉堡
- 功能区块：左右并排 → 堆叠
- 代码面板：保持宽度，需要时横向滚动
- 间距按比例压缩

### 图片行为
- 产品截图保持宽高比
- 深色截图在所有尺寸下与深色背景无缝融合
- 圆角（12px–16px）跨断点保持

## 9. 智能体提示指南

### 快速色彩参考
- 背景：Void Black（`#000000`）
- 主文本：Near White（`#f0f0f0`）
- 次级文本：Silver（`#a1a4a5`）
- 边框：Frost Border（`rgba(214, 235, 253, 0.19)`）
- 橙色点缀：`#ff801f`
- 绿色点缀：`#11ff99`（18% 不透明度）
- 蓝色点缀：`#3b9eff`
- 焦点环：`rgb(0, 0, 0) 0px 0px 0px 8px`

### 组件提示示例
- "Create a hero section on pure black (#000000) background. Headline at 96px Domaine Display weight 400, line-height 1.00, letter-spacing -0.96px, near-white (#f0f0f0) text, OpenType 'ss01 ss04 ss11'. Subtitle at 20px ABC Favorit weight 400, line-height 1.30. Two pill buttons: white solid (#ffffff, 9999px radius) and transparent with frost border (rgba(214,235,253,0.19))."
- "Design a navigation bar: dark background with frost border bottom (1px solid rgba(214,235,253,0.19)). Nav links at 14px ABC Favorit weight 500, letter-spacing +0.35px, OpenType 'ss01 ss03 ss04'. White pill CTA right-aligned."
- "Build a feature card: transparent background, frost border (rgba(214,235,253,0.19)), 16px radius. Title at 56px ABC Favorit weight 400, letter-spacing -2.8px. Body at 16px Inter weight 400, #a1a4a5 text."
- "Create a code block using Commit Mono 16px on dark background. Frost border container (24px radius). Syntax colors: orange (#ff801f), blue (#3b9eff), green (#11ff99), yellow (#ffc53d)."
- "Design an accent badge: background #ff5900 at 22% opacity, text #ffa057, 9999px radius, 12px Inter weight 500."

### 迭代指南
1. 从纯黑开始——一切漂浮在虚空中
2. 冰霜边框（`rgba(214, 235, 253, 0.19)`）是通用结构元素——不是灰色，不是中性
3. 三种字体，三种角色：Domaine（首屏）、ABC Favorit（章节）、Inter（正文）——从不越界
4. 展示字体上 OpenType 风格集是强制的——它们定义个性
5. 多色点缀背景用低不透明度（12–42%），文本用全不透明度
6. CTA 和徽章用胶囊形（9999px），容器用标准圆角（4px–16px）
7. 不用阴影——用冰霜边框在虚空中做深度
