---
name: google-ads-planning
description: 端到端规划与设计 Google Ads 广告系列——选择合适的广告类型、设计账户结构、选择出价策略、配置转化衡量，并决定上线什么、如何上线。覆盖新账户投放与现有账户优化。当用户提及 Google Ads、付费搜索（paid search）、搜索广告、展示广告、购物广告、视频 / YouTube 广告、Performance Max（P-MAX）、Demand Gen 或应用安装广告系列，或询问如何搭建 / 改进 / 上线 Google Ads 账户时，使用本技能。不适用于无关的付费媒体平台（Meta、X、TikTok 等）。
---

# Google Ads 规划

端到端规划一个 Google Ads 投放项目：广告类型选择、广告系列架构、出价策略、转化衡量，以及把计划变成真实投放账户的一系列运营决策。本技能覆盖新账户投放与现有账户优化两种场景。

## 实践优先的立场

Google Ads 规划应优先考虑让广告系列真正跑起来的实践，而不是只让广告系列"设置正确"的配置项。广告系列类型、账户结构、出价设置固然重要，但其优先级低于衡量质量、转化信号设计、创意质量、Feed / 落地页质量、预算充足度，以及严格的运营节奏。

在给出广告系列设置建议之前，先核查以下事项：

| 领域 | 核查什么 | 为什么重要 |
|---|---|---|
| 转化设计（Conversion design） | 主要转化（Primary）vs 次要转化（Secondary）动作、合格线索导入、动态价值、去重 | 信号不好，Smart Bidding 就会优化出坏结果 |
| 增量性（Incrementality） | 品牌词 vs 非品牌词、VTC 处理、地域 / 对照组可行性、财务口径的真实来源 | 平台 ROAS 只是参考信号，不是财务真相 |
| 创意体系（Creative system） | 差异化创意概念的数量、更新节奏、offer 清晰度、信任状（proof）、版式覆盖度 | 自动化救不了弱创意和陈旧创意 |
| Feed / 落地页 | 商品标题、价格、库存、落地页匹配度、页面速度、表单摩擦 | 广告会放大陆地页和 Feed 的质量 |
| 预算与量级 | 转化量、目标 CPA / ROAS 是否现实、预算-出价比 | 预算不足的广告系列永远学不会 |
| 控制项（Controls） | 否定关键词、品牌词排除、网址排除、展示位置 / 内容排除、屏蔽列表 | 广泛的自动化需要护栏 |
| 运营节奏 | 每日健康检查、每周复盘、每月审计、每季度策略重置 | 大多数失败来自要么放养、要么过度调整 |

### 核心运营规则

- 从业务目标和单位经济学出发，而不是从广告系列菜单出发。
- 把转化动作当作策略本身。优先选择最深层的、可靠且量级足够、延迟可接受的信号；默认把弱微转化（micro-conversion）放在次要位置或低价值位。
- 区分平台效果与增量性。品牌词截流、VTC、模型化转化和归因重叠会夸大 Search、P-MAX、Display、Demand Gen 和 Video 的真实贡献。
- 为学习而合并；仅在真实控制需求下拆分：预算、目标、利润率、地域、语言、转化价值、客户类型或负责人。
- 把创意当作核心杠杆。优先差异化概念而非表面变体，用 AI 从人工定义的切入角度出发生产 / 迭代，而不是让 AI 凭空编造定位。
- 不要追逐便宜的 CPC / CPM / CPV / CPI，除非下游质量、品牌安全和增量性都可接受。
- 有意识地使用自动化。人类仍然拥有转化定义、价值规则、排除项、品牌控制、Feed 质量、素材质量、预算和增量测试。
- 避免频繁干预。把有意义的改动打包执行，保留带意图标注的改动日志，等学习期和转化延迟过去之后再下判断。

### 节奏（Cadence）

默认采用下表的实践节奏，除非用户的账户背景提示需要调整：

