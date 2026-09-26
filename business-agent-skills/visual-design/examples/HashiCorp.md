# HashiCorp 灵感设计系统

## 1. 视觉主题与氛围

HashiCorp 的网站是具象化的企业基础设施——一套必须传达云基础设施管理复杂性、同时保持亲和的设计系统。视觉语言在两种模式间切换：信息区块用干净的白色浅色模式，首屏区与产品展示用戏剧性的深色模式（`#15181e`、`#0d0e12`），形成昼夜二元性，映照"在光明中构建、在黑暗中部署"的开发者工作流。

排印锚定于定制品牌字体（HashiCorp Sans，加载为 `__hashicorpSans_96f0ca`），它承载着实在的分量——字面意义上的。标题用 600–700 字重（font weight），行高（line-height）紧凑（1.17–1.19），形成密集、权威的文字块，传达企业自信。82px 字重 600、启用 OpenType `"kern"` 的首屏（hero）标题不是装饰——它是基础设施级的排印。

HashiCorp 的独特之处是多产品色彩体系。组合中的每个产品都有自己的品牌色——Terraform 紫（`#7b42bc`）、Vault 黄（`#ffcf25`）、Waypoint 青（`#14c6cb`）、Vagrant 蓝（`#1868f2`）——这些颜色经由 CSS 自定义属性（custom property）系统（`--mds-color-*`）作为点缀令牌（token）贯穿全站。这形成系统中的系统：父品牌是黑白配蓝色点缀，每个子产品注入自己的色彩身份。

组件系统用 `mds`（Markdown Design System）前缀，表明系统化、令牌驱动的方法：颜色、间距、状态都通过 CSS 变量管理。阴影出奇地微妙——用 `rgba(97, 104, 117, 0.05)` 的双层微阴影，几乎不可见，但刚好给交互表面与背景之间一点纵深分隔。

**关键特征：**
- 双模式：干净白色区块 + 戏剧性深色（`#15181e`）首屏/产品区
- 定制 HashiCorp Sans 字体，600–700 字重，`"kern"` 特性
- 经由 `--mds-color-*` CSS 自定义属性的多产品色彩体系
- 产品品牌色：Terraform 紫、Vault 黄、Waypoint 青、Vagrant 蓝
- 大写字距标签（13px，字重 600，1.3px 字间距（letter-spacing））
- 微阴影：0.05 透明度的双层——纵深靠耳语，不靠呐喊
- 令牌驱动的 `mds` 组件系统，语义化变量名
- 紧凑圆角（border-radius）：2px–8px，无胶囊形或圆形
- 次级文字用系统 UI 回退栈

## 2. 色板与角色

### 品牌主色
- **黑（Black）** (`#000000`)：主品牌色，浅色表面上的文字，`--mds-color-hcp-brand`
- **深炭（Dark Charcoal）** (`#15181e`)：深色模式背景，首屏区块
- **近黑（Near Black）** (`#0d0e12`)：最深的深色模式表面，深色上的表单输入

### 中性色阶
- **浅灰（Light Gray）** (`#f1f2f3`)：浅色背景，微妙表面
- **中灰（Mid Gray）** (`#d5d7db`)：边框，深色上的按钮文字
- **冷灰（Cool Gray）** (`#b2b6bd`)：边框点缀（0.1–0.4 透明度）
- **深灰（Dark Gray）** (`#656a76`)：辅助文字，次级标签，`--mds-form-helper-text-color`
- **炭灰（Charcoal）** (`#3b3d45`)：浅色上的次级文字，按钮边框
- **近白（Near White）** (`#efeff1`)：深色表面上的主文字

### 产品品牌色
- **Terraform 紫（Terraform Purple）** (`#7b42bc`)：`--mds-color-terraform-button-background`
- **Vault 黄（Vault Yellow）** (`#ffcf25`)：`--mds-color-vault-button-background`
- **Waypoint 青（Waypoint Teal）** (`#14c6cb`)：`--mds-color-waypoint-button-background-focus`
- **Waypoint 青悬停（Waypoint Teal Hover）** (`#12b6bb`)：`--mds-color-waypoint-button-background-hover`
- **Vagrant 蓝（Vagrant Blue）** (`#1868f2`)：`--mds-color-vagrant-brand`
- **紫点缀（Purple Accent）** (`#911ced`)：`--mds-color-palette-purple-300`
- **访问紫（Visited Purple）** (`#a737ff`)：`--mds-color-foreground-action-visited`

