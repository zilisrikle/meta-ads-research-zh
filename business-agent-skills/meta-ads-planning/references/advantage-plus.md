# Meta Advantage+ 自动化

当需要判断 Meta 自动化应该被使用、被限制，还是被手动结构替代时，使用本参考文档。衡量（measurement）实现细节见 `measurement-and-attribution.md`。

## 运行模式

1. Advantage+ 已从可选的 opt-in（主动选择加入）变为三个端到端系列家族（Sales、Leads、App）的默认开启状态。这些目标下的手动模式仍然存在，但越来越多地被定位为备选方案。
2. 「Advantage」（不带 plus）的广告组层级优化，大多数在新建广告组时自动应用，其中很多无法干净地关闭。Detailed Targeting（详细定位）扩展行为不再是一个复选框，它就是默认行为。
3. 归因窗口与 API 字段名会发生变化。在硬编码浏览转化（view-through）、点击转化（click-through）或互动转化（engage-through）假设之前，先核验当前 Ads Manager / API 的行为。
4. 当账户提供这些拆分时，保持点击转化、互动转化和浏览转化的报表相互分离；非链接的社交互动不应被解读为站外链接点击。
5. CAPI（转化 API, Conversions API）对认真对待投放的账户而言已不再是可选项。AEM（汇总事件衡量, Aggregated Event Measurement）已不再限制每个域名 8 个事件（上限已于 2025-06 取消）。只用 Pixel（像素）而不用 CAPI 会被视为衡量上的 bug。
6. 在当前界面中，「这个系列是 Advantage+」意味着：Advantage+ Audience（Advantage+ 受众）+ Advantage+ Placements（Advantage+ 版位）+ Advantage Campaign Budget（Advantage 广告系列预算）同时开启。Ads Manager 中的绿色「Advantage+」标签就是视觉确认。

---

## Advantage+ 套件

Advantage+ 的世界有三层。对规划技能而言，层级之所以重要，是因为各层的「可逆性」不同：端到端的 Advantage+ 系列一旦开启很难在不损失学习期的情况下撤销，而单个 Advantage Detailed Targeting 开关翻转的成本很低。

| 层级 | 范围 | 示例 | 翻转成本 |
|---|---|---|---|
| 系列层级（端到端） | 整个系列的自动化：受众 + 版位 + 预算 + 素材选择 | Advantage+ Sales（Advantage+ 销售系列）、Advantage+ Leads（Advantage+ 线索系列）、Advantage+ App（Advantage+ 应用系列） | 高：结构性决策，会重启学习期，改变报表的列 |
| 广告组层级 | 手动系列内单个维度的自动化 | Advantage+ Audience、Advantage+ Placements、Advantage Campaign Budget、相似受众/自定义受众（CA, Custom Audience）/详细定位扩展 | 中：按广告组生效，可能重启学习期 |
| 广告层级 / 素材层级 | 按素材在投放时应用的增强 | Advantage+ Creative（Advantage+ 创意）（视觉微调、图片扩展、视频 9:16 扩展、音乐、文案变体、生成式 AI） | 低：按功能单独开关，不重启系列 |

### 1.1 Advantage+ Sales 系列

原名「Advantage+ Shopping Campaigns」（ASC）。2024 年中改名并扩大覆盖范围，涵盖非纯 DTC（直接面向消费者）销售链路。

| 字段 | 值 |
|---|---|
| 自动化内容 | 受众、版位、广告组间预算分配、素材选择、实时投放 |
| 所需目标 | Sales（销售）（购买 / 基于价值的优化） |
| 所需信号 | Pixel + CAPI 的购买 / 价值事件，最好 EMQ（事件匹配质量, Event Match Quality）达到 6+ |
| 目录（Catalog）要求 | 不要求；目录广告可以接入，但 Advantage+ Sales 的覆盖范围比 DPA（动态产品广告）更广 |
| 剩余手动控制项 | 受众建议（CA、LAL、年龄、性别）、排除（仅 CA，硬性）、系列预算、归因设置、素材库 |
| 现有客户预算上限 | 该控制项的可用性反复变化。未核查当前账户界面前，不要承诺该控制项存在。 |
| 预算指引 | Meta 官方的经验法则：预算要足够让每个广告组每周跑出 50 次转化 |
| 多广告组 | 支持。Sales 系列可包含多个广告组，每个广告组最多 50 条广告。 |
| 报表列 | 在配置了 Customer Lists（客户名单）的地方，可查看新客 vs 老客拆分 |
| 风险：蚕食 | 若上限被移除，可能收割现有客户；需以后端的新客比例核验，而非只看平台归因的 ROAS（广告支出回报率, Return on Ad Spend） |
| 风险：目录曝光 | 若品牌 SKU（库存单位）范围窄，自动化可能把花费集中在少数产品上 |

