# 账户数据诊断

当用户提供 Google Ads 改进所需的账户数据时使用本参考：CSV / XLSX 导出、复制的表格、截图、Looker Studio 图表、Google Ads / GA4 / CRM / Merchant Center 报告、API 导出，或指标的书面汇总。格式不重要，关键是诊断任务：判断数据能证明什么、不能证明什么，以及哪些账户改动值得优先做。

## 运营规则

- 从业务结果倒推诊断：收入、合格 pipeline、留存用户、已预约工单或边际贡献优先于平台 CPA / ROAS。
- 区分信号质量和流量质量。糟糕的核心转化会让好流量看起来很差，也会让差流量看起来不错。
- 不要只信单一平台。在条件允许时，用 GA4、CRM / POS / 应用分析、Merchant Center 与 Google Ads 对账。
- 用花费加权的证据。一个花费极小的广告组 CPA 再难看，也不如下一个占 35% 花费但合格结果很弱的广告系列紧急。
- 不要同时改出价、预算、目标和结构。数据应该产出一份有序的行动计划，而不是一堆零散的编辑。
- 截图和摘要只看方向。只有当决策依赖细分、关联或精确总数时，才要求要原始行数据。

---

## 1. 数据接入与数据质量检查

分析效果之前，先确认数据集及其局限。

| 检查项 | 看什么 | 为什么重要 |
|---|---|---|
| 来源 | Google Ads、GA4、CRM、Merchant Center、应用 / MMP、财务、电话跟踪、Looker | 决定归因、日期逻辑和可关联的字段 |
| 日期基准 | 点击 / 展示日期、会话日期、转化日期、上传日期、成交日期 | 很多差异就是日期对不上造成的 |
| 日期范围 | 过去 7 / 14 / 30 / 90 天、季节性、转化延迟覆盖 | 短窗口对噪声过度反应；长窗口掩盖近期变化 |
| 时区 / 货币 | 账户时区、CRM 时区、GA4 媒体资源时区、本地货币 | 影响按日关联、CPA / ROAS 和预算消耗进度 |
| 筛选器 | 广告系列类型、广告系列状态、网络、转化动作、渠道、地域、设备 | 隐藏的筛选器会让数据失去代表性 |
| 归因 | Google Ads DDA / 末次点击、GA4 付费渠道 vs 付费 + 自然、CRM 首次 / 末次触点 | 决定各来源总数应不应该对得上 |
| 转化定义 | Primary vs secondary、关键事件 vs Google Ads 转化、CRM 阶段 | 决定出价在学什么 |
| 行粒度 | 账户、广告系列、广告组、关键词、搜索词、商品、落地页、转化动作、线索 | 决定能做哪些决策 |

如果数据不完整，仍然给出诊断，但要加 **数据局限（Data limitations）** 章节，把"有依据的发现"和"待确认"分开写。

---

## 2. 按来源的最低可用字段

这些不是必填列，而是做出强决策实际需要的字段。如果用户给的是截图，在可见列里找同样的概念即可。

### Google Ads 效果

| 决策 | 可用字段 |
|---|---|
| 广告系列健康度 | `date`、`campaign`、`campaign_id`、`campaign_type`、`status`、`budget`、`bid_strategy`、`cost`、`impressions`、`clicks`、`ctr`、`avg_cpc`、`conversions`、`cost_per_conversion`、`conversion_value`、`roas`、`impression_share`、`lost_is_budget`、`lost_is_rank` |
| 搜索 / 搜索词质量 | `search_term`、`keyword`、`match_type`、`campaign`、`ad_group`、`cost`、`clicks`、`conversions`、`conversion_value`、`search_term_match_type`、`final_url` |
| 转化信号 | `conversion_action`、`conversion_category`、`primary_or_secondary`、`count`、`value`、`all_conversions`、`view_through_conversions`、`ad_event_type` |
| 落地页 | `landing_page`、`expanded_landing_page`、`campaign`、`cost`、`clicks`、`conversions`、`conversion_rate`、`mobile_friendly_click_rate`、`valid_amp_click_rate` |
| P-MAX / Demand Gen | `channel`、`ad_event_type`、`asset_group`、`listing_group`、`search_theme`、`search_terms_insight`、`audience_signal`、`asset`、`asset_performance`、`cost`、`conversions`、`value` |
| Shopping / 商品 | `item_id`、`product_title`、`brand`、`product_type`、`google_product_category`、`custom_label_0-4`、`product_status`、`cost`、`clicks`、`conversions`、`value`、`roas` |

