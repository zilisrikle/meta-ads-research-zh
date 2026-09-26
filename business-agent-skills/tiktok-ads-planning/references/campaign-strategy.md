# 广告系列策略

用这份参考处理 TikTok Ads Manager 的结构、目标、定向、出价、自动化、学习期、放量和衡量。

## 广告系列结构

TikTok Ads Manager 采用三层结构：

```text
Campaign（广告系列）
  -> Ad Group（广告组）
      -> Ad（广告）
```

| 层级 | 主要决策 |
|---|---|
| 广告系列 | 目标、预算类型、广告系列名称、广告系列层级预算 / Campaign Budget Optimization（视可用性而定） |
| 广告组 | 优化事件、版位、定向、出价策略、预算、排期、归因设置 |
| 广告 | 身份、创意、文案、CTA、落地页、商品链接、追踪 |

除非有真实理由按目标、市场、转化事件、产品经济模型、受众、创意测试、版位或负责人拆分，否则保持结构简洁。

## 结构实操

默认合并，只为控制而拆分：

| 拆分理由 | 合理理由 |
|---|---|
| 目标 / 事件 | 优化目标或转化质量不同 |
| 经济模型 | CPA/ROAS 目标、毛利、AOV、LTV 或预算负责人不同 |
| 市场 / 语言 | 法律、物流、语言或创意需求不同 |
| 落地页 | 网站、App、TikTok Shop、即时表单、消息 |
| 版位质量 | TikTok vs Pangle / Global App Bundle 诊断 |
| 创意测试 | 干净的概念/达人/卖点测试 |
| 政策 | 受限类目或宣称/披露要求 |

不要按每个兴趣类目、细微的人口统计猜测或小的创意变体拆分。过度碎片化会饿死学习期，让结果更难解读。

## 目标

TikTok 的目标标签因账户和推送阶段而异。常见的规划标签包括：

| 漏斗 | 目标 / 流程 | 适用场景 |
|---|---|---|
| 认知 | Reach（覆盖） | 最大化曝光 |
| 考量 | Traffic（流量） | 为网站、App 或落地页引流 |
| 考量 | Video Views（视频观看） | 推动视频消费 |
| 考量 / 代管 | Brand Consideration（品牌考量） | 在有 TikTok Market Scope 准入时培养高意向中漏斗受众 |
| 考量 | Community Interaction（社群互动） | 在有货时增长粉丝/主页访问或账号互动 |
| 转化 | Lead Generation（线索收集） | 通过即时表单或网站表单收集线索 |
| 转化 | App Promotion（App 推广） | 推动 App 安装或 App 内事件 |
| 转化 / 电商 | Sales（销售） | 在当前账户流程中推动网站、App 或 TikTok Shop 销售 |
| 电商自动化 | Product GMV Max / LIVE GMV Max | 最大化 TikTok Shop GMV 或直播收入 |

最终落地说明请用当前 TikTok Ads Manager 的标签。旧界面可能显示 Website Conversions（网站转化）或 Product Sales（商品销售）；新界面可能把它们合并到 Sales 下。在账户确认准入前，把 Brand Consideration 当作受限目标处理。

## 预算与出价

常见的出价/优化选项包括：

| 选项 | 适用场景 |
|---|---|
| Maximum Delivery（最大投放） | 早期学习、广泛投放、历史数据有限 |
| Cost Cap（成本上限） | 需要 CPA/CPI 控制且信号充足 |
| Bid Cap（出价上限） | 需要硬性出价纪律且能接受有限投放；可用性因版位而异 |
| Highest Value（最高价值） | 价值信号可靠且需要放量时的价值优化 |
| Minimum ROAS（最低 ROAS） | 需要 ROAS 底线时的网页价值优化 |
| Target ROAS（目标 ROAS） | 账户支持 App VBO 目标时的 App 价值优化 |
| Target ROI（目标 ROI） | 有足够商品/订单信号的 GMV Max 广告系列 |

预算要对照预期转化量级评估。如果广告系列产生不了足够的有意义动作，减少碎片化，或暂时优化到量更大的事件。

除非账户中出现，否则不要依赖旧的 **Target Cost**（目标成本）术语。TikTok 越来越多地通过 Cost Cap 及相关 Smart+ 流程来做 CPA 控制。

## 定向

| 定向类型 | 示例 | 用途 |
|---|---|---|
| Broad（宽泛） | 最小限制 | 让 TikTok 从创意和转化信号中学习 |
| 人口统计 | 地理位置、语言、年龄、性别 | 市场与资质控制 |
| 兴趣/行为 | 兴趣类目、视频互动、达人/类目行为 | 受众塑形 |
| Purchase Intent（购买意向） | 有货时兴趣定向内的近期购物信号 | 电商和下漏斗拓新 |
| Custom Audiences（自定义受众） | 客户名单、网站访客、App 用户、互动受众 | 再营销与排除 |
| Lookalike Audiences（相似受众） | 基于 Custom Audience 种子建模 | 从高价值种子放量 |
| 搜索关键词 | Search Ads 关键词定向 | 捕捉主动的 TikTok 搜索意图 |
| 版位 | TikTok、Pangle、Global App Bundle、Lemon8、其他符合资质的版位 | 控制流量和 App/联盟组合 |

当转化信号和创意体系强劲时，宽泛定向通常是默认起点。受限类目、本地市场、达人/社群匹配或干净测试时，窄定向仍然有用。