| 节奏 | 关注点 | 避免 |
|---|---|---|
| 每日 | 花费异常、转化下跌、跟踪 / 政策 / Feed 故障、预算消耗进度 | 每日微调目标 / 预算 |
| 每周 | 搜索词报告、加否定关键词、素材表现、线索 / 商品质量、改动日志复盘、VTC / EVC 占比检查 | 因为几天的噪声就重构 |
| 双周 | 出价目标调整、广告文案 / 创意表现复盘、对照测试读数 | 同时改出价、预算、素材和目标 |
| 每月 | 否定词扫荡、创意更新、Feed / 标题优化、落地页复盘、预算再分配 | 让上线时的假设一直存在 |
| 每季度 | 转化动作重设计、增量性复盘、品牌 / 非品牌拆分、账户架构、目标经济模型 | 只汇报平台 ROAS |

### 衡量注意事项

- 用平台指标做战术优化，不当作最终财务真相。
- 品牌词与非品牌词分开汇报。对于成熟品牌，默认假设一部分品牌转化本就会自然发生，除非对照组或增量测试证明不是。
- 对于 P-MAX / Demand Gen / Display / Video，关注增量贡献、助攻需求、品牌搜索量提升和受众池增长；末次点击 CPA 往往不完整。
- VTC 默认可见但单独统计。区分 VTC 与观看后转化（engaged-view conversions，EVC）：EVC 是 Video / Demand Gen / P-MAX 中的可出价视频互动信号，而标准 VTC 通常只是报告指标（reporting-only），App、P-MAX 门店目标（Store Goals）和明确选择 VTC 优化的 Demand Gen 广告系列除外。
- 在预算和量级允许时，使用地域对照组、Customer Match 对照组、广告系列实验（campaign experiments）、转化提升研究（conversion lift）或前后对比分析。
- 用收入、pipeline、CRM、应用 LTV 或边际贡献对账。不要把各渠道的平台上报收入直接加总，当作已经去重。

### 常用 Google Ads 术语表

使用以下统一术语，而不是在每个广告类型手册里重复基础定义。

| 术语 | 含义 |
|---|---|
| CV | 转化（Conversion）：广告系列要驱动的动作 |
| CV value | 转化价值（CV value）：附加在转化上的金额或评分 |
| CPA | 获客成本（Cost per acquisition）：花费 / 转化数 |
| ROAS | 广告支出回报率（Return on ad spend）：转化价值 / 花费 |
| CTR | 点击率（Click-through rate）：点击 / 展示 |
| CPC | 单次点击成本（Cost per click） |
| CPM | 千次展示成本（Cost per thousand impressions） |
| CPV | 单次观看成本（Cost per video view） |
| CVR | 转化率（Conversion rate）：转化 / 点击或互动 |
| IS | 展示份额（Impression share） |
| LP | 落地页（Landing page） |
| DDA | 数据驱动归因（Data-driven attribution） |
| VTC | 浏览型转化（View-through conversion）：无点击、无互动的展示之后发生的转化 |
| EVC | 观看后转化（Engaged-view conversion）：有意义的视频互动之后发生的转化 |
| RSA | 自适应搜索广告（Responsive Search Ad） |
| RDA | 自适应展示广告（Responsive Display Ad） |
| DSA | 动态搜索广告（Dynamic Search Ad） |
| P-MAX / PMax | Performance Max |
| DGen | Demand Gen |
| GMC | Google Merchant Center |
| GTIN | 全球贸易项目代码（Global Trade Item Number） |
| SKU | 库存单位（Stock Keeping Unit） |
| NCA | 新客获取（New customer acquisition） |
| ACi | 应用安装广告系列（App campaign for installs） |
| ACe | 应用互动广告系列（App campaign for engagement） |
| ACpre | 应用预注册广告系列（App campaign for pre-registration） |
| SKAN | SKAdNetwork |
| AAK | AdAttributionKit |
| MMP / AAP | 移动衡量合作伙伴（Mobile Measurement Partner）/ 应用归因合作伙伴（App Attribution Partner） |
| LTV | 生命周期价值（Lifetime value） |
| KW | 关键词（Keyword） |
| tCPA | 目标 CPA（Target CPA） |
| tROAS | 目标 ROAS（Target ROAS） |
| tCPI | 目标 CPI（Target CPI） |
| tCPV | 目标 CPV（Target CPV） |
| tCPM | 目标 CPM（Target CPM） |
| CPI | 单次安装成本（Cost per install） |
| CTA | 行动号召（Call to action） |
| VTR | 观看率 / 视频观看率（View-through rate / video view rate），取决于报告场景 |
| CTV | 智能电视 / 联网电视（Connected TV） |
| AG | 素材组（Asset group） |
| LG | 商品组（Listing group） |
| CL | 自定义标签（Custom label） |
| PLA | 商品列表广告（Product Listing Ad） |
| MPN | 制造商零件编号（Manufacturer Part Number） |
| RLSA | 搜索广告再营销列表（Remarketing Lists for Search Ads） |
| ATT | 应用跟踪透明度（App Tracking Transparency） |
| SDK | 软件开发工具包（Software Development Kit） |
| Deep link | 深度链接：在应用内打开特定页面的链接 |
| Brand Lift | 品牌提升研究：衡量广告记忆度、知名度、考虑度或类似品牌结果变化的研究 |
## 输出灵活性（不要每次都写文档）

