# Meta 销售广告系列

## 0. 实操方法

### 0-1. 理念

销售目标广告系列优化的是直接购买或收入行为。销售广告系列出问题，几乎都是衡量、商品目录、优惠或目标地的问题。Ads Manager 界面（广告系列类型、出价、受众、版位）远不如喂给它的输入重要。

销售表现差时的影响力排序：

1. 购买 + 价值追踪质量（Pixel + CAPI 去重、价值/货币、content_ids）
2. 优惠/商品页/结账转化率
3. 目录和 feed 质量
4. 创意多样性和版位适配
5. 受众设计（Advantage+ vs 手动、排除、类似受众）
6. 出价姿态
7. 账户结构（合并 vs 拆分）
8. 预算分配

颠倒这个顺序是操作者最常见的错误。

### 0-2. 最重要的事

- **收入 ROAS 不是利润。** 永远要与后端收入、退款/退货、边际贡献和新客户率对账。
- **Advantage+ 销售放大输入，不修复输入。** 糟糕的衡量加上 Advantage+ 会产生自信但错误的投放和虚高的平台 ROAS。
- **目录质量就是定向质量和创意质量。** 标题、图片、价格、库存、商品 ID 和商品集直接影响资格和投放。
- **没有去重的 CAPI 比只有 Pixel 更糟。** 重复计数让优化学到虚假的量。
- **收割老客户是默认的失败模式。** 如果不衡量或限制，Advantage+ 会自然滑向那里。
- **Advantage+ 里创意不是可选项。** 系统需要 20-50 个多样化素材来真正测试，而不是 3 个换皮。

### 0-3. 按月购买量的决策矩阵

以 Meta 的每周每广告组学习期目标（50 个事件/周退出学习）为锚点。
| 月购买量 | 推荐的销售设置 | 备注 |
|---|---|---|
| 0-30 单/月 | 单个手动销售广告系列，1-2 个广告组，宽受众；`Purchase` 可靠就优化 `Purchase`，否则用 `InitiateCheckout` 或 `AddToCart`（能预测购买） | 量太低，任何广告组都学不动。不要把 Advantage+ 销售当唯一引擎。聚焦创意和优惠。 |
| 30-50 单/月 | 手动销售 + 一个小的 Advantage+ 销售测试（如果目录符合资格）。合并广告组。最大投放量出价。 | 学习期边缘。`Purchase` 事件积累期间，考虑用更高漏斗事件（加购 ATC、发起结账 IC）优化。 |
| 50-100 单/月 | Advantage+ 销售做主要获客，手动销售做再营销/排除。优化 `Purchase`。最大投放量。 | Advantage+ 里单个广告组大概率能退出学习。开始推创意量（15-25 条广告）。 |
| 100-300 单/月 | Advantage+ 销售主攻 + 手动销售做商品集、利润或地理拆分。开始测试单次成效费用目标。价值分布宽时价值优化可行。 | 甜蜜点。Advantage+ 销售里可以跑 2-3 个广告组（每个上限 50 条广告）。 |
| 300+ 单/月 | Advantage+ 销售 + Advantage+ 目录广告（宽定向和再营销）+ 手动拆分做利润/区域/新老客户。ROAS 目标可行。转化提升研究可行。 | 现在可以安全地做增量测试、价值规则和基于预测客户终身价值（pLTV, predictive Lifetime Value）的优化。 |

### 0-4. 账户表现不佳时的诊断顺序

1. Pixel + CAPI 去重健康度（事件管理工具 Events Manager、测试事件、事件匹配质量 EMQ）
2. 目录 feed 健康度（被拒商品、库存、图片质量、价格漂移）
3. 后端收入 vs Meta 归因收入的差距
4. 新客户率 vs 回头客率
5. 点击 vs 互动 vs 浏览归因的拆分
6. 创意概念数量、疲劳、版位适配
7. 受众结构（重叠、排除、再营销占比）
8. 出价姿态 vs 事件量

### 0-5. 常见陷阱（置顶清单）

- 大花费的销售上线只有 Pixel 没有 CAPI
- 不看利润、退款、新客户率就评估 ROAS
- 只看平台 ROAS 就放量 Advantage+ 销售
- 永远优化 `AddToCart` 或 `InitiateCheckout`，不推进到 `Purchase`
- 目录 feed 坏了还跑目录广告（图片质量低、缺价格、库存过期）
- 7 天学习期内频繁改预算/创意/目标
- 没有在现行界面中核实现有客户预算上限（Existing Customer Budget Cap）的状态（它被改过多次）
- 把 Advantage+ 当作商品市场匹配（PMF）差的解药

---

## 1. 销售目标界面

### 1-1. 销售目标覆盖什么

ODAX（以结果为导向的广告体验，Outcome-Driven Ad Experiences）菜单中的销售目标取代了旧的"转化"和"目录销售"目标。它是任何优化直接购买、订阅、试用或其他下游收入行为的广告系列的唯一目标。

### 1-2. 转化位置

销售广告系列有多个转化位置。这里的选择决定所需的信号栈和可用的效果目标；商店（Shops）和混合商店/网站选项处于过渡期，围绕它们做规划前请核实现行账户界面。

| 转化位置 | 所需信号栈 | 最适用 | 备注 |
|---|---|---|---|
| 网站 | Meta Pixel + CAPI，带价值/货币的购买（Purchase）事件 | 标准电商、SaaS 购买、线索到销售流程 | 默认。95% 的销售广告系列属于这里。 |
| 应用 | 应用 SDK（Facebook SDK / FB SDK）或 MMP（移动监测合作伙伴，Mobile Measurement Partner）集成的应用事件，iOS 用 AEM | 应用优先的商务、订阅应用 | iOS 衡量跑在 AEM + AdAttributionKit + MMP 上。带着这个约束做规划。 |
| 网站和商店 / 商店辅助目标地（可用性在变化） | Pixel + CAPI + 已连接的 Facebook/Instagram 商店 | Meta 仍提供商店辅助销售路由的账户 | Meta 一直在把商店结账往网站结账迁；核实现行账户界面是否还有 `Website and shop` 或购买旅程个性化。 |
| 电话 | 电话号码 + 点击拨打信号；可选 CRM 线下转化 | 本地服务、高客单咨询式 B2C | 优化质量取决于电话结果是否作为线下转化上传。 |
| 消息应用（Messenger、WhatsApp、Instagram Direct） | 从聊天平台发给 Meta 的购买事件；消息集成 | 对话式商务（亚太/拉美的服装、美妆、服务） | 需要"最大化通过消息的购买"效果目标，且过去 30 天有 5+ 个购买事件才符合资格。 |

