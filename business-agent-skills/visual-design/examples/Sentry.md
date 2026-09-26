# 源自 Sentry 的设计系统

## 1. 视觉主题与氛围

Sentry 的网站是一个深色优先的开发者工具界面，说着代码编辑器和终端窗口的语言。整个美学植根于深紫黑背景（`#1f1633`、`#150f23`），唤起 Sentry 为之而生的深夜调试时光。在这片墨色画布上，一套精心策划的紫色、粉色和独特的青柠绿点缀（`#c2ef4e`）构成既技术又活力的视觉系统。

排印（Typography）搭配是深思熟虑的："Dammit Sans" 以首屏尺寸（88px，字重 700）作为展示字体出现，个性十足、态度鲜明，与 Sentry 不羁的品牌语调（"Code breaks. Fix it faster."）相匹配；而 Rubik 是贯穿所有功能文本的主力 UI 字体——标题、正文、按钮、说明文字和导航。Monaco 提供代码片段和技术内容的等宽层，完成开发者工具三位一体。

Sentry 的独特之处在于拥抱"深色 IDE"美学，却不显得冷或 sterile（冰冷无菌）。暖紫色调替代了开发者工具典型的冷灰，大胆的插画元素（3D 角色、彩色产品截图）点缀深色画布。按钮系统用标志性的灰紫（`#79628c`）加内嵌阴影，营造出触感十足、近乎物理的质感——按钮像可以按进表面一样。

**关键特征：**
- 深紫黑背景（`#1f1633`、`#150f23`）——从不纯黑
- 暖紫点缀光谱：从深（`#362d59`）经中（`#79628c`、`#6a5fc1`）到艳（`#422082`）
- 青柠绿点缀（`#c2ef4e`），用于高可见 CTA 和高亮
- 粉/珊瑚点缀（`#ffb287`、`#fa7faa`），用于焦点态和次级高亮
- "Dammit Sans" 展示字体，在首屏尺寸展现品牌个性
- Rubik 为主 UI 字体，带大写字距标签
- Monaco 等宽字体用于代码元素
- 按钮内嵌阴影营造触感深度
- 毛玻璃效果 `blur(18px) saturate(180%)`

## 2. 色彩体系与角色

### 主品牌色
- **Deep Purple**（`#1f1633`）：主背景，品牌的定义色
- **Darker Purple**（`#150f23`）：更深区块、页脚、次级背景
- **Border Purple**（`#362d59`）：边框、分隔线、微妙结构线

### 点缀色
- **Sentry Purple**（`#6a5fc1`）：主交互色——链接、悬停态、焦点环
- **Muted Purple**（`#79628c`）：按钮背景、次级交互元素
- **Deep Violet**（`#422082`）：下拉选择、激活态、高强调表面
- **Lime Green**（`#c2ef4e`）：高可见点缀、特殊链接、徽章高亮
- **Coral**（`#ffb287`）：焦点态背景、暖点缀
- **Pink**（`#fa7faa`）：焦点轮廓、装饰点缀

### 文本色
- **Pure White**（`#ffffff`）：深色背景上的主文本
- **Light Gray**（`#e5e7eb`）：次级文本、弱化内容
- **Code Yellow**（`#dcdcaa`）：语法高亮、代码令牌

### 表面与叠加
- **Glass White**（`rgba(255, 255, 255, 0.18)`）：毛玻璃按钮背景
- **Glass Dark**（`rgba(54, 22, 107, 0.14)`）：玻璃元素的悬停叠加
- **Input White**（`#ffffff`）：表单输入背景（浅色语境）
- **Input Border**（`#cfcfdb`）：表单字段边框

### 阴影
- **Ambient Glow**（`rgba(22, 15, 36, 0.9) 0px 4px 4px 9px`）：深紫环境阴影
- **Button Hover**（`rgba(0, 0, 0, 0.18) 0px 0.5rem 1.5rem`）：抬升的悬停态
- **Card Shadow**（`rgba(0, 0, 0, 0.1) 0px 10px 15px -3px`）：标准卡片抬升
- **Inset Button**（`rgba(0, 0, 0, 0.1) 0px 1px 3px 0px inset`）：触感按压效果

## 3. 排印规则