按用户的实际需求调整输出形式。**没有硬性要求产出书面方案文档**——有时对话式回答或结构化的行内回复就是正确的交付物。

| 场景 | 输出 |
|---|---|
| 用户问聚焦的具体问题（例如"这个场景该用 P-MAX 还是 Search？"） | 直接给出答案和推理。不写文档。 |
| 用户想要覆盖全貌的规划指导 | 结构化回复，覆盖相关章节。默认仍是行内回复。 |
| 用户明确要求书面计划 / 方案 / 简报 | 按要求的交接形式，在行内或 Markdown 文件中产出书面交付物。 |
| 用户从零启动多广告系列账户，且交付物要交接给别人 | 书面方案通常更合适——但**先和用户确认再写**，不要想当然。 |

拿不准时，直接问用户想要书面交付物还是对话式回答。默认选择较轻的形式。

---

## 工作流

```
Step 0：模式识别           → 新投放还是现有账户优化
    ↓
Step 1：信息收集    → A 阶段（基础信息）→ 业务模式研读 → B 阶段（详细信息）
    ↓
Step 2：可行性 + 广告类型选择 → 预算可行性 → 合适的广告系列组合
    ↓
Step 3：策略制定       → 账户结构、出价、转化设计、预算分配、创意策略
    ↓
Step 4：实践主导的详细设计 + 交付 → 分广告类型的手册；按实际场景交付
```

---

## Step 0：模式识别

先判断属于以下哪种情况。拿不准就问。

| 模式 | 触发条件 | 路径 |
|---|---|---|
| **新投放** | Google Ads 还没跑过，或广告系列从零开始设计 | Step 1 → 2 → 3 → 4 |
| **现有账户优化** | 账户已在跑，有效果问题或改进目标 | Step 1 → 诊断 → 改进建议 → 按需进入 Step 3–4 |

### 现有账户优化工作流

对于现有账户，在 Step 1 之后先做诊断，再提改进建议。

- 如果用户以任何形式提供了账户数据——CSV / XLSX 导出、复制的表格、截图、看板、CRM 报告、Merchant Center 诊断、API 导出或指标汇总——先使用 [references/account-data-diagnostics.md](references/account-data-diagnostics.md)。
- 如果用户只描述了症状，用 [references/diagnostic-decision-trees.md](references/diagnostic-decision-trees.md) 对可能的根因排序。
- 始终区分有依据的发现和局限，按业务影响和证据强度对行动排序，并给出预期的稳定窗口和应避免的改动。

---

## Step 1：信息收集

### A 阶段：基础信息（给出广告类型方向所需的最低信息）

