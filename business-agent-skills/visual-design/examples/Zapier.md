# 源自 Zapier 的设计系统

## 1. 视觉主题与氛围

Zapier 的网站散发着温暖、亲切的专业感。它拒绝开发者工具冷单色极简主义，选用奶油 tint 画布（`#fffefb`），像未漂白的纸——相当于一本整理良好的笔记本的数字版。近黑（`#201515`）文本带微弱的红棕暖调，营造比机械更人性的氛围。这是为毫不费力而非技术感设计的自动化。

排印（Typography）系统是两种截然不同的个性的刻意交织。**Degular Display**——一款几何、宽体的展示字体——以 56–80px 处理首屏级标题，中等字重（500），行高极紧（0.90），标题像堆叠块一样垂直压缩。**Inter** 是其他一切的主力，从章节标题到正文文本和导航，回退到 Helvetica 和 Arial。**GT Alpina**——一款优雅的细字重衬线体，配激进的负字间距（-1.6px 到 -1.92px）——偶尔出现在柔和的编辑时刻。这个三字体系统让 Zapier 能切换语域——从大胆有力（Degular）到干净功能化（Inter）再到精致文学感（GT Alpina）。

品牌标志性的橙色（`#ff4f00`）无可错认——鲜明、饱和的红橙，精确落在交通锥的紧迫与日落的暖之间。它用得节制但果断：主 CTA 按钮、激活态下划线、点缀边框。在暖奶油背景上，这个橙色形成充满活力而不咄咄逼人的色彩关系。

**关键特征：**
- 暖奶油画布（`#fffefb`）而非纯白——有机、纸般的暖
- 带红调的近黑（`#201515`）——呼吸的文本，而非压迫
- 首屏标题用 Degular Display，0.90 行高——压缩、有冲击力、现代
- Inter 为通用 UI 字体，贯穿所有功能排印
- GT Alpina 作编辑点缀——细字重衬线配极端负字距
- Zapier Orange（`#ff4f00`）为唯一点缀——鲜明、暖、节制使用
- 暖中性色板：边框（`#c5c0b1`）、弱化文本（`#939084`）、表面 tint（`#eceae3`）
- 8px 基础间距系统，CTA 内边距慷慨（20px 24px）
- 边框优先的设计：`1px solid` 暖灰边框定义结构，而非阴影

## 2. 色彩体系与角色

### 主色
- **Zapier Black**（`#201515`）：主文本、标题、深色按钮背景。带红调的暖近黑——从不冰冷。
- **Cream White**（`#fffefb`）：页面背景、卡片表面、浅色按钮填充。不是纯白；泛黄的暖是刻意的。
- **Off-White**（`#fffdf9`）：次级背景表面，微妙的备用 tint。与奶油白几乎无法区分，但创造深度。

### 品牌点缀
- **Zapier Orange**（`#ff4f00`）：主 CTA 按钮、激活下划线指示、点缀边框。标志色——鲜明而暖。

### 中性阶梯
- **Dark Charcoal**（`#36342e`）：次级文本、页脚文本、强分隔线的边框色。带 70% 不透明度变体的暖深灰棕。
- **Warm Gray**（`#939084`）：三级文本、弱化标签、时间戳类内容。中档，带绿暖调。
- **Sand**（`#c5c0b1`）：主边框色、悬停态背景、分隔线。Zapier 结构元素的支柱。
- **Light Sand**（`#eceae3`）：次级按钮背景、浅边框、微妙卡片表面。
- **Mid Warm**（`#b5b2aa`）：备用边框调，用于特定 span 元素。

### 交互
- **Orange CTA**（`#ff4f00`）：主操作按钮和激活选项卡下划线。
- **Dark CTA**（`#201515`）：次级深色按钮，沙色悬停态。
- **Light CTA**（`#eceae3`）：三级/幽灵按钮，沙色悬停。
- **Link Default**（`#201515`）：标准链接色，与正文文本一致。
- **Hover Underline**：链接悬停时去掉 `text-decoration: underline`（反向模式）。

