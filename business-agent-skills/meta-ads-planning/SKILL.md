---
name: meta-ads-planning
description: "端到端规划与设计 Meta 广告投放方案，覆盖 Facebook、Instagram、Messenger、Threads、Audience Network 与 WhatsApp：选择广告目标、转化位置、表现目标、Advantage+ 自动化、广告系列结构、创意需求、版位、Pixel/CAPI/App/CRM 衡量、诊断与运营指令。当用户提到 Meta 广告、Facebook 广告、Instagram 广告、Meta 付费社交、Advantage+ Sales/Leads/App、线索广告、Reels 广告、App 推广、目录广告或 Meta 广告系列规划时使用本 skill。与付费媒体平台无关时跳过。"
---

# Meta 广告规划

端到端规划 Meta 广告方案：目标选择、广告系列架构、Advantage+ 姿态、创意体系、衡量设计、预算可行性、诊断以及上线/运营决策。本 skill 同时覆盖新账户上线与老账户改进。

## 实践优先立场

Meta 广告规划应优先保证让广告系列真正跑起来的实践，而不只是让广告系列"能上线"的设置。广告目标、版位和出价策略重要，但它们次于衡量质量、转化信号设计、创意质量、账户结构、预算充足度、客户控制和严格的运营节奏。

在提广告系列设置之前，先检查这些：

| 领域 | 要核实什么 | 为什么重要 |
|---|---|---|
| 转化设计 | Pixel/CAPI、App SDK/MMP、线下/CRM 回传、去重、事件深度、价值 | 信号错了，投放就学错结果 |
| 增量 | 新客 vs 老客、再营销偏置、浏览归因/互动归因占比、财务对账口径 | 平台 ROAS 是 steering 信号，不是业务真相 |
| 创意体系 | 差异化概念、钩子、证明、卖点、格式适配、刷新节奏 | 创意是塑造受众与说服的主要杠杆 |
| 预算与量 | 目标 CPA/ROAS 是否现实、预期事件量、预算-CPA 比 | 预算不足的广告组/广告系列学出来全是噪声 |
| 结构 | 合并度、拆分逻辑、受众重叠、预算控制需求 | 碎片化分散信号、放大波动 |
| 落地页 | 落地页、即时表单、结账页、应用商店、目录、私信流程、卖点匹配 | 广告放大的是落地页 |
| 控制项 | 客户排除/上限、地域/语言/年龄、特殊广告类别、品牌安全 | 自动化需要真实的业务护栏 |
| 运营节奏 | 学习窗口、改动日志、创意测试节奏、衡量复盘 | 多数失败源于疏于管理或过度干预 |

### 核心运营规则

- 从业务结果和单位经济效益出发，而不是从 Ads Manager 菜单出发。
- 把**广告目标 + 转化位置 + 表现目标 + 优化事件**当作战略本身。目标不是报告标签，它在指挥投放。
- 优先用最深层可靠、且量足够、延迟可接受的事件。弱的微事件只作临时或次要，除非它能预测下游质量。
- 区分平台表现与增量。再营销、浏览归因、建模转化、互动归因和老客投放都会夸大业务影响。
- 为学习而合并；只为真实控制需求拆分：目标、预算、经济效益、地域/语言、合规、客户类型、漏斗角色、创意测试或衡量。
- 当信号和创意质量撑得住时，默认用宽泛/Advantage+ 投放姿态，但真实业务约束要保留为控制项。
- 把创意当作主要杠杆。测试差异化概念，不做表面变体；Advantage+ 创意是人类策略的放大器，不是定位的替代品。
- 下游质量和增量可接受之前，不追便宜的 CPM、点击、互动、安装或原始线索。
- 避免频繁干预。批量做有意义的改动，等学习期、转化延迟和统计噪声沉淀后再判读。

### 节奏

除非账户情况另有要求，用以下默认运营节奏：

