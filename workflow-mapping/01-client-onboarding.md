# Meta 广告代理商：客户入驻流程

## 概述

当客户签约 Meta 广告代理商时，入驻期是整个合作周期中杠杆最高的一段时间。研究一致表明，第二、三个月出现的问题——关于效果的分歧、范围蔓延、数据质量问题——其根源都在第一周。Meta 广告的入驻流程与其他营销服务不同，因为 Meta 的生态系统需要特定的技术配置：像素（Pixel）安装、转化 API（Conversions API, CAPI）设置、商品目录连接、受众分享权限以及商务管理平台（Business Manager）合作伙伴访问权限，每一项都需要在投放前完成验证，才能让广告系列发挥最佳效果。

**置信度：高**——多个代理商指南一致支持，包括 OrangeTrail 的 2026 完整指南、Wevion 的分步框架和 Synup 的 8 步清单。

来源：
- https://orangetrail.io/blog/the-ultimate-guide-to-facebook-agency-ad-accounts/
- https://wevion.ai/en/blog/facebook-ads-agency-client-onboarding/
- https://synpost.synup.com/client-onboarding-process-for-marketing-agencies/
- https://functionpoint.com/blog/mastering-client-onboarding-a-comprehensive-checklist-for-your-agency
- https://almcorp.com/blog/client-onboarding-checklist-digital-agencies/

---

## 五阶段入驻框架

顶级代理商遵循结构化的五阶段流程，从签约到广告系列上线大约需要 21 天。

### 阶段 1：发现（第 1–2 天）
- 填写入驻问卷
- 初始资料审核与澄清
- 提交访问权限申请（商务管理平台合作伙伴访问）
- 启动品牌素材收集

### 阶段 2：策略（第 3–5 天）
- 安排并执行策略会议（60–90 分钟）
- 记录目标与 KPI 对齐情况
- 制定广告系列路线图
- 竞争格局复盘

### 阶段 3：设置（第 6–10 天）
- 验证商务管理平台配置
- 安装像素并测试事件
- 验证转化 API（CAPI）的实施情况
- 受众研究与初始受众创建

### 阶段 4：创意（第 11–15 天）
- 素材收集与质量审核
- 开发广告创意
- 撰写文案并审批信息传达
- 针对版位（动态 Feed、快拍、Reels）优化素材格式

### 阶段 5：上线（第 16–21 天）
- 搭建广告系列并做质量检查（QA）测试
- 客户终审与签字确认
- 上线，进入初始学习期
- 第一份效果报告（上线后第一周结束时）

**置信度：高**——该框架记录于 OrangeTrail 的 2026 代理商指南，并得到 Wevion 五步流程的佐证。

来源：
- https://orangetrail.io/blog/the-ultimate-guide-to-facebook-agency-ad-accounts/
- https://wevion.ai/en/blog/facebook-ads-agency-client-onboarding/

---

## 账户访问与权限设置

### 商务管理平台合作伙伴访问

代理商与客户之间访问权限的正确方式是**合作伙伴访问（partner access）**，而不是共享个人账号登录。这可以在授予代理商运营权限的同时，保留客户对其所有资产的所有权。

**分步流程：**

1. 客户在 Meta Business Suite 中进入「商务设置」
2. 客户从左侧导航菜单中选择「合作伙伴」（用户版块下方）
3. 客户点击「添加」，选择「向合作伙伴提供对资产的访问权限」
4. 客户输入代理商的商务管理平台 ID（代理商提供的 16 位数字）
5. 客户选择要分享的资产：广告账户、公共主页、像素、商品目录
6. 客户为每项资产分配权限级别
7. 客户确认请求
8. 代理商从自己的商务管理平台接受该请求

**关键区别：**「认领（claim）」资产会转移所有权（将资产从原所有者名下移除）。「请求访问」或「分享」保留原始所有权，仅授予特定权限。代理商应始终使用合作伙伴访问，绝不能认领客户的资产。

**置信度：高**——该流程记录于 Meta 官方帮助文档，并得到多个代理商指南的确认。

来源：
- https://www.facebook.com/business/help/1717412048538897
- https://tj21.com/how-to-share-partner-access-with-an-agency-in-metas-business-portfolio-without-losing-control-of-your-assets/
- https://almcorp.com/blog/facebook-business-manager-guide/
- https://mddcadservices.com/you-guide-to-onboarding-your-marketing-agency-for-meta-advertising/

