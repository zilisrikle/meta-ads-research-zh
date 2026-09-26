# Meta 广告代理商：发现与审计阶段

## 概述

发现与审计阶段是代理商在客户 Meta 广告账户上所做一切工作的诊断基础。它的目的有两个：(1) 以法医级的精度了解账户的当前状态；(2) 找出能带来最快效果的最高杠杆改动。用 Foxwell Digital 的话说：「没有合格的审计，你只是在猜测。有了审计，你得到的是基于扎实数据的清晰路线图——放大有效的，解决无效的。」

优秀的代理商不会把审计当成打勾走流程，而是将其视为覆盖五个相互关联领域的结构化诊断：数据完整性、广告系列结构、定向与合规、创意质量、运营治理。

**置信度：高**——有 AdManage 的 25 点清单、Foxwell Digital 的战略框架、CommonThread 的 27 点指南和 Adligator 的起飞前清单支持。

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://www.foxwelldigital.com/blog/how-we-approach-meta-ads-audits-a-strategic-framework-for-better-performance
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://leadsbridge.com/blog/facebook-ads-audit/

---

## 审计的类型

代理商根据不同场景做不同类型的审计：

1. **新客户入驻审计**——在做出任何改动之前做全面基线评估。记录继承的问题。建立起点指标。

2. **问题账户审计**——针对 ROAS 目标未达成、客户增长停滞或效果根本性崩溃的情况。重点找出造成最大下游损害的 3–5 个问题。

3. **优化审计**——账户表现尚可，但客户想挖掘未被利用的增长空间。重点是增量改进与放量机会。

4. **第二意见审计**——客户对当前表现满意，但想要新的视角。寻找可能带来增量提升的小改动。

5. **数据差异调查**——平台指标与业务指标对不上。需要对归因、追踪和报告方法做法医级分析。

6. **放量前审计**——在任何大幅加预算（50% 以上）之前做。30 分钟的起飞前检查，抓住那些在放量时会烧钱的问题。

**置信度：高**——审计类型记录于 Foxwell Digital 和 Adligator。

来源：
- https://www.foxwelldigital.com/blog/how-we-approach-meta-ads-audits-a-strategic-framework-for-better-performance
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling

---

## 完整审计框架：代理商检查什么

### 领域 1：衡量与数据完整性（最高优先级）

这一项先查，因为下游所有的优化决策都依赖数据的准确性。如果追踪坏了，其他所有发现都不可靠。

#### 1.1 像素与转化 API 覆盖

**检查什么：** 核心转化事件（购买 Purchase、线索 Lead、完成注册 CompleteRegistration、应用事件）是否同时通过 Meta 像素和转化 API（CAPI）传输？

**健康：** 浏览器端像素和服务端 CAPI 同时运行，提供冗余。

**坏了：** 只依赖单一通道。纯浏览器追踪会漏掉大量 iOS 流量。纯服务端追踪会漏掉依赖 JavaScript 的交互。

**代理商如何验证：**
- 打开事件管理工具（Events Manager），进入数据源，选择像素
- 确认像素处于活跃状态，最近 24 小时有事件接收
- 验证 CAPI 正在为所有关键转化事件发送服务端事件
- 检查两个通道的事件名称一致

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://www.sociummedia.com/blog/facebook-ads-audit-guide/

#### 1.2 事件去重

**检查什么：** 浏览器事件的 `eventID` 与服务端事件的 `event_id` 是否匹配？像素与 CAPI 的事件名称是否完全一致？

**为什么重要：** 没有正确去重，同一个转化会被计两次——虚增上报的效果，误导算法的优化。

**代理商如何验证：**
- 对比浏览器端与服务端实现的 `event_name`
- 确认每个事件都有匹配的 `event_id` 值
- 用事件管理工具的测试工具检测重复触发
- 检查加购与购买的比率：10 倍为正常；100 倍说明去重有问题或漏斗断了

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling

#### 1.3 事件匹配质量（EMQ）与高级匹配

**检查什么：** 事件匹配质量（Event Match Quality）得分趋势与高级匹配（Advanced Matching）配置。

**目标：** EMQ 得分 10 分制中 6.0 以上。低于 5.0 意味着严重的数据丢失——Meta 无法把转化正确归因到广告曝光。