### 叠加与表面
- **Semi-transparent Dark**（`rgba(45, 45, 46, 0.5)`）：叠加按钮变体、背景类元素。
- **Pill Surface**（`#fffefb`）：带沙边框的白色胶囊按钮。

### 阴影与深度
- **Inset Underline**（`rgb(255, 79, 0) 0px -4px 0px 0px inset`）：激活选项卡指示——用内嵌盒阴影的橙色下划线。
- **Hover Underline**（`rgb(197, 192, 177) 0px -4px 0px 0px inset`）：非激活选项卡悬停——沙色下划线。

## 3. 排印规则

### 字体族
- **展示**：`Degular Display`——首屏标题的宽体几何展示字体
- **主要**：`Inter`，回退：`Helvetica, Arial`
- **编辑**：`GT Alpina`——编辑时刻的细字重衬线体
- **系统**：`Arial`——表单元素和系统 UI 的回退

### 层级

| 角色 | 字体 | 字号 | 字重 | 行高 | 字间距 | 备注 |
|------|------|------|--------|-------------|----------|-------|
| 展示首屏 XL | Degular Display | 80px (5.00rem) | 500 | 0.90（紧） | normal | 最大冲击力、压缩块 |
| 展示首屏 | Degular Display | 56px (3.50rem) | 500 | 0.90–1.10（紧） | 0–1.12px | 主首屏标题 |
| 展示首屏 SM | Degular Display | 40px (2.50rem) | 500 | 0.90（紧） | normal | 小号首屏变体 |
| 展示按钮 | Degular Display | 24px (1.50rem) | 600 | 1.00（紧） | 1px | 大 CTA 按钮文本 |
| 章节标题 | Inter | 48px (3.00rem) | 500 | 1.04（紧） | normal | 主要章节标题 |
| 编辑标题 | GT Alpina | 48px (3.00rem) | 250 | normal | -1.92px | 细编辑标题 |
| 编辑副标题 | GT Alpina | 40px (2.50rem) | 300 | 1.08（紧） | -1.6px | 编辑副标题 |
| 大副标题 | Inter | 36px (2.25rem) | 500 | normal | -1px | 大子区块 |
| 副标题 | Inter | 32px (2.00rem) | 400 | 1.25（紧） | normal | 标准子区块 |
| 中副标题 | Inter | 28px (1.75rem) | 500 | normal | normal | 中等副标题 |
| 卡片标题 | Inter | 24px (1.50rem) | 600 | normal | -0.48px | 卡片标题 |
| 大正文 | Inter | 20px (1.25rem) | 400–500 | 1.00–1.20（紧） | -0.2px | 功能描述 |
| 正文强调 | Inter | 18px (1.13rem) | 600 | 1.00（紧） | normal | 强调正文文本 |
| 正文 | Inter | 16px (1.00rem) | 400–500 | 1.20–1.25 | -0.16px | 标准阅读文本 |
| 正文半粗 | Inter | 16px (1.00rem) | 600 | 1.16（紧） | normal | 强标签 |
| 按钮 | Inter | 16px (1.00rem) | 600 | normal | normal | 标准按钮 |
| 小按钮 | Inter | 14px (0.88rem) | 600 | normal | normal | 小按钮 |
| 说明文字 | Inter | 14px (0.88rem) | 500 | 1.25–1.43 | normal | 标签、元数据 |
| 说明大写 | Inter | 14px (0.88rem) | 600 | normal | 0.5px | 大写区块标签 |
| 微字 | Inter | 12px (0.75rem) | 600 | 0.90–1.33 | 0.5px | 微小标签，常大写 |
| 小微字 | Inter | 13px (0.81rem) | 500 | 1.00–1.54 | normal | 小元数据文本 |

### 原则
- **三字体系统、角色清晰**：Degular Display 只在首屏尺寸 commanding（统领）注意力。Inter 处理所有功能内容。GT Alpina 节制地增添编辑暖意。
- **压缩展示**：0.90 行高的 Degular 形成垂直压缩的标题块，现代而建筑感。
- **字重即层级信号**：Inter 用 400（阅读）、500（导航/强调）、600（标题/CTA）。Degular 用 500（展示）、600（按钮）。
- **标签大写**：区块标签（如 "01 / Colors"）和小分类用 `text-transform: uppercase` 加 0.5px 字间距。
- **负字距求优雅**：GT Alpina 细字重编辑标题用 -1.6px 到 -1.92px 字间距。