### 字体族
- **展示**：`Dammit Sans`——首屏标题的品牌个性字体
- **主 UI**：`Rubik`，回退：`-apple-system, system-ui, Segoe UI, Helvetica, Arial`
- **等宽**：`Monaco`，回退：`Menlo, Ubuntu Mono`

### 层级

| 角色 | 字体 | 字号 | 字重 | 行高 | 字间距 | 备注 |
|------|------|------|--------|-------------|----------|-------|
| 展示首屏 | Dammit Sans | 88px (5.50rem) | 700 | 1.20（紧） | normal | 最大冲击力、品牌语调 |
| 展示次级 | Dammit Sans | 60px (3.75rem) | 500 | 1.10（紧） | normal | 次级首屏文本 |
| 章节标题 | Rubik | 30px (1.88rem) | 400 | 1.20（紧） | normal | 主要章节标题 |
| 副标题 | Rubik | 27px (1.69rem) | 500 | 1.25（紧） | normal | 功能区块标题 |
| 卡片标题 | Rubik | 24px (1.50rem) | 500 | 1.25（紧） | normal | 卡片和块标题 |
| 功能标题 | Rubik | 20px (1.25rem) | 600 | 1.25（紧） | normal | 强调的功能名 |
| 正文 | Rubik | 16px (1.00rem) | 400 | 1.50 | normal | 标准正文文本 |
| 正文强调 | Rubik | 16px (1.00rem) | 500–600 | 1.50 | normal | 粗正文、导航项 |
| 导航标签 | Rubik | 15px (0.94rem) | 500 | 1.40 | normal | 导航链接 |
| 大写标签 | Rubik | 15px (0.94rem) | 500 | 1.25（紧） | normal | `text-transform: uppercase` |
| 按钮文本 | Rubik | 14px (0.88rem) | 500–700 | 1.14–1.29（紧） | 0.2px | `text-transform: uppercase` |
| 说明文字 | Rubik | 14px (0.88rem) | 500–700 | 1.00–1.43 | 0.2px | 常为大写 |
| 小说明 | Rubik | 12px (0.75rem) | 600 | 2.00（放松） | normal | 微妙注释 |
| 微标签 | Rubik | 10px (0.63rem) | 600 | 1.80（放松） | 0.25px | `text-transform: uppercase` |
| 代码 | Monaco | 16px (1.00rem) | 400–700 | 1.50 | normal | 代码块、技术文本 |

### 原则
- **双重个性**：Dammit Sans 在展示尺寸带来不羁的品牌个性；Rubik 为所有功能内容提供干净的专业感。
- **大写即系统**：按钮、说明文字、标签和微文本全部用 `text-transform: uppercase` 加微妙字间距（0.2px–0.25px），形成贯穿的"技术标签"模式。
- **字重分层**：Rubik 用 400（正文）、500（强调/导航）、600（标题/强）、700（按钮/CTA）——干净的四层字重系统。
- **标题紧、正文松**：所有标题用 1.10–1.25 行高；正文用 1.50；小说明放宽到 2.00，保证极小字号的可读性。

## 4. 组件样式

### 按钮

**主灰紫按钮**
- 背景：`#79628c`（rgb(121, 98, 140)）
- 文本：`#ffffff`，大写，14px，字重 500–700，字间距 0.2px
- 边框：`1px solid #584674`
- 圆角：13px
- 阴影：`rgba(0, 0, 0, 0.1) 0px 1px 3px 0px inset`（触感内嵌）
- 悬停：抬升阴影 `rgba(0, 0, 0, 0.18) 0px 0.5rem 1.5rem`

**玻璃白**
- 背景：`rgba(255, 255, 255, 0.18)`（毛玻璃）
- 文本：`#ffffff`
- 内边距：8px
- 圆角：12px（左对齐变体：`12px 0px 0px 12px`）
- 阴影：`rgba(0, 0, 0, 0.08) 0px 2px 8px`
- 悬停背景：`rgba(54, 22, 107, 0.14)`
- 用途：深色表面上的次级操作

**白色实心**
- 背景：`#ffffff`
- 文本：`#1f1633`
- 内边距：12px 16px
- 圆角：8px
- 悬停：背景过渡到 `#6a5fc1`，文本变白
- 焦点：背景 `#ffb287`（珊瑚），轮廓 `rgb(106, 95, 193) solid 0.125rem`
- 用途：深色背景上的高可见 CTA