**代理商如何提升 EMQ：**
- 启用高级匹配，传输哈希化的客户数据（邮箱、电话、姓名、城市、州、邮编）
- 验证事件近乎实时到达（延迟 6 小时意味着实时竞价优化窗口过去后信号才到）
- 检查事件值和货币设置正确

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling

#### 1.4 事件映射与自定义事件分类体系

**检查什么：** 账户是否在适用场景使用了 Meta 的标准事件？自定义事件的分类体系是否干净、合理？

**关键问题：** 用触发频率很低的自定义事件（如「qualified_lead」每周只触发 3 次）作为优化目标，会破坏算法学习，造成永久的「学习受限（Learning Limited）」状态。Meta 需要每个广告组每周约 50 次优化事件才能走出学习期。

**最佳实践：**
- 用标准事件（购买 Purchase、线索 Lead、发起结账 InitiateCheckout、加购 AddToCart、浏览内容 ViewContent）作为主要优化目标
- 自定义事件只留给真正非标准的业务行为
- 记录事件分类体系，确保所有广告系列保持一致

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 1.5 归因设置对齐

**检查什么：** 各广告系列的归因窗口是否一致，广告管理工具（Ads Manager）与内部报告是否对齐。

**标准窗口：** 点击 7 天、浏览 1 天（Meta 默认）。

**2026 年平台变化：** 自 2025 年 10 月起，Meta 从广告洞察 API 中移除了 `7d_view` 和 `28d_view` 归因窗口。仍在用这些窗口的报告系统会出现报错或数据缺失。

**关键审计发现：** 一位从业者报告，从账户中移除浏览 1 天归因后，上报的 ROAS 下降了 50%。这揭示了浏览归因对感知效果的虚增有多严重，尤其是在多渠道并行时。

**代理商如何检查：**
- 验证所有广告系列使用相同的归因窗口
- 对比广告管理工具的归因设置与 BI 看板的设置
- 测试移除浏览归因的影响，了解真实的点击归因效果

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://www.linkedin.com/posts/philkiel_last-week-i-made-an-offer-to-audit-meta-ad-activity-7391520378528956417-4dJ9

#### 1.6 收入与转化数据验证

**检查什么：** Facebook 上报的收入是否与后端/电商平台数据一致？

**阈值：**
- 差异在 15% 以内：健康
- 差异 15–25%：需要调查
- 差异超过 25%：追踪坏了——不要用 Meta 的数字做优化决策

**代理商如何验证：**
- 导出 Meta 广告管理工具 30/60/90 天窗口的购买数据
- 与同期 Shopify/WooCommerce/CRM 的收入数据对比
- 找规律——差异是稳定的，还是在某些广告系列期间飙升？
- 检查货币不匹配、测试订单污染或事件缺失

**置信度：高**

来源：
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://wevion.ai/en/blog/facebook-ads-agency-client-onboarding/

---

### 领域 2：广告系列结构与预算

#### 2.1 广告系列目标对齐

**检查什么：** 每个广告系列的目标是否真的匹配期望的业务结果？

**常见致命错误：** 建一个销售（Sales）目标的广告系列，转化位置却指向博客页面浏览而不是实际购买流程。这会让 CPA 看起来虚低，实际带来零收入。

**审计要点：**
- 品牌认知类广告系列应优化品牌认知指标
- 流量类广告系列应优化链接点击或落地页浏览
- 线索类广告系列应优化表单提交或线索事件
- 销售类广告系列应优化购买
- 转化位置必须匹配真实的用户路径（网站、应用、即时表单、消息、电话）

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 2.2 过度拆分评估

**检查什么：** 有多少个广告系列、广告组、受众拆分？它们是出于业务逻辑，还是只是历史习惯？

**问题所在：** 预算碎片化是 2026 年最大的结构性错误。Meta 的算法需要集中的预算才能优化。广告组太多、预算太薄，意味着没有一个能走出学习期。

**基准：**
- **健康：** 每个广告系列 3–5 个广告组，宽泛定向 + AI 匹配
- **弱：** 8–12 个广告组，预算薄，部分受众重叠
- **坏了：** 单个广告系列 15 个以上广告组；每个国家一个广告组但定向完全相同；6 个月前的老测试结构还在以极小花费运行