| 类别 | 问题 |
|---|---|
| **业务模式** | 电商 / 线索获取 / 应用 / 到店 / 品牌认知——哪一种？ |
| **Offer** | 在卖什么或推广什么？（产品、服务、免费试用、获客钩子等） |
| **目标** | 核心 KPI（CPA / ROAS / CPI / CPV 等）——如果有目标值也一并给出 |

A 阶段结束后，用 Step 2 的速查表给出广告类型方向。用户同意后再进入 B 阶段。

在给出方向之前，用 [references/business-model-playbooks.md](references/business-model-playbooks.md) 核对该业务模式的默认策略。避免广告类型选择变成"照着菜单点菜"。

### B 阶段：设计所需的详细信息

| 类别 | 问题 |
|---|---|
| **预算** | 月度或每日广告花费 |
| **落地页** | 目标页面的 URL |
| **目标受众** | 要触达谁（地域、年龄段、行业、搜索行为等） |
| **现有账户状态** | Google Ads 现在是否在跑？如果在跑，有什么问题 |
| **转化数据** | 过去 30 天的转化量（影响出价策略是否可行） |
| **商品 Feed** | 是否有 Merchant Center 商品 Feed？（电商场景） |
| **创意** | 已有的视频 / 图片素材，以及团队持续生产的能力 |
| **衡量** | 转化跟踪是否已部署？ |

如果项目里已有现成的背景信息（业务简介、竞品分析、品牌素材、历史审计报告等），先读完，不要重复问已知的信息。

B 阶段结束后，用 [references/budget-planning.md](references/budget-planning.md) 检查预算能否支撑拟议的广告系列组合、预期的转化量和出价策略。如果预算产生不了有意义的信号，先收窄结构再进入 Step 2。

转化跟踪还没上线、EEA / 英国 / 瑞士流量在投放范围内（那里 Consent Mode v2 是强制的），或用户提到跟踪问题、线索质量问题、衡量对账问题时，也要查 [references/measurement.md](references/measurement.md)。信号不好，后面每一步都会更糟。

---

## Step 2：可行性 + 广告类型选择

在确定最终组合之前，综合：

1. [references/business-model-playbooks.md](references/business-model-playbooks.md) 中的业务模式适配度。
2. [references/budget-planning.md](references/budget-planning.md) 中的预算与信号可行性。
3. 下面速查表中的广告系列类型适配度。

不要因为某个广告类型"能用"就推荐它。如果预算、转化质量、创意供给、Feed 质量或衡量支撑不起，先说清楚必须先修什么。

### 广告类型速查表

| 广告系列 | 版位 | 计费 | 漏斗阶段 | 自动化程度 | 适合 |
|---|---|---|---|---|---|
| **Search** | Google 搜索 | CPC | 底部（高意向） | 中 | 所有行业 |
| **Display** | GDN（200 万 + 网站） | CPC / CPM | 顶部（认知） | 中 | 所有行业 |
| **Shopping** | 搜索 / 购物标签页 | CPC | 底部（购买） | 中 | 电商 / 零售 |
| **Video** | YouTube / 视频合作伙伴 | CPV / CPM | 顶部–中部 | 中 | 所有行业 |
| **App** | 搜索 / Play / YouTube / GDN | CPI / CPA | 中部–底部 | 高 | 应用发行方 |
| **P-MAX** | 全部 7 个渠道 | CPA / ROAS | 全漏斗 | 非常高 | 所有行业 |
| **Demand Gen** | YouTube / Discover / Gmail / GDN | CPC / CPA / CPM | 顶部–中部 | 高 | 所有行业 |

### 选择参考资料

速查表只做第一轮方向判断。在定稿组合之前，先读：

