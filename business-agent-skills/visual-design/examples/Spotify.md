# 源自 Spotify 的设计系统

## 1. 视觉主题与氛围

Spotify 的网页界面是一个深色、沉浸式的音乐播放器，把听众包裹在近黑茧房（`#121212`、`#181818`、`#1f1f1f`）中，专辑封面和内容成为色彩的主要来源。设计哲学是"内容优先的黑暗"——UI 退入阴影，让音乐、播客和歌单发光。每个表面都是炭色调，营造剧场般的环境，唯一的真实色彩来自标志性的 Spotify Green（`#1ed760`）和专辑封面本身。

排印（Typography）用 SpotifyMixUI 和 SpotifyMixUITitle——源自 CircularSp 字体族（Lineto 的 Circular，为 Spotify 定制）的专有字体，配有包含阿拉伯语、希伯来语、西里尔语、希腊语、天城文和 CJK 字体的广泛回退栈，体现 Spotify 的全球覆盖。字体系统紧凑而功能化：700（粗体）用于强调和导航，600（半粗）用于次级强调，400（常规）用于正文。按钮用大写加正字间距（1.4px–2px），形成系统的、标签般的质感。

Spotify 的独特之处在于胶囊与圆形的几何。主按钮用 500px–9999px 圆角（全胶囊），圆形播放按钮用 50% 圆角，搜索输入框是 500px 胶囊。加上抬升元素上的厚重阴影（`rgba(0,0,0,0.5) 0px 8px 24px`）和独特的内嵌边框阴影组合（`rgb(18,18,18) 0px 1px 0px, rgb(124,124,124) 0px 0px 0px 1px inset`），最终呈现出像高端音频设备一样的界面——有触感、圆润、为触控而生。

**关键特征：**
- 近黑沉浸式深色主题（`#121212`–`#1f1f1f`）——UI 消失在内容背后
- Spotify Green（`#1ed760`）作为唯一品牌点缀——从不装饰，永远功能化
- SpotifyMixUI/CircularSp 字体族，全球文种支持
- 胶囊按钮（500px–9999px）与圆形控件（50%）——圆润、触控优化
- 大写按钮标签，宽字间距（1.4px–2px）
- 抬升元素上的厚重阴影（`rgba(0,0,0,0.5) 0px 8px 24px`）
- 语义色：负向红（`#f3727f`）、警告橙（`#ffa42b`）、公告蓝（`#539df5`）
- 专辑封面是主要色彩来源——UI 本身按设计是无彩的

## 2. 色彩体系与角色

### 主品牌色
- **Spotify Green**（`#1ed760`）：主品牌点缀——播放按钮、激活态、CTA
- **Near Black**（`#121212`）：最深背景表面
- **Dark Surface**（`#181818`）：卡片、容器、抬升表面
- **Mid Dark**（`#1f1f1f`）：按钮背景、交互表面

### 文本
- **White**（`#ffffff`）：`--text-base`，主文本
- **Silver**（`#b3b3b3`）：次级文本、弱化标签、非激活导航
- **Near White**（`#cbcbcb`）：稍亮的次级文本
- **Light**（`#fdfdfd`）：最大强调的近纯白

### 语义色
- **Negative Red**（`#f3727f`）：`--text-negative`，错误状态
- **Warning Orange**（`#ffa42b`）：`--text-warning`，警告状态
- **Announcement Blue**（`#539df5`）：`--text-announcement`，信息状态

### 表面与边框
- **Dark Card**（`#252525`）：抬升卡片表面
- **Mid Card**（`#272727`）：备用卡片表面
- **Border Gray**（`#4d4d4d`）：深色上的按钮边框
- **Light Border**（`#7c7c7c`）：描边按钮边框、弱化链接
- **Separator**（`#b3b3b3`）：分隔线
- **Light Surface**（`#eeeeee`）：浅色模式按钮（罕见）
- **Spotify Green Border**（`#1db954`）：绿色点缀边框变体

### 阴影
- **Heavy**（`rgba(0,0,0,0.5) 0px 8px 24px`）：对话框、菜单、抬升面板
- **Medium**（`rgba(0,0,0,0.3) 0px 8px 8px`）：卡片、下拉
- **Inset Border**（`rgb(18,18,18) 0px 1px 0px, rgb(124,124,124) 0px 0px 0px 1px inset`）：输入框边框阴影组合

## 3. 排印规则

### 字体族
- **标题**：`SpotifyMixUITitle`，回退：`CircularSp-Arab, CircularSp-Hebr, CircularSp-Cyrl, CircularSp-Grek, CircularSp-Deva, Helvetica Neue, helvetica, arial, Hiragino Sans, Hiragino Kaku Gothic ProN, Meiryo, MS Gothic`
- **UI/正文**：`SpotifyMixUI`，同样回退栈

### 层级