### GA4 / 网站分析

| 决策 | 可用字段 |
|---|---|
| 付费流量质量 | `date`、`session_source_medium`、`session_campaign`、`landing_page`、`device_category`、`sessions`、`engaged_sessions`、`engagement_rate`、`key_events`、`revenue` |
| 落地页诊断 | `landing_page`、按需加 `query_string`、`device_category`、`sessions`、`engagement_rate`、`key_event_rate`、`purchase_revenue`、`average_engagement_time` |
| 归因对账 | 由 GA4 关键事件创建的 Google Ads 转化、归因设置、回溯窗口、付费渠道 vs 付费 + 自然 |

### CRM / 销售 / 电话跟踪

| 决策 | 可用字段 |
|---|---|
| 线索质量 | `lead_id`、`created_at`、`source`、`medium`、`campaign`、`gclid`、`gbraid`、`wbraid`、`utm_campaign`、`utm_term`、`landing_page`、`lead_status`、`qualified_at`、`sql_at`、`opportunity_created_at`、`closed_won`、`revenue`、`lost_reason` |
| 线下导入就绪度 | click ID 或用户提供数据的可用性、转化时间戳、转化名称、转化价值、上传状态 / 报错 |
| 销售流程漏损 | 首触速度（speed-to-lead）、联系尝试次数、约访率、爽约率、分广告系列 / 搜索词 / 地域 / 设备的成交率 |

### Merchant Center / Feed

| 决策 | 可用字段 |
|---|---|
| 展示资格 | `item_id`、`status`、`issue`、`destination`、`availability`、`price`、`sale_price`、`link`、`image_link`、`gtin`、`mpn`、`brand` |
| Feed 质量 | `title`、`description`、`product_type`、`google_product_category`、`condition`、配送 / 退货标注、促销 |
| 利润导向优化 | `item_id`、利润等级、库存等级、畅销 / 季节性 / 自定义标签、退货率、库存状态 |

---

## 3. 诊断顺序

按顺序执行。如果账户出价学的转化都错了，不要先从剪关键词开始。

### A. 衡量与对账

| 数据中的信号 | 可能的含义 | 行动 |
|---|---|---|
| Google Ads 转化很高，CRM 合格线索很少 | Primary CV 太浅、流量质量差，或销售交接有问题 | 按广告系列 / 搜索词 / 落地页细分；导入合格线索或阶段价值；弱动作降为次要 |
| 转化数等于或接近点击数 | 标签可能在点击 / 页面加载时触发，而不是真实转化时 | 改出价前先审计标签部署位置 |
| Google Ads 和 GA4 差异很大 | 可能是转化延迟、归因模型、回溯窗口、标签设置、日期基准，或浏览 / 跨设备处理不同 | 先对比定义，再下结论说谁错了 |
| `All conv.` 里很多次要 / 微动作，但核心转化很少 | 有互动，但出价信号弱 | 微动作保持次要；改进 offer / 落地页，或选更深层且可靠的核心转化 |
| 线下转化上传成功但不显示 | 日期范围、时区、计数设置、处理延迟、click ID / 用户数据匹配问题 | 检查上传诊断，报表用点击 / 展示日期 |
| CRM 里没有 `gclid` / `gbraid` / `wbraid` / 用户提供数据 | 线下质量无法可靠回传 | 放大线索自动化之前先修好数据采集 |

### B. 预算、量级与学习可行性