技能立场：不要把 Advantage+ Sales 当作弥补衡量糟糕、产品市场匹配度差、目录 feed 质量差或学习期长期预算不足的补救手段。务必以后端新客比例核验，不要只看平台归因的 ROAS。

### 1.2 Advantage+ Leads 系列

| 字段 | 值 |
|---|---|
| 自动化内容 | 受众、版位、预算分配、素材匹配 |
| 所需目标 | Leads（线索） |
| 转化位置 | Instant Forms（即时表单 / 线索广告）、网站线索表单、Messenger、电话。可以混合。 |
| 「Advantage+ ON」状态的所需条件 | Advantage+ 受众 + Advantage+ 版位 + Advantage 广告系列预算 + 至少一个合格的广告组 |
| 剩余手动控制项 | 受众建议、地区/年龄/语言/CA 排除、转化位置选择、CRM（Conversion Leads, 转化线索）集成 |
| 质量优化 | Conversion Leads CRM 优化是推荐的质量杠杆；只有在 Lead ID（线索 ID）回传到 Meta 后才生效 |
| 资格注意事项 | 「适用于符合条件的账户」；账户层面的 rollout（分批发布）可能不同 |
| 风险 | 以原始线索量为优化目标会掩盖劣质线索；没有 CRM 反馈，Advantage+ Leads 会把垃圾线索放大 |

技能立场：除非品牌接受以原始表单填写量为优化目标，否则不要在仅用即时表单的设置下开启 Advantage+ Leads。在放量前先搭配 Conversion Leads + CRM 阶段反馈。

### 1.3 Advantage+ App 系列

| 字段 | 值 |
|---|---|
| 自动化内容 | 受众、版位、素材选择、出价策略（自动 CPI, 每次安装费用）、预算 |
| 所需目标 | App promotion（应用推广） |
| 所需集成 | Meta SDK 或 MMP（移动衡量合作伙伴, Mobile Measurement Partner）（AppsFlyer、Adjust、Branch、Kochava、Singular 等） |
| iOS 特别说明 | 必须与 SKAdNetwork / AdAttributionKit 以及 Meta AEM-for-app 共存；系列必须打正确的标签，避免事件去重问题 |
| 素材上限 | 每个系列最多 50 条素材 |
| 优化目标 | App install（应用安装）、app event（应用事件）、value（价值） |
| 剩余控制项 | 应用、国家、语言、OS 拆分（手动备选）、ATT 感知的事件映射 |
| 风险 | iOS 建模意味着点击转化和浏览转化是部分建模的；不经过 MMP 交叉核验，不能把平台 CPA（每次转化费用, Cost Per Action）当作真相 |

技能立场：绝不只看 Meta 归因的安装量来优化 iOS 应用安装系列。必须做 MMP / AEM-for-app 对账。若两平台的经济模型或事件量分化，要把 iOS 和 Android 分开。

### 1.4 Advantage+ Audience（广告组层级）

原名「Advantage Detailed Targeting expansion」加泛定位 AI；已合并为单个广告组层级对象。

| 字段 | 值 |
|---|---|
| 自动化内容 | 在手动输入的兴趣、自定义受众、相似受众、人口属性之外选择受众 |
| 硬性控制项（AI 不可违反） | 最低年龄、地区（geo）、语言、自定义受众排除 |
| 软性控制项（仅作为建议） | 作为纳入条件的自定义受众、相似受众、最低年龄以上的年龄范围、性别、详细定位兴趣 |
| 自动应用 | 在可用时自动应用到新系列。例外：Advantage+ Sales 默认隐含开启。手动广告组会收到「Switch to Advantage+ Audience」（切换到 Advantage+ 受众）提示。 |
| 对详细定位的影响 | 详细定位的作用是种子（seed），不是围栏（fence）。不能依靠它做合规隔离。 |
| 对相似受众的影响 | 相似受众现在是 Advantage+ Audience 内部的「初始提示」，不是真正的受众下限 |
| 详细定位排除 | 已从 Advantage+ Audience 中移除。从 2025-03-31 起，Meta 移除了新建广告组以及在 Ads Manager 创建的在投既有系列中的详细定位排除。 |
| 风险 | 若系列要求受众保证（合规、品牌安全、年龄限制），Advantage+ Audience 无法通过兴趣来实现 |