**Smart Targeting（智能定向）**可在系统预测能更好完成目标时，让投放跑出所选的兴趣与行为或受众设置。审慎使用，记录开启位置；需要严格受众锁定时避免使用。

Pangle 和 Global App Bundle 是外部或站外 App 流量，不是 TikTok 信息流流量。当版位质量或 App 场景重要时，诊断时保持分开。

## Smart+ 与自动化

Smart+ 是一种竞价自动化模式，不是独立的购买路径。Smart+ 产品自动完成定向、出价、创意组装和预算分配的部分工作。当前官方流程包括：

| Smart+ 流程 | 用途 |
|---|---|
| Smart+ Web Campaigns | Sales 下的网站转化目标 |
| Smart+ App Campaigns | App Promotion 的安装、App 内事件或 App 价值（视支持情况） |
| Smart+ Lead Generation Campaigns | 即时表单、网站、TikTok Direct Messages 或支持的即时消息线索 |
| Smart+ Traffic Campaigns | 点击或落地页浏览的流量目标 |
| Smart+ Catalog Ads | Smart+ 流程内的商品目录驱动型网站/App 产品广告 |

以下情况用 Smart+：

- 账户有足够的转化或电商信号。
- 创意供给多样且质量高。
- 业务能接受更少的人工控制。
- TikTok Ads Manager 之外有衡量手段。

不要把自动化当作策略的替代品。人类仍然拥有事件设计、产品经济模型、创意方向、身份/账户配置、政策、预算约束和真实来源报告。在升级版 Smart+ 体验中，部分账户可在同一流程中选择全自动、半自动或人工控制。

## 学习与放量

默认运营规则：

- 不要同时改预算、出价、定向、创意和事件定义。
- 批量改动并保留改动日志。
- 等转化延迟和学习期稳定后再评判 CPA/ROAS。
- 除非固定活动或大促需要爆发，否则逐步放量胜出者。
- 先更新创意，再大改账户结构。

TikTok 学习期指引称，波动通常在约 25 个广告系列结果或 7 天后趋于平稳。大幅调预算、改出价/ROAS 目标、改出价策略、改定向、暂停，以及不合理的创意量级变化，都可能触发或延长学习期。除非追踪、政策、投放或落地页质量坏了，否则把第一周当作稳定窗口。

## 运营节奏

| 节奏 | 重点 | 避免 |
|---|---|---|
| 每日 | 花费异常、追踪/政策故障、投放失败、明显的创意问题 | 每日微调出价/目标 |
| 每周 | 创意复盘、评论、搜索词、线索/订单质量、预算 pacing、改动日志 | 因为几天噪音就重建 |
| 每月 | 创意 pipeline、落地页、产品/信息流/店铺质量、CRM 或财务对账 | 让上线时的假设一直沿用 |
| 每季度 | 增量、渠道角色、归因窗口、商业模式适配、提升测试 | 把平台 ROAS 当作最终真相 |

## 再营销

常见再营销池：

| 池 | 典型用途 |
|---|---|
| 网站访客 | 提醒、证明、优惠、异议处理 |
| 加购 / 结算 | 高意向挽回 |
| 视频观看者 | 教育或序列化信息 |
| 线索表单打开/提交 | 线索质量与跟进 |
| App 用户 | 再互动与生命周期优惠 |
| TikTok Shop 互动者 | 商品、直播或 GMV Max 助推 |

再营销窗口与购买周期对齐。当报告或增量重要时，再营销与拓新分开。

## 衡量

| 目标 | 必需的衡量 |
|---|---|
| 网站转化 / Sales | TikTok Pixel 和/或 Events API、事件定义、相关时去重 |
| App 推广 | 认可的 Mobile Measurement Partner 或 TikTok SDK/App 事件；相关时做 SKAN/iOS 设置 |
| 线索收集 | 即时表单导出/API、CRM 阶段、合格线索回传 |
| TikTok Shop / GMV Max | Seller Center / Shop 连接、商品权限、订单/GMV 报告 |
| Search Ads | 关键词层级报告、UTM、落地页分析 |
| 品牌 / 预定 | 覆盖、频次、视频指标、提升研究或代理需求解读 |

战术优化用 TikTok Ads Manager，但要尽可能与订单、CRM、App 分析、TikTok Shop/Seller Center、财务报告和增量测试对账。

网页信号方面，Pixel + Events API 是条件允许时的默认稳健配置。如果同一事件走两个通道发送，两边都传 `event_id` 做去重。

App 广告系列上线前先核验当前的 iOS 和 MMP 技术栈。常见的规划示例包括 Adjust、AppsFlyer、Singular、Branch、Airbridge、Tenjin、TikTok SDK、SKAN 设置，以及有货时 TikTok 的建模/聚合 iOS 衡量选项。用账户的 MMP 和 App 分析作为留存、价值和 LTV 的真实来源。

新配置用当前标准事件名。TikTok 2025 年的标准事件更新把 `SubmitForm` 改名为 `Lead`、`CompletePayment` 改名为 `Purchase`；支持窗口内 legacy 名称可能继续可用，但新规划请用 `Lead` 和 `Purchase`。

品牌和上漏斗工作，在代管准入和预算允许时用 TikTok Brand Lift Study（品牌提升研究）或 Conversion Lift Study（转化提升研究）。否则提前定义好代理解读：覆盖、频次、观看质量、品牌搜索、直接流量、主页访问、Shop 活跃和下游再营销池。