**碎片化的审计数学：**
- 5 个广告组、每个每天 100 美元 = 总计 500 美元 = 没有一个能每周产生 50 次事件
- 2 个广告组、每个每天 250 美元 = 总计 500 美元 = 可以走出学习期

**一位从业者的发现：**「最烂的账户：月预算 3.5 万美元，却有 10 个以上的广告系列。修复方法？一个广告系列。最多两个。」

**常见的碎片化模式：**
- 每个国家一个广告组（合并成区域）
- 每个兴趣变体一个广告组（合并相似的兴趣组合）
- 从不归档的老测试单元（暂停或并入胜出者）

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://www.linkedin.com/posts/philkiel_last-week-i-made-an-offer-to-audit-meta-ad-activity-7391520378528956417-4dJ9
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 2.3 学习期状态

**检查什么：** 有多少广告组卡在「学习中（Learning）」或「学习受限（Learning Limited）」状态？

**数学要求：** 7 天内 50 次优化事件 = 每个广告组每天约 7 次转化。

**预算与事件的换算：**
- 如果 CPA = 40 美元，每天需要 7 次转化，则每个广告组每天最低需要约 280 美元
- 每个广告组每天只跑 100 美元，在这个 CPA 下数学上必然走不出学习期
- 日预算最低应为优化事件平均成本的 10 倍（稳定表现需要 15–20 倍）

**实际含义：** 如果账户有 8 个广告组、每个每天花 50 美元，而 CPA 目标是 40 美元，那这些广告组永远走不出学习期。修复方法是合并：2 个广告组、每个每天 200 美元。

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://optifox.in/blog/meta-ads-best-practices-2026/
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 2.4 预算分配分析

**检查什么：**
- CBO（广告系列预算优化）广告系列：是否某个广告组吃掉了 80% 以上的预算？（说明优化不均衡）
- ABO（广告组预算优化）广告系列：是否有长期花不出去的广告组？（说明投放有问题）
- 整体消耗节奏：账户是否花不到日预算的 80%？（说明系统性投放问题）

**推荐的分配框架：**
- 拉新：总预算的 60–70%
- 再营销/重定向：20–30%
- 测试：10–20%
- 新账户前 90 天：拉新 40%、测试 40%、再营销 20%

**置信度：高**

来源：
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://www.get-ryze.ai/blog/meta-ads-budget-planning-how-much-spend-2026
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

---

### 领域 3：定向、版位与合规

#### 3.1 版位策略

**检查什么：** 账户用的是 Advantage+ 版位（推荐），还是手动限定了特定版位？

**2026 年最佳实践：** Advantage+ 版位是 Meta「寻找投放机会最具成本效益的方式」。手动限制只应在有书面效果证据支持排除的情况下使用。

**常见错误：** 因为几个月前某份报告「看起来不好看」就排除 Reels，白白丢掉便宜的转化。

**审计检查项：**
- 所有主要版位是否都开着？（动态 Feed、快拍、Reels、Messenger、Instagram、Audience Network）
- 如果用了手动版位，是否有数据支持这些排除？
- 创意素材是否为每个版位格式做了优化？

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 3.2 受众重叠评估

**检查什么：** 广告组之间是否在抢同一批用户？

**工具：** 受众版块中的 Meta 受众重叠工具。

**阈值：** 广告组之间重叠超过 30%，意味着账户在和自己竞价，推高成本、扰乱归因。

**修复：** 合并重叠的受众。用排除把拉新和再营销分开。合并相似的兴趣组合。

**置信度：高**

来源：
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 3.3 受众健康指标

**检查什么：**

- **自定义受众规模：** 最小可用规模是 1,000 人以上。低于此规模的受众太小，无法有效定向。
- **类似受众种子新鲜度：** 超过 60 天的种子应用新数据更新。
- **种子质量：** 基于购买者的种子表现优于基于网站访客的种子。
- **邮箱名单新鲜度：** 至少每月上传一次。
- **再营销窗口大小：** 检查网站访客的 1 天、7 天、30 天、180 天窗口。
- **新访客占比：** 新访客占比低于 70% 的账户都显示衰退迹象——Meta 在反复把广告推给同一批暖受众。