| 节奏 | 重点 | 避免 |
|---|---|---|
| 每日 | 花费异常、追踪掉量、拒审、目录/商品流报错、线索/App 事件中断 | 每天改出价/目标 |
| 每周 | 创意疲劳、线索/商品/App 质量、预算节奏、客户/受众拆解 | 凭几天噪声就重构 |
| 双周 | 创意测试判读、新概念上线、表单/落地页/商店诊断 | 无战略问题的表面变体 |
| 每月 | 结构复盘、屏蔽名单、CAPI/EMQ 健康、归因/报告检查 | 让上线时的假设一直沿用 |
| 每季度 | 增量复盘、事件设计、Advantage+ vs 手动分工、业务模式战略 | 只报告平台 ROAS |

### 衡量说明

- Meta 指标用于战术优化，不作最终财务真相。
- 尽量让点击归因、互动归因、浏览归因、建模转化、新客、老客的假设可见。
- 用订单、CRM、POS、App 营收、商机池、LTV、贡献毛利或其他财务口径对账。
- 监控新客 vs 老客的贡献，尤其在 Advantage+ Sales 和再营销重的结构里。
- 预算和量允许时，用 Meta 实验、A/B 测试、转化提升（conversion lift）、地域对照组、CRM/客户对照组、MMM 或前后对比分析。
- 把报告工具/API 变化当作有时效的实施细节；在写死归因窗口声明前，先核实现行 Ads Manager/API 行为。

### 常用 Meta 广告术语

用这些统一术语，不在每个 playbook 里重复基础定义。

| 术语 | 含义 |
|---|---|
| 广告目标（Objective） | Awareness、Traffic、Engagement、Leads、App promotion 或 Sales |
| 转化位置（Conversion location） | 网站、App、即时表单、来电、Messenger、Instagram、WhatsApp、网站+App，或线下门店/线下路径（视目标而定） |
| 表现目标（Performance goal） | 目标内 Meta 优化的结果，如购买、线索、对话、触达、落地页浏览、安装或价值 |
| 优化事件（Optimization event） | 投放学习用的具体事件：Purchase、Lead、CompleteRegistration、App 事件、合格线索等 |
| Pixel | 浏览器端事件来源 |
| CAPI | 转化 API（Conversions API）服务器端事件来源 |
| 去重（Deduplication） | 用相同事件名和 `event_id` 匹配 Pixel 与 CAPI 事件 |
| EMQ | 事件匹配质量（Event Match Quality）诊断，检查匹配参数 |
| AEM | 汇总事件衡量（Aggregated Event Measurement）；网页/App 行为随时间变化，必须核实 |
| MMP | 移动衡量合作伙伴（Mobile Measurement Partner），如 AppsFlyer、Adjust、Singular、Branch 或 Kochava |
| SKAN / AAK | SKAdNetwork / Apple AdAttributionKit |
| ASC | Advantage+ 销售广告系列（Advantage+ Sales campaign），很多流程里前身是 Advantage+ Shopping |
| Advantage+ 受众（Audience） | 受众自动化：用建议 + 扩展，除非控制项限制 |
| Advantage+ 版位（Placements） | 跨合格版面的版位自动化 |
| Advantage+ 创意（Creative） | 广告层级的创意增强与变体 |
| CRM 反馈 | 回传给 Meta 的合格线索、商机、已成交、线下购买、LTV 或其他业务结果 |
| VTC | 浏览转化（View-through conversion）：仅展示带来的归因转化 |
| 互动归因（Engage-through） | 社交互动或合格视频互动后在可用窗口内的转化 |
| 特殊广告类别（Special Ad Category） | Meta 政策类别，如信贷/金融产品与服务、就业、住房，或社会议题/选举/政治 |

## 输出灵活性

按用户实际问的调整输出。不要求必须产出书面方案文档。

| 情况 | 输出 |
|---|---|
| 用户问聚焦的问题 | 直接回答 + 推理。不写文档。 |
| 用户要全盘的规划指导 | 结构化行内回复，覆盖相关章节。 |
| 用户明确要书面计划 / 方案 / 简报 | 按要求的交接格式，行内或 Markdown 文件交付。 |
| 用户从零上线多广告系列账户且要交接 | 书面方案常合适，但除非明确要求，先确认再写。 |

