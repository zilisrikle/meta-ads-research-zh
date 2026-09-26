# Meta 广告系列结构与命名

## 目录

- [1. 推荐的广告系列结构](#1-推荐的广告系列结构)
- [2. 预算分配](#2-预算分配)
- [3. 全漏斗策略](#3-全漏斗策略)
- [4. 学习期管理](#4-学习期管理)
- [5. 衡量与优化](#5-衡量与优化)
- [6. 常见错误](#6-常见错误)
- [7. 按业务类型的推荐设计](#7-按业务类型的推荐设计)
- [8. 命名规范](#8-命名规范)

---

## 1. 推荐的广告系列结构

### 核心原则：结构保持简单

现代 Meta 广告通常受益于更简单的广告系列结构。更少的广告系列和广告组能集中投放信号、减少重叠，给系统更大的学习空间。

### 模式 A：双广告系列结构

当账户需要清晰的"测试-放量"运营闭环时用。

```
Ad account
├── 1. Creative Testing Campaign
│   ├── Budget: 10-20% of total
│   ├── Role: Discover new creative concepts
│   ├── Settings: campaign budget, broad targeting
│   └── Ad set: consolidated ad set with multiple concepts
│
└── 2. Winning Ads Campaign
    ├── Budget: 80-90% of total
    ├── Role: Scale validated creative concepts
    ├── Settings: campaign budget, broad targeting
    └── Ad set: consolidated winners
```

### 模式 B：三广告系列全漏斗结构

当认知、转化承接和再营销需要独立的预算或信息时用。

```
Ad account
├── 1. Sales Campaign
│   ├── Budget allocation: largest
│   ├── Role: Conversion capture across the funnel
│   ├── Settings: Advantage+ sales campaign or campaign budget
│   └── Ad set: proven creative concepts
│
├── 2. Awareness Campaign
│   ├── Budget allocation: medium
│   ├── Role: Reach new users
│   ├── Optimization: reach, impressions, or ThruPlay
│   └── Ad set: video-led content
│
└── 3. Remarketing Campaign
    ├── Budget allocation: small
    ├── Role: Convert people with prior touchpoints
    ├── Settings: ad set budget when tighter control is needed
    └── Ad set: Custom Audience segments
```

### 再营销的处理

Meta 的自动化投放常在更宽的广告系列内部顺带处理部分再营销。

- Advantage+ 销售广告系列可以在拓客和类再营销机会之间分配花费。
- 账户较小时，很多广告主把独立再营销合并进主广告系列。
- 当某个自定义受众（Custom Audience）需要独立的信息、预算或衡量视角时，保留单独的再营销广告系列。

---

## 2. 预算分配

### 漏斗分配

| 阶段 | 预算占比 | 角色 |
|---|---:|---|
| 认知 / 考虑 | 60% | 新客获取与商机池建设 |
| 转化 | 40% | 直接转化与营收 |

这是起点，不是规则。预算有限的效果型账户可能先把更多预算放到最干净的转化闭环。

### 测试 vs 放量

| 用途 | 预算占比 |
|---|---:|
| 创意测试 | 10–20% |
| 放量已验证创意 | 80–90% |

### 预算放量规则

- CAC/CPA 可接受时逐步加预算。**每次 20–30%** 是保守的节奏经验，不是平台通用定律。
- 预算大跳会破坏学习。
- 按**概念和创意体系**放量，不只按单条广告放量。
- 有意义的预算调整后，观察几天表现。

---

## 3. 全漏斗策略

### 阶段 1：认知

| 项目 | 设置 |
|---|---|
| 广告目标 | Awareness |
| 定向 | 宽泛受众 |
| 创意 | 视频/Reels、品牌故事、问题 framing |
| 优化 | 触达、展示或 ThruPlay |
| KPI | CPM、触达、视频播放率、有条件时看广告记忆度提升（ad recall lift） |

### 阶段 2：考虑

| 项目 | 设置 |
|---|---|
| 广告目标 | Traffic / Engagement |
| 定向 | 视频观看者、互动受众、温受众 |
| 创意 | 轮播、视频、证言、对比 |
| 优化 | 落地页浏览、视频播放、互动 |
| KPI | CTR、落地页浏览、互动率 |

### 阶段 3：转化

| 项目 | 设置 |
|---|---|
| 广告目标 | Sales / Leads |
| 定向 | 网站访客、加购未购、线索、宽泛转化受众 |
| 创意 | 动态广告、精品栏广告、卖点主导素材、证明 |
| 优化 | Purchase、Lead 或其他最深层可靠事件 |
| KPI | CPA、ROAS、转化量、CVR |

### 排除

- 以获客为目标的纯拓客中，排除老客。
- 只有在漏斗设计要求严格的阶段隔离时，才排除近期互动者。
- 当排除导致投放吃不饱或与 Advantage+ 投放冲突时，避免过度排除。

---

## 4. 学习期管理

### 学习期是什么

学习期是 Meta 投放系统针对所选优化事件，探索受众、版位和出价的初始阶段。

### 要求

- Meta 通常以每个广告组每周约 50 个优化事件作为稳定学习的实用基准。把它当作规划基准，不是硬性的通过/失败线。
- 学习期内表现可能波动。
- 学习期内避免大改，除非有明确的搭建错误。

### 会重启学习的改动

- 定向改动。
- 出价策略改动。
- 大幅预算改动。
- 暂停或重启广告。
- 以实质改变投放的方式新增广告。
- 改广告创意或优化事件。

### 如何帮助学习

- 给够预算，匹配目标 CPA 和预期转化量。
- 选发生频率足够的优化事件。
- 转化量太低时，只作临时代理考虑上层漏斗事件，并说明质量代价。
- 先合并再考虑浅层事件。更多低质量事件并不能改善学习。

---

## 5. 衡量与优化

### 核心 KPI

| KPI | 用途 | 说明 |
|---|---|---|
| CPA | 获客效率 | 从单位经济效益计算 |
| ROAS | 营收效率 | 必须结合利润率解读 |
| CTR | 创意吸引力与相关性 | 不要孤立优化 |
| CPM | 触达成本 | 因行业、受众、季节而异 |
| 千人触达成本 | 受众触达成本与疲劳信号 | 上升可能意味着疲劳或竞价压力 |
| 频次（Frequency） | 广告疲劳信号 | 高频次要看场景；再营销可容忍更高 |
| CVR | 落地页/漏斗效率 | 常是落地页问题，不只是广告问题 |
| LTV:CAC | 长期盈利性 | 订阅和复购模式有用 |

### 优化动作

| 症状 | 动作 |
|---|---|
| 千人触达成本上升 | 改出价前先刷新创意 |
| 频次超过账户容忍度 | 加新创意、放宽受众，或给再营销预算设上限 |
| CTR 下降 | 重做钩子和创意概念 |
| CPA 上升 | 按创意、落地页、卖点、受众、衡量的顺序诊断 |
| 落地页 CVR 下降 | 优化落地页或漏斗；不要只当广告问题处理 |

### 归因设置

| 窗口 | 含义 | 适用场景 |
|---|---|---|
| 7 天点击 + 1 天互动归因 + 1 天浏览 | 多数账户的常见现代默认（如可用） | 报告拆分可见即可 |
| 7 天点击 | 强调点击的优化/报告口径 | 浏览/互动虚高令人担忧的标准购买路径 |
| 1 天点击 | 更严格的点击口径 | 测试、低考虑度产品、增量敏感分析 |
| 1 天互动归因 | 非链接互动或合格视频互动后的转化 | 视频/Reels 与社交互动的影响；单独报告 |
| 1 天浏览 | 仅展示后的转化 | 单独监控；再营销和高频次广告系列谨慎使用 |
| 更长的点击窗口（如可用） | 覆盖更长的考虑期 | 高客单、B2B、长销售周期 |

注：报告工具与 API 行为随时间变化。在依赖任何特定归因窗口或互动分类前，先按用户技术栈核实现行 Ads Manager/API 行为。

---

## 6. 常见错误

### 结构错误

- 广告系列太多：信号碎片化在小流量池里。
- 受众重叠：账户自己跟自己竞价。
- 频繁改结构：学习永远稳定不下来。
- 独立再营销预算太大：平台 ROAS 好看，增量贡献弱。

### 创意错误

- 一个格式用到疲劳才换。
- 凭观点而不是测试数据做决策。
- 创意不适配版位的宽高比、节奏或界面。

### 搭建错误

- 需要按转化优化却没装 Meta Pixel / 转化 API。
- 信号或创意质量不够就用 Advantage+ 销售广告系列。
- 预算剧烈调整。
- 不审核输出就过度信任自动化创意功能。
- 把详细定向排除、旧兴趣堆叠或老版 Advantage+ 购物控制项当作仍稳定的产品机制。

---

## 7. 按业务类型的推荐设计

### 电商 / D2C

| 项目 | 建议 |
|---|---|
| 广告系列目标 | **Sales**；就绪后用 Advantage+ 销售广告系列 |
| 结构 | 模式 A：测试 + 放量 |
| 优化事件 | Purchase；购买量不够时才用 AddToCart |
| 格式 | 精品栏广告、轮播、动态广告、视频 |
| 版位 | Advantage+ 版位；优先备好适配版位的 Instagram 信息流/Reels 创意 |
| 定向 | 宽泛 + 高质量自定义受众信号；类似受众（Lookalike）只作已验证的非核心测试 |
| 出价策略 | 最高量（Highest volume）→ ROAS 目标 |
| 预算分配 | 测试 10–20% / 放量 80–90% |
| 衡量注意 | 跟踪新客 vs 老客、利润率与增量 |

### B2B 线索型

| 项目 | 建议 |
|---|---|
| 广告系列目标 | **Leads** 或 **Sales** |
| 结构 | 模式 B：Sales/Leads + 认知 + 再营销 |
| 优化事件 | Lead、合格线索、表单提交或 CRM 回传事件 |
| 格式 | 线索广告 / 即时表单、视频、图片 |
| 版位 | Facebook 信息流 + Instagram 信息流；考虑排除 Audience Network |
| 定向 | 宽泛 + 客户名单/自定义受众信号 + 量支持时的网站访客再营销 |
| 出价策略 | 量稳定后用单次转化费用目标（Cost per result goal） |
| 说明 | 即时表单和网站转化要对比测试线索质量 |
| 衡量注意 | 没有合格线索或商机反馈就不要优化原始 CPL |

### SaaS / 订阅

| 项目 | 建议 |
|---|---|
| 广告系列目标 | **Leads** → **Sales** |
| 结构 | 模式 B |
| 优化事件 | 演示预约、注册、试用开始、订阅 |
| 格式 | 演示视频、功能轮播、图片 |
| 版位 | Facebook 信息流 + Instagram 信息流 |
| 定向 | 宽泛 + 网站访客、视频观看者等自定义受众 |
| 出价策略 | 最高量 → 单次转化费用目标 |
| 说明 | 免费试用或演示卖点常有效，但要检查质量 |

### 本地商户

| 项目 | 建议 |
|---|---|
| 广告系列目标 | **Awareness** + **Sales** 或 **Leads** |
| 结构 | 模式 B：认知 + Sales/Leads + 再营销 |
| 优化事件 | 认知用触达；转化用 Purchase、Lead、预约或来电 |
| 格式 | 门店巡礼视频、优惠图片、轮播 |
| 版位 | Facebook 信息流 + Marketplace + 适用时加 Instagram 信息流 |
| 定向 | 本地地域 + 宽泛受众 |
| 出价策略 | 最高量 |
| 说明 | 服务区域和本地证明很重要 |

### App 业务

| 项目 | 建议 |
|---|---|
| 广告系列目标 | **App promotion** |
| 结构 | 模式 A：测试 + 放量 |
| 优化事件 | 安装 → 应用内事件 → 价值 |
| 格式 | App 演示视频、可玩广告 |
| 版位 | Advantage+ 版位 |
| 定向 | 宽泛 + 适用时用高价值用户自定义受众信号；用类似受众作控制项前先核实可用性 |
| 出价策略 | 最高量 → 最高价值（Highest value） |
| 说明 | 安装和事件数据积累够后，往更高价值事件走 |

### 品牌认知

| 项目 | 建议 |
|---|---|
| 广告系列目标 | **Awareness** |
| 结构 | 单个认知广告系列 |
| 优化事件 | 触达、展示、广告记忆度提升、ThruPlay |
| 格式 | 视频/Reels、图片 |
| 版位 | Advantage+ 版位；备好 Reels 和 Stories 创意 |
| 定向 | 宽泛，最大化触达 |
| 出价策略 | 最高量 |
| 说明 | 用视频主导的品牌叙事 |

---

## 8. 命名规范

### 广告系列名

格式：`{Objective}_{AudienceOrFunnel}_{Structure}_{Note}`

示例：

- `Sales_Prospecting_ASC`
- `Sales_Retargeting_CartAbandoners`
- `Awareness_TOF_CBO`
- `Leads_InstantForm_US`
- `CreativeTest_Weekly`

### 广告组名

格式：`{Audience}_{Placement}_{Note}`

示例：

- `Broad_AllPlacements`
- `LAL1pct_Purchase_Feed`
- `Retargeting_WebVisitors30d`
- `CustomAudience_EmailList`

### 广告名

格式：`{Format}_{Angle}_{Variant}`

示例：

- `Video_Testimonial_v1`
- `Image_PainPoint_4x5`
- `Carousel_ProductLine_v2`
- `UGC_FounderStory_Reel`

### 规则

- 统一用下划线 `_`。
- 优先英文命名，兼容筛选器与导出。
- 除非是测试，否则不加日期；需要时追加 `_Test_YYMM`。
- 可读性用 PascalCase 分词。