### 权限级别与角色

| 角色 | 推荐访问权限 | 适用对象 |
|------|-------------------|-------------|
| 管理员（Admin） | 完全控制商务管理平台设置、用户、资产、账单 | 限于 2–3 名客户方高层 |
| 员工（Employee） | 仅访问被分配的资产，无商务管理平台级控制权 | 大多数内部团队成员 |
| 合作伙伴（Partner，代理商） | 仅限被分配的特定资产的临时访问 | 代理商——所有代理合作的标准做法 |
| 分析员（Analyst） | 仅查看报告 | 需要数据但无需操作权限的专员 |
| 广告主（Advertiser） | 创建/编辑广告系列，查看效果数据 | 代理商的标准需求 |
| 公共主页管理员（Page Admin） | 管理公共主页内容 | 互动型广告系列必需 |
| 财务（Finance） | 仅查看账单 | 客户方财务团队 |

**最佳实践原则：** 只授予完成工作所需的最小可行权限。如果某人不需要完全控制权也能履行职责，就不应该给他完全控制权。合作伙伴访问会自动过期，并为外部访问提供审计追踪。

**置信度：高**——记录于 Meta 官方角色结构和多个代理商指南。

来源：
- https://www.get-ryze.ai/blog/facebook-buisness-manager
- https://almcorp.com/blog/facebook-business-manager-guide/
- https://griffinwink.com/meta-business-manager-how-to-grant-account-access-to-your-marketing-team/

### 需要申请访问的资产

除广告账户外，代理商通常还需要访问：

- **Facebook 公共主页**——用于创建广告和互动型广告系列
- **Instagram 账号**——用于 Instagram 版位
- **Meta 像素/数据集**——追踪与优化的必备项，不可协商
- **商品目录（Product Catalogue）**——电商动态商品广告所需
- **自定义受众（Custom Audiences）**——再营销广告系列所需
- **Google Analytics / GA4**——跨平台归因
- **CRM 或邮件平台**——线索类客户（用于追踪线索质量）
- **电商平台后台**（Shopify、WooCommerce）——转化追踪验证
- **落地页搭建工具**——涉及落地页优化工作时
- **Google Tag Manager**——像素与追踪管理

**置信度：高**——在各代理商入驻指南中被一致列出。

来源：
- https://synpost.synup.com/client-onboarding-process-for-marketing-agencies/
- https://5day.io/blog/client-onboarding-for-marketing-agencies/
- https://wevion.ai/en/blog/facebook-ads-agency-client-onboarding/

---

## 商务管理平台配置与验证

### 企业验证

Meta 要求完成企业验证才能使用完整功能。截至 2026 年，Meta 的目标是 90% 的广告收入来自已验证的广告主（2026 年 3 月时为 70%）。未验证的账户会面临：
- 更低的消费限额（未验证的新账户每天 25–50 美元，已验证的代理商账户每天 5,000 美元以上）
- 部分广告功能受限
- 账户被限制的风险更高
- 账单选项有限

**验证流程：**
1. 进入商务设置中的「安全中心」
2. 提交企业法定名称、地址、电话号码
3. 提供验证文件（营业执照、水电费账单等）
4. Meta 审核（通常 1–5 个工作日）
5. 验证通过后解锁高级工具和更高的消费门槛

**关键风险：** 验证绝不能依赖某个人的个人凭证。如果验证使用的是某位前员工的登录账号，而该员工离职，整个账户可能无法访问。

**置信度：高**——记录于 Meta 官方指南和多个行业来源。

来源：
- https://www.get-ryze.ai/blog/facebook-buisness-manager
- https://admanage.ai/blog/facebook-ads-audit
- https://www.stackmatix.com/blog/meta-agency-ad-account

### 资产组织最佳实践

- 在商务管理平台内按逻辑分组相关资产
- 多品牌公司可考虑为每个品牌设立独立的商务管理平台
- 代理商：把客户资产放在客户自有的商务管理平台中，而不是代理商自有的
- 使用商务资产组（Business Asset Groups）按服务类型、地区或品牌组织资产
- 为所有管理员用户启用双重验证
- 清晰记录所有资产的所有权