**深紫（选择/下拉）**
- 背景：`#422082`
- 文本：`#ffffff`
- 内边距：8px 16px
- 圆角：8px

### 输入框

**文本输入框**
- 背景：`#ffffff`
- 文本：`#1f1633`
- 边框：`1px solid #cfcfdb`
- 内边距：8px 12px
- 圆角：6px
- 焦点：边框色保持 `#cfcfdb`，阴影 `rgba(0, 0, 0, 0.15) 0px 2px 10px inset`

### 链接
- **深色上默认**：`#ffffff`，下划线装饰
- **悬停**：颜色过渡到 `#6a5fc1`（Sentry Purple）
- **紫色链接**：默认 `#6a5fc1`，悬停下划线
- **青柠点缀链接**：默认 `#c2ef4e`，悬停到 `#6a5fc1`
- **深色语境链接**：`#362d59`，悬停到 `#ffffff`

### 卡片与容器
- 背景：半透明或深紫表面
- 圆角：8px–12px
- 阴影：`rgba(0, 0, 0, 0.1) 0px 10px 15px -3px`
- 背景滤镜：毛玻璃效果 `blur(18px) saturate(180%)`

### 导航
- 首屏内容上的深色透明页眉
- 导航链接用 Rubik 15px 字重 500
- 白色文本，悬停到 Sentry Purple（`#6a5fc1`）
- 分类用大写标签，0.2px 字间距
- 移动端：汉堡菜单，全宽展开

## 5. 布局原则

### 间距系统
- 基础单位：8px
- 阶梯：1px、2px、4px、5px、6px、8px、12px、16px、24px、32px、40px、44px、45px、47px

### 网格与容器
- 最大内容宽：1152px（XL 断点）
- 响应式内边距：2rem（移动端）→ 4rem（平板+）
- 内容在容器内居中
- 全宽深色区块，内部内容收纳

### 断点
| 名称 | 宽度 | 关键变化 |
|------|-------|-------------|
| 移动端 | < 576px | 单列、堆叠布局 |
| 小平板 | 576–640px | 轻微宽度调整 |
| 平板 | 640–768px | 2 列开始 |
| 小桌面 | 768–992px | 完整导航可见 |
| 桌面 | 992–1152px | 标准布局 |
| 大桌面 | 1152–1440px | 最大宽度内容 |

### 留白（Whitespace）哲学
- **深色呼吸空间**：区块间慷慨的垂直间距（64px–80px+），让深色背景充当视觉休息。
- **内容岛**：功能区块是漂浮在深紫海洋中的自包含块，各有内部间距节奏。
- **非对称内边距**：按钮用非对称内边距模式（12px 16px、8px 12px），感觉有机而非僵硬。

### 圆角阶梯
- 极小（6px）：表单输入、小交互元素
- 标准（8px）：按钮、卡片、容器
- 舒适（10px–12px）：大容器、玻璃面板
- 圆润（13px）：主灰紫按钮
- 胶囊（18px）：图片容器、徽章

## 6. 深度与层级

| 层级 | 处理 | 用途 |
|-------|-----------|-----|
| 下沉（-1 级） | 内嵌阴影 `rgba(0, 0, 0, 0.1) 0px 1px 3px inset` | 主按钮（触感按压感） |
| 扁平（0 级） | 无阴影 | 默认表面、深色背景 |
| 表面（1 级） | `rgba(0, 0, 0, 0.08) 0px 2px 8px` | 玻璃按钮、微妙卡片 |
| 抬升（2 级） | `rgba(0, 0, 0, 0.1) 0px 10px 15px -3px` | 卡片、悬浮面板 |
| 突出（3 级） | `rgba(0, 0, 0, 0.18) 0px 0.5rem 1.5rem` | 悬停态、弹窗 |
| 环境（4 级） | `rgba(22, 15, 36, 0.9) 0px 4px 4px 9px` | 首屏周围的深紫环境光晕 |

**阴影哲学**：Sentry 用内嵌阴影（按钮像按进表面）和环境光晕（内容从深色背景中辐射）的独特组合。深紫环境阴影（`rgba(22, 15, 36, 0.9)`）是标志性的——它营造生物发光般的质感，内容仿佛发出自己的紫调光。

