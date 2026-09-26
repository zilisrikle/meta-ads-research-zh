# 源自 Pinterest 的设计系统

## 1. 视觉主题与氛围

Pinterest 的网站是一块温暖、以灵感驱动的画布，把视觉发现当作生活方式杂志来对待。设计基于柔和的微暖白色背景，以 Pinterest Red（`#e60023`）作为唯一、大胆的品牌点缀色。与大多数科技平台的冷蓝色不同，Pinterest 的中性色阶带有明显的暖色调——灰色偏向橄榄色/沙色（`#91918c`、`#62625b`、`#e5e5e0`）而非冷钢色，营造出温馨、手作般的氛围，邀请用户浏览。

排印（Typography）使用 Pin Sans——一款定制专有字体，配有包含日文字体的广泛回退栈，体现了 Pinterest 的全球覆盖。在展示尺寸（70px，字重 600）下，Pin Sans 塑造出大而亲切的标题。在小尺寸下，系统很紧凑：按钮 12px，说明文字 12–14px。CSS 变量命名系统（`--comp-*`、`--sema-*`、`--base-*`）揭示了一个精密的三层设计令牌（token）架构：组件级、语义级和基础级令牌。

Pinterest 的独特之处在于慷慨的圆角（Border Radius）系统（12px–40px，圆形用 50%）和暖调按钮背景。次级按钮（`#e5e5e0`）带有明显的暖沙色调，而非冷灰色。主红色按钮采用 16px 圆角——圆润但不是胶囊形。加上暖色徽章背景（`hsla(60,20%,98%,.5)`——一层微妙的暖黄渲染）和以摄影为主导的布局，最终呈现出一种手作感、个人化的设计，而非企业化的冷感。

**关键特征：**
- 暖白画布配橄榄色/沙色调中性色——温馨，而非冰冷
- Pinterest Red（`#e60023`）作为唯一大胆点缀——从不含蓄，永远自信
- Pin Sans 定制字体，带全球回退栈（含 CJK）
- 三层令牌架构：`--comp-*` / `--sema-*` / `--base-*`
- 暖色次级表面：沙灰（`#e5e5e0`）、暖徽章（`hsla(60,20%,98%,.5)`）
- 慷慨的圆角：标准 16px，大容器可达 40px
- 摄影优先的内容——图钉/图片是主要视觉元素
- 深近紫文本（`#211922`）——暖，带一丝梅紫色调

## 2. 色彩体系与角色

### 主品牌色
- **Pinterest Red**（`#e60023`）：主 CTA、品牌点缀——大胆、自信的红
- **Green 700**（`#103c25`）：`--base-color-green-700`，成功/自然点缀
- **Green 700 Hover**（`#0b2819`）：`--base-color-hover-green-700`，按下态绿色

### 文本
- **Plum Black**（`#211922`）：主文本——带梅紫色调的暖近黑
- **Black**（`#000000`）：次级文本、按钮文本
- **Olive Gray**（`#62625b`）：次级描述、弱化文本
- **Warm Silver**（`#91918c`）：`--comp-button-color-text-transparent-disabled`，禁用文本、输入框边框
- **White**（`#ffffff`）：深色/彩色表面上的文本

### 交互
- **Focus Blue**（`#435ee5`）：`--comp-button-color-border-focus-outer-transparent`，焦点环
- **Performance Purple**（`#6845ab`）：`--sema-color-hover-icon-performance-plus`，性能相关功能
- **Recommendation Purple**（`#7e238b`）：`--sema-color-hover-text-recommendation`，AI 推荐
- **Link Blue**（`#2b48d4`）：链接文本色
- **Facebook Blue**（`#0866ff`）：`--facebook-background-color`，社交登录
- **Pressed Blue**（`#617bff`）：`--base-color-pressed-blue-200`，按下态

### 表面与边框
- **Sand Gray**（`#e5e5e0`）：次级按钮背景——暖、手作感
- **Warm Light**（`#e0e0d9`）：圆形按钮背景、徽章
- **Warm Wash**（`hsla(60, 20%, 98%, 0.5)`）：`--comp-badge-color-background-wash-light`，微妙的暖徽章背景
- **Fog**（`#f6f6f3`）：浅色表面（50% 不透明度）
- **Border Disabled**（`#c8c8c1`）：`--sema-color-border-disabled`，禁用边框
- **Hover Gray**（`#bcbcb3`）：`--base-color-hover-grayscale-150`，悬停边框
- **Dark Surface**（`#33332e`）：深色区块背景