**代理商常犯的错误：** 把客户的基础设施建在代理商自己的商务管理平台里。这会在合作结束时造成「客户资产回收难题」。核心资产的所有权应始终归客户所有；代理商通过合作伙伴访问获得权限。

**置信度：高**——所有来源都将其作为基本最佳实践强力推荐。

来源：
- https://almcorp.com/blog/facebook-business-manager-guide/
- https://whitebunnie.com/blog/what-is-a-meta-business-portfolio-how-to-structure-it-properly/
- https://tj21.com/how-to-share-partner-access-with-an-agency-in-metas-business-portfolio-without-losing-control-of-your-assets/

---

## 存量账户审计流程

接手已有 Meta 广告账户的客户时，代理商会在做出任何改动之前先进行基线审计。这份审计有两个目的：(1) 了解之前做过什么；(2) 记录继承的问题，避免这些问题被归咎于代理商的表现。

### 初始审计中代理商的检查要点

**像素与转化追踪（首要优先级）：**
- 确认像素处于活跃状态且有近期事件活动（最近 24 小时）
- 检查事件匹配质量（Event Match Quality）得分——目标 10 分制中 6.0 以上；低于 5.0 意味着严重的数据丢失
- 确认转化 API（CAPI）与浏览器像素并行运行（iOS 14 之后为强制要求）
- 验证正确的事件被触发（购买 Purchase、线索 Lead、发起结账 InitiateCheckout、浏览内容 ViewContent、加购 AddToCart）
- 检查像素与 CAPI 之间的事件去重（event_id 必须一致）
- 验证测试事件的值和货币设置
- 将 Facebook 上报的收入与后端数据对比（差异在 15% 以内为健康；超过 25% 说明追踪有问题）
- 检查加购与购买的比率（10 倍为正常；100 倍说明追踪坏了）

**账户健康检查：**
- 账户上的政策违规或限制
- 账单联系人与付款方式状态
- 之前的广告拒登及其解决情况
- 可能限制广告系列投放的消费限额
- 企业验证状态

**历史广告系列复盘：**
- 至少 90 天的月度花费与效果
- 广告系列命名规范与组织结构
- 各广告系列的归因设置是否一致
- 标记可能表明追踪问题的异常结果
- 识别值得保留 vs. 应暂停的广告系列

**受众评估：**
- 客户名单受众——新鲜度与匹配率
- 网站再营销受众——规模与像素功能
- 互动受众——视频观看者、公共主页互动者及其人群规模
- 类似受众（Lookalike）的种子质量与新鲜度（超过 60 天的种子需要更新）
- 广告组之间的受众重叠（超过 30% 意味着自己在和自己竞价）

**置信度：高**——该审计框架记录于 Wevion、AdManage、Adligator、Foxwell Digital 和 CommonThread。

来源：
- https://wevion.ai/en/blog/facebook-ads-agency-client-onboarding/
- https://admanage.ai/blog/facebook-ads-audit
- https://adligator.com/blog/facebook-ad-account-audit-checklist-before-scaling
- https://www.foxwelldigital.com/blog/how-we-approach-meta-ads-audits-a-strategic-framework-for-better-performance
- https://commonthreadco.com/blogs/coachs-corner/facebook-ads-audit-guide

---

## 品牌/产品沉浸——代理商如何深入了解客户的业务

在碰广告账户之前，顶级代理商会投入大量时间从根本上理解客户的业务：

### 研究阶段（问卷之前）
- 在网上研究公司与行业
- 评估现有营销工作，找出改进空间
- 研究竞争对手以积累背景信息
- 查看客户网站、社媒账号、内容与品牌形象
- 记录公开可得的公司文化、领导团队与市场定位信息

### 策略会议深度访谈（60–90 分钟）
结构化的会议覆盖：

1. **相互介绍**（5 分钟）——双方团队成员、角色、决策权
2. **业务深挖**（20 分钟）——产品/服务、利润率、竞争格局、季节性、商业模式
3. **目标与 KPI**（15 分钟）——30/60/90 天成功的样子、可接受的获客成本、客户生命周期价值
4. **受众与创意**（15 分钟）——买家画像、过往创意中的胜者/败者、品牌语调与规范
5. **技术复盘**（10 分钟）——当前追踪设置、网站性能、转化漏斗
6. **下一步**（10 分钟）——时间表、交付物、沟通节奏