**饱和信号：**
- 日预算达到受众总规模日触达的 10% 以上 = 饱和在即
- 花费增加但触达持平
- 频次超过 4.0 且点击率（CTR, Click-Through Rate）下降

**置信度：高**

来源：
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://www.linkedin.com/posts/philkiel_last-week-i-made-an-offer-to-audit-meta-ad-activity-7391520378528956417-4dJ9

#### 3.4 特殊广告类别合规

**检查什么：** 账户是否投放受监管类别的广告（住房、就业、金融产品、社会/政治议题）？

**如果是：** 必须声明特殊广告类别（Special Ad Category）。这会严重限制年龄、性别和地域定向，但法律要求必须声明。

**风险：** 分类错误会导致账户被限制或封停。印度自 2025 年 7 月 28 日起要求证券/投资类广告主完成验证。

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit

#### 3.5 排除策略

**检查什么：** 拉新广告系列排除了哪些人？

**健康账户必需的排除：**
- 已有客户/购买者
- 近期转化者（在购买周期窗口内）
- 邮箱订阅者（如果只跑纯拉新广告系列）
- 网站访客（180 天，纯冷启动拉新时）
- 社媒互动者（如果把冷受众和暖受众分开）

**2025 年平台变化：** 自 2025 年 3 月 31 日起，详细定向排除（基于兴趣的）已从广告管理工具中移除。现在的排除围绕客户生命周期逻辑，而不是基于兴趣的压制。

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://www.linkedin.com/posts/philkiel_last-week-i-made-an-offer-to-audit-meta-ad-activity-7391520378528956417-4dJ9

---

### 领域 4：创意质量、疲劳与落地页

#### 4.1 创意效果分析

**检查什么：** 哪些广告在跑量，哪些疲劳了，创意多样性够不够？

**要看的关键指标：**

| 指标 | 说明 | 基准 |
|--------|----------------|-----------|
| 钩子率（3 秒视频观看 / 展示） | 开头帧是否吸引人？ | 目标 30–45%；低于 30% = 开头帧有问题 |
| 点击率（CTR） | 广告能否激发兴趣？ | 大多数目标 0.9% 以上；1.5% 以上为强 |
| 频次 | 用户看到广告的频率？ | 冷受众 3.0 以下；5.0 以上 = 几乎肯定疲劳了 |
| CPA 随时间趋势 | 单次结果成本是否在涨？ | 定向稳定但 CPA 上涨 = 创意疲劳 |
| 质量排名 | 广告质量与竞品对比如何？ | 高于平均或平均为健康 |
| 互动率排名 | 用户是否在互动？ | 表现差的广告低于平均，说明创意有问题 |
| 转化率排名 | 点击者是否在转化？ | 低于平均说明落地页或优惠有问题 |

**关键区别：** 一条达到 CPA 目标的健康广告，即使质量排名低于平均，也不应该动（没有改动的绩效依据）。一条没达到 CPA 目标且质量排名低于平均的广告，应该被诊断。

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 4.2 创意多样性评估

**检查什么：**
- 每个广告组至少 3 条在跑创意（更少是风险）
- 格式组合：图片、视频、轮播
- 每个视觉配 2–3 个文案角度
- 14 天以上没加新创意 = 停滞风险

**顶级代理商在审计中最常见的发现：**

据 Jon Loomer 的 LinkedIn 分析，最有影响力的审计领域是广告层级的创意分析：「内容测试（测试量与多样性）是我们在帮助客户时影响最大的因素。所以我们在做审计时，大部分时间都花在这上面。」

一位从业者发现：「一个账户 55% 的花费只给了 10 条广告……而在跑的有 318 条。另外 308 条？只是在烧管理时间。」

**置信度：高**

来源：
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://www.linkedin.com/posts/jonloomer_meta-ads-audit-evaluation-of-ad-sets-activity-7299937640483627008-M7wG
- https://www.linkedin.com/posts/philkiel_last-week-i-made-an-offer-to-audit-meta-ad-activity-7391520378528956417-4dJ9

#### 4.3 用 AIDA 框架做创意审计

CommonThread 的 27 点审计把 AIDA 行为模型转化为可衡量的广告指标：