### 语义色
- **动作蓝（Action Blue）** (`#1060ff`)：深色上的主动作链接
- **链接蓝（Link Blue）** (`#2264d6`)：浅色上的主链接
- **亮蓝（Bright Blue）** (`#2b89ff`)：当前链接、悬停点缀
- **琥珀（Amber）** (`#bb5a00`)：`--mds-color-palette-amber-200`，警告状态
- **浅琥珀（Amber Light）** (`#fbeabf`)：`--mds-color-palette-amber-100`，警告背景
- **Vault 淡黄（Vault Faint Yellow）** (`#fff9cf`)：`--mds-color-vault-radar-gradient-faint-stop`
- **橙（Orange）** (`#a9722e`)：`--mds-color-unified-core-orange-6`
- **红（Red）** (`#731e25`)：`--mds-color-unified-core-red-7`，错误状态
- **藏青（Navy）** (`#101a59`)：`--mds-color-unified-core-blue-7`

### 阴影
- **微阴影（Micro Shadow）** (`rgba(97, 104, 117, 0.05) 0px 1px 1px, rgba(97, 104, 117, 0.05) 0px 2px 2px`)：默认卡片/按钮抬升
- **焦点轮廓（Focus Outline）**：`3px solid var(--mds-color-focus-action-external)`——系统化焦点环

## 3. 排印规则

### 字体家族
- **主品牌**：`__hashicorpSans_96f0ca`（HashiCorp Sans），回退：`__hashicorpSans_Fallback_96f0ca`
- **系统 UI**：`system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, Helvetica, Arial`

### 层级

| 角色（Role） | 字体（Font） | 字号（Size） | 字重（Weight） | 行高（Line Height） | 字间距（Letter Spacing） | 说明（Notes） |
|------|------|------|--------|-------------|----------------|-------|
| 展示首屏 | HashiCorp Sans | 82px (5.13rem) | 600 | 1.17（紧） | normal | 启用 `"kern"` |
| 区块标题 | HashiCorp Sans | 52px (3.25rem) | 600 | 1.19（紧） | normal | 启用 `"kern"` |
| 特性标题 | HashiCorp Sans | 42px (2.63rem) | 700 | 1.19（紧） | -0.42px | 负字距 |
| 副标题 | HashiCorp Sans | 34px (2.13rem) | 600–700 | 1.18（紧） | normal | 特性块 |
| 卡片标题 | HashiCorp Sans | 26px (1.63rem) | 700 | 1.19（紧） | normal | 卡片与面板标题 |
| 小标题 | HashiCorp Sans | 19px (1.19rem) | 700 | 1.21（紧） | normal | 紧凑标题 |
| 正文强调 | HashiCorp Sans | 17px (1.06rem) | 600–700 | 1.18–1.35 | normal | 粗体正文 |
| 正文大 | system-ui | 20px (1.25rem) | 400–600 | 1.50 | normal | 首屏描述 |
| 正文 | system-ui | 16px (1.00rem) | 400–500 | 1.63–1.69（宽松） | normal | 标准正文 |
| 导航链接 | system-ui | 15px (0.94rem) | 500 | 1.60（宽松） | normal | 导航项 |
| 正文小 | system-ui | 14px (0.88rem) | 400–500 | 1.29–1.71 | normal | 次级内容 |
| 图注 | system-ui | 13px (0.81rem) | 400–500 | 1.23–1.69 | normal | 元数据、页脚链接 |
| 大写标签 | HashiCorp Sans | 13px (0.81rem) | 600 | 1.69（宽松） | 1.3px | `text-transform: uppercase` |

### 原则
- **品牌/系统分工**：HashiCorp Sans 用于标题与品牌关键文字；system-ui 用于正文、导航与功能文字。品牌字体承载分量，system-ui 承载言辞。
- **字距调整永远开**：所有 HashiCorp Sans 文字启用 OpenType `"kern"`——字母装配（letterfitting）不可妥协。
- **紧凑标题**：每个标题都用 1.17–1.21 行高，形成密集堆叠的文字块，感觉像基础设施——坚实、承重。
- **宽松正文**：正文用 1.50–1.69 行高（明显慷慨），在密集标题下形成舒适的阅读节奏。
- **大写标签作路标**：13px 大写配 1.3px 字间距，是系统化的分类/区块标记——永远是 HashiCorp Sans 字重 600。

## 4. 组件样式

### 按钮

**主深色（Primary Dark）**
- 背景：`#15181e`
- 文字：`#d5d7db`
- 内边距（padding）：9px 9px 9px 15px（不对称，左侧更多）
- 圆角：5px
- 边框：`1px solid rgba(178, 182, 189, 0.4)`
- 阴影：`rgba(97, 104, 117, 0.05) 0px 1px 1px, rgba(97, 104, 117, 0.05) 0px 2px 2px`
- 焦点：`3px solid var(--mds-color-focus-action-external)`
- 悬停：用 `--mds-color-surface-interactive` 令牌