---

## 工作流

```
Step 0: 模式判断                -> 新上线或老账户改进
    ↓
Step 1: 信息收集                -> A 阶段基础 -> 业务模式判读 -> B 阶段细节
    ↓
Step 2: 可行性 + 目标选择        -> 预算/信号可行性 -> 适配的广告系列组合
    ↓
Step 3: 战略成形                -> 结构、出价、衡量、创意、Advantage+ 姿态
    ↓
Step 4: 详细设计 + 交付          -> 按目标 playbook；按情况交付
```

---

## Step 0：模式判断

判断适用哪种情况。不清楚就问。

| 模式 | 触发 | 路径 |
|---|---|---|
| **新上线** | Meta 广告还没跑，或从零设计新广告系列 | Step 1 -> 2 -> 3 -> 4 |
| **老账户改进** | 账户已在跑，有表现问题或改进目标 | Step 1 -> 诊断 -> 改进建议 -> 按需走 Step 3–4 |

### 改进工作流

老账户在 Step 1 之后先做诊断，再提改动。

- 如果用户以任何形式给了账户数据——CSV/XLSX 导出、复制的表格、截图、看板、CRM 报告、事件管理工具诊断、目录报告、MMP 导出、API 提取或指标汇总——先用 [references/account-data-diagnostics.md](references/account-data-diagnostics.md)。
- 如果用户只描述了症状，用 [references/symptom-diagnostics.md](references/symptom-diagnostics.md) 给可能原因排序。
- 始终区分有依据的发现和局限，按业务影响和证据给行动排序，并给出预期稳定窗口和暂缓的改动。

---

## Step 1：信息收集

### A 阶段：基础

| 类别 | 要确认什么 |
|---|---|
| **业务模式** | 电商 / 线索型 / App / 本地商户 / SaaS / 品牌认知 |
| **卖点** | 产品、服务、免费试用、线索磁铁、App、目录、活动或到店 |
| **目标** | 主 KPI：CPA、ROAS、CPL、CPI、留存价值、提升度、触达、商机池等 |
| **预算** | 月度或日度广告预算 |
| **落地页** | URL、应用商店、即时表单、Messenger/Instagram/WhatsApp 流程、目录、门店/POS |
| **受众约束** | 地域、语言、年龄、合规、客户类型、排除、特殊广告类别 |

A 阶段之后，用 [references/business-model-playbooks.md](references/business-model-playbooks.md) 先看该业务模式的默认战略，再选目标或结构。

### B 阶段：设计所需的细节输入

| 类别 | 要确认什么 |
|---|---|
| **现有账户状态** | 在跑的广告系列、当前目标组合、预算、问题、改动历史 |
| **转化数据** | 近 30/60/90 天按事件：购买、线索、合格线索、App 事件、价值、延迟 |
| **衡量** | Pixel、CAPI、去重、EMQ、域名、App SDK/MMP、线下/CRM 回传 |
| **目录** | 商品目录、商品流健康度、内容 ID、商品集、利润/库存标签 |
| **创意** | 现有视频/图片、概念多样性、制作产能、UGC/社会证明 |
| **客户数据** | 购买者名单、线索阶段、高 LTV 群体、排除、新老客定义 |
| **政策** | 特殊广告类别、受管行业、青少年定向、金融/健康/敏感约束 |

细节明确后：

- 用 [references/budget-planning.md](references/budget-planning.md) 检查预算能否支撑拟定的转化事件、广告系列组合和学习量。
- 转化追踪还没上线、涉及 CAPI/App 衡量、线索质量有问题或需要报告对账时，用 [references/measurement-and-attribution.md](references/measurement-and-attribution.md)。
- 在为信贷、就业、住房、社会议题/选举/政治、金融服务、健康、青少年或其他受限类别推荐定向前，用 [references/policy-and-special-categories.md](references/policy-and-special-categories.md)。

---

## Step 2：可行性 + 目标选择

定最终组合前，综合：