(

### 1-3. 销售下可用的效果目标

效果目标 = Meta 投放优化的事件。可用选项因转化位置而异。

| 效果目标 | 转化位置 | 适用场景 | 风险 |
|---|---|---|---|
| 最大化转化量 | 网站 / 应用 / 商店辅助目标地（如可用） | 购买优化的默认选择 | 把每个转化当等值，不管订单金额 |
| 最大化转化价值 | 网站 / 应用 | 平均订单价值（AOV, Average Order Value）分布宽；pLTV 生效 | 每个事件都要有价值/货币；量阈值约 50/周 |
| 最大化落地页浏览 | 网站 | 购买量太低时的过渡步骤 | 不要当终点目标；尽快转到购买 |
| 最大化链接点击 | 网站 / 消息 | 仅诊断；不是真正的销售目标 | 不会优化下游转化 |
| 最大化对话 | 消息 | 点击 Messenger 的量 | 对话 ≠ 购买 |
| 最大化通过消息的购买 | 消息 | 有购买事件反馈的对话式商务 | 需要购买事件信号回传 Meta |
| 最大化展示 | 任何 | 仅触达式 | 实践中不要用于销售目标 |

操作规则：选与实际收入事件匹配的目标。只有当账户级购买量低于约 30 单/月时才用过渡事件（LPV、加购、发起结账），且是明确的过渡姿态。

### 1-4. 销售路径要求

上线所需的信号：

| 信号 | 要求方 | 备注 |
|---|---|---|
| Meta Pixel | 所有网站转化位置 | 浏览器侧基线。所有关键页面都要触发。 |
| 转化 API（CAPI, Conversions API） | 生产级销售优化 | 2026 年鉴于 iOS 应用追踪透明度（ATT, App Tracking Transparency）和浏览器信号丢失，准确优化的强制要求。 |
| 事件去重 | Pixel + CAPI 双发 | `event_name` 和 `event_id` 匹配；48 小时去重窗口。 |
| 目录 | Advantage+ 目录广告、精品栏（Collection）广告、商店广告 | 任何 feed 驱动的销售版面都需要。 |
| 应用 SDK 或 MMP | 应用转化位置 | iOS 需要 AEM + ATT 感知的设置。 |

---

## 2. 账户与广告系列结构设计

### 2-1. 账户级前置条件

上线销售前：

- 所有涉及的域名都装了 Pixel，包括子域名。
- CAPI 已集成（服务端或经网关）。浏览器转化 API（CAPIG, Conversions API for Browsers）是直接服务端集成不可行时的最低成本兜底。
- 事件管理工具的测试事件显示去重对至少购买、加购、发起结账、内容查看（ViewContent）生效。
- 域名已在商务管理平台（Business Manager）验证。
- 网站汇总事件衡量（AEM, Aggregated Event Measurement）在现行事件管理工具流程中通常是自动汇总的。在假设任何手动 8 事件优先级流程前请核实现行界面。
- 目录已创建且 feed 已排期（如果会用任何 feed 驱动的格式）。
- 已评估特殊广告类别（Special Ad Categories）（金融、就业、住房、社会/政治 → 定向受限）。

### 2-2. 广告系列数量

默认规则：尽量少建广告系列。每个广告系列都是独立的学习单元。

| 拆成新广告系列的理由 | 是否 |
|---|---|
| 转化位置不同（网站 vs 应用 vs 消息） | 是 |
| 经济模型不同的国家或主要市场 | 是 |
| 同一商务管理平台下的不同品牌 | 是 |
| 严格的预算隔离要求（合规、财务） | 是 |
| 新品发布需要隔离以干净衡量 | 是（临时） |
| 出价策略不同（最大投放量 vs ROAS 目标） | 是 |
| 受众类型不同（宽定向 vs 再营销） | 可能——越来越多由 Advantage+ 处理 |
| 创意主题/概念不同 | 否 |
| 人口属性不同 | 否（让 Advantage+ 受众处理） |
| 版位不同 | 否（除非有证据，否则依赖 Advantage+ 版位） |

反模式：一个商品一个广告系列、一个受众一个广告系列、一个创意一个广告系列。这会打碎学习、放大重叠，没有广告组能达到 50/周转化。

### 2-3. 广告组数量

Advantage+ 销售内，2026 年实践上限是每广告组 50 条广告；广告系列级和迁移限制也可能适用（比如旧版 ASC 参考用过单广告组最多 150 个创意组合）。批量导入或复制大广告系列前请核实现行界面/API 限制。
手动销售内：

| 账户阶段 | 每个广告系列推荐的广告组数 |
|---|---|
| 0-30 单/月 | 1 个广告组，宽定向 |
| 30-100 单/月 | 最多 2-3 个广告组（如宽定向 / 再营销 / 类似受众） |
| 100-300 单/月 | 3-5 个广告组（加商品集、地理、客户状态拆分） |
| 300+ 单/月 | 5-8 个广告组，每个都有清晰的差异化经济理由 |

如果一个广告组按当前花费和 CPA 大概率达不到每周 50 转化，就该合并。

### 2-4. 推荐结构模板

**模板 A——50 单/月以下（小店）**

```
Campaign: Sales | Manual | Highest volume | Purchase
  Ad Set: Broad | Advantage+ Audience | Advantage+ Placements
    Ads: 6-10 (2-3 concepts x 2-3 formats)
```

**模板 B——50-300 单/月（放量中的 D2C）**

```
Campaign A: Sales | Advantage+ Sales | Highest volume | Purchase
  Ad Set 1: default Advantage+ Sales ad set
    Ads: 15-30 (testing 4-6 concepts)

Campaign B: Sales | Manual | Highest volume | Purchase
  Ad Set: Retargeting (180-day site visitors, video viewers, IG engagers)
    Ads: 4-8 (DPA-style + creative)
```

现有客户预算上限 / 等效的新客户控制只在现行界面可用时使用。如果不可用，用客户名单排除或显式控花费的双广告组结构。
**模板 C——300+ 单/月（有利润信号的成熟 D2C）**

```
Campaign A: Advantage+ Sales | Highest volume or ROAS goal | Purchase value
  Existing customer cap: 15-25%
  Ads: 30-50 across hero / UGC / DPA-style / Reels-native
Campaign B: Manual Sales | Catalog | Advantage+ catalog ads (broad) | Highest value
  Product Set: high-margin SKUs, custom_label_0 = "high_margin"
Campaign C: Manual Sales | Catalog | Advantage+ catalog ads (retargeting)
  Audience: Viewed but not purchased (14d, 30d), ATC not purchased (7d), past purchasers (180d for cross-sell)
Campaign D: Manual Sales | Highest volume | Purchase
  Ad Set: New customer-only (excluded customer list, excluded purchased pixel audience)
```

### 2-5. 合并 vs 拆分的启发式

合并当：
- 每个广告组都低于 50 事件/周
- 受众定义重叠 >30%
- 同一创意在多个广告组都赢
- 说不清拆分的业务理由（不是媒介购买习惯）

拆分当：
- 存在可衡量的经济差异（如商品集之间销货成本 COGS 差 3 倍）
- 合规约束强制（特殊广告类别、地理限制）
- 创意格式真的不能共存于一个广告组（如 DPA 驱动的目录 vs 静态主视觉）
- 预算隔离是硬性要求

---

## 3. Advantage+ 销售（深挖）

### 3-1. 是什么

Advantage+ 销售是 Meta 的自动化销售广告系列类型。之前叫"Advantage+ 购物广告系列"（ASC, Advantage+ Shopping Campaigns）；改名为"Advantage+ 销售"反映了它现在支持的不只是商店式转化。
### 3-2. 自动化了什么

系统自动化四个杠杆：

| 杠杆 | 自动化行为 |
|---|---|
| 受众 | Advantage+ 受众在建议之外扩展；人口/年龄/地域可以是软信号；只有部分控制是硬性的。 |
| 版位 | Advantage+ 版位在 Facebook、Instagram、Reels、Stories、Audience Network、Messenger 间分配。 |
| 创意 | 最多可测试 150 个创意组合；系统调换文案、标题、行动号召、音乐、裁剪。 |
| 预算 | 广告系列级 Advantage+ 预算跨广告组；旧版 ASC/API 行为正在迁入统一的 Advantage+ 结构，广告组花费控制是否可用请核实现行界面/API。 |

(

### 3-3. 剩下的手动控制

即使在 Advantage+ 销售里，以下仍是操作者的决策：

- 转化位置（网站 / 应用 / 仍可用的商店辅助目标地）
- 效果目标（转化量 vs 价值）
- 出价策略（最大投放量 / 单次成效费用目标 / ROAS 目标 / 出价上限）
- 现有客户预算上限或替代的客户状态控制（可用时——见 3-4）
- 国家/地区（硬约束，不是建议）
- 最低年龄（硬性，尤其受限商品）
- 语言（软性）
- 自定义受众排除（硬性，最重要的手动杠杆）
- 广告创意输入（你提供素材；系统组合）
- 从已有成功帖文导入广告

### 3-4. 现有客户预算上限（易变）

现有客户预算上限的行为易变，与 Meta 把旧版 ASC 流程迁入统一 Advantage+ 广告系列设置的迁移有关。承诺可用性、具体上限范围、移除或扩展控制集之前，请核实现行账户界面/API。

| 量 / 模式 | 推荐上限 |
|---|---|
| 新账户 / 不稳定账户 | 20-30% 给现有客户 |
| 稳态 | 15-25% |
| 激进的新客户增长 | 10-15% |
| 品牌脉冲 / 会员期 | 30-40% |

技能硬性规则：**承诺具体上限行为前永远核实现行界面/API**。如果某账户或国家没有上限字段，兜底用客户名单排除、区分现有和非现有客户的双广告组结构，或带明确预算的独立再营销广告系列。

### 3-5. 资格与学习要求

| 要求 | 规格 |
|---|---|
| Pixel + CAPI | 强烈推荐；2026 年没有 CAPI 优化质量会实质下降 |
| 带价值/货币的购买事件 | 价值优化必需，所有销售都推荐 |
| 每周购买事件 | 50+ 退出学习；<50 意味着永远在学习，CPA 不稳定 |
| 日预算 | 至少目标 CPA × 50 / 7（如 30 美元 CPA → 日预算下限 214 美元） |
| 创意数量 | 最少 6，推荐 15-30，最优 20-50 个多样化素材 |
| 域名验证 | 必需 |
| AEM 事件优先级 | 网站 AEM 是自动汇总的；不核实现行事件管理工具就不要假设手动 8 事件优先级流程 |

### 3-6. 广告导入

Advantage+ 销售可以导入已有的高表现帖文/广告。用例：
- 用手动广告系列验证过的创意冷启动新的 Advantage+ 广告系列
- 从手动销售广告系列迁移而不丢失素材级学习（帖文互动会带过去，但广告系列级学习会重置）

### 3-7. 何时用 Advantage+ 销售 vs 手动

| 情况 | 推荐 |
|---|---|
| 每周 50+ 购买、Pixel+CAPI 干净、TAM 宽 | Advantage+ 销售主攻 |
| 20+ SKU 的目录且需求宽 | Advantage+ 销售 + Advantage+ 目录广告 |
| 严格的客户/地理/年龄/合规要求 | 手动销售 |
| ICP 狭窄的小众 B2B | 手动销售 |
| 无事件量的新品发布 | 手动销售（用过渡事件）直到量稳定 |
| 有老客户收割风险且上限不可用 | 带排除的手动销售 |
| SKU 间利润差异大 | 带利润标签商品集的手动销售，或 feed 侧利润标签 |
| 隔离测试新创意概念 | 手动（如果隔离重要）或受控创意槽位的 A+ |

---

## 4. 目录广告 / Advantage+ 目录广告（深挖）

### 4-1. 两种形态

目录广告（以前叫动态商品广告 DPA, Dynamic Product Ads 的格式）分为两种运营模式：

| 模式 | 受众类型 | 用途 |
|---|---|---|
| Advantage+ 目录广告做再营销 | 自定义受众：看过商品、加购、历史购买者 | 下层漏斗再触达 |
| Advantage+ 目录广告做宽定向 | feed 信号 + Advantage+ 受众驱动的开放受众 | 通过 feed 做潜客开发（以前叫 DABA） |

### 4-2. Feed 要求（必填字段）

目录广告用的 feed 的必填字段：

| 字段 | 规格 | 备注 |
|---|---|---|
| `id` | 字符串，≤100 字符，唯一、稳定 | content_ids 匹配的主键 |
| `title` | 字符串，≤200 字符（推荐 ≤100） | 前 25 字符杠杆最大 |
| `description` | 字符串，≤9999 字符（推荐 200-500） | 影响相关性 |
| `availability` | 枚举：in stock / out of stock / preorder / available for order / discontinued | 缺货商品不会投放 |
| `condition` | 枚举：new / refurbished / used | 必填 |
| `price` | 数字 + ISO 4217 货币 | 必填 |
| `link` | URL | 必须是 HTTPS，必须与域名验证匹配 |
| `image_link` | URL | 最小 500×500，推荐 1024×1024+，JPEG/PNG/GIF，<8MB，HTTPS |
| `brand` | 字符串 | 必填 |

### 4-3. 条件必填字段

| 字段 | 何时必填 |
|---|---|
| `gtin`、`mpn` | 很多品类的新商品必填；通常 (`brand`, `gtin`, `mpn`) 三选一必填 |
| `item_group_id` | 有变体（尺码、颜色）的商品必填 |
| `google_product_category` / `fb_product_category` | 用于品类级优化 |
| `sale_price` + `sale_price_effective_date` | 有折扣活动时 |
| `shipping` | 部分市场必填，用于准确比价 |

### 4-4. 自定义标签与分段

用自定义标签（`custom_label_0`-`custom_label_4` 或等价于 Meta 商品集筛选器的 `custom_data` 字段）编码利润感知决策：

| 标签槽位 | 建议用途 |
|---|---|
| `custom_label_0` | 利润层级（高/中/低） |
| `custom_label_1` | 库存健康度（积压 / 正常 / 紧缺） |
| `custom_label_2` | 新客户钩子（是/否） |
| `custom_label_3` | 生命周期阶段（主推 / 长尾 / 清仓） |
| `custom_label_4` | 季节 / 促销标记 |

这些标签进而驱动目录广告系列用的商品集定义。

### 4-5. 商品集设计

| 商品集 | 定义 | 用途 |
|---|---|---|
| 全部商品 | 整个目录 | 宽定向潜客开发 |
| 主推 SKU | `custom_label_3 = hero` | 获客导向 |
| 高利润 | `custom_label_0 = high` | 按利润加权放量 |
| 新客户钩子 | `custom_label_2 = yes` | 新客户广告组 |
| 交叉销售搭配 | 手动映射 | 购买后再营销 |
| 补货 | 消耗品 | 30/60/90 天节奏再营销 |

### 4-6. Feed 同步卫生

| 同步方式 | 频率 | 最适用 |
|---|---|---|
| 直连平台集成（Shopify、WooCommerce） | 实时 | 大多数店铺 |
| 排期 feed（URL 拉取） | 1-24 小时 | 中型店铺 |
| API 推送 | 实时 | 自研栈、高周转库存 |
| 手动上传 | 不适用 | 仅初始播种 |

常见 feed 问题：

| 问题 | 影响 |
|---|---|
| 图片 URL 返回 403 / 404 | 商品被拒，无投放 |
| 商品页 URL 不一致（`?utm` 变体） | content_ids 匹配失败，再营销和动态广告效果差 |
| 库存过期 | 缺货广告还在投，浪费花费 |
| feed 与站点价格漂移 | 目录政策违规，商品暂停 |
| 缺 `brand`/`gtin` | 商店和搜索版面资格下降 |
| 标题堆砌关键词 | CTR 下降，品牌调性受损 |

### 4-7. 目录匹配与 content_ids

要做动态个性化，Pixel+CAPI 的购买、加购和内容查看事件必须带 `content_ids`（字符串数组）和 `content_type`（"product" 或 "product_group"），且与目录中的 `id`（或 `item_group_id`）匹配。对不上，系统无法个性化，广告系列退化为非动态投放。

---

## 5. 精品栏（Collection）广告 + Instant Experience

### 5-1. 格式规格

精品栏广告有一个主素材（视频或图片）加下方 3 格商品网格。点击后打开 Instant Experience 全屏点击后目标地，在 Meta 应用内加载。

### 5-2. 模板

| 模板 | 最适用 | 目录要求 |
|---|---|---|
| 即时店面（Instant Storefront） | 4+ SKU 的目录；网格式发现 | 是 |
| 即时画报（Instant Lookbook, Lifestyle Catalog） | 编辑式 / 品牌叙事 + 可购商品 | 是 |
| 即时获客（Instant Customer Acquisition） | 移动端落地页的单商品或服务转化 | 可选（无目录也行） |
| 即时表单（Instant Form） | 应用内表单填写 | 否 |
| 即时故事（Instant Storytelling） | 叙事式商品揭晓 | 可选 |

### 5-3. 销售场景何时用精品栏

销售场景用精品栏当：
- 移动流量主导（>70%）
- 商品视觉化（服装、美妆、家居、食品）
- 目录干净且图片丰富
- 想在应用内压缩发现 → 考虑链路

不用精品栏当：
- 目录图片不一致或分辨率低
- 商品需要重规格/对比
- 移动结账坏了或慢
- 用户旅程需要 Instant Experience 装不下的表单、计算器或配置器

### 5-4. 尺寸

| 素材 | 规格 |
|---|---|
| 封面图 | 1:1（1080×1080）或 16:9；最小 1200×628 |
| 封面视频 | 1:1 或 16:9；≤120 秒；适合循环 |
| 商品格图片 | 方形裁剪，来自目录 |
| Instant Experience | 全屏移动端，1080×1920 有效画布 |

### 5-5. 操作备注

- 精品栏只在移动版位渲染。桌面看到的是兜底（单图/单视频）。
- 单独追踪 Instant Experience 打开、滚动深度、链接点击——它们揭示发现发生了但没转化。
- 精品栏在 Advantage+ 销售和手动销售下都能用。

---

## 6. 通过消息购买

### 6-1. 是什么

销售目标的效果目标，优化发生在 Messenger（及支持的国家/地区的 Instagram Direct / WhatsApp 越来越多）内的购买事件。选择路径：转化位置 = 消息应用 → 效果目标 = 最大化通过消息的购买。

### 6-2. 要求

| 要求 | 规格 |
|---|---|
| 购买事件回传 Meta | 必需；过去 30 天 ≥5 个购买事件才符合资格 |
| 转化位置 | 消息应用 |
| 消息平台已连接 | Messenger 为主；WhatsApp/IG Direct 取决于国家和账户 |
| 回复运营 | 有团队或聊天机器人能承接点击量的进线 |

### 6-3. 何时用

- 对话能成交的高接触商品（中客单服装、美妆组合、服务）
- 聊天商务是常态的亚太/拉美市场
- 已在用 WhatsApp/Messenger 运营的品牌
- 网页结账有摩擦的市场中的店铺

### 6-4. 何时不用

- 纯自助结账跑得挺好
- 销售团队跟不上回复 SLA
- 没有机制把购买事件回传 Meta（只能手动录单）
- 商品是标品 / 低接触——聊天是负担，不是解锁

### 6-5. 报告注意事项

Messenger 内的"标记为已付款"功能目前只进报告，不进转化优化。优化需要通过 CAPI/Pixel 发送以消息互动为键的实际 `Purchase` 事件（带价值/货币）。
---

## 7. 出价策略

### 7-1. 策略阶梯

| 策略 | 机制 | 何时用 | 风险 |
|---|---|---|---|
| 最大投放量（Highest volume） | 无约束；花满预算追最便宜的结果 | 默认。新账户、学习、探索 | 容易的竞价耗尽后 CPA 会随时间漂移上升 |
| 单次成效费用目标（Cost per result goal） | 软性目标平均 CPA | 经济模型可预测的稳定账户；≥50 转化/周 | 目标太紧会跑不动量 |
| 出价上限（Bid cap） | 竞价中的硬性出价天花板 | 严格的 CPA 控制；手动竞价纪律 | 上限低于市场出清价会跑不动量 |
| ROAS 目标 | 软性目标平均 ROAS | 价值优化成熟、≥50 个带价值转化/周、价值分布宽 | 设太高 Meta 会暂停投放 |
| 最高价值（Highest value） | 无 ROAS 约束；追最高价值买家 | 从最大投放量到 ROAS 目标的桥梁；引入 ROAS 目标前 4-6 周 | 花费会漂向高 AOV 但可能低利润 |
| 最低/最高花费目标 | 广告系列预算内广告组花费下限/上限 | 多广告组 Advantage+ 广告系列需要广告组均摊时 | 增加复杂度；在账户界面中核实可用性 |

### 7-2. 分阶段姿态

| 阶段 | 姿态 |
|---|---|
| 新账户 / 0-30 单/月 | 最大投放量。不设 CPA 目标。哪个事件有量就优化哪个。 |
| 30-50 单/月 | 最大投放量。开始追踪自然 CPA 分布，理解现实的单次成效费用目标。 |
| 50-100 单/月 | 最大投放量仍是默认。单次成效费用目标可以在并行广告组测试，不做全局切换。 |
| 100-300 单/月 | 单次成效费用目标可用于求稳。价值优化开着时用最高价值。 |
| 300+ 单/月且价值事件稳定 | ROAS 目标可行。初始设为观察到的平均 ROAS 减 10-20%（宽松），几周内收紧。 |

### 7-3. 出价上限数学

出价上限应反映最大可接受单次成效费用 + 竞价溢价。实操启发式：目标 CPA × 1.2 到 1.5。
示例：目标 CPA = 20 美元 → 出价上限 = 24-30 美元。

### 7-4. ROAS 目标陷阱

- ROAS 目标设在广告系列自然平均之上 → 投放停滞。
- 广告系列中途从最大投放量切到 ROAS 目标 → 学习重置。
- 每个事件都没开价值优化就设 ROAS 目标 → 对着不完整的信号优化。
- 退款没回传 → ROAS 目标对着虚高的价值优化。

### 7-5. 价值优化就绪度

跑最高价值或 ROAS 目标的要求：

- 所有购买事件都带 `value` 和 `currency`（不是 0，不是占位）
- 价值分布足够宽，高低有意义（顶部和底部五分位的 AOV 差 >2 倍）
- 每广告组每周 ≥50 个带价值购买
- CAPI 去重已验证——重复计数的收入是 ROAS 目标失败最常见的原因
- 退款/退货流程理想情况下回推负价值或退款事件

---

## 8. 受众设计

### 8-1. Advantage+ 受众 vs 原始受众

Advantage+ 受众把你的受众选择当*信号*，而不是硬定向。当模型预期有更好结果时，它会投到你的选择之外。硬控制仍有：

- 国家/地区（硬性）
- 最低年龄（硬性，尤其未成年保护）
- 语言（基本硬性）
- 自定义受众排除（硬性——最重要的杠杆）

只在以下情况用原始受众定向（旧版详细定向流程）：
- 合规或特殊广告类别要求
- ICP 太窄，Advantage+ 受众会稀释（早期 B2B、非常小众的垂直）

### 8-2. 销售用的自定义受众

| 受众 | 用途 | 窗口 |
|---|---|---|
| 网站访客 | 中层漏斗再营销 | 30/60/180 天 |
| 商品浏览者（`ViewContent`） | 下层漏斗再营销 | 14/30 天 |
| 加购未购买 | 高意图再营销 | 7/14 天 |
| 发起结账未购买 | 最高意图再营销 | 3/7 天 |
| 历史购买者 | 交叉销售、复购、排除 | 90/180/365 天 |
| 客户名单（CRM 上传） | 排除或类似受众种子 | 不适用 |
| 互动（IG/FB 主页） | 温潜客开发 | 60/90 天 |
| 视频观众（75%） | 温潜客开发 | 30/90 天 |

### 8-3. 类似受众（Lookalike）

| 来源 | 质量 |
|---|---|
| 最近 90/180 天购买者（高 LTV） | 最好 |
| 客户名单（带哈希邮箱 + 电话的完整文件） | 新鲜且有代表性时强 |
| 加购 / 发起结账用户 | 中等；有用但可能反映临时意图 |
| 全部网站访客 | 低（太宽，与 Advantage+ 受众拉不开差距） |
| 仅互动 | 弱，除非验证过对购买优化有效 |

操作规则：2026 年类似受众越来越被 Advantage+ 受众替代。用它们当：
- 你有一份特别干净的高价值客户名单
- 你想要硬排除框（"与现有客户类似"再排除现有客户）
- 你为合规做手动销售，需要明确定义的受众

不要堆 5+ 个类似受众百分比档——重叠会杀死学习。

### 8-4. 排除策略（承重墙）

永远排除：

| 排除 | 原因 |
|---|---|
| 历史购买者（目标是获客时） | 防收割；支撑新客户率 KPI |
| 活跃客户 / 订阅者 | 避免给付费客户推获客广告 |
| 近期线索（销售只做 upsell 时） | 避免重复计数或漏斗叠加 |
| 员工 / 内网 IP | 干净 |

Advantage+ 销售里客户名单排除仍是硬性的。当现有客户预算上限不可用或不够时，把它当作事实上的新客户控制。

---

## 9. 创意策略

### 9-1. 销售的格式优先级

| 格式 | 优先级 | 备注 |
|---|---|---|
| Reels 原生竖屏视频（9:16，4-15 秒） | 最高 | Reels 供给是最大的增长面；Reels、Stories、动态通用 |
| 静态 4:5 图片（动态优先） | 高 | 制作便宜，各版位稳健 |
| 轮播（方形 1:1 或 4:5） | 高 | 目录和利益点罗列强 |
| 目录（DPA / Advantage+ 目录） | 高 | SKU 驱动再营销必需 |
| 精品栏 + Instant Experience | 中 | 移动商务专用 |
| 方形 1:1（旧版） | 低 | 为兼容保留，不是首选 |
| 纯 Stories 静态 | 低 | Reels 覆盖了大部分 |

### 9-2. 素材量与概念数

| 账户阶段 | 概念 | 每概念变体 | 广告总数 |
|---|---|---|---|
| 0-30 单/月 | 2-3 | 2-3 | 6-10 |
| 30-100 单/月 | 4-6 | 3-4 | 15-25 |
| 100-300 单/月 | 6-10 | 3-5 | 25-50 |
| 300+ 单/月 | 10+ | 4-6 | 40-150 |

概念 = 核心想法（问题框架、利益、社交证明、机制、创始人故事、对比等）。
变体 = 同一概念跨格式 / 钩子 / 时长的重建。

### 9-3. Reels 和 Stories 安全区

截至 2026 年 3 月，Stories 和 Reels 在 1440×2560 画布上共享统一安全区：

| 区域 | 关键内容避开 |
|---|---|
| 顶部 | 14%（约 358px）——用户名、赞助标签 |
| 底部 | 20-35%（约 512-896px）——文案 + CTA 界面；Reels 比 Stories 扩张更多 |
| 两侧 | 各 6%（约 87px） |

关键内容放在水平中间约 80% 以内，以扛住不同设备的智能缩放（Smart Zoom）。

### 9-4. 宽高比默认

| 版面 | 推荐 |
|---|---|
| Reels / Stories | 9:16（1080×1920） |
| 动态 | 4:5（1080×1350） |
| 轮播 | 1:1（1080×1080） |
| 信息流视频 | 16:9 |

Meta 实际上已弃用 1:1 作为动态图片和视频的默认，转向 4:5。

### 9-5. 创意疲劳规则

| 信号 | 阈值 | 行动 |
|---|---|---|
| 频次（7 天） | >3.0 | 查疲劳，刷新钩子 |
| 频次（30 天） | >7.0 | 加新概念 |
| 14 天内 CTR 下降 | -30% | 刷新创意 |
| 钩子率（3 秒停留） | <25% | 换掉前 3 秒 |
| 停留率（15 秒） | <10% | 重剪节奏或换掉 |
| 出站 CTR vs 链接 CTR | 差距 >2 倍 | 落地页摩擦或优惠错配 |

### 9-6. Advantage+ 创意

Advantage+ 创意自动应用增强（裁剪、音乐、文案变体、亮度）和 AI 生成变体。规则：

- 上线前 QA 每个增强（尤其 AI 生成的文字叠加）
- 品牌调性会漂移；AI 生成的文案要过审
- 品牌一致性关键时（奢侈品、受监管）选择性关闭增强

### 9-7. UGC 和社交证明

销售中，用户生成内容（UGC, User-Generated Content）在单次成效费用上持续打败制作型创意，尤其 2025-2026 年的 D2C。操作启发式：在投的销售创意中至少 30% 是 UGC 风格。
---

## 10. 目录 Feed 质量（操作检查清单）

### 10-1. 每日检查

| 检查 | 通过标准 |
|---|---|
| Feed 同步成功运行 | 上次同步 <24 小时，无报错 |
| 商品拒绝数 | < 目录总量的 2% |
| 缺货但有在投广告 | 0 |
| 价格不一致报错 | 0 |

### 10-2. 每周检查

| 检查 | 通过标准 |
|---|---|
| 图片拒绝（低质量、水印、文字叠加） | < 目录总量的 1% |
| 商品集定义与最新自定义标签匹配 | 是 |
| 标题长度分布 | <10% 超过 100 字符 |
| 描述填充率 | >95% |
| 目录政策违规 | 0 |

### 10-3. 每月检查

| 检查 | 通过标准 |
|---|---|
| Top 20 SKU 页面都可达、移动友好、快 | 是 |
| 利润标签最新 | 是 |
| 库存标签最新 | 是 |
| 季节标记更新 | 是 |
| 新 SKU 的 feed 覆盖 | 目录新增后 7 天内 100% |

---

## 11. 衡量设计

### 11-1. Pixel + CAPI 必需规格

对每个关键事件（购买、加购、发起结账、内容查看、线索、订阅、开始试用）：

| 字段 | 必需 | 备注 |
|---|---|---|
| `event_name` | 是 | Pixel 和 CAPI 之间必须逐字节匹配 |
| `event_id` | 是（去重用） | 浏览器端生成一次，Pixel 和 CAPI 都传同一个 |
| `event_time` | 是 | 最多回传 7 天；更早的被拒 |
| `action_source` | 是 | 网页用 `website`，`app`、`chat`、`business_messaging`、`physical_store` 等 |
| `event_source_url` | 网页是 | 触发事件的页面 URL |
| `user_data`（哈希） | 必需 | 邮箱、电话、fbp、fbc、external_id、IP、user agent、点击 ID |
| `value` + `currency` | 购买、订阅、开始试用必需 | ISO 4217 |
| `content_ids` + `content_type` | 目录匹配必需 | 必须与目录 `id` 匹配 |
| `contents` | 推荐 | 含 id、quantity、item_price 的数组 |
| `order_id` | 推荐 | 帮助服务端去重和 CRM 对账 |

### 11-2. 标准事件模板

| 事件 | 必填字段 | 推荐但可选 |
|---|---|---|
| `Purchase` | value、currency、content_ids、content_type | order_id、contents、num_items |
| `Subscribe` | value、currency | predicted_ltv |
| `StartTrial` | value（常为 0）、currency | predicted_ltv |
| `AddToCart` | content_ids、content_type | value、currency |
| `InitiateCheckout` | content_ids、content_type | value、currency、num_items |
| `ViewContent` | content_ids、content_type | value、currency、content_category |
| `Lead` | （无必填） | content_name、content_category、value |

### 11-3. 去重

规格：
- 窗口：Pixel 和 CAPI 事件之间 48 小时去重
- 匹配键：`event_name` + `event_id`（主）；`fbp` + `external_id` 是兜底但较弱
- 浏览器优先：两者在约 5 分钟内都到达时，Meta 优先采用浏览器事件
- 在事件管理工具 → 测试事件中验证；找"Deduplicated"标签

常见去重失败：

| 失败 | 原因 | 修复 |
|---|---|---|
| 同一事件被计数两次 | event_id 缺失或不一致 | 生成一次，两边传一样的 |
| event_name 大小写不一致（`Purchase` vs `purchase`） | 实施漂移 | 统一并锁定 |
| event_time 偏差 >2 小时 | 服务器时钟漂移 | NTP 同步或传精确的浏览器时间戳 |
| 自定义事件名与标准事件名混用 | 意外改名 | 改名统一 |
| 部分浏览器只有一侧 | 广告拦截只拦了 Pixel | CAPI 补位；Meta 去重仍然正常 |

### 11-4. 事件匹配质量（EMQ, Event Match Quality）

EMQ 是每个事件 0-10 的分数，表示发送的客户数据与 Meta 账户的匹配程度。

操作规则：
- 把 EMQ 当健康检查，不当 KPI
- 购买目标 7+；上层漏斗 6+
- 发送哈希邮箱、电话、fbp、fbc、external_id、IP、user agent，及（允许时）地址字段来提升 EMQ
- 不要卡着 EMQ 不上线；先建基线再迭代

### 11-5. 后端对账

50+ 单/月的销售账户必需的报告：

| 报告 | 来源 | 频率 |
|---|---|---|
| 后端收入 vs Meta 归因收入 | CRM/电商 + Ads Manager | 每周 |
| 新 vs 回头客户率 | CRM/电商 | 每周 |
| 首单边际贡献 | 财务 | 每月 |
| Meta 归因订单的退款/退货率 | CRM/财务 | 每月 |
| 点击 vs 互动 vs 浏览贡献 | Ads Manager 归因拆分 | 每周 |
| 商品级 ROAS 和利润 | 目录 × CRM | 每月 |
| 对照或地理测试结果（如在跑） | 测试平台 | 按测试 |

### 11-6. 2026 归因窗口

Ads Insights 的归因窗口支持会随时间变化。做看板或基线推荐前，重查浏览、点击和互动归因的支持情况。

Meta 更改点击或互动归因定义时，重置报告基线并标注变更。

操作含义：

- 不标注就不要对比 2026-01-12 前后的周期
- 有暴露时，把点击、互动、浏览归因报成独立列
- 互动归因不是"网站访问"——它是社交互动后发生的转化
- 后端对账现在应该与链接点击归因更贴近；剩余差距是互动 + 浏览 + 建模

### 11-7. 转化提升 / 品牌提升 / 销售提升

| 测试 | 衡量 | 何时 |
|---|---|---|
| 转化提升 | Meta 曝光带来的增量购买/线索 | 稳定花费 90+ 天，被测单元 ≥5,000-10,000 美元/周 |
| 品牌提升 | 广告回忆度、认知度、考虑度、好感度 | 主要是品牌认知/互动广告系列，销售上也可以叠加 |
| 销售提升 / 线下提升 | 用匹配交易衡量的门店/线下增量销售 | 有 POS feed 的全渠道零售商 |

把平台 ROAS 当增量的上限。真实提升取决于品类、品牌强度、再营销占比和实际对照结果。

### 11-8. 新 vs 老客户报告

必需，用来看 Advantage+ 销售是在获客还是在收割：

| 指标 | 来源 |
|---|---|
| 新客户率（订单中首单占比） | CRM |
| 每个新客户成本（广告系列花费 / 新客户数） | CRM × Ads Manager |
| 新客户 LTV | CRM 队列 |
| Meta 归因收入中回头客户占比 | CRM × Ads Manager |

如果账户界面里有现有客户预算上限，主动监控它。如果没有，这份报告就是防收割的唯一检查。

---

## 12. 诊断决策树

```
START
│
├─ Q1: Is Pixel + CAPI dedup verified?
│   ├─ NO → STOP. Fix measurement before any campaign decision.
│   └─ YES → continue
│
├─ Q2: Is Meta-reported revenue tracking with backend?
│   ├─ NO, Meta higher → Likely existing-customer harvesting, view-through/engage-through inflation, dedup miss
│   │   → Run new-customer report
│   │   → Verify dedup
│   │   → Check Existing Customer Budget Cap
│   │   → Consider holdout test
│   └─ YES → continue
│
├─ Q3: Is volume (purchases/week per ad set) ≥50?
│   ├─ NO → 
│   │   ├─ Consolidate ad sets
│   │   ├─ Drop to upper-funnel event temporarily (ATC/IC) but plan exit
│   │   ├─ Increase budget if CPA permits
│   │   └─ Reduce concept count temporarily
│   └─ YES → continue
│
├─ Q4: Is platform ROAS healthy but profit weak?
│   ├─ YES → Margin/refund/discount mix problem
│   │   ├─ Add custom_label margin tiers to feed
│   │   ├─ Run product-set splits by margin
│   │   └─ Consider Highest value with margin-weighted values
│   └─ NO → continue
│
├─ Q5: Is CPA rising over time at flat creative?
│   ├─ YES → Creative fatigue
│   │   ├─ Refresh top-funnel hooks
│   │   ├─ Add new concepts (not variants)
│   │   └─ Verify frequency >3.0
│   └─ NO → continue
│
├─ Q6: Is ATC high but Purchase low?
│   ├─ YES → Checkout friction
│   │   ├─ Audit shipping cost surprise
│   │   ├─ Audit payment options
│   │   ├─ Audit mobile checkout speed
│   │   └─ Audit form fields
│   └─ NO → continue
│
├─ Q7: Are catalog/dynamic ads showing wrong products?
│   ├─ YES → content_ids mismatch
│   │   ├─ Verify Pixel/CAPI content_ids match catalog id
│   │   ├─ Verify item_group_id for variants
│   │   └─ Verify feed sync recency
│   └─ NO → continue
│
└─ Q8: Is Advantage+ Sales underperforming manual?
    ├─ Consider:
    │   ├─ Creative volume <15? Add 10-20 concepts.
    │   ├─ Existing customer share excessive? Set/lower cap.
    │   ├─ Audience exclusions missing? Add customer list.
    │   ├─ Conversion event too low-funnel? Move to Purchase.
    │   ├─ Budget below 50/wk threshold? Increase or consolidate.
    │   └─ ROAS goal too tight? Loosen 10-20%.
```

---

## 13. 常见陷阱（完整版）

### 13-1. 衡量陷阱

- 2026 年只有 Pixel 没有 CAPI 就上线销售——优化质量实质下降
- Pixel + CAPI 都发了但从不验证去重 → 重复计数
- 自定义事件名与标准事件名冲突 → 优化被拆散
- 购买缺 `value`/`currency` → 跑不了价值优化
- `event_time` 时钟偏差 → 事件被拒或错位
- 把事件匹配质量当 KPI 而不是卫生指标
- 对比周期时忽视 2026 年归因窗口变化

### 13-2. 目录陷阱

- 库存过期——缺货商品还在投
- 图片质量低于阈值——静默拒绝
- 标题堆砌关键词——CTR 下降且品牌调性受损
- 平台迁移后 `id` 冲突——历史再营销断裂
- 缺 `brand`/`gtin`——商店和搜索资格丢失
- 没有自定义标签——跑不了利润感知的商品集
- 手动单次上传而不是排期同步——漂移不可避免

### 13-3. 广告系列结构陷阱

- 一商品一广告系列的碎片化
- 按 Advantage+ 受众更擅长的人口属性拆分
- 多个类似受众百分比档叠加 → 受众重叠
- 获客广告系列忘了客户名单排除
- 学习期内改创意（会重置）
- 学习期中途改预算 >20%（会重置）

### 13-4. 出价陷阱

- ROAS 目标设在自然平均之上 → 投放停滞
- 学习期中途切换策略
- 出价上限低于市场出清价 → 跑不动量且无从诊断
- <50 事件/周的单次成效费用目标 → 不稳定
- 跑最高价值或 ROAS 目标时不回传退款事件

### 13-5. Advantage+ 销售特有陷阱

- 把 Advantage+ 当作商品市场匹配差的解药
- 2026 年没有 CAPI 就用 Advantage+ 销售——优化质量大损失
- 不核实现行界面就假设现有客户预算上限的状态（2024-2026 年改过多次）
- 只放 3-5 条创意——系统没法从 5 个输入测出 150 个组合
- 迁移时不导入成功的手动广告
- 一个 Advantage+ 销售广告系列横跨经济模型差异很大的多个国家

### 13-6. 创意陷阱

- 默认用 1:1 而不是 4:5 / 9:16
- 关键内容放在 Reels/Stories 不安全区（顶部 14%、底部 20-35%）
- 全是一个概念的变体而不是多个不同概念
- 重文字叠加被 AEM/智能缩放裁掉
- 只有精致制作型创意，没有 UGC
- Reels 创意不重剪直接当静态动态用
- 让 Advantage+ 创意 AI 生成文案而不做品牌 QA

### 13-7. 受众陷阱

- Advantage+ 销售里排除所有温受众（违背系统）
- 完全不排除 → 老客户收割拉满
- 5+ 个类似受众档叠加 → 重叠杀死学习
- 与 Advantage+ 受众扩展矛盾的详细定向
- 自定义受众窗口太长（如 365 天网站访客当"温"受众）

### 13-8. 操作陷阱

- 学习期内编辑
- 日预算变动 >20%
- 每天暂停/重启广告组
- 不做归因标注就对比 2026-01-12 前后
- <50 事件/周跑 A/B 测试（没有统计效力）
- 用一次创意测试评判一个概念（方差主导）

---

## 14. 易变能力检查清单（在现行界面中核实）

这些功能多次上线、回滚或变更范围。承诺行为前**永远核实现行账户界面**。

| 能力 | 核实什么 |
|---|---|
| 现有客户预算上限 | Advantage+ 销售里有吗？百分比可调？默认值？ |
| 现有客户报告 | Ads Manager 细分里有吗？ |
| Advantage+ 销售多广告组 | 本账户有吗？最多几个？ |
| 每广告组 50 条广告上限 | 强制吗？旧的单广告组 150 条还能跑吗？ |
| Advantage+ 目录广告——宽定向模式 | 有，还是并入 Advantage+ 销售目录流程了？ |
| 商店集成 / 商店广告 | 本国家有吗（写作时仅美国）？ |
| 最高/最低花费广告组目标 | 预算控制里可见吗？ |
| ROAS 目标 vs 最高价值 | 都有吗？ |
| Advantage+ 创意增强 | 有单素材开关吗？AI 生成文案审批流？ |
| 浏览器转化 API（CAPIG） | 设置选项可见吗？ |
| 转化线索优化 | 资格解锁了吗？ |
| 转化提升可用性 | 层级/账户阈值？ |
| 品牌提升可用性 | 花费阈值？ |
| 特殊广告类别定向限制 | 现行类别和排除？ |
| 归因窗口选项（1d_view、点击窗口） | 本账户暴露了哪些？ |
| 互动归因 | 报成独立列了吗？ |
| 现有客户预算上限字段命名 | 界面标签改过；字段可能在 Advantage+ 销售设置、广告组或广告系列 |

销售易变项的现行官方检查：

- Meta 销售目标：https://www.facebook.com/business/ads/ad-objectives/sales
- Meta Advantage+ 销售：https://www.facebook.com/business/ads/meta-advantage-plus/sales-campaigns
- Meta Advantage+ 目录广告：https://www.facebook.com/business/ads/meta-advantage-plus/catalog-ads
- Meta 精品栏广告：https://www.facebook.com/business/ads/collection-ad-format
- Meta 通过消息购买：https://www.facebook.com/business/ads/click-to-message-ads/purchases-through-messaging/
- Meta CAPI 去重：https://developers.facebook.com/docs/marketing-api/conversions-api/deduplicate-pixel-and-server-events

---

## 15. 速查表

### 15-1. 销售目标一览

| 问题 | 答案 |
|---|---|
| 取代了什么？ | 销售是旧转化 + 目录销售目标的合并 |
| 默认转化位置 | 网站 |
| 必需信号栈 | Pixel + CAPI + 带价值/货币的购买事件 |
| 默认出价 | 最大投放量 |
| 学习阈值 | 每广告组每周 50 转化 |
| 默认版位 | Advantage+ 版位（全版面） |
| 默认受众 | Advantage+ 受众 |

### 15-2. 按业务的子类型推荐

| 业务 | 推荐的销售子类型 |
|---|---|
| Pixel+CAPI 干净、≥100 单/月的 D2C | Advantage+ 销售主攻 + 手动再营销 |
| 大目录的 D2C | Advantage+ 销售 + Advantage+ 目录广告（宽定向 + 再营销） |
| 用购买或订阅事件的订阅 / SaaS | 优化订阅/购买的 Advantage+ 销售，价值分布宽时用最高价值 |
| 高利润小众 | 带利润标签商品集的手动销售 |
| 本地 / 服务区域 | 严格地理 + 电话或消息转化位置的手动销售 |
| 对话式商务（聊天驱动） | 消息转化位置、通过消息购买目标的销售 |
| 有商店集成的店铺 | 只在现行界面仍暴露时用商店辅助目标地的 Advantage+ 销售 |
| 移动优先的应用商务 | 应用转化位置、AEM + MMP 衡量的销售 |

### 15-3. 关键阈值

| 阈值 | 值 |
|---|---|
| 学习期退出 | 每广告组每周 50 转化 |
| 学习的日预算下限 | 目标 CPA × 50 / 7 |
| 目录图片最小尺寸 | 500×500（推荐 1024×1024） |
| 目录标题最大长度 | 200 字符（推荐 ≤100） |
| Pixel/CAPI 去重窗口 | 48 小时 |
| `event_time` 最大回传 | 7 天 |
| 线下事件上传窗口 | 62 天 |
| 通过消息购买资格 | 过去 30 天 5+ 个购买事件 |
| Reels/Stories 安全区（顶部） | 14% |
| Reels 安全区（底部） | 35% |
| Stories 安全区（底部） | 20% |

---

## 16. 交叉引用

- 用 `measurement-and-attribution.md` 查 Pixel/CAPI、去重、EMQ、线下事件、归因和增量。
- 用 `advantage-plus.md` 查 Advantage+ 销售控制、现有客户易变性和现行归因/API 行为。
- 用户提供 Ads Manager、事件管理工具、后端、CRM 或目录数据时用 `account-data-diagnostics.md`。
- 用 `creative-strategy.md` 和 `creative-production.md` 查概念策略、Reels/Stories 适配、UGC/社交证明和格式制作。
- 优惠涉及信贷、住房、就业、健康、社会议题或其他受监管话题时用 `policy-and-special-categories.md`。