**次白色（Secondary White）**
- 背景：`#ffffff`
- 文字：`#3b3d45`
- 内边距：8px 12px
- 圆角：4px
- 悬停：`--mds-color-surface-interactive` + 低阴影抬升
- 焦点：`3px solid transparent` 轮廓
- 干净、极简的外观

**产品色按钮（Product-Colored Buttons）**
- Terraform：背景 `#7b42bc`
- Vault：背景 `#ffcf25`（深色文字）
- Waypoint：背景 `#14c6cb`，悬停 `#12b6bb`
- 每个产品按钮结构模式相同，但用各自的品牌色

### 徽章 / 标签 / 胶囊
- 背景：`#42225b`（深紫）
- 文字：`#efeff1`
- 内边距：3px 7px
- 圆角：5px
- 边框：`1px solid rgb(180, 87, 255)`
- 字体：16px

### 输入框

**文本输入框（深色模式）（Text Input (Dark Mode)）**
- 背景：`#0d0e12`
- 文字：`#efeff1`
- 边框：`1px solid rgb(97, 104, 117)`
- 内边距：11px
- 圆角：5px
- 焦点：`3px solid var(--mds-color-focus-action-external)` 轮廓

**复选框（Checkbox）**
- 背景：`#0d0e12`
- 边框：`1px solid rgb(97, 104, 117)`
- 圆角：3px

### 链接
- **浅色上的动作蓝**：`#2264d6`，悬停 → blue-600 变量，悬停下划线
- **深色上的动作蓝**：`#1060ff` 或 `#2b89ff`，悬停下划线
- **深色上的白色**：`#ffffff`，透明下划线 → 悬停显现下划线
- **浅色上的中性**：`#3b3d45`，透明下划线 → 悬停显现下划线
- **深色上的浅色**：`#efeff1`，类似悬停模式
- 所有链接悬停色用 `var(--wpl-blue-600)`

### 卡片与容器
- 浅色模式：白底，微阴影抬升
- 深色模式：`#15181e` 或更深的表面
- 圆角：卡片与容器 8px
- 产品展示卡片带渐变边框或点缀光

### 导航
- 干净的横排导航，带巨型菜单下拉
- HashiCorp 标志左对齐
- 链接用 system-ui 15px 字重 500
- 产品分类按生命周期管理分组
- 页眉有"Get started"与"Contact us" CTA
- 首屏区块的深色模式变体

## 5. 布局原则

### 间距系统
- 基础单位：8px
- 刻度：2px、3px、4px、6px、7px、8px、9px、11px、12px、16px、20px、24px、32px、40px、48px

### 网格与容器
- 最大内容宽度：~1150px（xl 断点）
- 全宽深色首屏区块，内容收纳在内
- 卡片网格：2–3 列布局
- 桌面尺度下慷慨的横向内边距

### 断点
| 名称（Name） | 宽度（Width） | 关键变化（Key Changes） |
|------|-------|-------------|
| 小手机 | <375px | 紧凑单列 |
| 手机 | 375–480px | 标准移动端 |
| 小平板 | 480–600px | 微调 |
| 平板 | 600–768px | 2 列网格开始 |
| 小桌面 | 768–992px | 完整导航可见 |
| 桌面 | 992–1120px | 标准布局 |
| 大桌面 | 1120–1440px | 最大宽度内容 |
| 超宽 | >1440px | 居中，边距慷慨 |

### 留白哲学
- **企业级呼吸空间**：区块间慷慨的垂直间距（48px–80px+）传达稳定与严肃。
- **标题密集、正文宽松**：紧行高标题坐在宽松正文之上，在每个区块顶部形成视觉"重心在上"。
- **深色即画布**：深色首屏区块用额外垂直内边距，让 3D 插画与渐变呼吸。

### 圆角刻度
- 极小（2px）：链接、小内联元素
- 微妙（3px）：复选框、小输入框
- 标准（4px）：次级按钮
- 舒适（5px）：主按钮、徽章、输入框
- 卡片（8px）：卡片、容器、图片

## 6. 纵深与阴影层级

| 层级（Level） | 处理（Treatment） | 用途（Use） |
|-------|-----------|-----|
| 扁平（0 级） | 无阴影 | 默认表面、文字块 |
| 耳语（1 级） | `rgba(97, 104, 117, 0.05) 0px 1px 1px, rgba(97, 104, 117, 0.05) 0px 2px 2px` | 卡片、按钮、交互表面 |
| 焦点（2 级） | `3px solid var(--mds-color-focus-action-external)` 轮廓 | 焦点环——颜色匹配上下文 |