| 数据中的信号 | 可能的含义 | 行动 |
|---|---|---|
| 广告系列每月真实核心转化 <10 | Smart Bidding 的目标决策很脆弱 | 合并，或用 Max Clicks / 人工出价，或无紧目标的 Max Conversions，或选一个经验证的、量级更大的深层信号 |
| 日预算低于目标 CPA | 学习会很慢、很吵 | 收窄范围、提高预算，或在经济模型允许时降低出价目标 |
| 高利润的 Search / Shopping 预算型展示份额损失（Lost IS budget）很高 | 有需求，但预算卡住了投放 | 先从低质量花费里挪预算，再考虑加总预算 |
| CVR 不错但排名型展示份额损失（Lost IS rank）很高 | 广告评级 / 相关性 / 出价限制了有利可图的量 | 改进质量得分驱动因素、素材、落地页，考虑调整出价 / 目标 |
| 花费分散在很多低量级广告系列 | 学习被碎片化 | 按目标 / 经济模型 / 地域 / 利润率合并，而不是为了报表整齐 |

### C. 流量相关性

| 数据中的信号 | 可能的含义 | 行动 |
|---|---|---|
| 高花费搜索词 0 转化且意图弱 | 浪费的需求捕获 | 加否定词，收紧匹配 / AI Max 控制，重写广告做筛选 |
| 搜索词在转化，但当前关键词没覆盖 | 新的需求模式 | 加为关键词 / 主题，对齐 RSA / 落地页，用否定词保护 |
| 广泛匹配 / AI 扩展带来弱搜索词 | 自动化护栏弱或信号差 | 加否定词、网址排除、品牌控制；信号修好之前不要关自动化 |
| CTR 高但 CVR 低 | 广告吸引了好奇或不匹配的用户 | 加筛选条件，明确价格 / 服务范围，落地页匹配度做强 |
| CTR 和 CVR 都低 | 受众 / 搜索词 / 信息错了 | 围绕购买意图重建定向和文案 |
| CTR 低但 CVR 强 | 小众高意向流量 | 提高广告相关性，但别把质量优化没了 |

### D. 落地页 / 目标页面

| 数据中的信号 | 可能的含义 | 行动 |
|---|---|---|
| 跨广告系列、某落地页点击多但 CVR 低 | 目标页面是瓶颈 | 修 offer 清晰度、加载速度、移动端体验、表单 / 结账、信任状、信息匹配 |
| 移动流量明显弱于桌面 | 移动端体验、速度、表单摩擦、点击拨号缺失 | 审计移动端落地页；UX 复盘之后再考虑按设备分预算 |
| 落地页互动低但搜索词意图不错 | 页面没兑现承诺 | 首屏对齐搜索词 / 广告；降摩擦；加信任状和 CTA |
| P-MAX / AI Max 把流量送到弱页面 | 网址扩展问题 | 加网址排除或限制最终网址扩展 |
### E. 创意 / 素材质量

| 数据中的信号 | 可能的含义 | 行动 |
|---|---|---|
| 素材组里很多相似的文案变体 | 概念多样性弱 | 做差异化切入角度：痛点、结果、信任状、offer、异议处理、筛选 |
| Demand Gen / Video 有花费但互动低 | 钩子 / 版式不匹配 | 重做前 1–3 秒，竖版 / 方版变体、产品使用场景、信任状 |
| P-MAX 没有视频或图片覆盖弱 | 库存受限，或自动生成的素材占主导 | 按素材组主题加真实视频 / 图片 |
| RSA 素材泛泛、广告相关性弱 | 搜索词与信息不匹配 | 按意图主题和买家语言重写 |

### F. 商品 / Feed / 利润

| 数据中的信号 | 可能的含义 | 行动 |
|---|---|---|
| 商品无展示资格或资格受限 | Feed / 政策 / 账户问题压制了投放 | 改出价前先修 Merchant Center 诊断 |
| 高花费、低 ROAS 的 SKU | 商品经济模型或页面竞争力弱 | 分组 / 排除、调整目标、改进价格 / 配送 / 落地页，或传递利润感知的价值 |
| 低利润商品主导了 ROAS | 收入价值掩盖了利润问题 | 加利润自定义标签或传递边际贡献价值 |
| 畅销品分到的花费太少 | 广告系列 / 商品组或目标限制 | 量级够就拆成素材组 / 广告系列 |
| 缺货商品还在拿流量 | Feed 库存状态对不上 | 修 Feed 同步，加库存护栏 |