### 语义色
- **Error Red**（`#9e0a0a`）：复选框/表单错误状态

## 3. 排印规则

### 字体族
- **主要**：`Pin Sans`，回退：`-apple-system, system-ui, Segoe UI, Roboto, Oxygen-Sans, Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, Helvetica, ヒラギノ角ゴ Pro W3, メイリオ, Meiryo, ＭＳ Ｐゴシック, Arial`

### 层级

| 角色 | 字体 | 字号 | 字重 | 行高 | 字间距 | 备注 |
|------|------|------|--------|-------------|----------|-------|
| 展示标题 | Pin Sans | 70px (4.38rem) | 600 | normal | normal | 最大冲击力 |
| 章节标题 | Pin Sans | 28px (1.75rem) | 700 | normal | -1.2px | 负字距 |
| 正文 | Pin Sans | 16px (1.00rem) | 400 | 1.40 | normal | 标准阅读 |
| 说明粗体 | Pin Sans | 14px (0.88rem) | 700 | normal | normal | 强元数据 |
| 说明文字 | Pin Sans | 12px (0.75rem) | 400–500 | 1.50 | normal | 小文本、标签 |
| 按钮 | Pin Sans | 12px (0.75rem) | 400 | normal | normal | 按钮标签 |

### 原则
- **紧凑的字号阶梯**：范围为 12px–70px，跨度很大——大多数功能文本为 12–16px，形成密集的、类应用的信息层级。
- **暖的字重分布**：标题 600–700，正文 400–500。不使用超细字重——字体始终感觉厚实。
- **标题负字距**：28px 标题用 -1.2px，营造温馨、亲密的章节标题。
- **单一字体族**：Pin Sans 包办一切——未检测到次级展示字体或等宽字体。

## 4. 组件样式

### 按钮

**主红按钮**
- 背景：`#e60023`（Pinterest Red）
- 文本：`#000000`（黑色——在红色上用黑色对比是不同寻常的选择）
- 内边距：6px 14px
- 圆角：16px（慷慨圆润，不是胶囊形）
- 边框：`2px solid rgba(255, 255, 255, 0)`（透明）
- 焦点：通过 CSS 变量的语义边框 + 轮廓

**次级沙色按钮**
- 背景：`#e5e5e0`（暖沙灰）
- 文本：`#000000`
- 内边距：6px 14px
- 圆角：16px
- 焦点：同一语义边框系统

**圆形操作按钮**
- 背景：`#e0e0d9`（暖浅色）
- 文本：`#211922`（梅黑）
- 圆角：50%（圆形）
- 用途：图钉操作、导航控制

**幽灵/透明按钮**
- 背景：透明
- 文本：`#000000`
- 无边框
- 用途：三级操作

### 卡片与容器
- 以摄影为主的图钉卡片，圆角慷慨（12px–20px）
- 大多数卡片没有传统盒阴影
- 白色或暖雾色背景
- 部分图片容器有 8px 白色粗边框

### 输入框
- 邮箱输入框：白色背景，`1px solid #91918c` 边框，16px 圆角，11px 15px 内边距
- 焦点：通过 CSS 变量的语义边框 + 轮廓系统

### 导航
- 白色或暖色背景上的简洁页眉
- Pinterest 标志 + 居中搜索栏
- 导航链接用 Pin Sans 16px
- 激活态用 Pinterest Red 点缀

### 图片处理
- 图钉式瀑布流网格（Pinterest 标志性布局）
- 圆角：图片 12px–20px
- 摄影作为主要内容——每个图钉都是一张图片
- 精选图片容器用粗白色边框（8px）

## 5. 布局原则

### 间距系统
- 基础单位：8px
- 阶梯：4px、6px、7px、8px、10px、11px、12px、16px、18px、20px、22px、24px、32px、80px、100px
- 大跨度：32px → 80px → 100px 用于区块间距

### 网格与容器
- 图钉内容的瀑布流网格（标志性布局）
- 居中内容区块，最大宽度慷慨
- 全宽深色页脚
- 搜索栏作为主要导航元素

### 留白（Whitespace）哲学
- **灵感密度**：瀑布流网格把图钉排得很紧——内容密度本身就是价值主张。留白存在于区块之间，而非网格内部。
- **上方呼吸，下方密集**：首屏/功能区块获得慷慨的内边距；图钉网格紧凑而沉浸。