技能立场：只有当 X 是自定义受众或硬性人口属性（年龄/地区/语言）时，才把「排除受众 X」编码为硬性控制项。其他任何东西都只是提示。

### 1.5 Advantage+ Placements（广告组层级）

| 字段 | 值 |
|---|---|
| 自动化内容 | 在 Facebook、Instagram（Feed / Stories / Reels / Explore）、Messenger、WhatsApp 营销消息、Audience Network、符合条件的 Threads 之间选择版位 |
| 默认 | Advantage+ Sales、Leads、App 默认开启；大多数目标的手动广告组默认开启但可编辑 |
| 「Advantage+ ON」状态据称要求 | 是，选择全部版位即视为 Advantage+ Placements 开启 |
| 手动控制项 | 按版位退出（Edit placements > 取消勾选）、排除 Audience Network、单独排除 Reels / Stories / Feed |
| 风险：Audience Network | 当 CPL（每条线索成本, Cost Per Lead）低得可疑时，常被指为低质量线索来源；需用互动和下游 CRM 核验 |
| 风险：仅 Reels 适配 | 素材必须是 9:16 且构图考虑安全区。自动裁剪不等于 Reels 原生素材。 |

技能立场：默认开启 Advantage+ Placements；只在有证据时退出版位（版位层级的 CPA、按来源的线索质量）。不要因为素材是为 Feed 做的就退出 Reels；应该去做 Reels 适配的素材。

### 1.6 Advantage Campaign Budget（CBO, 广告系列预算优化）

2026 年 CBO 在「Advantage」（不带 plus）品牌下延续。在运行有 Advantage+ 形态的手动目标（Sales、Leads、App）时，它是 Advantage+ ON 状态的必需项。

| 字段 | 值 |
|---|---|
| 自动化内容 | 实时在广告组之间分配预算 |
| 手动控制项 | 系列层级的每日/总预算、广告组花费限额（每个广告组最低/最高）、出价策略 |
| 风险 | 若未设置最低/最高限额，可能在单个广告组上超支；收割更便宜但质量更低的转化 |
| 与 Advantage+ 系列的交互 | 若 Advantage+ Sales / Leads / App 开启，CBO 是隐含的；端到端 Advantage+ 系列内不能跑 ABO（广告组预算优化） |

技能立场：运行手动目标时，若广告组共享同一学习事件，优先用 CBO + 广告组花费限额；若广告组以固定预算测试概念不同的受众/素材，优先用 ABO。

### 1.7 Advantage+ Lookalike（相似受众扩展）

残留的广告组层级功能，现多已被 Advantage+ Audience 吸收。

| 字段 | 值 |
|---|---|
| 自动化内容 | 当 AI 预测转化更高时，允许投放超出相似受众的百分比区间（如超出 1%） |
| 2026 年状态 | 功能上已被 Advantage+ Audience 取代。对许多目标而言，相似受众已不再是首要定位对象。 |
| 风险 | 报告「相似受众表现」会产生误导，因为实际投放已超出源受众范围 |

技能立场：2026 年不要把相似受众编码为独立策略。把它作为 Advantage+ Audience 的输入之一。

### 1.8 Advantage Detailed Targeting（兴趣扩展）

| 字段 | 值 |
|---|---|
| 自动化内容 | 当预测表现更好时，投放到所列兴趣之外 |
| 2026 年状态 | 被 Advantage+ Audience 吸收。Meta 持续合并颗粒度细的详细定位选项；部分账户/工具曾预警受影响的选项将于 2026-01-15 停止投放。在当前 Ads Manager 界面中核验受影响的兴趣。 |
| 详细定位排除 | 从 2025-03-31 起，从新建广告组以及在 Ads Manager 创建的在投既有系列中移除 |
| 风险 | 技能不得承诺兴趣层级的围栏；兴趣无法约束投放 |