| AIDA 阶段 | 指标 | 公式 | 目标 |
|------------|--------|---------|--------|
| 注意（Attention） | 3 秒视频观看 / 展示 | （3 秒视频观看 / 展示）x 100 | 30% 以上 |
| 兴趣（Interest） | 平均观看时长 | 来自视频指标 | 约 4 秒 |
| 欲望（Desire） | 点击率 | （链接点击 / 展示）x 100 | 0.9% 以上 |
| 行动（Action） | 广告支出回报率 | 收入 / 广告花费 | 因业务而异 |

**应用：** 一条注意率 55% 但点击率低的广告，创意钩子强但信息传达弱——改文案，不改视觉。一条注意率低但看了的人点击率高的广告，内容有吸引力但开头弱——改前 3 秒。

**置信度：高**

来源：
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

#### 4.4 落地页与广告的连贯性

**检查什么：** 落地页是否延续了赚到点击的那个承诺、证明、优惠和行动号召（CTA）？

**诊断信号：** 如果点击率高，但落地页浏览率或转化率崩了，问题通常出在广告到页面的交接上，而不是广告本身。

**健康的落地页：**
- 2 秒内加载完成
- 首屏信息与广告完全一致
- 无需滚动就能看到同样的优惠/行动号召
- 广告承诺与页面体验无断裂

**坏掉的落地页：**
- 加载 5 秒以上
- 信息与广告无关
- 行动号召要滚动才能找到
- 「寻宝游戏」式用户体验

**2025 年平台变化：** 自 2025 年 6 月起，即时体验（Instant Experience）不再计为落地页浏览。这影响落地页浏览指标的计算方式。

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

---

### 领域 5：治理、运营与放量准备

#### 5.1 权限与访问审计

**检查什么：** 正确的人是否仍然拥有公共主页、广告账户、商务管理平台和个人主页的访问权限？

**常见问题：** 账户访问依赖某个创始人或前员工的登录。一旦这个人离职，广告系列就发不出去。

**最佳实践：** 多名团队成员应有适当的访问级别。权限结构应有文档记录。定期审计已离职员工的访问权限。

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit

#### 5.2 命名规范与 UTM 追踪

**检查什么：** 能否从任何一条广告追溯到市场、受众、概念、格式、创作者和落地页？UTM 参数是否一致地追加？

**推荐的命名层级：**
1. 广告系列名：市场/垂直行业/目标
2. 广告组名：受众/定向类型
3. 广告名：创意变体/优惠/格式

示例：`[PROSP] US_Lookalike1%_Video_Purchase`

**UTM 要求：**
- 所有 URL 一致地添加参数
- 结构在各渠道、各时间段保持一致
- 命名能经受团队扩张和人员流动

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling

#### 5.3 社交证明保留

**检查什么：** 放量胜出广告时，团队是否保留了帖子 ID（Post ID）以保住积累的互动（点赞、评论、分享）？

**平台机制：** 从已有帖子创建广告会保留积累的互动；复制广告则从零开始积累社交证明。一条有 2,000 点赞、300 评论的广告会影响新观众的感知。复制导致的丢失代价很大。

**最佳实践：** 放量时，只要广告本身没变，始终复用帖子 ID。只有真正的新创意才建新广告。

**置信度：高**

来源：
- https://admanage.ai/blog/facebook-ads-audit

---

## 竞品分析方法与工具

### 免费方法

1. **Meta 广告资料库**（https://www.facebook.com/ads/library）——所有竞品研究的必备起点。
   - 按品牌名搜索，查看任何广告主的所有在投广告系列
   - 按产品类型搜索，发现该品类在投的品牌
   - 按关键词搜索，分析信息传达与季节性促销
   - 跑得最久的广告大概率是有效的——按投放时长做模式匹配
   - 查看品牌内容版块，看品牌与哪些创作者合作

2. **手动素材库**——截图在信息流中遇到的竞品广告。这能抓到广告资料库里看不到的再营销广告。

3. **LinkedIn 员工数追踪**——监控竞品团队规模与招聘动态（营销团队扩张 = 广告投入增加）。

4. **Google「site:」指令 + Wayback Machine**——追踪竞品落地页随时间的变化。

### 付费竞品情报工具