| 决策 | 参考 |
|---|---|
| 按业务模式和漏斗角色的默认组合 | [references/business-model-playbooks.md](references/business-model-playbooks.md) |
| 预算充足度、预期转化量、哪些先不做 | [references/budget-planning.md](references/budget-planning.md) |
| Search / AI Max 细节 | [references/search-ads.md](references/search-ads.md) |
| P-MAX 控制项、Feed / 素材组、易变功能的核查 | [references/pmax.md](references/pmax.md) |
| Demand Gen 版位、渠道控制、社交风创意的适配 | [references/demand-gen.md](references/demand-gen.md) |
| Display、Shopping、Video 或 App 细节 | [Step 4](#step-4-detailed-design--delivery) 中的对应手册 |

对于 AI Max、P-MAX 否定关键词 / 渠道报告、Demand Gen 渠道控制等易变功能，实施前先核对 Google 官方文档的最新说明。

### 选定后，与用户确认方向

给出：

1. 推荐的广告类型和理由。
2. 广告系列组合概览——几个广告系列、各自的角色。
3. 每个广告系列的出价策略。
4. 预算分配。

深入 Step 3 之前，先拿到确认。
## Step 3：策略制定

这一步决定跨广告系列的策略。分广告类型的详细设计在 Step 4 用手册完成。

按以下顺序：

1. 确认业务模式策略和广告系列角色。
2. 确认预算可行性和预期转化量。
3. 定义转化信号和增量性立场。
4. 决定账户结构、出价和预算分配。
5. 用 [references/creative-strategy.md](references/creative-strategy.md) 定义创意策略。

### 账户结构

Google Ads 是三层结构：账户 → 广告系列 → 广告组（→ 广告 + 关键词）。

| 层级 | 设计原则 |
|---|---|
| 广告系列 | 按目标、预算、经济模型、地域 / 语言、负责人拆分。结构保持业务逻辑允许的最小规模；过度拆分会分散 AI 的训练信号。 |
| 广告组 | 每个广告组一个清晰主题。关键词和 RSA 足够学习即可，避免为了报表好看做微型结构。 |

### 命名规范

上线前先定一套命名规范。命名直接影响筛选、报表和一眼可读性。

#### 广告系列命名

**格式：** `{Type}_{Goal}_{Target}_{Geo}_{Note}`

| 元素 | 取值 | 说明 |
|---|---|---|
| Type | `Search` `PMax` `Display` `Shopping` `Video` `DGen` `App` | 广告系列类型缩写 |
| Goal | `CV` `Lead` `Sales` `Awareness` `Traffic` `Install` | 核心目标 |
| Target | `Brand` `NonBrand` `Competitor` `Remarketing` `Prospecting` `AllProducts` | 受众分段 |
| Geo | `US` `Tokyo` `EU` 等 | 地域定向（全球投放可省略） |
| Note | 自由填写（商品类目、测试名等） | 按需填写 |

**示例：**

| 广告系列 | 名称 |
|---|---|
| 品牌搜索 | `Search_CV_Brand_US` |
| 非品牌搜索 | `Search_Lead_NonBrand_NYC` |
| P-MAX（全品类） | `PMax_Sales_AllProducts` |
| P-MAX（类目） | `PMax_Sales_Shoes` |
| 展示再营销 | `Display_CV_Remarketing` |
| 展示认知 | `Display_Awareness_Prospecting` |
| Demand Gen | `DGen_Lead_Prospecting` |
| 视频认知 | `Video_Awareness_YouTube` |
| 购物 | `Shopping_Sales_Electronics` |
| 应用 | `App_Install_iOS` |

#### 广告组 / 素材组命名

**格式：** `{Theme}_{Subcategory}`

| 广告系列类型 | 主题示例 | 子类目示例 |
|---|---|---|
| Search | 关键词类目（`CRM`、`Pricing`、`Comparison`） | 匹配类型（`Exact`、`Phrase`、`Broad`） |
| P-MAX | 目标分段（`NewCustomer`、`Retarget`） | 商品类目或信息 |
| Display | 定向类型（`Interest`、`Custom`、`Placement`） | 具体分段名 |
| Demand Gen | 版位或目标（`YouTube`、`Discover`、`Carousel`） | 受众名 |
| Shopping | 商品类目（`Shoes`、`Bags`） | 价格带或品牌 |

**示例：** `CRM_Exact` / `Pricing_Phrase` / `NewCustomer_HighValue` / `Interest_ITManager`

#### 命名规则

- **统一使用下划线 `_`。** 不要混用连字符或空格。
- **建议用英文命名。** 与 Google Ads 筛选器和外部工具的兼容性更好。
- **名称里不要带开始日期。** Google Ads 会单独记录开始日期。只有测试广告系列可在末尾加 `_Test_YYMM`。
- **用 PascalCase 写词元**（`NonBrand`、`AllProducts`）。
- **维护一份共用的缩写清单。** 防止账户内各人各写各的。

### 出价策略

在选择、更换或回滚出价策略前，先看 [references/budget-planning.md](references/budget-planning.md)。至少核查：

1. 过去 30 天的核心转化量。
2. 转化延迟 vs 转化窗口。
3. CPA / ROAS 稳定性。
4. 信号深度：购买 / SQL / 合格线索 vs 微转化。
5. 预算-出价比。

不要按期望设目标。从观测到的实际表现出发，避免初始 tCPA / tROAS 目标紧于当前实际值，逐步调整。

### 转化设计

用 [references/measurement.md](references/measurement.md) 设计 Primary / Secondary / 微转化、价值传递、Consent Mode v2、增强型转化（Enhanced Conversions）、OCI / ECfL、Tag Gateway / sGTM、VTC / EVC 处理、归因、iOS / SKAN 和增量性方法。

把高层规则写进执行计划：选择最深层的、可靠且量级和延迟都可接受的 Primary 转化；未经验证的弱代理指标只做次要转化；做重要决策时，把点击归因、EVC、VTC 和业务真实口径的结果分开看。

### 预算分配

应用分配规则前，先看 [references/budget-planning.md](references/budget-planning.md)。70 / 20 / 10 只是默认的运营分配模式，前提是预算大到每个桶都够学习。

**70 / 20 / 10 法则：**

- 70% — 已验证的广告系列和关键词
- 20% — 测试（出价、定向、广告文案）
- 10% — 新关键词、新受众或新广告类型

### 运营节奏

以[实践优先](#practice-first-stance)一节的节奏表作为标准运营节奏。分广告类型的手册可以加渠道专属检查项，但不能推翻基本规则：频繁监控异常、打包有意义的改动、不对短期噪声做反应。

### 常见坑

| 失败模式 | 解法 |
|---|---|
| 转化跟踪不一致 | 用 GTM Preview + GA4 DebugView 验证 |
| 未开启增强型转化 | 打开——Cookie 受限环境下的防御手段是必需的 |
| 初始 tCPA 设得低于当前平均 CPA | 从观测到的平均值或更高起步；绝不低于——否则量瞬间崩掉 |
| 学习期频繁改动 | 阶梯式调整目标 ±10–15%，每次间隔 ≥2 周；换策略会重新触发学习，调目标值不会 |
| P-MAX 预算不足 | 下限 3× tCPA 或 $150 / 天。低于约 30 转化 / 30 天时，降 tCPA 目标而不是饿死广告系列 |
| 目标劫持——多个深浅不一的 Primary CV 混在一起 | 每个广告系列目标只保留一个核心动作；微转化降为 Secondary |
| 只看含 VTC / EVC 的 ROAS 做判断 | 同时并行评估点击归因、EVC 和业务真实口径的结果 |
| 广告与落地页不匹配 | 每个广告组配专属落地页 |

---

## Step 4：详细设计 + 交付

对每个选定的广告类型，查对应的手册做实践和设置两层设计。实践部分驱动建议，设置是实施层。

在写素材规格或文案草稿之前，用 [references/creative-strategy.md](references/creative-strategy.md) 定义受众、offer、信任状、异议处理、版式适配和测试切入角度。再用分广告类型的手册做渠道专属设置和规格。

### 分广告类型手册

| 广告类型 | 手册 | 核心设计主题 |
|---|---|---|
| Search | [references/search-ads.md](references/search-ads.md) | 意图捕获、品牌 / 非品牌分离、广泛匹配护栏、RSA / 落地页实践、搜索词质量 |
| Display | [references/display-ads.md](references/display-ads.md) | 再营销 vs 拉新角色、便宜流量 vs 质量、VTC 处理、频次、展示位置卫生 |
| Shopping | [references/shopping-ads.md](references/shopping-ads.md) | Feed 质量、标题策略、利润标签、商品组、与 P-MAX 共存 |
| App | [references/app-campaigns.md](references/app-campaigns.md) | 事件深度选择、预算-出价比、创意体系、iOS / SKAN 现实 |
| Video | [references/video-campaigns.md](references/video-campaigns.md) | 钩子 / offer / ABCD、Shorts / CTV 角色、频次、提升测量、效果 vs 认知 |
| P-MAX | [references/pmax.md](references/pmax.md) | 转化信号质量、素材组实践、品牌控制、网址扩展、Feed / 利润策略 |
| Demand Gen | [references/demand-gen.md](references/demand-gen.md) | 社交风创意、相似受众种子质量、助攻需求、版位组合、与 P-MAX 重叠 |

### 跨主题设计参考

在工作流中使用这些资料，不是可有可无的选读：

| 参考 | 使用时机 | 核心设计主题 |
|---|---|---|
| [references/business-model-playbooks.md](references/business-model-playbooks.md) | 按业务模式选策略 | B2B 线索获取、本地服务、电商、高客单、应用、到店 |
| [references/budget-planning.md](references/budget-planning.md) | 判断预算现实中能支撑什么 | CPA / ROAS 经济模型、预期转化量、广告系列组合可行性 |
| [references/measurement.md](references/measurement.md) | 设计转化信号、选归因、规划增量性、诊断衡量问题 | Consent Mode v2、增强型转化、OCI / ECfL、模型化转化、VTC 政策、归因、提升研究、iOS / SKAN、Tag Gateway / sGTM |
| [references/account-data-diagnostics.md](references/account-data-diagnostics.md) | 解读用户提供的账户数据（CSV / XLSX、截图、复制的表格、看板、CRM / Feed 报告或指标汇总） | 数据接入、必填字段、Ads / GA4 / CRM / Merchant 对账、实用的"如果…那么…"决策、优先级排序 |
| [references/diagnostic-decision-trees.md](references/diagnostic-decision-trees.md) | 优化现有账户或诊断效果不佳 | 花费 / 转化问题、线索质量、CTR / CVR、CPC 上涨、P-MAX / Demand Gen 陷阱 |
| [references/creative-strategy.md](references/creative-strategy.md) | 设计素材、文案、创意简报或制作交接 | 切入角度、信任状、异议处理、版式适配、P-MAX / Demand Gen / 视频创意体系、共用素材尺寸基线 |

### 交付输出

按[输出灵活性](#output-flexibility-dont-always-write-a-document)匹配输出形式。除非用户要求或计划明确要交接给其他团队 / 代理商 / 客户，否则不要产出书面交付物。

如果书面计划是合适的交付物，通常覆盖：

1. 策略摘要——目标、受众、为什么是这个广告类型组合。
2. 广告系列清单——每个广告系列的用途、出价、预算分配。
3. 分广告类型设计——由手册驱动。
4. 创意需求——需要哪些素材、规格、谁来生产。
5. 衡量设计——转化定义、VTC 政策、跟踪部署。
6. 运营时间线——上线 → 学习 → 优化各阶段。
7. KPI 与成功标准。

对于 Markdown 文件交付物，结构保持实用，不要模板化：章节清晰，该用决策表的地方用决策表，明确写出假设，列出具体的上线和衡量动作。不要填那些不改变广告系列决策的通用章节。

按业务模式和漏斗角色的常见多广告系列组合，参考 [references/business-model-playbooks.md](references/business-model-playbooks.md)。