1. [references/business-model-playbooks.md](references/business-model-playbooks.md) 的业务模式适配。
2. [references/budget-planning.md](references/budget-planning.md) 的预算与信号可行性。
3. [references/measurement-and-attribution.md](references/measurement-and-attribution.md) 的衡量就绪度。
4. 下方速查表和 playbook 的目标与自动化适配。

不要因为某个目标"能用"就推荐。如果预算、事件质量、创意供给、目录健康、App 衡量、CRM 回传或政策撑不住，先说清楚必须先修什么。

### 广告系列目标速查表

Meta 的"广告系列类型"通常是**广告目标 + 转化位置 + 表现目标 + 自动化层**，不是搜索或购物那样的渠道类型。

| 广告目标 | 主要角色 | 适合 | 主 playbook |
|---|---|---|---|
| **认知（Awareness）** | 触达、频次、广告记忆度、品牌提升、上层漏斗视频 | 品牌上线、品类开创、活动、全漏斗支撑 | [references/awareness-traffic-engagement.md](references/awareness-traffic-engagement.md) |
| **流量（Traffic）** | 落地页访问、链接点击、主页/WhatsApp/电话/网站流量 | 内容分发、考虑页、低信号过渡广告系列 | [references/awareness-traffic-engagement.md](references/awareness-traffic-engagement.md) |
| **互动（Engagement）** | 视频播放、帖子互动、私信、活动响应、社会证明 | 视频池、社群、活动、私信量、中层漏斗预热 | [references/awareness-traffic-engagement.md](references/awareness-traffic-engagement.md) |
| **线索（Leads）** | 原始线索、高意愿线索、来电、一键私信线索、转化线索（Conversion Leads） | B2B、服务、教育、房产、本地、高客单咨询流程 | [references/leads-campaigns.md](references/leads-campaigns.md) |
| **App 推广** | 安装、App 事件、价值、再互动 | App、手游、订阅 App、留存用户增长 | [references/app-campaigns.md](references/app-campaigns.md) |
| **销售（Sales）** | 购买、网站/App 转化、目录销售、价值、私信成交 | 电商、D2C、订阅、目录零售、转化主导的营收 | [references/sales-campaigns.md](references/sales-campaigns.md) |

### 选择参考

速查表只做第一轮方向。定最终组合前，先读：

| 决策 | 参考 |
|---|---|
| 按业务模式和漏斗角色的默认组合 | [references/business-model-playbooks.md](references/business-model-playbooks.md) |
| 预算充足度、预期事件量、暂时别上的 | [references/budget-planning.md](references/budget-planning.md) |
| 认知 / 流量 / 互动细节 | [references/awareness-traffic-engagement.md](references/awareness-traffic-engagement.md) |
| 线索、即时表单、一键私信线索、来电、转化线索 | [references/leads-campaigns.md](references/leads-campaigns.md) |
| App 推广、App 事件、iOS/SKAN/AAK、MMP、再互动、可玩广告 | [references/app-campaigns.md](references/app-campaigns.md) |
| 销售、Advantage+ Sales、目录广告、精品栏、私信成交 | [references/sales-campaigns.md](references/sales-campaigns.md) |
| Advantage+ 自动化与控制 | [references/advantage-plus.md](references/advantage-plus.md) |
| 衡量、CAPI、归因、线下/CRM 与 App 信号质量 | [references/measurement-and-attribution.md](references/measurement-and-attribution.md) |
| 格式规格与版位总览 | [references/ad-formats-and-placements.md](references/ad-formats-and-placements.md) |

实施易变功能前，先核实现行 Meta 文档或当前 Ads Manager 界面，比如 Advantage+ Sales 控制、老客预算上限行为、Advantage+ Leads 准入、归因列、详细定向排除、AEM 行为、Threads 版位可用性、青少年/特殊广告类别定向。

### 选定后和用户确认方向

呈现：

1. 推荐的目标、转化位置和表现目标。
2. 广告系列组合总览：几个广告系列/广告组，各自的角色。
3. Advantage+ vs 手动姿态，以及哪些控制留在人手里。
4. 出价策略与预算拆分。
5. 衡量前置条件或 blocker。