**策略会议上代理商会问的关键问题：**
- 「90 天内成功是什么样子？」
- 「你们的平均客户生命周期价值和可接受的获客成本是多少？」
- 「过去什么有效？什么失败了？」
- 「你们的理想客户是谁？他们在网上哪里活动？」
- 「你们的旺季和淡季分别是什么时候？」
- 「你们的行业有哪些合规或监管限制？」

**置信度：高**——记录于多个代理商入驻框架。

来源：
- https://orangetrail.io/blog/the-ultimate-guide-to-facebook-agency-ad-accounts/
- https://functionpoint.com/blog/mastering-client-onboarding-a-comprehensive-checklist-for-your-agency
- https://5day.io/blog/client-onboarding-for-marketing-agencies/

---

## 入驻问卷——顶级代理商会问什么

### 业务基本情况
1. 你们的公司名称、网站 URL 和所属行业是什么？
2. 你们提供什么产品/服务？最畅销的是什么？
3. 主要产品/服务的利润率是多少？
4. 你们的 3–5 个主要竞争对手是谁？
5. 你们公司的独特卖点是什么？
6. 详细描述你们的理想客户（人口属性、心理属性、痛点）
7. 从认知到购买的典型客户旅程是什么样的？
8. 你们的平均订单金额（电商）或客单价（线索类）是多少？
9. 预估的客户生命周期价值是多少？

### 营销历史
10. 以前做过哪些营销活动，哪些效果最好？
11. 尝试过哪些没效果的营销活动，为什么？
12. 当前每月营销预算是多少，如何分配？
13. 请提供所有现有数字营销账户的访问权限（Google Analytics、Meta 商务管理平台等）
14. 有现成的 CRM 吗？如果有，是哪个平台？
15. 使用什么邮件营销平台？

### Meta 相关问题
16. 你们的主要转化事件是什么？（购买、线索、注册等）
17. 以前投过 Meta 广告吗？投了多久、预算多少？
18. 有现成的自定义受众、类似受众或再营销人群吗？
19. Meta 像素是否已安装？转化 API（CAPI）是否已设置？
20. 是否在商务管理中心（Commerce Manager）设置了商品目录？
21. 是否有行业特定的广告政策限制？（金融、医疗、酒精、减肥等）
22. 是否遇到过账户限制或广告拒登？

### 品牌与创意
23. 有现成的品牌规范吗（Logo、颜色、字体、语调）？
24. 在哪里可以找到过去广告系列中表现最好的创意素材？
25. 有哪些话题、图片或信息传达方式是禁区？
26. 有视频内容吗？有 UGC（用户生成内容）吗？

### 运营与沟通
27. 你们的主要对接人是谁？
28. 谁有权审批广告创意和广告系列改动？
29. 偏好的沟通方式是什么（Slack、邮件、电话）？
30. 希望多久收到一次进度更新？
31. 当我们需要你们提供东西时，现实的回复周期是多久？

### 成功指标
32. 下个季度最重要的 3 个业务目标是什么？
33. 目前如何衡量营销成功？
34. 对你们的业务来说，理想的单次获客成本（CPA, Cost Per Acquisition）是什么样子？
35. 多少的广告支出回报率（ROAS, Return on Ad Spend）或投资回报率（ROI, Return on Investment）算成功？

**置信度：高**——综合自 AgencyAnalytics（37 个问题）、DashClicks（20 个问题）、Leadsie（27 个问题）和 OrangeTrail 的框架。

来源：
- https://agencyanalytics.com/blog/client-onboarding-questionnaire
- https://www.dashclicks.com/blog/client-onboarding-questionnaire
- https://www.leadsie.com/blog/client-onboarding-questionnaire
- https://orangetrail.io/blog/the-ultimate-guide-to-facebook-agency-ad-accounts/
- https://www.connexify.io/blog/social-media-client-onboarding-questionnaire-free-template

---

## 时间表：第 1 周 vs. 第 2 周

### 第 1 周：打基础