**阴影哲学**：HashiCorp 用的是现代网页设计中最微妙的阴影体系。5% 透明度的双层阴影几乎不可见——它们存在的目的不是制造视觉纵深，而是发出交互信号。如果你能看见阴影，它就太强了。这种克制传达了企业的稳定价值——没有漂浮，没有不确定。

## 7. 要做与不要做

### 要做
- 标题与品牌文字用 HashiCorp Sans，正文与 UI 文字用 system-ui
- 所有 HashiCorp Sans 文字启用 `"kern"`
- 产品品牌色只用于各自产品（Terraform = 紫、Vault = 黄等）
- 区块标记用大写标签：13px 字重 600，1.3px 字间距
- 阴影保持"耳语"级（0.05 透明度双层）
- 用 `--mds-color-*` 令牌体系保证颜色应用一致
- 保持紧标题 / 松正文的节奏（行高 1.17–1.21 vs 1.50–1.69）
- 无障碍用 `3px solid` 焦点轮廓

### 不要做
- 产品品牌色不要跨产品使用（Vault 内容上不用 Terraform 紫）
- 阴影透明度不要超过 0.1——耳语级是刻意的
- 不要用胶囊形按钮（>8px 圆角）——锐利、极简的圆角是结构性的
- 标题不要跳过 `"kern"` 特性——字体需要它
- 小正文不要用 HashiCorp Sans——它是为 17px+ 标题设计的
- 同一组件不要混用产品色——每个产品一种颜色
- 深色背景不要用纯黑（`#000000`）——用 `#15181e` 或 `#0d0e12`
- 不要忘了按钮的不对称内边距——9px 9px 9px 15px 是刻意的

## 8. 响应式行为

### 断点
| 名称（Name） | 宽度（Width） | 关键变化（Key Changes） |
|------|-------|-------------|
| 移动 | <768px | 单列，汉堡导航，CTA 堆叠 |
| 平板 | 768–992px | 2 列网格，导航开始展开 |
| 桌面 | 992–1150px | 完整布局，巨型菜单导航 |
| 大屏 | >1150px | 最大宽度居中，边距慷慨 |

### 收起策略
- 首屏：82px → 52px → 42px 标题字号
- 导航：巨型菜单 → 汉堡
- 产品卡片：3 列 → 2 列 → 堆叠
- 深色区块保持全宽但压缩内边距
- 按钮：移动端内联 → 全宽堆叠

## 9. Agent 提示词指南

### 快速色彩参考
- 浅色背景：`#ffffff`、`#f1f2f3`
- 深色背景：`#15181e`、`#0d0e12`
- 浅色文字：`#000000`、`#3b3d45`
- 深色文字：`#efeff1`、`#d5d7db`
- 链接：`#2264d6`（浅色）、`#1060ff`（深色）、`#2b89ff`（当前）
- 辅助文字：`#656a76`
- 边框：`rgba(178, 182, 189, 0.4)`、`rgb(97, 104, 117)`
- 焦点：`3px solid` 产品适配色

### 示例组件提示词
- 「在深色背景（#15181e）上创建首屏。标题 82px HashiCorp Sans 字重 600，行高 1.17，字距调整启用，白色文字。副文字 20px system-ui 字重 400，行高 1.50，#d5d7db 文字。两个按钮：主深色（#15181e，5px 圆角，9px 15px 内边距）与次白色（#ffffff，4px 圆角，8px 12px 内边距）。」
- 「设计产品卡片：白底，8px 圆角，rgba(97,104,117,0.05) 双层阴影。标题 26px HashiCorp Sans 字重 700，正文 16px system-ui 字重 400 行高 1.63。」
- 「构建大写区块标签：13px HashiCorp Sans 字重 600，行高 1.69，字间距 1.3px，大写变换（text-transform uppercase），#656a76 颜色。」
- 「创建产品专属 CTA 按钮：Terraform → #7b42bc 背景，Vault → #ffcf25 配深色文字，Waypoint → #14c6cb。统一：5px 圆角，500 字重文字，16px system-ui。」
- 「设计深色表单：#0d0e12 输入框背景，#efeff1 文字，1px solid rgb(97,104,117) 边框，5px 圆角，11px 内边距。焦点：3px solid 点缀色轮廓。」

### 迭代指南
1. 永远先做模式决策：信息用浅色（白），首屏/产品用深色（#15181e）
2. HashiCorp Sans 只用于标题（17px+），其他一律 system-ui
3. 阴影在耳语级（0.05 透明度）——看得见就减弱
4. 产品色是神圣的——每个产品恰好拥有一种颜色
5. 焦点环永远是 3px 实线，颜色匹配产品上下文
6. 大写标签是系统化的路标模式——13px、600、1.3px 字距