| 工具 | 适用场景 | 核心功能 |
|------|----------|-------------|
| AdSpy | 原始广告间谍、超大数据库 | 跨多平台搜索 |
| BigSpy | 跨平台广告监控 | 全网覆盖广 |
| Chromatic Labs | AI 驱动的创意分析 | 自动化的钩子、行动号召、视觉趋势分析 |
| AdStellar | 竞品到广告系列的端到端工作流 | 克隆竞品广告、生成变体、上线 |
| Motion | 创意分析 | 跨广告系列的效果趋势追踪 |
| Panoramata | 跨渠道竞品追踪 | 邮件、广告、落地页一站式 |
| Sensor Tower / Pathmatics | 企业级竞品情报 | 声量份额、花费预估 |
| MagicBrief | 创意库与简报 | 把竞品广告整理成可执行的简报 |

### 竞品分析中代理商看什么

1. **广告投放时长模式**——投放 14 天以上的广告大概率是经过验证的胜出者，值得研究
2. **格式分布**——竞品广告中视频、图片、轮播各占多少？
3. **创意主题**——竞品反复使用哪些信息传达角度？
4. **测试行为**——多条相似但有小差异的广告 = 在做主动 A/B 测试
5. **季节性模式**——竞品在 Q4、促销季如何调整？
6. **新广告系列上线**——过去 7 天有 3 个以上竞品上线新广告系列，预示 CPM 可能上涨

**置信度：高**——工具与方法在多个来源中有充分记录。

来源：
- https://dancingchicken.com/post/top-tools-for-meta-ads-competitor-analysis
- https://www.adstellar.ai/blog/competitor-ad-analysis-tools-for-meta
- https://pixis.ai/blog/how-to-analyze-meta-ads-competitors-quickly-and-accurately/
- https://midsummer.agency/blog/meta-ads-library/
- https://elevate-digital-solutions.com/best-competitor-research-and-analysis-tools/

---

## 代理商使用的业务分析框架

### SWOT 应用于付费社媒

在搭建广告系列策略之前，代理商先评估客户的定位：

- **优势（Strengths）：** 现有创意素材、品牌认知、客户数据、网站转化率、过往广告系列经验、邮件名单规模
- **劣势（Weaknesses）：** 无历史广告数据、落地页弱、创意素材有限、再营销受众小、合规限制
- **机会（Opportunities）：** 新受众细分、未测试的创意格式、季节性趋势、竞品空位、新兴版位（Reels、Messenger）
- **威胁（Threats）：** 竞品 CPM 压力、隐私政策变化、平台算法调整、市场饱和、广告成本上涨

### 收入影响框架

代理商梳理 Meta 广告与业务收入的连接：
1. **直接收入归因**——通过像素/CAPI 追踪到的购买
2. **助攻收入**——首次触达建立认知，后续通过其他渠道转化
3. **光环效应**——品牌认知广告系列带动自然搜索、直接流量和零售销量
4. **生命周期价值放大**——再营销与留存广告系列延长客户关系

### 单位经济模型分析

在设定任何目标之前，顶级代理商先算：
- **平均订单金额（AOV, Average Order Value）**——一笔典型交易值多少钱？
- **客户生命周期价值（LTV, Lifetime Value）**——一个客户在整个关系周期值多少钱？
- **毛利率**——收入中有多少百分比是利润？
- **最大可接受 CPA**——LTV x 毛利率 / 目标 ROAS
- **盈亏平衡 ROAS**——1 / 毛利率（例如 50% 毛利率 = 2.0 倍盈亏平衡 ROAS）

**置信度：中高**——业务分析框架在各代理商间标准化程度较低；综合自多个来源。

来源：
- https://sproutsocial.com/insights/social-media-swot-analysis/
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide
- https://www.abetheagency.com/guide/b2b-meta-benchmarks-for-facebook-advertising-services
- https://haus.io/blog/optimizing-meta-ads-a-playbook-for-brands

---

## 历史表现分析——哪些模式重要

### 多指标关联分析

孤立地看指标没有意义。代理商研究指标之间的关系来理解账户动态：

**示例模式（来自 CommonThread 的 Bambu Earth 案例）：**
- CPM 从 2020 年 8 月开始上涨（广告主竞争加剧）
- 点击率从 2020 年 12 月开始改善（创意开发更强）
- 尽管 CPM 上涨，CPC 保持稳定，因为点击率的提升抵消了成本上涨
- **洞察：** 创意改进可以中和外部成本压力