## 4. 组件样式

### 按钮

**主橙按钮**
- 背景：`#ff4f00`
- 文本：`#fffefb`
- 内边距：8px 16px
- 圆角：4px
- 边框：`1px solid #ff4f00`
- 用途：主 CTA（"Start free with email"、"Sign up free"）

**主深色按钮**
- 背景：`#201515`
- 文本：`#fffefb`
- 内边距：20px 24px
- 圆角：8px
- 边框：`1px solid #201515`
- 悬停：背景变为 `#c5c0b1`，文本变为 `#201515`
- 用途：大次级 CTA 按钮

**浅色/幽灵按钮**
- 背景：`#eceae3`
- 文本：`#36342e`
- 内边距：20px 24px
- 圆角：8px
- 边框：`1px solid #c5c0b1`
- 悬停：背景变为 `#c5c0b1`，文本变为 `#201515`
- 用途：三级操作、筛选按钮

**胶囊按钮**
- 背景：`#fffefb`
- 文本：`#36342e`
- 内边距：0px 16px
- 圆角：20px
- 边框：`1px solid #c5c0b1`
- 用途：标签式选择、筛选胶囊

**叠加半透明**
- 背景：`rgba(45, 45, 46, 0.5)`
- 文本：`#fffefb`
- 圆角：20px
- 悬停：背景变为全不透明 `#2d2d2e`
- 用途：视频播放按钮、悬浮操作

**选项卡/导航（内嵌阴影）**
- 背景：透明
- 文本：`#201515`
- 内边距：12px 16px
- 阴影：`rgb(255, 79, 0) 0px -4px 0px 0px inset`（激活橙色下划线）
- 悬停阴影：`rgb(197, 192, 177) 0px -4px 0px 0px inset`（沙色下划线）
- 用途：横向选项卡导航

### 卡片与容器
- 背景：`#fffefb`
- 边框：`1px solid #c5c0b1`（暖沙边框）
- 圆角：5px（标准）、8px（精选）
- 默认无阴影抬升——边框定义收纳
- 悬停：边框色微妙增强

### 输入框与表单
- 背景：`#fffefb`
- 文本：`#201515`
- 边框：`1px solid #c5c0b1`
- 圆角：5px
- 焦点：边框色变为 `#ff4f00`（橙）
- 占位符：`#939084`

### 导航
- 奶油背景上的简洁横向导航
- Zapier 标志左对齐，104x28px
- 链接：Inter 16px 字重 500，`#201515` 文本
- CTA：橙色按钮（"Start free with email"）
- 选项卡导航用内嵌盒阴影下划线技法
- 移动端：汉堡折叠

### 图片处理
- 产品截图用 `1px solid #c5c0b1` 边框
- 圆角：5–8px
- 仪表盘/工作流截图在功能区块中突出
- 首屏内容背后浅渐变背景

### 特色组件

**工作流集成卡片**
- 成对展示连接的应用图标
- 应用间箭头或连接指示
- 沙边框收纳
- 应用名用 Inter 字重 500

**统计计数器**
- 大展示数字用 Inter 48px 字重 500
- 下方 `#36342e` 弱化描述
- 用于社会证明指标

**社会证明图标**
- 圆形图标按钮：14px 圆角
- 沙边框：`1px solid #c5c0b1`
- 用于页脚社交媒体关注链接

## 5. 布局原则

### 间距系统
- 基础单位：8px
- 阶梯：1px、4px、6px、8px、10px、12px、16px、20px、24px、32px、40px、48px、56px、64px、72px
- CTA 按钮用慷慨内边距：大 20px 24px，标准 8px 16px
- 区块内边距：垂直 64px–80px

### 网格与容器
- 最大内容宽：约 1200px
- 首屏：居中单列，顶部内边距大
- 功能区块：集成卡片用 2–3 列网格
- 区块间全宽沙边框分隔线
- 页脚：多列深色背景（`#201515`）

