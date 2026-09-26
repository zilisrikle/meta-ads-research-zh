# Meta Leads（线索）系列

## 运行实践

线索系列要跑好，**线索路径、表单/消息设计、资质筛选逻辑、跟进运营和 CRM 反馈闭环**必须围绕单一业务结果（合格的 pipeline / 预约 / 关单收入）设计，而不是围绕原始 CPL（每条线索成本）。

### 最重要的事

- **CPL 便宜不是成功。** 以合格线索率、接通率、预约率、SQL（销售合格线索）/商机率、关单率和收入为优化目标。把原始 CPL 只当作前端运营指标。
- **摩擦是杠杆，不是缺陷。** Higher Intent（高意向）表单和资质筛选问题故意降低量来提升质量。合适的摩擦取决于客单价、销售人力和历史线索质量。
- **符合条件时，Conversion Leads（转化线索）是最强的质量解锁。** 当账户满足就绪条件（Meta 开发者指引列出：每月至少 200 条线索、15-16 位 Meta Lead ID 已映射、每日上传、优化阶段在 28 天内、阶段 CVR（转化率）1-40%），Meta 会朝更深的 CRM 阶段而非表单填写优化。不满足这些条件时不要承诺效果。
- **Advantage+ Leads 是自动化优先，不是魔法按钮。** Advantage+ 开启时，受众、版位和系列预算同时自动化；只有自定义受众排除、18+ 年龄、地区/语言和账户层级控制会被遵守而不关闭 Advantage+。在检查表单、offer（卖点）、跟进和 CRM 反馈之前，不要把线索质量差归咎于 Advantage+。
- **跟进速度胜过表单长度。** 对高意向的 Meta 来源线索，运营要围绕极快的确认和分流设计；click-to-message（点击发消息）线索基本是实时的，意向流失很快。
- **来源拆分能揭示质量差异点。** 版位、广告、表单、受众、性别/年龄、地区、素材的拆分，能看出哪些组合产出合格 pipeline、哪些产出垃圾。
- **EMQ（事件匹配质量, Event Match Quality）是诊断指标，不是 KPI。** EMQ 越高，Meta 能把越多线索事件匹配到用户身份，Conversion Leads / 从线索做相似受众 / 客户名单工作流的效果越好。7+ 视为「好」，9+ 视为「优秀」；不要写「EMQ 必须 8+」这种硬规则。
- **Special Ad Categories（特殊广告类别）会压缩定位/衡量/优化工具箱。** 如果 offer 是金融、就业、住房或政治/社会议题，规划必须在系列搭建之前做。自定义受众、相似受众、ZIP（邮编）、年龄/性别以及很多线索表单问题类型都会受限。

### 诊断速查表

| 症状 | 先查 | 可能的动作 |
|---|---|---|
| CPL 便宜，SQL 差 | 表单类型、资质筛选问题、跟进 SLA（服务等级协议）、版位拆分、受众宽窄 | More Volume（多线索量）切 Higher Intent，加 1-3 个筛选问题，建 CRM 反馈，只在有证据时排除弱版位 |
| 表单打开多，提交少 | 表单长度、介绍页清晰度、信任背书、条件逻辑死胡同 | 缩短表单、强化介绍/价值主张、修复条件分支 |
| 线索多，接通极少 | 电话/邮箱有效性、预填 vs 手输、autofill（自动填充）滥用 | 开 SMS（短信）验证、电话/邮箱关掉自动填充、电话用手输短文本 |
| WhatsApp / Messenger 量大但无资质筛选 | 欢迎语具体度、响应速度、筛选话术 | 预填消息、自动确认 SLA 压到 5 秒内、筛选 chatbot（聊天机器人） |
| 电话量大、质量低 | 国家代码、营业时间排期、电话广告素材先筛选 | 用总预算 + 分时投放、广告文案先筛选、电话接入 CRM 做 call-tracking（通话追踪） |
| 早期量好，之后崩 | 受众耗尽、素材疲劳、学习期重置、归因窗口变化 | 刷新素材、放宽受众、保持不编辑、审计归因窗口设置 |
| Conversion Leads 不优化 | 量低于当前下限（Meta 开发者指引列出至少 200/月）、lead ID 不匹配、阶段 CVR 在 1-40% 之外、延迟 >28 天 | 先退回 Maximize Leads（最大化线索数），重建反馈闭环 |

### 常见坑

- 因为 CPL 低就放量；从不审计 CRM 阶段。
- 在受监管行业跑 More Volume 表单，而那里线索质量最重要。
- 把「Conversion Leads」当按钮，而不是几个月的 CRM-CAPI 工程。
- 学习期还没稳定就 24-48 小时拉掉广告组；线索系列至少给 7 天有意义的事件量。
- 价值还没讲清楚就堆太多筛选问题，压垮漏斗。
- 让 Audience Network 占线索系列的大多数展示，却不核验来源质量。
- 承诺了 Advantage+ Leads，又过度限制受众/版位，悄悄关掉了 Advantage+。
- 忘记账户被标 Special Ad Category 后，自定义受众排除、相似受众和很多人口属性过滤器都会消失。