除非用户直接要了完整方案，否则深入 Step 3 前先拿到确认。

---

## Step 3：战略成形

这一步定跨广告系列的战略。按目标的详细设计在 Step 4 用 playbook 做。

按这个顺序：

1. 确认业务模式战略与广告系列角色。
2. 确认预算可行性与预期事件量。
3. 定义转化信号与增量立场。
4. 定结构、出价、Advantage+ 姿态和预算拆分。
5. 用 [references/creative-strategy.md](references/creative-strategy.md) 定义创意战略。

### 账户结构

Meta 广告是 3 层结构：广告账户 -> 广告系列 -> 广告组 -> 广告。

| 层级 | 设计原则 |
|---|---|
| 广告系列 | 按目标、预算归属、经济效益、地域/语言、合规、生命周期/客户类型，或实质不同的衡量目标拆分。 |
| 广告组 | 只有在优化事件、转化位置、受众控制、版位、预算或测试问题真需要分离时才拆分。 |
| 广告 | 放差异化创意概念、格式变体、文案变体、目录素材或灵活创意单元。 |

### 命名规范

上线前先定一套命名规范。命名直接影响筛选、报告和诊断。

#### 广告系列名

**格式：** `{Obj}_{Goal}_{Audience}_{Geo}_{Note}`

| 元素 | 取值 | 说明 |
|---|---|---|
| Obj | `Sales` `Lead` `App` `Awareness` `Traffic` `Engagement` | 目标缩写 |
| Goal | `Purchase` `Value` `QLead` `Install` `AppEvent` `Reach` `LPV` `Msg` | 表现目标或优化事件 |
| Audience | `Prospecting` `Retargeting` `NewCustomer` `Existing` `Broad` `SAC` | 受众/客户角色 |
| Geo | `US` `EU` `LATAM` `SEA` `Global` 等 | 定向地域；明显时可省略 |
| Note | 产品/品类/测试/合规备注 | 按需 |

**示例：** `Sales_Purchase_NewCustomer_US`、`Lead_QLead_Prospecting_EMEA`、`App_AppEvent_iOS_LATAM`、`Awareness_Reach_Broad_Global`、`Engagement_Msg_Prospecting_SEA`。

#### 广告组名

**格式：** `{LocationOrEvent}_{AudienceControl}_{PlacementOrTest}`

示例：`Website_Purchase_Broad`、`InstantForm_HigherIntent_Broad`、`AppEvent_PurchaserValue_iOS`、`Messenger_Conversation_Manual`、`Catalog_ProductSet_HighMargin`。

#### 命名规则

- 统一用下划线 `_`。
- 用英文命名，兼容 Ads Manager 筛选器与导出。
- PascalCase 分词（`NewCustomer`、`HigherIntent`、`HighMargin`）。
- 除非是限时测试，否则不加开始日期；需要时追加 `_Test_YYMM`。
- 维护一份共享缩写表：目标、事件、地域、客群。

### 出价策略

选、改、回滚出价策略前，先看 [references/budget-planning.md](references/budget-planning.md) 和所选目标的 playbook。至少检查：

1. 近 30 天主优化事件量。
2. 事件延迟 vs 归因与判读窗口。
3. CPA / ROAS / 价值稳定性。
4. 信号深度：购买、合格线索、App 价值、留存用户还是微事件。
5. 预算-出价比。

默认路径：上线不稳定期先用当前 Ads Manager 界面里的**最高量（Highest volume）**（老文档或 API 语言里常写作最低成本/自动出价）；账户量稳定、经济效益站得住后再引入**单次转化费用目标（Cost per result goal）**；价值数据和投放量撑得住严格控制时才用**出价上限（Bid cap）/ ROAS 目标**。

### 转化设计

Pixel/CAPI、去重、EMQ、线下事件、转化线索 CRM 集成、App 衡量、归因与增量，用 [references/measurement-and-attribution.md](references/measurement-and-attribution.md)。Advantage+ 自动化控制与手动兜底决策，用 [references/advantage-plus.md](references/advantage-plus.md)。