| 角色 | 字体 | 字号 | 字重 | 行高 | 字间距 | 备注 |
|------|------|------|--------|-------------|----------|-------|
| 章节标题 | SpotifyMixUITitle | 24px (1.50rem) | 700 | normal | normal | 粗标题字重 |
| 功能标题 | SpotifyMixUI | 18px (1.13rem) | 600 | 1.30（紧） | normal | 半粗章节头 |
| 正文粗体 | SpotifyMixUI | 16px (1.00rem) | 700 | normal | normal | 强调文本 |
| 正文 | SpotifyMixUI | 16px (1.00rem) | 400 | normal | normal | 标准正文 |
| 按钮大写 | SpotifyMixUI | 14px (0.88rem) | 600–700 | 1.00（紧） | 1.4px–2px | `text-transform: uppercase` |
| 按钮 | SpotifyMixUI | 14px (0.88rem) | 700 | normal | 0.14px | 标准按钮 |
| 导航链接粗体 | SpotifyMixUI | 14px (0.88rem) | 700 | normal | normal | 导航 |
| 导航链接 | SpotifyMixUI | 14px (0.88rem) | 400 | normal | normal | 非激活导航 |
| 说明粗体 | SpotifyMixUI | 14px (0.88rem) | 700 | 1.50–1.54 | normal | 粗元数据 |
| 说明文字 | SpotifyMixUI | 14px (0.88rem) | 400 | normal | normal | 元数据 |
| 小字粗体 | SpotifyMixUI | 12px (0.75rem) | 700 | 1.50 | normal | 标签、计数 |
| 小字 | SpotifyMixUI | 12px (0.75rem) | 400 | normal | normal | 细则 |
| 徽章 | SpotifyMixUI | 10.5px (0.66rem) | 600 | 1.33 | normal | `text-transform: capitalize` |
| 微字 | SpotifyMixUI | 10px (0.63rem) | 400 | normal | normal | 最小文本 |

### 原则
- **粗/常规二元**：大多数文本要么 700（粗体）要么 400（常规），600 少用。这通过字重对比而非字号变化形成清晰的视觉层级。
- **大写按钮即系统**：按钮标签用大写 + 宽字间距（1.4px–2px），形成与内容文本区分的系统"标签"语调。
- **紧凑字号**：范围 10px–24px——比大多数系统窄。Spotify 的字体紧凑而功能化，为浏览歌单而设计，而非读文章。
- **全球文种支持**：广泛的回退栈（阿拉伯语、希伯来语、西里尔语、希腊语、天城文、CJK）反映 Spotify 180+ 市场的覆盖。

## 4. 组件样式

### 按钮

**深色胶囊**
- 背景：`#1f1f1f`
- 文本：`#ffffff` 或 `#b3b3b3`
- 内边距：8px 16px
- 圆角：9999px（全胶囊）
- 用途：导航胶囊、次级操作

**深色大胶囊**
- 背景：`#181818`
- 文本：`#ffffff`
- 内边距：0px 43px
- 圆角：500px
- 用途：主应用导航按钮

**浅色胶囊**
- 背景：`#eeeeee`
- 文本：`#181818`
- 圆角：500px
- 用途：浅色模式 CTA（Cookie 同意、营销）

**描边胶囊**
- 背景：透明
- 文本：`#ffffff`
- 边框：`1px solid #7c7c7c`
- 内边距：4px 16px 4px 36px（图标非对称）
- 圆角：9999px
- 用途：关注按钮、次级操作

**圆形播放**
- 背景：`#1f1f1f`
- 文本：`#ffffff`
- 内边距：12px
- 圆角：50%（圆形）
- 用途：播放/暂停控制

### 卡片与容器
- 背景：`#181818` 或 `#1f1f1f`
- 圆角：6px–8px
- 大多数卡片无可见边框
- 悬停：背景轻微变亮
- 阴影：抬升时 `rgba(0,0,0,0.3) 0px 8px 8px`

### 输入框
- 搜索输入框：`#1f1f1f` 背景，`#ffffff` 文本
- 圆角：500px（胶囊）
- 内边距：12px 96px 12px 48px（图标感知）
- 焦点：边框变为 `#000000`，轮廓 `1px solid`

### 导航
- 深色侧边栏，激活态用 SpotifyMixUI 14px 字重 700，非激活 400
- 非激活项用 `#b3b3b3` 弱化色，激活用 `#ffffff`
- 圆形图标按钮（50% 圆角）
- 左上绿色 Spotify 标志

## 5. 布局原则

### 间距系统
- 基础单位：8px
- 阶梯：1px、2px、3px、4px、5px、6px、8px、10px、12px、14px、15px、16px、20px

### 网格与容器
- 侧边栏（固定）+ 主内容区
- 网格式专辑/歌单卡片
- 底部全宽正在播放栏
- 响应式内容区填充剩余空间

### 留白（Whitespace）哲学
- **深色压缩**：Spotify 内容排布密集——歌单网格、曲目列表、导航都间距紧凑。深色背景在元素间提供视觉休息，无需大间距。
- **内容密度优先于呼吸空间**：这是应用，不是营销站。每个像素都服务于聆听体验。