本文件中用到的术语表见 [SKILL.md 术语表](../SKILL.md#common-meta-ads-glossary)。

---

## 按月线索量 / 合格线索率的决策矩阵

该矩阵用 Meta Leads 最重要的两个轴，替代 Google Ads 的「月转化数」矩阵。

| 月线索量 | 合格线索率 | 推荐路径 | 效果目标 | 表单姿态 | 出价策略 |
|---|---|---|---|---|---|
| <50 | 未知 / 不稳定 | 手动 Leads，Instant Form（More Volume），或 Pixel / CAPI 干净时用网站表单 | Maximize number of leads（最大化线索数） | 短，1-2 个筛选问题，自定义介绍 | Highest volume（最高投放量）；不做成本控制 |
| 50-200 | <20% 合格 | 手动 Leads，切 Higher Intent，加 2-3 个筛选问题；并行建 CRM 反馈 | Maximize number of leads | Higher Intent + 电话是核心资产时加 SMS 验证 | Highest volume；7 天 CPL 稳定后考虑 Cost per result goal（单次成效费用目标） |
| 50-200 | 20-50% 合格 | 手动 Leads，More Volume + 筛选问题，或 Higher Intent | Maximize number of leads | 紧凑表单，1-3 个筛选 | Highest volume；学习稳定后用 Cost per result goal |
| 200-500 | <30% 合格 | 建 Conversion Leads 就绪（目标 28 天阶段 <40% CVR），Higher Intent 表单，90 天内 CRM 反馈上线 | Maximize number of leads（过渡）→ 就绪后 Maximize conversion leads（最大化转化线索数） | Higher Intent 或网站表单；SMS 验证 | 过渡期 Highest volume；Conversion Leads 稳定后 Cost per result goal |
| 200-500 | 30-50% 合格 | Advantage+ Leads + CRM 反馈；Conversion Leads 优化 | Maximize conversion leads | Higher Intent 或 Rich Creative（富创意） | 按合格线索成本目标设 Cost per result goal |
| 500+ | 30%+ 合格 | 默认 Advantage+ Leads + Conversion Leads；受限品类或严格控制用手动 Leads | Maximize conversion leads | Higher Intent，高考虑度 offer 用 Rich Creative | 按阶段经济模型用 Cost per result goal 或 bid cap（出价上限） |
| 500+ | <30% 合格 | 先诊断（表单/受众/素材）。不要只看量就放量。 | Maximize number of leads（过渡） | Higher Intent + 筛选问题 + SMS 验证 | Highest volume + 手动排除控制 |

备注：

- Meta 的 Conversion Leads 开发者指引列出：每月至少 200 条线索、每日上传、28 天阶段时限、1-40% 阶段 CVR，作为就绪指引。按所涉账户确认当前界面/合作伙伴文档，把量级阈值当作下限，不是冲刺目标。
- 「Maximize number of conversion leads」效果目标是独立选项，Conversion Leads CRM 数据接好、Meta 看到更深阶段事件后出现。
- 典型的就绪建设端到端要 1-3 个月；反馈稳定后优化器完全收敛据称约 45-60 天。把 90 天作为 Conversion Leads 的规划周期。

---

## 线索路径选择矩阵

这是 Meta Leads 系列「选哪条路」的标准决策树。

### 路径汇总表

| 路径 | 转化位置 | 适合 | 关键要求 | 质量风险 |
|---|---|---|---|---|
| Instant Form（More Volume） | Meta 托管表单 | 移动端放量、漏斗顶部内容、webinar（网络研讨会）/指南/等候名单 | 除 Page（主页）和隐私政策外无其他要求 | 意向低、自动填充驱动的提交 |
| Instant Form（Higher Intent） | Meta 托管表单，带复核页 + 可选 SMS 验证 | 中/高风险服务、B2B、受监管行业 | 受众能接受复核页 | 量更低，CPL 可能更高 |
| Instant Form（Rich Creative） | Meta 托管表单，带品牌板块 | 高考虑度、品牌主导、多产品、社会证明重 | 强素材资产和文案 | 卡片弱则表单深度疲劳 |
| 网站线索表单（带 CAPI） | 广告主网站 | 自定义 UX、完整第一方数据捕获、复杂筛选 | Pixel + CAPI 去重；移动端速度；网站上有线索表单 | 移动端摩擦降量 |
| Instant Form + 网站（多目的地） | Meta 按用户路由 | 对冲量 + 质量 | 两个目的地都在线 | 需要拆分报表 |
| Click-to-Messenger 线索 | Messenger | 对话式筛选、Facebook 原生受众 | Page Inbox（主页收件箱）/ chatbot SLA | 响应慢就死 |
| Click-to-Instagram-Direct 线索 | IG Direct | IG 优先的创作者/生活方式/DTC 服务受众 | IG 收件箱 SLA / IG 自动化 | 同上 |
| Click-to-WhatsApp 线索 | WhatsApp Business / WABA（WhatsApp Business Platform） | LATAM（拉美）/ EMEA（欧非中东）/ 印度、高考虑度、本地受监管 | WhatsApp Business 账户、预填消息、坐席/bot SLA | WABA 模板审批、区域定价 |
| Calls（带电话的线索广告） | 电话拨号器 | 紧急本地服务、家政服务、医疗预约 | 国家电话号码、营业时间 | 漏接、非营业时间浪费 |
| Conversion Leads | 以上任一 + CRM CAPI | 成熟线索项目，每月至少约 200 条线索（核验当前界面/合作伙伴文档） | Lead ID 映射 + 每日上传 + 28 天/<40% 阶段 CVR | 设置配错 = 优化无增益 |
| Advantage+ Leads | 任一转化位置 | 想自动化的合格账户 | 宽泛受众容忍度、仅自定义受众排除、18+、全版位 | 质量问题需要反馈闭环 |

### 选择逻辑（路径 → 质量 → 匹配）

```
1. offer 是否受监管（金融 / 就业 / 住房 / 政治）？
   YES → Special Ad Category。表单问题受限、无人口属性定位、
         无相似受众、无客户名单受众（美国 2025+）。用 Higher Intent
         + 手动或受限的 Advantage+；允许范围内用 CAPI 传第一方信号。
   NO  → 继续。

2. 买家在留资前是否期待实时对话？
   YES → Click-to-message。选：
         - WhatsApp：LATAM / EMEA / 印度或本地惯例。
         - Messenger：Facebook 原生 B2C。
         - Instagram Direct：IG 主导的品牌或创作者。
         备选：对话聊天运营弱则用电话。
   NO  → 继续。

3. 移动端 + Meta 原生表单可接受吗？
   YES → Instant Form。选：
         - Higher Intent：B2B / 服务 / 受监管 / 高客单 DTC。
         - Rich Creative：品牌叙事 + 多产品 / 社会证明。
         - More Volume：仅当下游筛选便宜时。
   NO  → 网站表单（带 Pixel + CAPI 去重）。仅当网站的表单 UX 或
         CRM 逻辑明显更好时用。

4. 量 + 合格线索率够 Conversion Leads 吗？
   （Meta 开发者指引列出至少 200/月、lead ID 已映射、每日上传、阶段 <28 天、阶段 CVR 1-40%）
   YES → 建 Conversion Leads；然后把效果目标切到
         「Maximize conversion leads」。
   NO  → 留在「Maximize number of leads」；并行建就绪条件。

5. 账户符合 Advantage+ Leads 资格且能接受宽泛受众？
   YES → 默认 Advantage+ Leads；核验 Advantage+ 状态保持「On」
         （受众/版位不要过度限制）。
   NO  → 手动 Leads，控制受众和版位；记录原因。
```

---

## 表单设计 playbook

表单设计是 Meta Lead 系列中仅次于路径选择的最高杠杆。

### 表单类型

| 表单类型 | 机制 | 何时用 | 何时避开 |
|---|---|---|---|
| More Volume | 用预填字段一键提交 | 漏斗顶部、低风险资产（指南、webinar、名单）、素材已筛选受众 | 线索质量关键；电话有效性重要；客单价高 |
| Higher Intent | 加复核页，强制用户确认预填信息再提交；支持电话号码 SMS 验证 | B2B、服务、金融/保险/汽车/房产（允许范围内）、高客单 DTC | 真正的漏斗顶部内容，任何摩擦都会把量压到下限以下 |
| Rich Creative | 可定制板块：Intro（介绍）图、最多 3 个 Benefits（权益）、最多 4 个 Build Your Story（品牌故事）子板块（How It Works / More About Us / How We're Different / Highlights，每个 2-5 步）、Products（产品）轮播最多 5 张卡、Social Proof（社会证明）、Incentives（激励） | 高考虑度购买，提交前需要信任和教育 | 简单 offer；素材薄；运营驱动的品类，速度 > 精致 |

### 问题类型与上限

| 问题类型 | 行为 | 最佳用途 | 备注 |
|---|---|---|---|
| Prefilled standard fields（预填标准字段） | 邮箱、电话、全名、城市、州、国家、ZIP、DOB（出生日期，允许范围内） | 漏斗顶部降摩擦 | 预填 = 低意向。有效性重要的字段关掉自动填充。 |
| Multiple Choice（多选） | 约 6 个选项内展示干净 | 筛选（预算区间、角色、服务兴趣） | 选项互斥且穷尽 |
| Short Answer（短文本） | 自由文本，按 Meta 界面设上限 | 有效性重要时电话用手输；具体意向（项目描述） | 手输电话减少自动填充机器人 |
| Conditional（条件） | 按上一题答案动态出选项（CSV 驱动） | 服务区域路由、多产品流程 | 路由路径：「Go to question」（跳到问题）、「Submit Form」（提交表单）、「Close Form」（关闭表单，不提交给非线索） |
| Appointment Request（预约请求） | 日期时间选择器 | 本地服务、演示、咨询 | 和日历/CRM 同步；否则预约会掉 |
| Business Locator（门店定位） | 按距离列出门店 | 多门店零售/诊所/经销商 | 门店可很多；用户选最近的 |
| Image Select（图片选择） | 最多 8 个图片选项 | 视觉选择（风格偏好、产品类型） | 文字模糊时筛选更强 |
| Slider（滑杆） | 区间输入（如 1-10） | 自评意向/预算区间 | 适合打分；单独用弱 |

把 3-6 个自定义问题当作完成的实用上限。建长表单前先在 Ads Manager 核验当前 Instant Form 字段上限。

### 问题设计规则

| 规则 | 原因 |
|---|---|
| 从低摩擦 → 高摩擦排序 | 锚定承诺；答了 1-2 题的用户更可能答完 |
| 非必要问题标为可选 | 条件问题不能设为可选；其他都可以 |
| 筛选用 Multiple Choice 不用 Short Answer | CRM 里分段更干净；流失更低 |
| 用 Conditional → Close Form 剔除 | 在进 CRM 前拦下不匹配的提交；该线索不被收集 |
| 加自定义介绍页 | 提问前讲清价值，可减少不匹配的提交 |
| 打电话很关键则电话用手输 Short Answer | 缓解陈旧号码的自动填充 |

### 完成（感谢）页

可配置元素：

- Headline（标题）+ description（描述）
- Action button（行动按钮）：访问网站、下载 PDF、打电话、兑换 promo code（优惠码）、通过 Messenger / WhatsApp 发消息
- 条件逻辑把线索和非线索分开时，用不同的完成页（只有线索看到下一步 CTA）

最佳实践：

| 目标 | 推荐的完成页 CTA |
|---|---|
| 跟进速度 | 「Call us now」（立即致电）+ 点击拨号 |
| 自助内容 | 「Download the guide」（下载指南）PDF 链接 |
| 销售交接 | 「Book a time」（预约时间）链接到排期工具（Calendly / 原生排期） |
| 对话延续 | 「Message us on WhatsApp / Messenger」（在 WhatsApp / Messenger 上联系我们） |
| 优惠兑换 | 「Use code XYZ on the site」（在网站用 XYZ 优惠码）+ 网站按钮 |

### 隐私 / 免责声明

- 隐私政策链接是强制的。
- 自定义法律免责声明是可选的；很多受监管品类（保险、金融、医疗、就业）按 Meta 政策要求必须有。
- Special Ad Categories 下，即时表单中关于年龄、性别、感情状态、地理位置的个人信息问题受限。

### 质量功能（2024-2025 上线）

| 功能 | 用途 | 备注 |
|---|---|---|
| SMS passcode verification（短信验证码验证） | 验证输入的电话号码能联系到本人 | 服务/电话跟进类大幅提升电话有效性；完成率小幅下降 |
| Work email verification（工作邮箱验证） | B2B 中过滤消费者免费邮箱 | B2B SaaS/服务用，个人邮箱污染 pipeline 时 |
| Turn off autofill（关闭自动填充） | 强制指定字段手输 | 减少自动填充的陈旧数据；CPL 上升 |
| Flexible form delivery（灵活表单投放） | Meta 自动排序问题/换背景 | 不能手动测变体时有用；用拆分报表核验 |
| Allow multiple responses（允许多选） | 复选框式多选题 | 产品兴趣/服务篮子问题用 |
| Promo code incentive（优惠码激励） | 完成页展示折扣/激励 | DTC 和活动报名好用 |
| Lead delivery by email（邮件发送线索） | 除 CRM 外再发一份线索明细到邮箱 | 备用通道；不要当主通道 |
| Instant form templates（即时表单模板） | 按目标预置的表单结构 | 快速上线；和质量策略冲突的默认值要换掉 |

---

## Conversion Leads 就绪检查清单

推荐 Conversion Leads 优化前用本清单。任何一行是「否」或「未知」，都不要承诺优化增益。

| 条目 | 要求状态 | 如何核验 |
|---|---|---|
| 线索路径是 Instant Form / Lead Ads | 是 | 系列目标 = Leads，转化位置 = Instant Form / Website Form / Multi-dest（多目的地） |
| Meta Lead ID 已映射到 CRM 记录 | 是，每条线索都有 | 抽查 CRM 线索记录的 `lead_id` 字段 |
| 线索量足以支撑 CRM 阶段优化 | 是 | 过去 30-90 天该广告账户的线索数 |
| CRM → Meta 上传节奏稳定 | 是 | 查 CRM 合作伙伴集成 / CAPI 日志的每日事件 |
| 优化阶段发生得够快 | 是 | 销售周期数据：线索到优化阶段的中位时长 |
| 阶段转化率既不太稀疏也不太宽泛 | 是 | 过去 90 天 到达阶段的线索 / 总线索 |
| 线索事件的 EMQ 健康 | 是 | Events Manager 诊断和 EMQ |
| CRM CAPI 事件上的 `lead_event_source` 标识了 CRM | 是 | 检查原始事件 payload（载荷） |
| 自定义事件名符合预期的阶段分类或已通过合作伙伴映射 | 是 | CRM 合作伙伴集成已验证 |
| 有备用集成 | 是 | 主合作伙伴挂了，线索照样同步 |

搭建时间估计：

- 1-4 周：有原生连接器的 CRM 合作伙伴集成。
- 1-3 个月：需要映射 lead ID、重建 CRM 阶段的账户达到完全就绪。
- 上线后 45-60 天：深层阶段事件上的优化器收敛。

效果目标切换：

- 建设期：效果目标保持「Maximize number of leads」。
- 阶段事件每日流动、EMQ 稳定后：切到「Maximize number of conversion leads」。
- 切换后预期 1-2 周重新校准。该窗口内不要动其他变量。

审计中要指出的失败模式：

| 失败 | 症状 | 修复 |
|---|---|---|
| Stage CVR 低于 1% | 优化器找不到信号 | 选更浅的阶段（如 Contacted 已联系，代替 Closed-Won 已关单） |
| Stage CVR 高于 40% | 阶段太宽松；信号噪声大 | 选更深的阶段 |
| 阶段延迟 > 28 天 | 优化器丢弃事件 | 选更早的阶段（Qualified 已筛选 / SQL） |
| CRM 记录缺 Lead ID | 无法匹配 | 重做 CRM 入口埋点；能回填就回填 |
| 周末无每日上传 | 优化器视为断档 | 用持续同步的合作伙伴或定时任务 |
| EMQ < 5 | 匹配率太低 | 邮箱 + 电话 SHA-256 哈希，加所有可用标识，补 `external_id` |

---

## Click-to-message（点击发消息）运营

Click-to-message 线索广告的成败在**响应速度和筛选话术**。把它当运营型系列，不是纯媒体采买。

### 转化位置

| 位置 | 优势 | 约束 | 所需运营 |
|---|---|---|---|
| Messenger | Facebook 原生、低摩擦、B2C 好做 | 某些市场不常用 | 收件箱值守 + 快捷回复 + 非工作时间 bot |
| Instagram Direct | 创作者/生活方式/DTC 强 | IG 收件箱大规模管理有时更难 | IG 原生自动化或 BSP（业务方案提供商） |
| WhatsApp Business | LATAM、EMEA、印度、MENA 强；支持结构化模板和端到端对话销售 | 大规模需要 WhatsApp Business 账户或 WhatsApp Business Platform（WABA）；用户发消息后 24 小时客服窗口；外发需模板审批 | BSP 集成（如 360dialog、Twilio、WATI）、坐席或 AI chatbot、opt-in（选择加入）管理 |
| Instant Form + Messenger（混合） | 表单预填 + 自动聊天交接 | 设置略复杂 | 同 Messenger |
| Instant Form + WhatsApp | 同样的混合模式 | 规模化外发跟进需要 WABA | 同 WhatsApp |

### 欢迎语与快捷回复

| 元素 | 最佳实践 |
|---|---|
| Pre-filled message（预填消息） | 具体到产品/意向（如「Hi, I'd like a quote for X service」），而不是泛泛的「Hi」，提升筛选 |
| Welcome message（欢迎语）长度 | 短且注意截断；用户一眼看到意向和下一步 |
| Quick replies（快捷回复） | 2-4 个按钮映射意向：「Get a quote」（获取报价）、「See pricing」（看价格）、「Talk to a rep」（找销售聊） |
| Qualifying questions（筛选问题） | 聊天流中最多约 6 个；从低摩擦（服务兴趣）到高摩擦（预算、联系方式）推进 |
| Auto-acknowledgement（自动确认） | 秒级回复；最好 5 秒内，bot 或模板回复 |
| Human SLA（人工 SLA） | 热线索目标 5 分钟内首次人工回复；越快越好，实质差异 |
| Disqualification（剔除） | 用户不匹配就给自助资源，不要已读不回 |
| Tagging（打标签） | 给对话结果打标签（qualified 已筛选 / unqualified 未筛选 / sale 成交），回流进优化 |

### 效果指标（按重要性排序）

| 指标 | 说明 |
|---|---|
| Cost per messaging conversation started（每次消息对话成本） | 前端竞价效率；不是质量 |
| 业务侧回复率 | 运营健康度 |
| 首次响应时间（中位、p95） | 运营 SLA |
| 合格对话率 | 流入质量 + 筛选话术 |
| 预约/报价率 | pipeline 价值 |
| Cost per qualified conversation（每次合格对话成本） | 放量的主要读数 |
| Cost per closed deal / revenue（每关单成本/收入） | 业务真相 |

### 效果目标

| 目标 | 何时用 |
|---|---|
| Maximize number of leads（用消息转化位置） | click-to-message 线索系列的默认项 |
| Maximize conversations（最大化对话数） | 仅当对话数本身就是业务结果时（互动/预热池） |
| Conversion Leads（聊天结果的 CRM 事件接好后） | 有聊天 CRM 阶段反馈的成熟运营 |

### Click-to-WhatsApp 细节

| 细节 | 备注 |
|---|---|
| 渠道来源 | 规模化用 WABA；WhatsApp Business app 只适合很小的账户 |
| 24 小时服务窗口 | 用户发消息后 24 小时内可自由回复；之外只能用审批过的模板 |
| Templates（模板） | Meta 预审批；类别包括 marketing（营销）、utility（实用）、authentication（验证） |
| 转化衡量 | 从 BSP 或 Conversions API for WhatsApp 发转化事件；发了购买事件后，Sales 目标下有「Purchases through messaging」（消息内购买）优化（跨目标） |
| 2026 定价变化 | WABA 定价模型 2026-01-01 起更新为按类别计费；在 BSP 仪表盘核验当前成本模型 |

---

## Calls 路径（带电话的线索广告）

### 何时用

| 适配 | 示例 |
|---|---|
| 紧急本地服务 | 水管工、开锁、拖车 |
| 预约类品类 | 家政服务、汽车、医疗（受品类限制） |
| 复杂产品 | 保险报价、房贷 discovery（受 Special Ad Category 规则约束） |
| 纯移动受众 | 本地 SMB 服务 |

### 设置细节

| 设置 | 行为 |
|---|---|
| 转化位置 | Calls（Leads 目标下） |
| 电话号码 | 国家代码 + 号码；每条广告一个 |
| 优化 | 优化约 60 秒的确认通话；据称相对链接点击优化成本降低约 59% |
| 上报指标 | Estimated number of call confirmation clicks（预估的通话确认点击数，即点确认拨号的人数） |
| 营业时间 | 用 Lifetime budget（总预算）+ Ad scheduling（广告排期）限制在营业时间投放 |
| 国家 | 国家可用性随 Meta 产品 rollout 而异；在 Ads Manager 核验 |
| Call tracking | 配 call-tracking 提供商（CallRail、800.com 等），做 CRM 归因和质量打分 |

### 运营规则

- 电话系列永远按营业时间排期；非营业时间的电话进语音信箱，浪费预算。
- 素材文案先做筛选（价格区间、 eligibility 资格、服务区域）——朝 60 秒通话优化并不能防垃圾来电。
- 电话侧用条件逻辑的等价物：筛选并路由的 IVR（如「需要在 [地区] 服务请按 1」）就是电话侧的条件表单问题。
- 把通话 disposition（处置结果）接回 CRM，启用 Conversion Leads 或线下事件优化。

### 何时不用

- 服务区域大但支持是区域性的：漏接造成负面体验。
- 非营业时间线索也有价值：表单或消息能接住，不会丢。
- 合规要求未经事先书面同意不得语音联系。

---

## 按阶段的出价与预算

### 效果目标

| 效果目标 | 优化信号 | 何时用 |
|---|---|---|
| Maximize number of leads | 表单填写/消息/电话/网站线索事件 | 新账户默认；Conversion Leads 还没接好 |
| Maximize number of conversion leads | CRM 阶段事件经 Conversions API | 满足 Conversion Leads 就绪；深漏斗优化 |
| Cost per result goal | 稳定的目标 CPL | 表现稳定 7+ 天后；学习期不用 |
| Bid cap | 竞价出价的硬上限 | 每线索毛利清楚的激进放量；可接受高方差 |
| Maximize quality（最大化质量） | 隐含在 Higher Intent 表单；不是独立的出价策略 | 用表单类型 + 筛选问题，不要指望有个叫「最高质量线索」的出价开关（当前界面没有这个独立目标） |

### 按线索项目阶段的预算姿态

| 项目阶段 | 预算姿态 | 原因 |
|---|---|---|
| 全新账户/未验证 pixel | 每个广告组 $50-150 / 天；手动；短测试 | 别在坏衡量上浪费 |
| 跟踪已验证、学习中 | 每个广告组至少 目标 CPL × 50 / 周 | 约等于 Meta 学习下限（每个广告组每周约 50 个事件）；低于此预期「Learning Limited」 |
| 稳定，Conversion Leads 前 | 保持在学习下限或以上；考虑系列预算优化 | 给广告组轮换留空间 |
| Conversion Leads 上线 | 7-14 天所有变量不动；然后每 5-7 天加 20-30% | 阶段事件优化器收敛期别重置学习 |
| 成熟 | Cost per result goal 或经 Conversion Leads + CRM 反馈的 ROAS；靠素材多样化放量 | 业务经济模型决定 headroom（上探空间） |

### Leads 的学习期规则

| 行为 | 做法 |
|---|---|
| 学习下限 | 每个广告组每周约 50 个优化事件 |
| 重置触发 | 大幅编辑预算（>20%）、出价策略、目标受众、优化事件、素材；暂停 > 7 天 |
| 编辑打包 | 每周初一次性做完所有计划中的改动；标注日期 |
| 量级排查 | 合并广告组、放宽受众、允许 Advantage+ Audience 扩展、简化表单 |
| 事件深度回退 | Conversion Leads 拿不到每周 50 个阶段事件就退回 Maximize Leads，同时建量 |

### 出价策略决策

```
跟踪不稳或量 <30/周  → Highest volume / Maximize leads，不设上限
稳定 30-60/周，目标 CPL 已知 → Cost per result goal，设为 7 天实际 CPL 上浮 10-20%
稳定 60+/周，CVR 可预测 → Cost per result goal 按实际 CPL；符合条件用 Conversion Leads
激进 ROAS 导向放量，成熟 → Cost per result goal 或 bid cap，按合格线索成本目标
Special Ad Category，受限 → 默认 Maximize leads；多周稳定后再设上限
```

---

## 受众设计

### 默认姿态

| 姿态 | 何时 | 备注 |
|---|---|---|
| Advantage+ Audience（无固定输入） | 大多数能放宽受众的账户 | 学习最快；Advantage+ Leads「On」状态必需 |
| Advantage+ Audience + 地区/年龄/语言约束 | 服务区域或受监管品类 | 硬性控制在 Advantage+ 下存活；建议是软性的 |
| Advantage+ Audience + 自定义受众排除 | 压制已有客户/历史线索 | 自定义受众排除是唯一保持 Advantage+「On」的排除类型 |
| 手动 saved audience（保存的受众） | 严格定位必需（合规、地区、客户类型） | 会关掉 Advantage+「On」标准；记录原因 |
| 从转化者/客户名单/Conversion Leads 阶段做的相似受众 | 允许的场景 | 线索最强的手动受众；Special Ad Categories 下不允许 |
| 从 Lead Ads 表单打开者、视频观看者、IG/FB 互动者做的自定义受众 | 再营销 | 谨慎叠加；不要无质量过滤地再营销所有互动者 |

### Advantage+ Leads 下的受众控制

按 Meta Advantage+ Leads 页面，系列「Advantage+ On」需要：

- 系列预算保持开启（Advantage+ Campaign Budget）
- 至少一个广告组用 Advantage+ Audience
- 只做自定义受众排除（想保持「On」就不要排除保存的人口属性/兴趣受众）
- 最低年龄 18+
- Advantage+ Placements 开启
- 账户层级控制（账户级黑名单/版位屏蔽）可应用而不关掉「On」

违反任何一条，Advantage+ 就关掉，受众/预算/版位自动化退回手动行为。在 Campaign Opportunities（系列机会）和 Advantage+ 状态里审计。

### Special Ad Categories 对受众的影响

系列被标为金融/就业/住房/政治时：

- 这些广告组无详细定位（兴趣/行为）。
- 年龄范围上限 18-65+。
- 无性别定位。
- 无 ZIP/邮编定位。
- 无相似受众。
- 无保存受众的人口属性类排除。
- 美国 2025：Special Ad Categories 的客户名单自定义受众有更严的数据使用规则，功能上受限；核验当前状态。
- 线索表单中关于年龄、性别、感情状态、地理位置的问题受限。

---

## 素材策略

Leads 的素材是**筛选型素材**，不只是注意力素材。钩子好但筛选模糊，会产出大量不匹配的线索。

### 按素材筛选的原则

| 原则 | 落实 |
|---|---|
| 讲清 offer 给谁 | 视觉、字幕、屏上文字点名买家（如「[地区] 的房主」「5-50 人 B2B 创始人」） |
| 相关时给出价格区间或资格提示 | 「Plans from $X/mo」「持证专业人士专享」「[城市] 可服务」 |
| 用证明 | 评价、认证、结果数字、具名客户（允许的话） |
| 避免纯好奇钩子 | 好奇心拉高 CTR（点击率），干掉下游筛选 |
| 表单首屏和广告文案对齐 | 同样的 offer 名、同样的品牌、同样的价值主张 |
| 移动端优先构图 | Reels / Stories 用 9:16 视频；feed 用 4:5；安全区内文字清晰 |
| 永远加字幕 | 尤其 Reels / Stories 默认静音 |

### Leads 的版式角色

| 版式 | 强项 | 弱项 |
|---|---|---|
| 静态图 | 直接 offer、快速测试、再营销表单打开者 | 复杂教育 |
| 视频 / Reels | 演示、社会证明、创始人 POV、产品 walkthrough | 极简单的 offer（杀鸡用牛刀） |
| 轮播 | 多产品、对比、分步流程 | 单一高速 offer |
| Collection / Instant Experience（精品栏/即时体验） | 线索中少见；offer 背后是产品集转服务咨询时有用 | 纯线索收集 |

### Leads 的概念测试脚手架

| 槽位 | 概念 |
|---|---|
| 1 | 直接 offer（清晰的价格/资格/结果） |
| 2 | 痛点框架（问题 → 产品 → 结果） |
| 3 | 社会证明（测评/案例/具名结果） |
| 4 | 创始人/专家 POV（信任 + 观点） |
| 5 | 流程讲解（30 秒讲清怎么运作） |
| 6 | 结果导向（具体指标/前后对比） |

一次跑 4-6 个概念，每个概念 2-3 个钩子。赢的概念每 4-8 周刷新；输的概念更快刷新。

### Rich Creative 表单 ↔ 广告素材配对

Rich Creative 表单的作用是把广告的品牌体验延伸进表单。以下情况配对：

- 品牌教育是筛选的一部分（如复杂服务）。
- 表单里的多产品轮播和广告轮播对应。
- 表单里的社会证明呼应广告的测评文案。

广告是薄的直接响应资产时不要用 Rich Creative 表单；表单的深度会显得脱节。

---

## 衡量设计

### CRM 反馈闭环（真正的衡量）

```
Meta 线索事件（表单 / 消息 / 电话）
   → CRM 入口（捕获 lead_id、ad_id、adset_id、campaign_id、form_id、source、timestamp）
   → 线索筛选阶段（Contacted 已联系、Qualified 已筛选、SQL、Opportunity 商机、Closed-Won 已关单）
   → CRM → Conversions API（CRM 集成）上报阶段事件
   → Meta 每日收到阶段事件，按 lead_id + 哈希 PII 匹配
   → 效果目标「Maximize conversion leads」在阶段事件上优化
```

### 闭环所需的事件字段

| 字段 | 来源 | 用途 |
|---|---|---|
| `lead_id`（15-16 位 Meta lead ID） | Lead Ads webhook / Graph API 返回 | Conversion Leads 匹配的主键 |
| `event_name` | CRM 阶段映射到 Meta 事件分类或合作伙伴映射的自定义事件 | 告诉 Meta 优化哪个阶段 |
| `event_time`（Unix epoch） | CRM 阶段流转时间 | 延迟检查；优化要求距线索创建 <28 天 |
| `lead_event_source` | 标识 CRM（如 `salesforce`、`hubspot`、自定义） | Conversion Leads CAPI 事件必需 |
| `action_source` | `system_generated` 或其他合适的值 | CAPI 参数 |
| `user_data` | 哈希邮箱、哈希电话、`external_id`、可选 fbp / fbc | 匹配键；提升 EMQ |
| `value` 和 `currency` | 适用时 | 下游 ROAS/价值优化必需 |

### Marketing API / webhook 模式

| 组件 | 行为 |
|---|---|
| Webhook 订阅 | app 订阅 Page 的 `leadgen` 字段（`POST /PAGE_ID/subscribed_apps?subscribed_fields=leadgen`）；Page 必须安装了该 app |
| Webhook payload | 包含 `leadgen_id`、`page_id`、`form_id`、`ad_id`、`created_time`；不直接含线索 PII |
| 线索拉取 | `GET /{leadgen_id}?access_token={page_access_token}` 返回完整线索字段/值对 |
| Permissions（权限） | `pages_show_list`、`ads_management`、`ads_read`、`leads_retrieval`、`pages_read_engagement`、`pages_manage_metadata`、`pages_manage_ads`（需 App Review 审核） |
| Token 类型 | Long-lived Page Access Token（长效主页访问令牌）（优于 user access token，限流原因） |
| Retention（留存） | Ads Manager / Lead Center 的 Lead CSV 存 90 天；法律/CRM 存档自己立即存 |
| Rate limits（限流） | 和 access tier 及该 Page 过去 90 天的线索量挂钩；监控 4xx / 429 响应并退避 |
| Bulk 模式 | 回填时分页拉 `GET /{form_id}/leads`；稳态优先 webhook + 拉取 |

### EMQ 优化清单

| 杠杆 | 效果 |
|---|---|
| 发哈希邮箱（SHA-256，小写后哈希） | 单个最大的 EMQ 提升 |
| 发哈希电话（E.164 格式后 SHA-256） | 提升大，尤其服务/电话类 |
| 发 `external_id`（CRM 记录 ID） | 跨事件关联 |
| 网站适用时传 `fbp` 和 `fbc` | 改善网站表单的浏览器-服务端匹配 |
| 发姓/名（哈希） | 边际提升 |
| 发城市/州/国家/ZIP（哈希） | 边际提升；大账户有帮助 |

7+ 视为好，9+ 视为优秀。不要追 10；9 之后投入回报递减。

### 要监控的报表拆分

| 拆分 | 看什么 |
|---|---|
| 版位 | Audience Network / Reels / Feed / Stories 占比；合格线索率实质更差的版位暂停，要有 7+ 天证据 |
| 表单 / 广告 / 广告组 | 按表单类型和素材的质量差异 |
| 年龄 / 性别（品类允许时） | 受众匹配信号 |
| 地区 / 国家 | 地域错配；服务区域外溢 |
| 设备 | 网站表单的移动 vs 桌面转化差异 |
| 时段 / 星期 | 电话和消息运营的排班规划 |

### 归因窗口变化背景

- Ads Insights API 的归因窗口随时间变化。归因窗口支持变化时给仪表盘加注，尤其浏览转化和互动转化报表。
- 2026-03 重分类：click-through 收窄为链接点击；engage-through 成为独立类别。在有拆分的地方，Leads 分开报点击转化、互动转化、浏览转化。
- Lead Ads 的浏览转化和互动转化常虚增表面量；CRM/后端才是真相。

---

## 诊断树

账户表现不佳时用本树。自上而下走。第一个答案明显「否」或「弱」的节点就停下行动。

```
1. offer 是否在 Special Ad Category？
   ├─ YES：政策 / 表单问题 / 受众约束是否遵守？
   │       ├─ NO  → 先修政策合规；系列可能在自动应用的限制下
   │       │        悄悄跑着
   │       └─ YES → 继续
   └─ NO：继续

2. 转化路径适合买家吗？
   ├─ CPL 便宜但没人接 → 路径错了（表单太容易 / 自动填充滥用）
   ├─ 表单打开多、提交少 → 表单长度或介绍弱
   ├─ 消息多、无筛选 → 消息运营错或路径整个错了
   └─ 电话系列电话质量低 → 查广告预筛选 + 营业时间

3. 表单 / 消息设计为质量优化了吗？
   ├─ 高风险品类用 More Volume → 切 Higher Intent
   ├─ 无筛选问题 → 加 1-3 个高杠杆问题
   ├─ 有效性重要的字段开了自动填充 → 关掉自动填充
   ├─ 电话关键的系列无 SMS 验证 → 开 SMS 验证
   └─ 无条件逻辑剔除 → 加 Conditional → Close Form 路径

4. 响应运营达到 SLA 了吗？
   ├─ 热线索跟进速度 > 5 分钟 → 修路由 / 寻呼
   ├─ Click-to-WhatsApp 首次响应 > 5 秒 → 加 bot / 模板回复
   └─ 非工作时间 / 周末静默 → 排班或自动回复

5. CRM 反馈接好了吗？
   ├─ 无 CRM 集成 → 先手动做阶段；建集成
   ├─ Lead ID 未映射 → 重做入口埋点
   ├─ 阶段未每日流动 → 修同步
   └─ 阶段事件延迟 > 28 天 → 选更早的阶段优化

6. 账户够 Conversion Leads 就绪吗？
   ├─ 低于当前 Conversion Leads 量级阈值（Meta 开发者指引列出至少 200/月）
   │  或阶段 CVR 在 1-40% 之外 → 留在 Maximize leads；建就绪条件
   └─ 就绪 → 效果目标切 Maximize conversion leads；变量保持 2 周

7. Advantage+ Leads 如预期「On」吗？
   ├─ 受众过度限制（saved audience 作纳入）→ 把 Advantage+ 关掉了
   ├─ 手动排除版位且没走账户层级路径 → 关掉了
   ├─ 系列预算设到广告组层级 → 关掉了
   └─ 标准都满足 → 「On」；在 Campaign Opportunities 核验

8. 事件量够学习吗？
   ├─ <50 事件 / 广告组 / 周 → 合并广告组、放宽受众、降深层事件
   └─ 量够但波动 → 7-14 天不编辑

9. 素材是瓶颈吗？
   ├─ 频次上升、CTR / hold rate（留存率）下降 → 素材疲劳 → 上新概念
   ├─ 素材都长得像 → 组合多样性低 → 4-6 概念 × 2-3 钩子
   └─ 概念多样但不匹配 → 筛选型素材错了 → 加资格提示

10. 衡量可靠吗？
    ├─ 网站表单只有 Pixel 无 CAPI → 加 CAPI 去重
    ├─ EMQ < 6 → 改善哈希标识
    ├─ 归因窗口和后端对不上 → 对齐报表窗口
    └─ Engage-through 虚增上报线索 → 仪表盘拆分 CT / ET / VT
```

---

## 常见坑

1. **把 CPL 当成功指标。** 合格线索率 5% 的 $3 CPL 系列，在典型销售人力经济模型下，不如合格率 50% 的 $30 CPL。
2. **没做 CRM 工作就承诺 Conversion Leads。** Conversion Leads 是 1-3 个月的实现工程，不是设置开关。
3. **受监管行业跑 More Volume 表单。** 质量崩盘可预见；切 Higher Intent + 筛选问题 + SMS 验证。
4. **学习期每天编辑系列。** 重置优化器；只做每周打包编辑。
5. **忽略版位拆分。** Audience Network 和某些 Reels 版位可能在线索系列占大多数展示并产出低质量线索；排除要有 7+ 天证据。
6. **电话关键的系列让自动填充驱动提交。** 用手输 Short Answer + SMS 验证。
7. **价值没讲清就堆太多筛选问题。** 客户在答第一题前就走了。
8. **过度限制受众或版位悄悄关掉 Advantage+。** 定期核验 Advantage+ 状态；不要假设徽章一直在。
9. **Special Ad Category 素材未经政策审核。** 这些品类的线索表单问题类型会悄悄消失。
10. **click-to-message 和电话路径无 CRM 侧反馈。** 没有 CRM disposition（处置），这些路径跑不了 Conversion Leads。
11. **忘记 webhook 安全。** 验证 inbound Lead webhook 的 `X-Hub-Signature-256` HMAC；不要信任原始 `leadgen_id` payload。
12. **线索留存断档。** Meta 在 Ads Manager 存线索 90 天；同步断了，90 天以上的线索可能在 Ads Manager（CSV）拉不回来（webhook 历史能否重拉取决于表单留存）。
13. **业务目标是线索，WhatsApp 线索却优化「Maximize conversations」。** 效果目标和真实结果对齐；对话量不等于合格线索量。
14. **把「最高质量线索」当独立出价策略。** 当前 Ads Manager 没有这个效果目标。质量靠表单类型 + 筛选 + Conversion Leads 工程化，不是靠出价。

---

## 实施前易变检查

把指引变成硬规则前，务必在当前 Ads Manager / API 核验这些：

| 领域 | 检查 |
|---|---|
| Advantage+ Leads 可用性 | 账户资格 flag、默认开启状态、Campaign Opportunities 界面 |
| Advantage+ Leads「On」标准 | 受众扩展默认、仅自定义受众排除规则、18+ 要求、全版位要求、系列预算要求 |
| 当前界面的表单类型 | More Volume / Higher Intent / Rich Creative 的命名和功能 gating |
| 问题类型 | Image Select、Slider、Conditional、Appointment Request、Business Locator（Store Locator）的可用性 |
| SMS 验证可用性 | 国家支持、退出条件 |
| 工作邮箱验证可用性 | 账户 / 地区 |
| 账户的 Conversion Leads 就绪 | Lead ID 映射、每日上传、阶段时限、阶段 CVR、EMQ |
| 效果目标选项 | 当前界面中「Maximize number of leads」vs「Maximize number of conversion leads」的措辞 |
| 出价策略选项 | Cost per result goal / bid cap；当前下限 |
| Lead Ads 报表的归因窗口 | 1 天/7 天点击、1 天浏览（2026-01-12 变化后）、互动转化列可用性 |
| Special Ad Categories | 当前品类列表；线索表单问题限制；美国客户名单规则 |
| Click-to-WhatsApp 可用性 | 国家、BSP 选项、站内购买支持 |
| Calls 路径 | 国家可用性、营业时间排期、总预算要求 |
| Webhook 权限 | `leads_retrieval` 的 App Review 状态 |

易变线索条目的当前官方核查点：

- Meta Lead Ads: https://www.facebook.com/business/ads/ad-objectives/lead-generation
- Meta Lead Ads with forms: https://www.facebook.com/business/ads/ad-objectives/lead-generation/lead-ads-with-forms
- Meta Lead Ads with messaging: https://www.facebook.com/business/ads/ad-objectives/lead-generation/lead-ads-with-messaging
- Meta Advantage+ Leads: https://www.facebook.com/business/ads/meta-advantage-plus/leads
- Meta Conversion Leads CRM integration: https://developers.facebook.com/docs/marketing-api/conversions-api/conversion-leads-integration
| Retention（留存） | Lead CSV 留存（当前约 90 天）和 webhook 重拉行为 |

---

## 交叉引用

- CAPI、去重、CRM/线下反馈细节用 `measurement-and-attribution.md`。
- 筛选型素材原则用 `creative-strategy.md`。
- 诊断既有账户用 `symptom-diagnostics.md` 和 `account-data-diagnostics.md`。
- Special Ad Categories 和受监管线索规划用 `policy-and-special-categories.md`。
- Advantage+ Leads、归因、易变界面/API 行为用 `advantage-plus.md`。