### 1.9 Advantage Custom Audience（自定义受众扩展）

| 字段 | 值 |
|---|---|
| 自动化内容 | 当 AI 预测转化更高时，投放到种子 CA 之外 |
| 2026 年状态 | 实际已并入 Advantage+ Audience 的纳入行为 |
| 硬性控制项 | CA 排除仍是硬围栏；CA 纳入不是 |
| 风险 | 仅靠纳入条件无法实现「只再营销给 CA」 |

技能立场：要做真正的再营销（只展示给访问过页面的人），用「排除除 CA 之外的所有人」的方式叠加排除宽泛受众，但要接受这可能迫使系统退出 Advantage+ Audience。在当前界面中核验。

### 1.10 Advantage+ Creative（广告层级）

Advantage+ Creative 是一组在投放时按素材应用的增强包，外加不断增长的生成式 AI 功能。每个增强项可独立开关。

#### 视觉增强

| 功能 | 作用 | 位置 | 风险 |
|---|---|---|---|
| Image expansion（图片扩展） | 生成式 AI 填充画布，适配 Feed / Stories / Reels 的不同长宽比 | 单图、轮播 | logo / 文字边缘可能出现不符合品牌调性的伪影 |
| Visual touch-ups（视觉微调） | 自动裁剪、亮度、对比度、色彩调整 | 单图 | 品牌观感轻微漂移 |
| Aspect ratio variation（长宽比变体） | 生成 9:16 / 1:1 / 4:5 版本 | 单图、视频 | 构图可能破坏安全区 |
| Background generation（背景生成） | 生成式 AI 生成新的产品背景，可按提示词驱动 | 目录 / 单图 | 幻觉式上下文（如错误的场景） |
| 3D animation / image animation（3D 动画 / 图片动画） | 给静图加轻微动效 | 单图 | 在某些品类会分散对 CTA（行动号召）的注意力 |

#### 文案增强

| 功能 | 作用 | 风险 |
|---|---|---|
| Text variations（文案变体） | 重组并改写主文案、标题、描述；可能调换顺序 | 品牌语调漂移、宣称准确性 |
| Highlight key sentences（高亮关键句） | 加粗或强调正文中的部分内容 | 可能过度强调监管敏感的措辞 |
| Text generation（文案生成） | 生成式 AI 基于上传素材和品牌输入建议新的标题 / 主文案变体 | 发布前需要合规审核 |
| Text translation（文案翻译） | 将主文案和标题翻译成支持的语言（西班牙语、葡萄牙语、德语、法语、越南语、中文、印地语、他加禄语、孟加拉语等） | 本地化质量；法律文案必须人工审核 |

#### 音频 / 视频增强

| 功能 | 作用 | 风险 |
|---|---|---|
| Music（音乐） | 从 Meta 曲库自动选曲；广告主可固定一首曲目 | 曲风不匹配；版权由 Meta 管理 |
| Video expansion (to 9:16)（视频扩展至 9:16） | 生成式 AI 扩展画面边缘，使横屏/方形视频适配 Reels | 边缘伪影；主体重新居中问题 |
| Video animation from still（静图转视频动画） | 生成式 AI 把单图转成短动画视频 | 幻觉式动效，可能误导产品表达 |
| Voice dubbing / AI voice（配音 / AI 语音） | 自动配音、多语言配音 | 声音人设漂移；某些品类有法律限制 |
| Most relevant comment（最相关评论） | 在 Facebook / Instagram 广告下方展示一条热门评论 | 若展示逻辑选错，可能有负面评论风险 |

#### 品牌控制

| 功能 | 作用 |
|---|---|
| Brand Kit / brand consistency（品牌套件 / 品牌一致性） | 上传 logo、颜色、字体；AI 在生成的变体中应用 |
| Persona-targeted variants（按人群画像的变体） | 按受众画像生成多个广告变体（如价值追求型 vs 风格驱动型等） |

技能立场：视觉微调 + 图片扩展是最安全的默认项。生成式文案和音乐是对品牌风险最高的，需要按系列做明确的创意审核。对于受监管品类（金融、健康、就业），所有生成式变体在发布前必须人工审核。

### 1.11 Advantage+ Catalog Ads（目录广告）

目录广告（原 Dynamic Product Ads / DPA，动态产品广告）现在归入 Advantage+ 品牌。Catalog 广告版位是独立的投放位。