**要看的关键关系：**
- CPM（不可控）+ 点击率（可控）= CPC
- CPC + 转化率 = CPA
- CPA + 平均订单金额 = ROAS
- CPM 上涨 + 点击率稳定 = 创意在顶住市场压力
- CPM 上涨 + 点击率下跌 = 创意疲劳叠加成本压力
- CPC 稳定 + 转化率下跌 = 落地页或优惠问题，不是广告问题

### 时间段分析

**代理商在 90 天以上周期看什么：**
- 月度花费与效果的趋势
- 季节性模式（Q4 的 CPM 通常上涨 25–40%）
- 大改动后的学习期恢复模式
- 归因窗口随时间的一致性
- 预算消耗节奏模式（上一家代理商是否大手大脚花完就停？）

### 先行指标 vs. 滞后指标

**先行指标（预测未来表现）：**
- CPMr（千触达成本，Cost per 1,000 Accounts Reached）——CPMr 上涨预示 4–8 周后转化出问题
- 钩子率（3 秒视频观看 / 展示）——钩子率下降预示点击率下跌
- 新访客占比——低于 70% 预示效果衰退
- 创意疲劳信号——频次上涨 + 点击率下降

**滞后指标（确认已经发生的事）：**
- ROAS——告诉你发生了什么，不告诉你为什么
- CPA——多个上游变量的结果
- 收入——业务结果，不是广告系列诊断

**置信度：高**

来源：
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide
- https://stepondigital.com/facebook-ad-creative-audit-guide-4-step-framework-for-better-roas-in-2025/
- https://www.adstellar.ai/blog/facebook-ad-historical-data-analysis

---

## 代理商如何区分速赢与长期机会

### 速赢（第 1–7 天实施）

这些是「非受迫性失误」，可以立即修复：

1. **像素/追踪修复**——坏掉的像素、缺失的 CAPI、重复事件
2. **暂停浪费的广告系列**——花费超过 2 倍目标 CPA 且无改善趋势的广告组
3. **合并碎片化结构**——把 10 个广告系列合并成 2–3 个
4. **添加客户排除**——防止已有客户看到拉新广告
5. **修复受众重叠**——合并重叠的广告组
6. **修正归因设置**——确保各广告系列一致
7. **暂停疲劳创意**——冷受众频次超过 5.0 的广告
8. **修复出价上限**——移除过低的人为出价上限（有个账户 CPA 是 35 美元，出价上限却设了 5 美元）
9. **更新类似受众种子**——更新超过 60 天的种子
10. **修正特殊广告类别**——正确声明受监管行业

### 长期机会（第 2–12 周）

这些需要系统性工作：

1. **创意测试基础设施**——搭建可重复的测试框架
2. **全漏斗架构**——搭建合格的拉新/再营销/留存结构
3. **落地页优化**——改善点击后的体验
4. **受众扩张**——测试新细分、新国际市场
5. **广告格式多样化**——增加视频、Reels、轮播格式
6. **转化 API 实施**——如果服务端追踪还没上
7. **高级衡量**——实施增量测试、营销组合建模
8. **创意产能**——搭建持续创意更新的生产管线

**置信度：高**

来源：
- https://www.ecdigitalstrategy.com/blog/meta-ads-strategy-quick-wins/
- https://www.linkedin.com/posts/philkiel_last-week-i-made-an-offer-to-audit-meta-ad-activity-7391520378528956417-4dJ9
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide
- https://www.foxwelldigital.com/blog/how-we-approach-meta-ads-audits-a-strategic-framework-for-better-performance

---

## 审计报告格式与交付物

### 标准审计报告结构

1. **执行摘要**——1 页。账户健康评估。最重要的 3–5 个发现。建议的优先行动。

2. **数据完整性评估**——像素状态、CAPI 覆盖、事件映射、EMQ 得分、归因对齐。本节决定后续所有发现是否可信。

3. **账户结构分析**——广告系列组织、广告组数量、预算分布、学习期状态、命名规范。

4. **受众评估**——重叠分析、受众规模、种子新鲜度、排除策略、饱和信号。