### 留白（Whitespace）哲学
- **暖呼吸空间**：区块间慷慨的垂直间距（64px–80px），但内容区相对密集——Zapier 在奶油画布内高效打包信息。
- **建筑式压缩**：0.90 行高的 Degular Display 标题垂直压缩，与周围开阔的间距形成对比。
- **区块节奏**：全程奶油背景，区块用沙色边框而非背景色变化分隔。

### 圆角阶梯
- 紧（3px）：小行内 span
- 标准（4px）：按钮（橙 CTA）、标签、小元素
- 内容（5px）：卡片、链接、通用容器
- 舒适（8px）：精选卡片、大按钮、选项卡
- 社交（14px）：社交图标按钮、胶囊类元素
- 胶囊（20px）：播放按钮、大胶囊按钮、悬浮操作

## 6. 深度与层级

| 层级 | 处理 | 用途 |
|-------|-----------|-----|
| 扁平（0 级） | 无阴影 | 页面背景、文本块 |
| 边框（1 级） | `1px solid #c5c0b1` | 标准卡片、容器、输入框 |
| 强边框（1b 级） | `1px solid #36342e` | 深色分隔线、强调区块 |
| 激活选项卡（2 级） | `rgb(255, 79, 0) 0px -4px 0px 0px inset` | 激活选项卡下划线（橙） |
| 悬停选项卡（2b 级） | `rgb(197, 192, 177) 0px -4px 0px 0px inset` | 悬停选项卡下划线（沙） |
| 焦点（无障碍） | `1px solid #ff4f00` 轮廓 | 交互元素的焦点环 |

**阴影哲学**：Zapier 刻意避开传统的阴影抬升。结构几乎完全通过边框定义——标准收纳用暖沙（`#c5c0b1`）边框，强调用深炭（`#36342e`）边框。唯一的类阴影技法是选项卡下划线用的内嵌盒阴影，`0px -4px 0px 0px inset` 阴影形成底部条指示。这种边框优先的手法让设计接地、有形，而非漂浮。

### 装饰性深度
- 激活选项卡上的橙色内嵌下划线在元素底部创造视觉"分量"
- 沙色悬停下划线提供无布局偏移的预览态
- 主内容无背景渐变——奶油画布始终一致
- 页脚用全深色背景（`#201515`）做对比反转

## 7. 应该做与不应该做

### 应该做
- Degular Display 只用于首屏级标题（40px+），0.90 行高营造压缩冲击力
- 所有功能 UI 用 Inter——导航、正文文本、按钮、标签
- 背景用暖奶油（`#fffefb`），从不用纯白
- 文本用 `#201515`，从不用纯黑——红暖很重要
- Zapier Orange（`#ff4f00`）留给主 CTA 和激活态指示
- 沙（`#c5c0b1`）边框作为主结构元素，替代阴影
- 大 CTA 用慷慨的按钮内边距（20px 24px），匹配 Zapier 宽敞的按钮风格
- 选项卡导航用内嵌盒阴影下划线，而非 border-bottom
- 区块标签和微分类用大写加 0.5px 字间距

### 不应该做
- Degular Display 不用于正文文本或 UI 元素——它只用于展示
- 不用纯白（`#ffffff`）或纯黑（`#000000`）——Zapier 的色板是暖偏移的
- 卡片不用盒阴影抬升——用边框代替
- Zapier Orange 不散布全 UI——它留给 CTA 和激活态
- 大 CTA 按钮不用紧凑内边距——Zapier 的按钮刻意宽敞
- 不忽视暖中性系统——边框应用 `#c5c0b1`，而非灰色
- GT Alpina 不用于功能 UI——它只在细字重做编辑点缀
- GT Alpina 不用正字间距——它用激进的负字距（-1.6px 到 -1.92px）
- 主按钮不用圆胶囊形（9999px）——胶囊用于标签和社交图标

## 8. 响应式行为