| 日期 | 工作 | 负责人 |
|-----|----------|-------|
| 第 1 天 | 欢迎邮件 + 介绍团队；发送入驻问卷；请求商务管理平台合作伙伴访问 | 代理商 |
| 第 1–2 天 | 客户填写问卷；客户向所有必需资产授予商务管理平台访问权限 | 客户 |
| 第 2–3 天 | 验证所有访问权限已收到；运行基线审计（像素、追踪、账户健康、历史表现） | 代理商 |
| 第 3 天 | 策略会议（60–90 分钟）：目标、KPI、受众、创意方向 | 双方 |
| 第 3 天 | 书面记录目标对齐：主要 KPI、目标值、月度预算、90 天合作结构 | 代理商 |
| 第 4–5 天 | 账户与工具设置：团队角色、告警、报告模板、命名规范 | 代理商 |
| 第 5 天 | 第一波学习型广告系列上线（受众验证、创意基线、漏斗完整性） | 代理商 |
| 第 1 周末 | 初始效果报告 | 代理商 |

### 第 2 周：搭建

| 日期 | 工作 | 负责人 |
|-----|----------|-------|
| 第 6–8 天 | 搭建广告系列架构：全漏斗结构（拉新、再营销、重定向） | 代理商 |
| 第 8–10 天 | 受众研究与创建：自定义受众、类似受众、兴趣叠加 | 代理商 |
| 第 9–12 天 | 创意开发：广告文案、图片/视频素材、格式变体（Feed、快拍、Reels） | 代理商 |
| 第 12–14 天 | 内部 QA：广告系列复核、追踪验证、命名规范检查 | 代理商 |
| 第 2 周末 | 客户复核广告系列设置与创意并审批 | 双方 |

### 第 3 周：上线

| 日期 | 工作 | 负责人 |
|-----|----------|-------|
| 第 15–16 天 | 客户对广告系列做最终签字确认 | 客户 |
| 第 16–17 天 | 广告系列上线，设定学习期预期 | 代理商 |
| 第 17–21 天 | 每日监控、初始优化、学习期管理 | 代理商 |
| 第 21 天 | 第一次完整效果复盘会议 | 双方 |

**置信度：高**——时间表结构在 OrangeTrail、Wevion、Synup 和 ALM Corp 的框架中保持一致。

来源：
- https://orangetrail.io/blog/the-ultimate-guide-to-facebook-agency-ad-accounts/
- https://wevion.ai/en/blog/facebook-ads-agency-client-onboarding/
- https://synpost.synup.com/client-onboarding-process-for-marketing-agencies/
- https://almcorp.com/blog/client-onboarding-checklist-digital-agencies/

---

## 常见入驻错误及顶级代理商的规避方法

### 错误 1：急于上线
**后果：** 代理商为了赶某个上线日期而跳过发现阶段。基于不完整信息搭建的广告系列表现不佳。
**纠正：** 遵循结构化时间表。绝不跳过发现阶段。向客户设定预期：2–3 周的准备期是标准做法，是在保护他们的投入。

### 错误 2：KPI 约定模糊
**后果：** 客户说「提高销量」。没有具体指标、目标或时间表书面记录。三个月后，双方对合作是否成功产生争议。
**纠正：** 书面记录具体指标、目标和时间表。「90 天内以 1 万美元月花费实现 4 倍 ROAS」是 KPI。「提高销量」不是。

### 错误 3：访问权限收集不完整
**后果：** 代理商在没有拿到全部所需权限的情况下开工。延误层层传导。启动会议的一半时间花在排查客户为什么在设置里找不到「访问权限与安全」上。
**纠正：** 使用完整的访问权限清单。在所有必需权限授予并验证之前，不要进入设置阶段。把权限收集和启动会议分开。

### 错误 4：默认客户懂行
**后果：** 客户不了解商务管理平台、像素或 Meta 广告的运作方式。他们因为从未被教育过而做出糟糕的决策。
**纠正：** 把流程讲清楚。提供教育资料。客户不知道自己不知道的 Meta 广告知识。

### 错误 5：没有建立沟通节奏
**后果：** 客户期待每日更新；代理商只提供周报。或者反过来。双方的挫败感都在累积。
**纠正：** 前期就确定报告时间表和响应预期。代理商多久回复邮件？更新什么时候发？把会议排进日历。