### 圆角阶梯
- 极小（2px）：徽章、显式标签
- 微妙（4px）：输入框、小元素
- 标准（6px）：专辑封面容器、卡片
- 舒适（8px）：区块、对话框
- 中（10px–20px）：面板、叠加元素
- 大（100px）：大胶囊按钮
- 胶囊（500px）：主按钮、搜索输入框
- 全胶囊（9999px）：导航胶囊、搜索
- 圆形（50%）：播放按钮、头像、图标

## 6. 深度与层级

| 层级 | 处理 | 用途 |
|-------|-----------|-----|
| 基础（0 级） | `#121212` 背景 | 最深层、页面背景 |
| 表面（1 级） | `#181818` 或 `#1f1f1f` | 卡片、侧边栏、容器 |
| 抬升（2 级） | `rgba(0,0,0,0.3) 0px 8px 8px` | 下拉菜单、悬停卡片 |
| 对话（3 级） | `rgba(0,0,0,0.5) 0px 8px 24px` | 弹窗、叠加、菜单 |
| 内嵌（边框） | `rgb(18,18,18) 0px 1px 0px, rgb(124,124,124) 0px 0px 0px 1px inset` | 输入框边框 |

**阴影哲学**：Spotify 作为深色主题应用，阴影明显厚重。0.5 不透明度、24px 模糊的阴影为对话框和菜单营造戏剧性的"漂浮于黑暗"效果，0.3 不透明度、8px 模糊则提供更微妙的卡片抬升。输入框上独特的内嵌边框阴影组合营造凹陷的触感。

## 7. 应该做与不应该做

### 应该做
- 用近黑背景（`#121212`–`#1f1f1f`）——通过色调变化表现深度
- Spotify Green（`#1ed760`）只用于播放控制、激活态和主 CTA
- 所有按钮用胶囊形（500px–9999px）——播放控制用圆形（50%）
- 按钮标签用大写 + 宽字间距（1.4px–2px）
- 排印保持紧凑（10px–24px 范围）——这是应用，不是杂志
- 深色背景上的抬升元素用厚重阴影（0.3–0.5 不透明度）
- 让专辑封面提供色彩——UI 本身是无彩的

### 不应该做
- Spotify Green 不作装饰或背景用——只功能化
- 主表面不用浅色背景——深色沉浸是核心
- 按钮不跳过胶囊/圆形几何——方形按钮破坏品牌识别
- 不用细/微妙阴影——深色背景上阴影必须厚重才可见
- 不加额外的品牌色——绿 + 无彩灰就是完整色板
- 不用放松的行高——Spotify 的排印紧凑而密集
- 不暴露原始灰边框——用基于阴影或内嵌的边框代替

## 8. 响应式行为

### 断点
| 名称 | 宽度 | 关键变化 |
|------|-------|-------------|
| 小手机 | <425px | 紧凑移动端布局 |
| 手机 | 425–576px | 标准移动端 |
| 平板 | 576–768px | 2 列网格 |
| 大平板 | 768–896px | 扩展布局 |
| 小桌面 | 896–1024px | 侧边栏可见 |
| 桌面 | 1024–1280px | 完整桌面布局 |
| 大桌面 | >1280px | 扩展网格 |

### 折叠策略
- 侧边栏：完整 → 折叠 → 隐藏
- 专辑网格：5 列 → 3 → 2 → 1
- 正在播放栏：所有尺寸保持
- 搜索：胶囊输入框保持，宽度调整
- 导航：移动端侧边栏 → 底部栏

## 9. 智能体提示指南

### 快速色彩参考
- 背景：Near Black（`#121212`）
- 表面：Dark Card（`#181818`）
- 文本：White（`#ffffff`）
- 次级文本：Silver（`#b3b3b3`）
- 点缀：Spotify Green（`#1ed760`）
- 边框：`#4d4d4d`
- 错误：Negative Red（`#f3727f`）

### 组件提示示例
- "Create a dark card: #181818 background, 8px radius. Title at 16px SpotifyMixUI weight 700, white text. Subtitle at 14px weight 400, #b3b3b3. Shadow rgba(0,0,0,0.3) 0px 8px 8px on hover."
- "Design a pill button: #1f1f1f background, white text, 9999px radius, 8px 16px padding. 14px SpotifyMixUI weight 700, uppercase, letter-spacing 1.4px."
- "Build a circular play button: Spotify Green (#1ed760) background, #000000 icon, 50% radius, 12px padding."
- "Create search input: #1f1f1f background, white text, 500px radius, 12px 48px padding. Inset border: rgb(124,124,124) 0px 0px 0px 1px inset."
- "Design navigation sidebar: #121212 background. Active items: 14px weight 700, white. Inactive: 14px weight 400, #b3b3b3."

### 迭代指南
1. 从 #121212 开始——一切都生活在近黑黑暗中
2. Spotify Green 只用于功能高亮（播放、激活、CTA）
3. 全部胶囊化——大 500px，小 9999px，圆形 50%
4. 按钮大写 + 宽字距——系统的标签语调
5. 抬升用厚重阴影（0.3–0.5 不透明度）——浅阴影在深色上不可见
6. 专辑封面提供所有色彩——UI 保持无彩