| 字段 | 值 |
|---|---|
| 自动化内容 | 为每次展示挑选合适的产品、图片，以及越来越多地挑选视频 |
| 目录输入 | 标准产品 feed（id、标题、图片、价格、库存、链接、描述、成色、品牌、GTIN、自定义标签） |
| 版式 | 轮播、合集、单图（默认）；通过 Dynamic Media for Catalog Ads（目录广告动态媒体）现已支持视频版式 |
| 目录视频 | 可在 SKU 层级挂视频；Meta 按展示选择视频或静态图 |
| 目录增强 | 目录素材可用背景生成、图片扩展、文字叠加、视频动画 |
| 风险 | feed 错误（缺图、「缺货」、链接损坏）会悄悄杀死产品投放；不健康的目录是隐形故障 |

技能立场：目录 feed 健康是一级诊断项。在优化预算之前先做目录 QA：被拒商品、低质量图片、缺失 GTIN、价格不匹配、URL 错误、库存同步新鲜度。

### 1.12 「Advantage+」在一个系列上到底指什么？

该短语在当前界面中有两种定义，规划技能必须把它们分开。

| 定义 | 触发条件 | 视觉标识 |
|---|---|---|
| 端到端 Advantage+ 系列 | 通过「Advantage+ Sales / Leads / App 系列」流程创建 | 创建时系列类型标签为「Advantage+」 |
| 手动系列上的「Advantage+ ON」徽章 | 同时满足三项：Advantage+ Audience 开启 + Advantage+ Placements 开启 + Advantage Campaign Budget 开启，外加至少一个合格的广告组 | Ads Manager 系列行中的绿色「Advantage+」胶囊 |

技能立场：当投手说「这个系列是 Advantage+」时，先澄清他指的是哪一种定义。端到端形态的约束更严格（受众控制有限、无 ABO），而带绿色徽章的手动系列更灵活。

---

## Advantage+ vs 手动决策树

```
业务结果是 Advantage+ 支持的结果吗？
  Sales（销售）（购买 / 价值）          -> Advantage+ Sales 是候选项
  Leads（线索）（表单 / 高质量线索）      -> Advantage+ Leads 是候选项
  App install（应用安装）/ app event（应用事件） -> Advantage+ App 是候选项
  Awareness（品牌认知）/ Traffic（流量）/ Engagement（互动）/ Reach（覆盖）/ Video views（视频观看）
                                         -> 没有端到端的 Advantage+ 形态；
                                            用手动 + Advantage+ Audience + Advantage+ Placements + Advantage+ Creative

如果结果被支持：
  衡量是否健康？
    Pixel + CAPI 已部署、去重已验证、EMQ 6+、价值/货币稳定 -> 继续
    否                                                       -> 停。先修衡量；用手动 + 更紧的定位。

  事件量是否足够？
    每个广告组每周 >= 50 次转化，或系列层级 CBO 可行 -> 继续
    否                                                -> 手动，用更宽泛的事件（如 AddToCart，加购）或更少的广告组。

  是否有硬性受众约束？
    只有最低年龄、地区、语言、CA 排除              -> Advantage+ 可用
    需要兴趣围栏、相似受众下限、年龄范围上限、性别排除 -> 手动；Advantage+ Audience 保证不了这些。

  素材是否适配版位？
    Reels 适配的 9:16、安全区合规、多个变体       -> Advantage+ Placements 可用；允许 Advantage+ Creative 增强
    只有 Feed 用的横版素材、无 Reels 版本          -> 要么做 Reels 适配素材，要么手动 + 仅 Feed。

  品牌是否受监管（金融 / 就业 / 住房 / 健康 / 政治）？
    是                                             -> 手动；Advantage+ Audience 和生成式素材都会增加政策风险。
    否                                             -> Advantage+ 放行，但受以上条件约束。

如果结果不被支持（Awareness / Traffic / Engagement / Reach / Video）：
  用手动系列目标，并：
    Advantage+ Audience 开启（除非受众有超出年龄/地区/语言/CA 的硬性约束）
    Advantage+ Placements 开启
    多广告组时 Advantage Campaign Budget 开启
    Advantage+ Creative 增强按需选择（视觉微调 + 图片扩展是安全默认项）
```

---