### G. 增量性与归因

| 数据中的信号 | 可能的含义 | 行动 |
|---|---|---|
| 品牌广告系列 / P-MAX ROAS 很高但总收入没动 | 品牌截流、再营销偏差或归因虚高 | 品牌 / 非品牌分开；用品牌排除；对比混合的业务结果 |
| 报表转化以 VTC 或展示类广告事件为主 | 非点击归因可能夸大了因果效应 | 点击 / EVC / VTC 分开汇报，重要场景做提升测试 |
| Demand Gen 直接 CPA 弱，但品牌搜索 / 再营销在涨 | 可能存在助攻需求 | 评估混合 CPA、搜索提升、受众增长，做对照组可行性分析 |
| 平台 ROAS 和财务利润对不上 | 收入价值没扣 COGS / 退货 / 折扣 | 用边际贡献 ROAS 或利润感知的价值 |

---

## 4. 按报表类型的阅读指南

| 数据产物 | 先问的问题 | 能支撑的决策 |
|---|---|---|
| 广告系列报表 | 花费集中在哪？哪些广告系列有有意义的转化量？哪些受预算 / 排名限制？ | 预算转移、合并、出价目标调整 |
| 搜索词 / 搜索词洞察 | 哪些词烧钱但没有合格结果？哪些转化的词值得更多覆盖？ | 否定关键词、加关键词、AI Max / 广泛匹配护栏 |
| 转化动作细分 | `Conversions` vs `All conv.` 分别被哪些动作驱动？弱动作是不是 primary？ | 转化重设计、primary / secondary 清理 |
| 落地页报告 / GA4 落地页表 | 哪些页面拿了付费流量但互动 / 转化失败？是移动端的问题吗？ | 落地页修复、网址排除、最终网址策略 |
| 竞价洞察 / 展示份额 | 损失的量是预算、排名还是竞争压力造成的？ | 加 / 挪预算、提高相关性、调整目标 |
| 质量得分构成 | 问题在预期点击率、广告相关性还是落地页体验？ | 文案 / 广告组 / 落地页优先级；不是 KPI 目标 |
| P-MAX 渠道报告 | 哪些渠道、广告格式、广告事件类型在烧钱和出转化？ | 素材 / Feed / 渠道诊断、报表审慎度、实验 |
| 素材报告 | 哪些概念在学习、哪些是累赘？ | 创意更新和概念扩展 |
| 商品诊断 / 商品表 | 哪些商品不能投放或花费低效？ | Feed 修复、商品组 / 素材组细分 |
| CRM pipeline 导出 | 哪些广告系列产出 SQL、商机、收入或垃圾线索？ | 线下导入、价值规则、搜索词 / 受众 / 落地页清理 |
| 更改历史 | 效果变化是不是发生在改预算、出价、目标、Feed 或素材之后？ | 稳定窗口和回滚 / 保持决策 |

---

## 5. 优先级模型

用这个评分给发现排序。优先修花费大、业务影响大、证据确凿、学习风险低的问题。

| 因素 | 1 分 | 3 分 | 5 分 |
|---|---|---|---|
| 花费影响 | 长尾小量 | 明显 | 头部花费驱动 |
| 业务影响 | 表面指标 | CPA / ROAS 波动 | 收入、pipeline、利润或跟踪完整性 |
| 证据确信度 | 一张截图 / 个案 | 分平台的分段数据 | 跨来源对账 |
| 修复成本 | 多团队项目 | 需要一定实施 | 立刻可改的账户 / Feed / 文案 |
| 学习风险 | 重置主要学习 | 中等 | 低风险护栏或衡量修复 |

公式仅作参考，不要当死分：

```
Priority = (Spend impact + Business impact + Evidence confidence + Fix ease) - Learning risk
```

默认按以下顺序修：