### 圆角阶梯
- 标准（12px）：小卡片、链接
- 按钮（16px）：按钮、输入框、中型卡片
- 舒适（20px）：功能卡片
- 大（28px）：大容器
- 区块（32px）：选项卡元素、大面板
- 首屏（40px）：首屏容器、大功能块
- 圆形（50%）：操作按钮、选项卡指示器

## 6. 深度与层级

| 层级 | 处理 | 用途 |
|-------|-----------|-----|
| 扁平（0 级） | 无阴影 | 默认——图钉依靠内容而非阴影 |
| 微妙（1 级） | 极轻阴影（来自令牌） | 浮层、下拉 |
| 焦点（无障碍） | `--sema-color-border-focus-outer-default` 环 | 焦点态 |

**阴影哲学**：Pinterest 几乎不用阴影。瀑布流网格依靠内容（摄影）创造视觉趣味，而非层级效果。深度来自表面色的暖度与容器的慷慨圆角。

## 7. 应该做与不应该做

### 应该做
- 用暖中性色（`#e5e5e0`、`#e0e0d9`、`#91918c`）——暖橄榄/沙色调就是品牌识别
- Pinterest Red（`#e60023`）只用于主 CTA——它大胆而唯一
- 只用 Pin Sans——一种字体包办一切
- 用慷慨的圆角：按钮/输入框 16px，卡片 20px+
- 保持瀑布流网格密集——内容密度就是价值
- 用暖徽章背景（`hsla(60,20%,98%,.5)`）营造微妙的暖渲染
- 主文本用 `#211922`（梅黑）——比纯黑更暖

### 不应该做
- 不用冷灰中性色——永远暖/橄榄调
- 主文本不用纯黑（`#000000`）——用梅黑（`#211922`）
- 按钮不用胶囊形——16px 圆角是圆润而非胶囊
- 不加厚重阴影——Pinterest 天生扁平，深度来自内容
- 卡片不用小圆角（<12px）——慷慨的圆角是核心
- 不引入额外的品牌色——红 + 暖中性色就是完整色板
- 不用细字重——Pin Sans 最低 400

## 8. 响应式行为

### 断点
| 名称 | 宽度 | 关键变化 |
|------|-------|-------------|
| 移动端 | <576px | 单列、紧凑布局 |
| 大屏手机 | 576–768px | 2 列图钉网格 |
| 平板 | 768–890px | 扩展网格 |
| 小桌面 | 890–1312px | 标准瀑布流网格 |
| 桌面 | 1312–1440px | 完整布局 |
| 大桌面 | 1440–1680px | 扩展网格列数 |
| 超宽 | >1680px | 最大网格密度 |

### 折叠策略
- 图钉网格：5+ 列 → 3 → 2 → 1
- 导航：搜索栏 + 图标 → 简化移动端导航
- 功能区块：左右并排 → 堆叠
- 首屏：70px → 按比例缩小
- 页脚：深色多列 → 堆叠

## 9. 智能体提示指南

### 快速色彩参考
- 品牌色：Pinterest Red（`#e60023`）
- 背景：White（`#ffffff`）
- 文本：Plum Black（`#211922`）
- 次级文本：Olive Gray（`#62625b`）
- 按钮表面：Sand Gray（`#e5e5e0`）
- 边框：Warm Silver（`#91918c`）
- 焦点：Focus Blue（`#435ee5`）

### 组件提示示例
- "Create a hero: white background. Headline at 70px Pin Sans weight 600, plum black (#211922). Red CTA button (#e60023, 16px radius, 6px 14px padding). Secondary sand button (#e5e5e0, 16px radius)."
- "Design a pin card: white background, 16px radius, no shadow. Photography fills top, 16px Pin Sans weight 400 description below in #62625b."
- "Build a circular action button: #e0e0d9 background, 50% radius, #211922 icon."
- "Create an input field: white background, 1px solid #91918c, 16px radius, 11px 15px padding. Focus: blue outline via semantic tokens."
- "Design the dark footer: #33332e background. Pinterest script logo in white. 12px Pin Sans links in #91918c."

### 迭代指南
1. 处处暖中性色——橄榄/沙灰，绝不用冷钢色
2. Pinterest Red 只用于 CTA——大胆而唯一
3. 按钮/输入框 16px 圆角，卡片 20px+——慷慨但不是胶囊形
4. Pin Sans 是唯一字体——UI 紧凑 12px，展示 70px
5. 摄影承载设计——UI 保持温暖极简
6. 文本用梅黑（#211922）——比纯黑更暖