## 易变能力清单

本清单中的条目在 2024-2026 年间发生过变化；在编码为技能中的硬规则之前应重新核验。

| 条目 | 为何易变 | 动作 |
|---|---|---|
| Sales 系列的 Existing Customer Budget Cap（现有客户预算上限） | 该控制项及其命名反复变化 | 承诺前先在当前界面核验 |
| 归因窗口 API 支持 | 报表窗口可能被移除或改名 | 在做仪表盘建议前先检查 Marketing API / Ads Insights 行为 |
| Click-through（点击转化）vs engage-through（互动转化）定义 | 互动分类方式发生过变化 | 定义变化时重置基线 |
| AEM 事件上限 / AEM 界面 | 事件上限和设置界面发生过变化 | 除非当前界面要求，否则不要让用户手动选固定的事件上限 |
| Detailed Targeting 和排除 | 兴趣类目和排除控制项经常变化 | 未在界面核查前，不要编码具体的兴趣名称或排除可用性 |
| 相似受众作为首要定位 | 常被并入 Advantage+ Audience 行为 | 不要编码为独立的默认策略 |
| Advantage+ ON 徽章标准 | 界面标签和要求措辞可能变化 | 在当前界面核验确切措辞 |
| Catalog 视频 / Dynamic Media for Catalog Ads | 可用性因账户和 rollout 而异 | 按账户确认可用性 |
| Conversion Leads 要求 | CRM 阶段和事件要求对设置敏感 | 承诺资格前重读开发者文档 |
| EMQ 分数阈值 | EMQ 是诊断信号，不是稳定的规划契约 | 只作方向性信号使用 |
| 生成式 AI 功能 | 按账户可用性不同 | 按账户核验并评估品牌适配度 |
| Special Ad Category（特殊广告类别）定位限制 | 政策变化可能改变定位和表单 | 上线前重读 Meta 广告标准 |

以上易变条目的当前官方核查点：

- Meta Advantage+ Sales: https://www.facebook.com/business/ads/meta-advantage-plus/sales-campaigns
- Meta Advantage+ Leads: https://www.facebook.com/business/ads/meta-advantage-plus/leads
- Meta Advantage+ App: https://www.facebook.com/business/ads/meta-advantage-plus/app-campaigns
- Meta Advantage+ Audience: https://www.facebook.com/business/ads/meta-advantage-plus/audience
- Meta Advantage+ Placements: https://www.facebook.com/business/ads/meta-advantage-plus/placements
- Meta Advantage+ Creative: https://www.facebook.com/business/ads/meta-advantage-plus/creative
- Meta Conversions API server events: https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/server-event
- Meta CAPI deduplication: https://developers.facebook.com/docs/marketing-api/conversions-api/deduplicate-pixel-and-server-events
- Meta Conversion Leads CRM integration: https://developers.facebook.com/docs/marketing-api/conversions-api/conversion-leads-integration

---

## 如何在规划中使用本参考文档

当建议依赖以下任何一项时，使用本文件：

- 是否用 Advantage+ Sales、Advantage+ Leads、Advantage+ App，还是手动结构。
- 哪些 Advantage+ 控制项仍由人工掌控：客户定义、排除、目录 / feed 质量、转化事件、价值、素材质量、预算和增量测试。
- 如何解读 2026 年归因 / API 变化，尤其是点击转化 vs 互动转化 vs 浏览转化。
- Pixel / CAPI / 线下 / CRM / 应用事件是否足以支撑拟用的优化事件。
- 仪表盘或 API 管道是否需要归因窗口迁移。
- 承诺功能前必须在当前界面核验哪些控制项。

当本文件与更短的概览文件冲突时，以本文件为准，并对易变控制项加上「核验当前界面 / API」的说明。

---

## 按目标划分的 Advantage+ 控制项

下面的网格是规划技能在选择按目标 playbook（打法）时引用的「横向层」。按行阅读：「对于目标 X，哪些 Advantage+ 控制项是可用、默认、硬性或不可用」。