1. 衡量完整性：核心转化、重复 / 缺失跟踪、CRM 关联、Feed 展示资格。
2. 浪费控制：不相关的搜索词、排除 / 弱网址、拒登 / 缺货商品、明显的展示位置 / 受众浪费。
3. 预算再分配：把花费从弱质量的坑挪到已验证的高意向 / 高利润 / 高质量分段。
4. 目标页面与创意：落地页、Feed 标题 / 图片 / 价格、RSA 和素材组概念质量。
5. 出价与结构：目标、广告系列拆分 / 合并、P-MAX / Demand Gen 扩展。
6. 增量测试：品牌、再营销、VTC 占比高、P-MAX 重叠、Demand Gen 助攻。

---

## 6. 数据驱动诊断的输出格式

当用户提供账户数据时，返回：

1. **已审阅的数据**——来源、日期范围、行粒度、筛选器和局限。
2. **数据质量警示**——缺失字段、日期 / 归因对不上、可疑的总数、关联不完整。
3. **核心诊断**——3–7 条按优先级排序的发现，带证据。
4. **决策表**——改什么、为什么、预期效果、稳定窗口，以及不要同时改什么。
5. **还需要的数据**——只要求那些会改变决策的数据。

发现示例格式：

| 优先级 | 证据 | 解读 | 行动 | 避免 |
|---|---|---|---|---|
| P1：线索质量导入 | Search 和 P-MAX 有 180 个表单转化，CRM 只有 14 个 SQL 和 2 个商机；弱 SQL 集中在广泛非品牌搜索词 | 出价在学便宜的表单填写，不是合格 pipeline | 把合格线索 / 商机作为 primary 或价值信号导入；加搜索词和落地页筛选 | 质量信号修好之前，不要加预算或放宽 tCPA |
| P2：搜索词浪费 | 90 天里 22% 的 Search 花费给了 0 合格线索的信息型搜索词 | 流量相关性漏损 | 加否定词，拆高意向完全 / 词组匹配，检查 AI Max / 广泛匹配扩展 | 高意向词还在跑就不要暂停整个广告系列 |
| P3：商品展示资格 | 18% 的高利润 SKU 在诊断里是"无资格 / 资格受限" | Feed 压制了高利润库存 | 修 Merchant Center 问题、GTIN / 库存 / 价格对不上，再按高利润标签分组 | Feed 覆盖恢复之前，不要评判 Shopping / P-MAX |

---

## 7. 易变说法的核查来源

平台行为尽量以 Google 官方文档为准：

- 搜索词报告：https://support.google.com/google-ads/answer/2472708?hl=en
- 从搜索词找否定关键词：https://support.google.com/google-ads/answer/7102466?hl=en
- 主要与次要转化动作：https://support.google.com/google-ads/answer/11461796?hl=en
- 转化目标与广告系列目标行为：https://support.google.com/google-ads/answer/10995103?hl=en
- Google Ads 数据差异：https://support.google.com/google-ads/answer/7457111?hl=en
- 线下转化导入差异：https://support.google.com/google-ads/answer/13321563?hl=en
- 落地页报告：https://support.google.com/google-ads/answer/7543502?hl=en
- 质量得分构成：https://support.google.com/google-ads/answer/6167118?hl=en
- 竞价洞察：https://support.google.com/google-ads/answer/2579754?hl=en
- P-MAX 渠道效果：https://support.google.com/google-ads/answer/16260130?hl=en
- 商品诊断：https://support.google.com/google-ads/answer/12097493?hl=en
- Merchant Center 问题：https://support.google.com/merchants/answer/12153802?hl=en
- Shopping / P-MAX 自定义标签：https://support.google.com/google-ads/answer/6275295?hl=en
- 线索的增强型转化 / 线下导入：https://support.google.com/google-ads/answer/14274408?hl=en
- 带线索增强型转化的 Google Ads Data Manager：https://support.google.com/google-ads/answer/15707550?hl=en

非官方的审计模式可以为优先级排序提供参考，但当作启发式方法对待。实用的从业者参考包括 Optmyzr 的 PPC 审计文档和 WordStream 的 Google Ads 审计 / 浪费花费分析。