### 断点
| 名称 | 宽度 | 关键变化 |
|------|-------|-------------|
| 小手机 | <450px | 紧凑单列、首屏文本缩小 |
| 手机 | 450–600px | 标准移动端、堆叠布局 |
| 大手机 | 600–640px | 轻微横向呼吸空间 |
| 小平板 | 640–680px | 2 列网格开始 |
| 平板 | 680–768px | 卡片网格扩展 |
| 大平板 | 768–991px | 完整卡片网格、扩展内边距 |
| 小桌面 | 991–1024px | 桌面布局开始 |
| 桌面 | 1024–1280px | 完整布局、最大内容宽 |
| 大桌面 | >1280px | 居中、慷慨边距 |

### 触控目标
- 大 CTA 按钮：20px 24px 内边距（舒适的 60px+ 高度）
- 标准按钮：8px 16px 内边距
- 导航链接：16px 字重 500，间距足够
- 社交图标：14px 圆角圆形按钮
- 选项卡项：12px 16px 内边距

### 折叠策略
- 首屏：Degular 80px 展示在小屏缩到 40–56px
- 导航：横向链接 + CTA 折叠为汉堡菜单
- 功能卡片：3 列网格 → 2 列 → 单列堆叠
- 集成工作流插画：保持宽高比，可简化
- 页脚：多列深色区块折叠为堆叠
- 区块间距：64–80px 在移动端减到 40–48px

### 图片行为
- 产品截图在所有尺寸保持沙边框处理
- 集成应用图标在响应式容器内保持固定尺寸
- 首屏插画按比例缩放
- 全宽区块保持边缘到边缘处理

## 9. 智能体提示指南

### 快速色彩参考
- 主 CTA：Zapier Orange（`#ff4f00`）
- 背景：Cream White（`#fffefb`）
- 标题文本：Zapier Black（`#201515`）
- 正文文本：Dark Charcoal（`#36342e`）
- 边框：Sand（`#c5c0b1`）
- 次级表面：Light Sand（`#eceae3`）
- 弱化文本：Warm Gray（`#939084`）

### 组件提示示例
- "Create a hero section on cream background (`#fffefb`). Headline at 56px Degular Display weight 500, line-height 0.90, color `#201515`. Subtitle at 20px Inter weight 400, line-height 1.20, color `#36342e`. Orange CTA button (`#ff4f00`, 4px radius, 8px 16px padding, white text) and dark button (`#201515`, 8px radius, 20px 24px padding, white text)."
- "Design a card: cream background (`#fffefb`), `1px solid #c5c0b1` border, 5px radius. Title at 24px Inter weight 600, letter-spacing -0.48px, `#201515`. Body at 16px weight 400, `#36342e`. No box-shadow."
- "Build a tab navigation: transparent background. Inter 16px weight 500, `#201515` text. Active tab: `box-shadow: rgb(255, 79, 0) 0px -4px 0px 0px inset`. Hover: `box-shadow: rgb(197, 192, 177) 0px -4px 0px 0px inset`. Padding 12px 16px."
- "Create navigation: cream sticky header (`#fffefb`). Inter 16px weight 500 for links, `#201515` text. Orange pill CTA 'Start free with email' right-aligned (`#ff4f00`, 4px radius, 8px 16px padding)."
- "Design a footer with dark background (`#201515`). Text `#fffefb`. Links in `#c5c0b1` with hover to `#fffefb`. Multi-column layout. Social icons as 14px-radius circles with sand borders."

### 迭代指南
1. 永远用暖奶油（`#fffefb`）背景，从不用纯白——暖定义 Zapier
2. 边框（`1px solid #c5c0b1`）是结构支柱——避免阴影抬升
3. Zapier Orange（`#ff4f00`）是唯一点缀色；其他一切都是暖中性色
4. 三种字体、严格角色：Degular Display（首屏）、Inter（UI）、GT Alpina（编辑）
5. 大 CTA 按钮需要慷慨内边距（20px 24px）——Zapier 按钮感觉宽敞
6. 选项卡导航用内嵌盒阴影下划线，而非 border-bottom
7. 文本永远暖：深色 `#201515`，正文 `#36342e`，弱化 `#939084`
8. 区块分类用 12–14px 大写标签，0.5px 字间距