### 错误 6：忽视合规要求
**后果：** 代理商为金融服务客户投放广告时未声明特殊广告类别（Special Ad Category）。广告被拒登。账户被限制。
**纠正：** 早期识别行业限制。金融、医疗、酒精、减肥——每个行业都有特定的 Meta 政策要求，会影响定向和创意。

### 错误 7：没有把一切都记录下来
**后果：** 客户忘记自己批过什么。改动没有留下书面记录。产生争议。
**纠正：** 记录所有决策、审批和策略变更。当客户忘记自己批过什么时，文档保护所有人。

### 错误 8：接手坏掉的账户却没有记录
**后果：** 代理商接手一个像素配置错误、受众陈旧、有政策违规的账户。三个月后，客户把合作开始前就存在的问题归咎于代理商。
**纠正：** 做彻底的基线审计，在做出改动前书面记录每一个继承的问题。把审计发现书面分享给客户。

### 错误 9：在启动会议上收集访问权限
**后果：** 启动会议本该讨论策略、目标、KPI 和竞争格局，却变成了商务管理平台导航的技术支持会。
**纠正：** 把权限收集和策略启动会分开。会前发送访问权限指引（含 Loom 视频演示）。会议只用于战略对齐。

### 错误 10：每个客户都从零开始
**后果：** 客户经理复制一封旧的入驻邮件，改几个平台名称就发出。指引里引用的 Meta 商务管理平台界面是六周前的旧版。客户在第 3 步就卡住了。
**纠正：** 按服务类型标准化。为 PPC 客户、SEO 客户、电商品牌各建一套入驻流程。记录每种类型适用的平台、权限级别和入驻问题。

**置信度：高**——这些错误在 Leadsie、OrangeTrail、AgencyAccess 和 Wevion 的记录中保持一致。

来源：
- https://orangetrail.io/blog/the-ultimate-guide-to-facebook-agency-ad-accounts/
- https://www.leadsie.com/blog/client-onboarding-mistakes
- https://www.agencyaccess.co/blog/agency-client-onboarding-best-practices
- https://wevion.ai/en/blog/facebook-ads-agency-client-onboarding/
- https://altosagency.com/blog/article/onboarding-social-media-clients

---

## 法律：合同、保密协议（NDA）、服务水平协议（SLA）

### 标准代理商合作协议的组成部分

1. **签约方**——代理商与客户双方的法定全称、地址和联系方式。

2. **服务范围**——具体描述代理商会做什么、不会做什么。例如：「代理商将管理 Meta 广告系列，包括策略、创意开发、广告系列搭建、优化和报告。代理商不管理 Google Ads、SEO 或自然社媒内容。」

3. **合作期限**——通常初始合作期最少 3 个月，之后按月续约或按年续约。3 个月是最少期限是标准做法，因为第一个月建基线、第二个月优化、第三个月放量。

4. **服务费/报酬**——三种常见结构：
   - **固定月费**，覆盖约定的工作范围
   - **按广告花费提成**（通常 10–20%）
   - **混合制**——基础服务费 + 与 KPI 挂钩的绩效奖金

5. **客户责任**——客户必须提供的：及时的访问权限、创意素材、审批回复周期、预算承诺，以及指定的对接人。

6. **知识产权**——客户拥有为其创建并已付费的内容。未采用或被否决的方案归代理商所有。代理商有权将作品用于作品集（受 NDA 约束的除外）。

7. **保密/保密协议（NDA）**——双方同意对专有信息保密。具体覆盖广告账户数据、效果指标、客户数据和业务策略。

8. **终止条款**——通常任一方可提前 30 天书面通知终止。终止时：客户承担所有不可取消的合同，代理商移交所有访问权限和材料。

9. **责任与赔偿**——代理商的责任限于合同项下提供的服务。客户就其产品或服务引发的索赔对代理商进行赔偿。

10. **合规**——代理商承诺遵守所有适用法律法规，尤其是与数字广告和数据隐私相关的（GDPR、CCPA）。

### 服务水平协议（SLA）的要素

正式化 SLA 的代理商通常包括：