| 目标 | A+ 端到端形态 | A+ Audience | A+ Placements | A+ CBO | A+ Creative | Catalog 广告 | Conversion Leads | iOS 应用约束 |
|---|---|---|---|---|---|---|---|---|
| Sales（销售）（购买 / 价值） | 可用（A+ Sales） | 默认开启；硬围栏 = 最低年龄、地区、语言、CA 排除 | 默认开启 | A+ Sales 中默认开启；手动中可选 | 可用 | 支持（DPA / 目录） | 不适用 | 网页衡量；AEM 自动 |
| Leads（线索） | 可用（A+ Leads） | 默认开启 | 默认开启 | A+ Leads 中默认开启 | 可用 | 不支持 | 强烈推荐 | 不适用 |
| App promotion（应用推广） | 可用（A+ App） | 默认开启，但受应用商店/地区约束 | 默认开启 | A+ App 中默认开启 | 可用，功能较少 | 仅应用目录（catalog mobile app ads，目录移动应用广告） | 不适用 | 需要 SKAN + AdAttributionKit + AEM-for-app |
| Awareness（品牌认知） | 无 A+ 端到端形态 | 手动默认；A+ Audience 可选加入 | 默认开启 | CBO 可选 | 创意增强有限 | 不支持 | 不适用 | iOS 浏览转化高度建模 |
| Traffic（流量） | 无 A+ 端到端形态 | 手动默认；A+ Audience 可选加入 | 默认开启 | CBO 可选 | 可用 | 不支持 | 不适用 | 有限 |
| Engagement（互动） | 无 A+ 端到端形态 | 手动默认 | 默认开启 | CBO 可选 | 可用 | 不支持 | 不适用 | 有限 |
| Reach（覆盖） | 无 A+ 端到端形态 | 手动默认 | 默认开启 | CBO 可选 | 有限 | 不支持 | 不适用 | 有限 |
| Video views（视频观看） | 无 A+ 端到端形态 | 手动默认 | 默认开启 | CBO 可选 | 有限 | 不支持 | 不适用 | Engage-through 5s 阈值重要 |
| Messages（消息） | 部分地区有消息类小众 A+ 形态 | 手动或 A+ Audience | 默认开启（含符合条件的 WhatsApp 营销） | CBO 可选 | 可用 | 不支持 | 不适用 | 有限 |

按行的可用性随 rollout 变化；按账户在当前界面核验。

## 手动备选结构

当 Advantage+ 端到端形态不合适时，规划回退到手动目标 + 广告组层级的 Advantage 控制项。推荐骨架：

### 9.1 Sales 备选（手动购买）

```
Campaign
  Objective: Sales（销售）（购买，价值数据允许时按价值优化）
  Bid strategy: Highest value（最高价值）、ROAS goal（ROAS 目标）或 Cost per result goal（单次成效费用目标），取决于价值数据和目标稳定性
  Budget: 2 个以上广告组共享同一受众家族时，用 CBO + 广告组花费上限
Ad set A: Broad（宽泛）+ A+ Audience，种子 = 无（真正的宽泛）
  Hard controls: country（国家）、language（语言）、age min（最低年龄）
  CA exclusions: customers（客户）、returning visitors（回访用户）（按策略决定）
Ad set B: Retargeting（再营销）（仅 CA 纳入）
  CA inclusion: site visitors 30d（30 天网站访客）/ ATC 30d（30 天加购）/ IG-FB engagers 30d（30 天 IG-FB 互动用户）
  Note: 按 2026 年规则，CA 纳入不构成投放围栏；在界面中核验
Ad set C（可选）: 按合规或业务原因做的地区/人群切分
Ads
  每个广告组 4-8 条概念差异化的素材
  单图、单视频、轮播混合
  Reels / Stories 用 9:16 版本
  A+ Creative：视觉微调 + 图片扩展开启；文案变体和音乐按需选择
```

### 9.2 Leads 备选（手动线索）

```
Campaign
  Objective: Leads（线索）
  Conversion location: 按漏斗定；Instant Forms 适合低意向放量，网站适合更高意向
  Optimization: CRM 阶段反馈接好后用 Conversion Leads；否则用 Lead。
Ad set A: Broad + A+ Audience
  Hard controls: country、language、age min
Ad set B: Retargeting（再营销）（温暖网站 / 互动用户的 CA 纳入）
Ads
  3-6 个概念；线索磁铁钩子、社会证明、合规范围内的紧迫感
  9:16 + 1:1 + 4:5 版本
  A+ Creative：视觉微调开启；生成式文案在品牌审核前关闭
```

### 9.3 App 备选（手动应用）