5. **创意表现**——头部与尾部广告、疲劳指标、格式多样性、AIDA 指标分析。

6. **竞争格局**——竞品在跑什么、格式趋势、信息传达模式。

7. **历史表现趋势**——90 天以上的多指标分析。先行 vs. 滞后指标趋势。

8. **速赢**——按预期影响排序。可立即实施。

9. **战略建议**——30/60/90 天路线图。先修什么、建什么、测什么。

10. **附录**——原始数据导出、截图、详细指标表。

### 常见报告交付物

- PDF 或 Google Slides 演示文稿（用于面向客户的复盘）
- 含原始数据分析的电子表格（内部参考）
- 广告管理工具、事件管理工具、广告资料库的截图
- 建议改动的投前/投后预测
- 有负责人和截止日期的优先行动计划

### 审计报告最佳实践

- 每个发现标注三种状态之一：**健康**、**弱**、**坏了**
- 聚焦找出造成最大下游损害的 3–5 个问题
- 按优先级修复：先数据，再结构，再创意，再运营
- 尽量给出具体的金额影响（「修复像素去重预计让上报 CPA 降低约 15–20%」）
- 用视觉辅助呈现——指标表方便快速浏览，要点列表呈现建议

**置信度：中高**——报告格式因代理商而异；这里是各来源的常见模式。

来源：
- https://adespresso.com/blog/facebook-ads-audit-template/
- https://admanage.ai/blog/facebook-ads-audit
- https://www.foxwelldigital.com/blog/how-we-approach-meta-ads-audits-a-strategic-framework-for-better-performance
- https://whatagraph.com/templates/facebook-ads-report
- https://stepondigital.com/facebook-ad-creative-audit-guide-4-step-framework-for-better-roas-in-2025/

---

## 完整的 30 分钟起飞前审计清单

用于放量前检查或日常维护，Adligator 的框架每个版块 5 分钟、共六个版块：

| 版块 | 时长 | 关键检查 | 阈值 |
|---------|----------|------------|------------|
| 像素与追踪 | 5 分钟 | 像素活跃（24 小时）、EMQ 6.0 以上、CAPI 运行中、事件去重 | 收入差异 <15% |
| 广告系列结构 | 5 分钟 | 广告系列 <8 个、受众重叠 <30%、命名规范 | 学习受限已标记 |
| 创意表现 | 5 分钟 | 冷受众频次 <4.0、每广告组 3 条以上创意、点击率稳定 | 14 天以上无新创意 |
| 受众健康 | 5 分钟 | 种子新鲜度 <60 天、受众 >1,000 人、频次趋势 | 日触达 10% 以上 = 饱和 |
| 预算与出价 | 5 分钟 | 花费节奏 >80%、CBO 分布均衡、已设上限 | CBO 集中度 >85% |
| 竞争格局 | 5 分钟 | 监控新上线、追踪格式变化、投放 14 天以上的胜出者 | 3 个以上竞品上线 = CPM 上涨 |

**置信度：高**

来源：
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling

---

## 值得深入调查的线索

1. **差异模式分析**——Foxwell Digital 识别出的常见差异模式（平台数据强/实际收入弱、平台数据弱/实际收入强），可以系统化为诊断工具。

2. **CPMr 作为先行指标**——StepOn Digital 发现 CPMr 上涨预示 4–8 周后转化出问题，值得为预测性审计方法深入研究。

3. **创意优先的审计趋势**——多个来源（Jon Loomer、Phil Kiel）表明，对于结构已经简化的成熟账户，广告层级的创意分析现在比广告系列/广告组层级的分析更重要。

4. **AI 增强的审计工具**——AdManage.ai、AdAmigo.ai、Markifact 等平台正在自动化审计流程。了解它们的能力边界，可以看清审计中哪些环节真正可被商品化、哪些仍需要人的判断。

5. **新访客占比指标**——Phil Kiel 发现新访客占比低于 70% 的账户出现效果衰退，这个诊断指标很有力。值得研究各行业的最佳比例。

6. **浏览归因虚增**——有账户移除浏览 1 天归因后 ROAS 下降 50%，这说明全行业存在系统性高估。值得深入研究代理商如何向客户披露这个问题。