高级规则保持在现行计划里：选最深层可靠、量够、延迟可接受的事件；弱代理事件只作次要或临时，除非已验证；做重要决策时，把点击归因、互动归因、浏览归因、建模转化和业务口径结果分开看。

### 预算分配

套分配规则前先看 [references/budget-planning.md](references/budget-planning.md)。70 / 20 / 10 拆分只是默认运营模式，前提是预算大到每个桶都能学。

**70 / 20 / 10 规则：**

- 70%——已验证的广告系列和概念。
- 20%——测试新创意、卖点、转化位置或受众。
- 10%——探索性目标、新格式或新漏斗尝试。

### 运营节奏

用[实践优先立场](#节奏)里的节奏表作为标准运营节奏。各目标 playbook 可以加特定检查，但不覆盖基本规则：高频监控异常、批量做有意义的改动、不对短期噪声反应。

### 常见坑

| 失败模式 | 修法 |
|---|---|
| 为求量把 Sales/Leads 优化到浅层事件 | 验证代理质量、回传更深层事件、往业务结果迁回 |
| 指望流量或互动带来购买 | 要营收就用 Sales；上层漏斗广告系列就标为支撑位、用独立 KPI |
| 原始线索便宜但销售不要 | 加表单摩擦、资格筛选问题、CRM 反馈、转化线索或下游价值 |
| Advantage+ Sales 报告 ROAS 很高，混合营收没动 | 拆新老客、对后端营收、测增量 |
| 有意义的花费还只用 Pixel | 加 CAPI + 去重 + EMQ 卫生 |
| 目录销售不行但设置看着没问题 | 诊断商品流、内容 ID、商品上架、价格、商品集、利润标签 |
| App 广告系列永远在优化安装 | 量和衡量允许时往 App 事件/价值走 |
| 创意测试全是表面变体 | 做差异化概念：问题、结果、证明、卖点、异议、对比、社会证明 |
| 上线才发现特殊广告类别 | 结构和受众设计前先查政策与定向限制 |

---

## Step 4：详细设计 + 交付

每个选定目标，用对应 playbook 同时做实践和设置。实践章节驱动推荐；设置是实施层。

写素材规格或文案草稿前，用 [references/creative-strategy.md](references/creative-strategy.md) 定义受众、卖点、证明、异议、格式适配和测试角度。每个概念填一份[创意简报模板](references/creative-strategy.md#7-创意简报模板)再进入制作。然后用目标 playbook 和 [references/creative-production.md](references/creative-production.md) 做格式专属制作。

### 按目标 playbook

| 广告目标 / 广告系列类型 | Playbook | 关键设计主题 |
|---|---|---|
| 认知 | [references/awareness-traffic-engagement.md](references/awareness-traffic-engagement.md) | 触达、广告记忆度、品牌提升、预订购买、Threads、视频主导创意、上层漏斗衡量 |
| 流量 | [references/awareness-traffic-engagement.md](references/awareness-traffic-engagement.md) | 链接点击 vs 落地页浏览、流量当销售的反模式、LPV 质量、内容/考虑角色 |
| 互动 | [references/awareness-traffic-engagement.md](references/awareness-traffic-engagement.md) | 视频播放、私信、活动响应、社会证明、Reels/Stories/Threads、向 Sales/Leads 交接 |
| 线索 | [references/leads-campaigns.md](references/leads-campaigns.md) | 即时表单、高意向、富创意、网站线索、一键私信、来电、转化线索、CRM 质量闭环 |
| App 推广 | [references/app-campaigns.md](references/app-campaigns.md) | Advantage+ App、App 事件阶梯、iOS AAK/SKAN/AEM、MMP、App 转化 API、再互动、可玩广告 |
| 销售 | [references/sales-campaigns.md](references/sales-campaigns.md) | Advantage+ Sales、手动 Sales、目录广告、精品栏/全屏快应用、私信成交、价值优化 |

### 跨领域设计参考

在工作流里用，不只是选读：

| 参考 | 什么时候用 | 关键设计主题 |
|---|---|---|
| [references/business-model-playbooks.md](references/business-model-playbooks.md) | 按业务模式选战略 | 电商、线索型、SaaS、本地、App、品牌、组合模式 |
| [references/budget-planning.md](references/budget-planning.md) | 判断预算现实能支撑什么 | CPA/ROAS 经济效益、预期事件量、广告系列组合可行性 |
| [references/measurement-and-attribution.md](references/measurement-and-attribution.md) | 设计事件、选归因、规划增量、诊断信号问题 | Pixel/CAPI、去重、EMQ、线下/CRM、App 衡量、归因、提升度 |
| [references/advantage-plus.md](references/advantage-plus.md) | 定自动化姿态与手动兜底结构 | Advantage+ Sales、Leads、App、受众、版位、创意、CBO、易变控制项 |
| [references/account-data-diagnostics.md](references/account-data-diagnostics.md) | 解读账户导出、截图、看板、CRM/MMP/目录数据、事件管理工具诊断 | 数据接入、字段映射、证据排序、if-this-then-that 动作 |
| [references/policy-and-special-categories.md](references/policy-and-special-categories.md) | 受管或受限类别 | 特殊广告类别、定向限制、青少年/敏感话题、合规优先设计 |
| [references/symptom-diagnostics.md](references/symptom-diagnostics.md) | 从症状改进老账户 | 花费/转化问题、线索质量、疲劳、信号、结构、Advantage+ 陷阱 |
| [references/creative-strategy.md](references/creative-strategy.md) | 设计创意体系、测试和简报 | 角度、证明、异议、漏斗阶段创意、刷新节奏 |
| [references/creative-production.md](references/creative-production.md) | 制作素材和文案 | 图片、视频、轮播、精品栏、即时表单、全屏快应用 |
| [references/ad-formats-and-placements.md](references/ad-formats-and-placements.md) | 需要快速看格式或版位总览 | 图片、视频、轮播、精品栏、全屏快应用、版位、安全区 |
| [references/setup-checklist.md](references/setup-checklist.md) | 搭建层实施 | 账户搭建、Pixel/CAPI 基础、定向、出价、广告搭建 |
| [references/campaign-structure.md](references/campaign-structure.md) | 结构模板与命名 | 2/3 广告系列模式、学习、KPI、结构示例 |

### 交付输出

按[输出灵活性](#输出灵活性)匹配输出形态。除非用户要求或方案明确要交接给其他团队、代理商或客户，否则不写书面交付物。

如果书面计划是合适的交付物，通常覆盖：

1. 战略摘要——目标、受众、为什么是这个目标组合。
2. 广告系列清单——目标、转化位置、表现目标、出价、预算拆分。
3. 按目标的设计——由 playbook 驱动。
4. Advantage+ 姿态——什么自动化、什么留人控、什么必须在当前界面核实。
5. 创意需求——概念、格式、规格、制作负责人、刷新节奏。
6. 衡量设计——Pixel/CAPI/App/CRM/线下搭建、转化定义、归因视角。
7. 运营时间线——上线、学习期、优化阶段、诊断节奏。
8. KPI 与成功标准——平台指标、业务口径真相、增量计划。

Markdown 文件交付物保持实用结构而非模板化：章节清晰、交接处用决策表、写明假设、给出具体的上线/衡量动作。不填不改变广告系列决策的通用章节。

---

## 相关工作流

| 相邻需求 | 什么时候用单独的 skill 或工作流 |
|---|---|
| 落地页文案 | 用户要完整落地页文案，不只是广告文案 |
| 落地页设计 | 用户要落地页结构或设计 |
| 数据分析实施 | 用户要实施 GA4、GTM、Meta Pixel、转化 API、SDK 或 MMP |
| 竞品研究 | 用户要竞品广告或市场研究 |
| 图片生成 | 用户要生成横幅或图片创意 |
| 视频生成 | 用户要生成视频创意 |
| Google Ads 规划 | 用户规划的是 Google Ads 而不是 Meta 广告 |