- **响应时间：**「所有通过我们项目管理平台提交的新客户需求，我们将在 2 个工作日内确认收到。」
- **会后纪要：**「客户经理将在会后 1 个工作日内发送会议纪要。」
- **报告节奏：** 每周效果快照、每月详细报告、每季度战略复盘。
- **反馈回复周期：**「客户将在 3 个工作日内提供反馈。如需更长时间，请在 1 个工作日内提出，以便代理商调整项目排期。」
- **上线后支持：**「上线后 4 周内免费提供范围内的保修支持。」
- **升级通道：**「如客户需要更快响应，请通过[升级通道]提交，代理商将在 3 个工作小时内响应；将收取最低加急费。」

### Meta 相关的合同补充条款

- **广告花费责任：** 明确谁直接向 Meta 支付广告花费（通常是客户直接付给 Meta；代理商负责管理）
- **账户所有权：** 明确声明客户拥有所有广告账户、公共主页、像素和受众
- **效果免责声明：** 由于平台算法变化、市场环境和竞争，不保证具体效果
- **数据处理：** 线索类广告系列中客户数据的存储、分享与保护方式
- **平台合规：** 代理商有责任遵守 Meta 的广告政策

**置信度：中高**——综合自合同模板库、代理商 SLA 指南和 FUZE Agency 等代理商的具体 Meta 广告条款。

来源：
- https://esign.com/employment/independent-contractor/retainer/advertising-agency/
- https://legaltemplates.net/form/employment-contract/independent-contractor/consulting/retainer/advertising-agency/
- https://www.teamwork.com/blog/retainer-agreement-template/
- https://sakasandcompany.com/agency-service-level-agreements/
- https://fuzeagency.co.uk/terms-and-conditions/meta-ads-terms
- https://mccmeetingspublic.blob.core.usgovcloudapi.net/daltga-meet-92b0878f53e54672a4ced08c48cd0e05/ITEM-Attachment-001-b560fee8f4304170bff465c636d66c12.pdf

---

## 入驻期间使用的工具

### 第 1 天（免费/内置）
- **Meta 商务管理平台**——资产管理、合作伙伴访问、权限
- **Meta 事件管理工具（Events Manager）**——像素验证、事件测试、CAPI 验证
- **Meta 广告管理工具（Ads Manager）**——广告系列管理、报告
- **Meta 广告资料库（Ad Library）**——竞品研究（免费、公开）
- **Google Analytics / GA4**——跨平台归因
- **Google Tag Manager**——追踪管理
- **Loom**——客户访问权限指引的视频演示
- **Google Forms / Typeform**——入驻问卷

### 专用工具（预算允许时）
- **Leadsie**——用一个链接在 31+ 平台自动完成访问权限申请
- **Whatagraph / AgencyAnalytics**——自动化的客户报告看板
- **Triple Whale / Northbeam**——超越 Meta 报告的归因与衡量
- **Motion**——创意分析与效果追踪
- **AdSpy / BigSpy / Adligator**——竞品广告情报
- **1Password / LastPass**——安全的凭证共享

**置信度：高**——各代理商入驻指南中一致提到的工具。

来源：
- https://www.leadsie.com/blog/essential-steps-for-agency-client-onboarding
- https://agencyanalytics.com/blog/client-onboarding-questionnaire
- https://www.agencyaccess.co/blog/agency-client-onboarding-best-practices

---

## 值得深入调查的线索

1. **Leadsie 声称通过自动化访问权限申请将入驻周期缩短 50%**——值得调查该工具是否真的消除了拖慢大多数入驻流程的反复沟通。

2. **从商务管理平台（Business Manager）到企业资产组合（Business Portfolio）的更名**——Meta 正在更名和重组其企业管理工具。有些指南仍引用「商务管理平台」，而 Meta 官方文档现在使用「企业资产组合」。值得追踪这对入驻指引的影响。

3. **90 天已验证广告主目标**——Meta 推动到 2026 年底 90% 的广告收入来自已验证广告主，这可能迫使未验证企业在验证变为强制要求之前完成验证。

4. **转化 API（CAPI）的采用率**——多个来源表明许多广告主仍只依赖浏览器端像素追踪。了解采用率有助于把 CAPI 设置包装成入驻期间的增值服务。

5. **入驻自动化平台**——Synup OS、ClickUp 和 Monday.com 等工具正在打造代理商专用的入驻工作流。了解自动化格局可能发现产品化机会。