```
Campaign
  Objective: App promotion（应用推广）（手动）
  Optimization: 数据支持时用 app event（应用事件）（安装后）；否则用安装
Ad set 按 OS（iOS / Android）拆分，按需按地区拆分
  Hard controls: country、language、OS、age min
Ads
  6-12 个概念
  竖屏（9:16）优先，适配 Reels / Stories
  Playable（可玩广告）/ cinemagraph（动态图片）/ UGC 混合
  A+ Creative：视觉微调 + 图片扩展开启
```

### 9.4 Awareness / Traffic / Engagement 备选

这些目标从没有 Advantage+ 端到端形态。用：

- Awareness：Reach 优化、频次上限（如 2/7 天）、宽泛受众、A+ Placements 开启
- Traffic：Landing page views（落地页浏览）（不是 link clicks，链接点击）优化、A+ Audience 开启、A+ Placements 开启
- Engagement：谨慎选择互动类型（post engagement 帖子互动 / video views 视频观看 / Page likes 主页赞）；互动很少是下游价值的驱动因素

技能立场：不要给带来收入的系列推荐 Engagement 或 Traffic 目标。它们优化的是便宜的代理指标，会破坏归因可比性。

---

## 按品类的 Advantage+ Creative 开关矩阵

规划技能可推荐的默认值，以按账户的品牌审核为准。

| 品类 | 视觉微调 | 图片扩展 | 长宽比变体 | 背景生成 | 音乐 | 文案变体 | 生成式文案 | 视频动画 | 配音 |
|---|---|---|---|---|---|---|---|---|---|
| 电商（DTC 综合） | 开启 | 开启 | 开启 | 开启 | 默认关闭，可测试 | 开启 | 开启，需品牌审核 | 默认关闭，在 hero SKU 上测试 | 关闭 |
| 电商（奢侈品 / 时尚） | 开启 | 关闭 | 开启 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| 本地服务 | 开启 | 开启 | 开启 | 关闭 | 开启 | 开启 | 关闭 | 关闭 | 关闭 |
| B2B SaaS | 开启 | 关闭 | 开启 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| 教育 / 课程 | 开启 | 开启 | 开启 | 关闭 | 关闭 | 开启 | 关闭 | 关闭 | 关闭 |
| 健康 / 养生（受监管） | 开启 | 关闭 | 开启 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| 金融（受监管） | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| 就业（特殊广告类别） | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| 住房（特殊广告类别） | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| 政治（特殊广告类别） | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 | 关闭 |
| 应用 / 游戏 | 开启 | 开启 | 开启 | 开启 | 开启 | 开启 | 开启 | 开启 | 默认关闭 |
| 餐饮 / 食品 | 开启 | 开启 | 开启 | 关闭 | 开启 | 开启 | 关闭 | 关闭 | 关闭 |
| 旅行 | 开启 | 开启 | 开启 | 关闭 | 开启 | 开启 | 默认关闭 | 默认关闭 | 关闭 |

技能立场：在受监管品类中，每个生成式变体在发布前必须经过人工审核；AI 建议的措辞即使技术上为真，也可能违反平台政策或监管政策。

---

## 易变规划问题

技能应明确标注「在当前界面 / API 中核验」、而不是当作事实编码的条目：

1. 当前 Ads Manager 中绿色「Advantage+」胶囊的确切措辞（可能被改名）。
2. 投手账户中 Existing Customer Budget Cap（现有客户预算上限）是可用、被移除，还是发生了变化。
3. `1d_engaged_view` 是否是互动转化的现行 API 参数名，还是 Meta 已改名 API token。
4. 投手账户中生成式视频动画、配音和 Brand Kit 功能的按账户可用性。
5. 投手账户中 Conversion Leads 是否支持非线索广告的网站线索表单。
6. Advantage+ Creative 文案翻译当前支持的语言列表（每季度变化）。
7. 投手目录类型下 Catalog 视频 / Dynamic Media for Catalog Ads 的可用性。
8. 投手垂直领域在 2026-01-15 合并后幸存的详细定位兴趣类目。
9. 投手账户上 Integration Quality API（集成质量 API）beta 是否启用。
10. AEM-for-app 与投手 MMP 共存是否干净（在 MMP 仪表盘中核验映射）。

技能应把投手核验后的答案记录为规划输入，而不是假设。

---