## 7. 应该做与不应该做

### 应该做
- 用深紫背景（`#1f1633`、`#150f23`）——从不用纯黑（`#000000`）
- 主按钮用内嵌阴影，营造触感按压效果
- Dammit Sans 只用于首屏/展示标题——其他一切用 Rubik
- 按钮和标签用 `text-transform: uppercase` 加 `letter-spacing: 0.2px`
- 青柠绿点缀（`#c2ef4e`）节制使用，冲击力最大化
- 分层表面用毛玻璃效果（`blur(18px) saturate(180%)`）
- 保持暖紫阴影调——阴影应带紫调，而非中性灰
- 用 Rubik 四层字重系统：400（正文）、500（导航/强调）、600（标题）、700（CTA）

### 不应该做
- 背景不用纯黑（`#000000`）——永远用暖紫黑
- Dammit Sans 不用于正文或 UI 元素——它只用于展示
- 边框不用标准灰（`#666`、`#999`）——用紫调灰（`#362d59`、`#584674`）
- 按钮不去掉大写处理——这是全系统模式
- 不用尖角（0px 圆角）——所有交互元素最小 6px
- 同一组件不混用青柠绿点缀和珊瑚/粉点缀
- 主按钮不用扁平（非内嵌）阴影——触感是标志性的
- 大写文本不忘字间距——最小 0.2px

## 8. 响应式行为

### 断点
| 名称 | 宽度 | 关键变化 |
|------|-------|-------------|
| 移动端 | <576px | 单列、汉堡导航、堆叠 CTA |
| 平板 | 576–768px | 2 列功能网格开始 |
| 小桌面 | 768–992px | 完整导航、左右布局 |
| 桌面 | 992–1152px | 最大宽度容器、完整布局 |
| 大屏 | >1152px | 内容最大宽度保持、慷慨边距 |

### 折叠策略
- 首屏文本：88px Dammit Sans → 60px → 移动端缩放
- 导航：横向 → 汉堡加滑出
- 功能区块：左右并排 → 堆叠卡片
- 按钮：行内 → 移动端全宽堆叠
- 容器内边距：4rem → 2rem

## 9. 智能体提示指南

### 快速色彩参考
- 背景：`#1f1633`（主）、`#150f23`（更深）
- 文本：`#ffffff`（主）、`#e5e7eb`（次级）
- 交互：`#6a5fc1`（链接/悬停）、`#79628c`（按钮）
- 点缀：`#c2ef4e`（青柠高亮）、`#ffb287`（珊瑚焦点）
- 边框：`#362d59`（深色）、`#cfcfdb`（浅色语境）

### 组件提示示例
- "Create a hero section on deep purple background (#1f1633). Headline at 88px Dammit Sans weight 700, line-height 1.20, white text. Sub-text at 16px Rubik weight 400, line-height 1.50. White solid CTA button (8px radius, 12px 16px padding), hover transitions to #6a5fc1."
- "Design a navigation bar: transparent over dark background. Rubik 15px weight 500, white text. Uppercase category labels with 0.2px letter-spacing. Hover color #6a5fc1."
- "Build a primary button: background #79628c, border 1px solid #584674, inset shadow rgba(0,0,0,0.1) 0px 1px 3px, white uppercase text at 14px Rubik weight 700, letter-spacing 0.2px, radius 13px. Hover: shadow rgba(0,0,0,0.18) 0px 0.5rem 1.5rem."
- "Create a glass card panel: background rgba(255,255,255,0.18), backdrop-filter blur(18px) saturate(180%), radius 12px. White text content inside."
- "Design a feature section: #150f23 background, 24px Rubik weight 500 heading, 16px Rubik weight 400 body text. 14px uppercase lime-green (#c2ef4e) label above heading."

### 迭代指南
1. 永远从深紫背景开始——色板是为深色模式而建的
2. 按钮用内嵌阴影，首屏区块用环境紫光晕
3. 大写 + 字间距是标签、按钮、说明文字的系统模式
4. 青柠绿（#c2ef4e）是"跳"色——每区块最多用一次
5. 叠加面板用毛玻璃，主表面用实心紫
6. Rubik 承担 90% 排印——Dammit Sans 只用于首屏
