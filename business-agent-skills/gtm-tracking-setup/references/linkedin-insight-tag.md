# LinkedIn Insight Tag - GTM 实施手册

> 真实信息来源：[LinkedIn 营销 API 文档](https://learn.microsoft.com/en-us/linkedin/marketing/)——Insight Tag、转化 API（CAPI）与受众（Audiences）文档的入口。
> LinkedIn API 版本遵循 `YYYYMM` 节奏；每个版本在停用（sunset）前至少支持 12 个月。

---

## 1. 转化类型参考（Conversion Type Reference）

LinkedIn 不提供固定的标准 JS 事件名称列表。你需要在 Campaign Manager（或通过营销 API（Marketing API））中创建一条**转化规则（conversion rule）**，为其分配一个固定枚举中的**转化类型（conversion type）**，并通过 URL 匹配或 `window.lintrk('track', { conversion_id: <ID> })` 从网站触发。每条规则获得一个独立的数字**转化 ID（Conversion ID）**。

### Campaign Manager 转化行为（当前界面——13 个简化选项）（Campaign Manager Conversion Behaviors (current UI — 13 simplified options)）

| 漏斗阶段 | 转化行为 | 适用场景 |
|---|---|---|
| 漏斗顶部（Top of funnel） | 关键页面浏览（Key page view） | 浏览重要页面或版块 |
| 漏斗顶部（Top of funnel） | 视频观看（Video view） | 播放视频 |
| 漏斗顶部（Top of funnel） | 注册活动（Register event） | 注册参加活动 |
| 漏斗中部（Middle of funnel） | 注册（Sign up） | 完成注册 / 试用 |
| 漏斗中部（Middle of funnel） | 下载（Download） | 下载文件或安装应用 |
| 漏斗中部（Middle of funnel） | 线索（Lead） | 线索表单提交 |
| 漏斗中部（Middle of funnel） | 预约（Book appointment） | 预约 / 安排约会 |
| 漏斗中部（Middle of funnel） | 联系（Contact） | 表单、电话或其他联系 |
| 漏斗底部（Bottom of funnel） | 合格线索（Qualified Lead） | MQL / SQL 识别 |
| 漏斗底部（Bottom of funnel） | 提交申请（Submit application） | 提交申请或报价请求 |
| 漏斗底部（Bottom of funnel） | 加购（Add to cart） | 发起购物车 / 结账 |
| 漏斗底部（Bottom of funnel） | 购买（Purchase） | 完成购买 |
| 其他（Other） | 其他（Other） | 兜底（点击、心愿单、分享、捐赠等） |

### 营销 API `type` 枚举（完整列表，用于 `POST /rest/conversions`）（Marketing API `type` enum (full list, used on `POST /rest/conversions`)）

```
ADD_TO_CART, DOWNLOAD, INSTALL, KEY_PAGE_VIEW, LEAD, PURCHASE, SIGN_UP, OTHER,
SAVE, START_CHECKOUT, SCHEDULE, VIEW_CONTENT, VIEW_VIDEO, ADD_BILLING_INFO,
BOOK_APPOINTMENT, REQUEST_QUOTE, SEARCH, SUBSCRIBE, AD_CLICK, AD_VIEW,
COMPLETE_SIGNUP, SUBMIT_APPLICATION, PHONE_CALL, INVITE, LOGIN, SHARE,
DONATE, ADD_TO_LIST, START_TRIAL, OUTBOUND_CLICK, CONTACT, QUALIFIED_LEAD
```

`QUALIFIED_LEAD` 以 `qualifiedLeads` 与 `costPerQualifiedLead` 报告指标呈现，可用作线索广告系列（Lead Generation campaigns）的优化目标。

### 转化规则属性（每条规则定义一次）（Conversion Rule Attributes (defined once per rule)）

| 字段 | 说明 |
|---|---|
| `name` | 人类可读的规则名称 |
| `type` | 上述枚举中的转化类型 |
| `valueType` | `DYNAMIC`（按事件覆盖）、`FIXED`（使用规则的存储值）或 `NO_VALUE`。默认为 `DYNAMIC`。 |
| `postClickAttributionWindowSize` | 1、7、30 或 90 天。默认 30。对于 `SUBMIT_APPLICATION`、`PURCHASE`、`ADD_TO_CART`、`QUALIFIED_LEAD`、`LEAD` 允许 365 天窗口。Microsoft Learn 的错误示例中还提到 28 是可选值；使用前请对照当前 API 版本核验。 |
| `viewThroughAttributionWindowSize` | 1、7、30 或 90 天。默认 7。同一批长窗口类型允许 365 天窗口。 |
| `attributionType` | `LAST_TOUCH_BY_CAMPAIGN`（默认）或 `LAST_TOUCH_BY_CONVERSION`。 |
| `enabled` | 布尔值。只有启用的规则才计数。 |
| `conversionMethod` | 服务端规则用 `CONVERSIONS_API`。要做 Insight Tag <-> CAPI 去重，请创建**两条独立规则**（一条浏览器端、一条 CAPI 端），并将它们关联到同一广告系列。 |

---

## 2. Insight Tag 设置（Insight Tag Setup）

LinkedIn Insight Tag 是一段 JavaScript 代码片段，会在加载的每个页面上自动向 `px.ads.linkedin.com/collect/` 发送页面浏览请求。基础标签（base tag）即页面浏览信号，是匹配受众（Matched Audiences）、网站访客画像（Website Demographics）与转化跟踪的基础。

> **敏感数据限制**：LinkedIn 禁止在收集或展示敏感数据（消费者健康、消费者金融服务等）的页面上放置 Insight Tag。

### 前置条件（Prerequisites）

- 来自 Campaign Manager > **数据（Data）> 信号管理器（Signals manager）> Insight Tag** 的 **合作伙伴 ID（Partner ID）**。选择**"I will use a tag manager"**以显示合作伙伴 ID（Partner ID）。
- （事件特定转化用）一个或多个**转化 ID（Conversion IDs）**：在 Campaign Manager > 衡量（Measurement）> 转化跟踪（Conversion tracking）中创建，跟踪方式选择 **"Event-specific"**。
- **LinkedIn Insight Tag** 社区模板（Community Template，v2.0），由 `linkedin` 发布于 GTM 社区模板库（GTM Community Template Gallery）。该模板内部处理 `insight.min.js` 的加载与 `_linkedin_partner_id` / `lintrk` 的初始化——**不需要自定义 HTML 基础代码**。

### 官方 GTM 模板——配置字段（Official GTM Template — Configuration Fields）

| 字段 | 说明 |
|---|---|
| **合作伙伴 ID / Insight Tag ID（Partner ID / Insight Tag ID）** | 数字型合作伙伴 ID（Partner ID）。支持逗号分隔多个值以触发多个标签。 |
| **转化 ID（最多 3 个）（Conversion IDs (max. 3)）** | 可选。来自 Campaign Manager 的逗号分隔数字型转化 ID（Conversion IDs）。 |
| **自定义 URL 覆盖（Custom URL override）** | 可选。当 LinkedIn 要求时，覆盖默认页面 URL。 |
| **事件 ID（Event ID）** | 可选。传入 `lintrk('track', ...)`，用于与转化 API（CAPI）去重。 |

> 官方模板没有专门的转化价值（Conversion Value）、货币（Currency）或订单 ID（Order ID）字段。要发送按事件的货币价值，请在转化规则上配置 `valueType: FIXED`，或使用服务端 GTM（sGTM）+ 转化 API（CAPI）发送动态值。

### 标签（Tags）

两种常见模式。

**A. 单基础标签 + Campaign Manager 中的 URL 匹配转化**（低维护）。用 URL 匹配定义每条转化规则（例如 URL 包含 `/thank-you`）。

| 标签名称 | 模板 | 触发器 | 同意 |
|---|---|---|---|
| LinkedIn Insight - Base | LinkedIn Insight Tag（仅合作伙伴 ID（Partner ID）） | 所有页面（All Pages） | ad_storage |

**B. 基础标签 + 按事件的转化标签**（有 `dataLayer` 时首选）。

| 标签名称 | 模板 | 触发器 | 同意 |
|---|---|---|---|
| LinkedIn Insight - Base | LinkedIn Insight Tag（仅合作伙伴 ID（Partner ID）） | 所有页面（All Pages） | ad_storage |
| LinkedIn Insight - Purchase | LinkedIn Insight Tag（合作伙伴 ID（Partner ID）+ 转化 ID（Conversion ID）） | CE - purchase | ad_storage |
| LinkedIn Insight - Lead | LinkedIn Insight Tag（合作伙伴 ID（Partner ID）+ 转化 ID（Conversion ID）） | CE - generate_lead | ad_storage |
| LinkedIn Insight - SignUp | LinkedIn Insight Tag（合作伙伴 ID（Partner ID）+ 转化 ID（Conversion ID）） | CE - sign_up | ad_storage |
| LinkedIn Insight - AddToCart | LinkedIn Insight Tag（合作伙伴 ID（Partner ID）+ 转化 ID（Conversion ID）） | CE - add_to_cart | ad_storage |
| LinkedIn Insight - KeyPageView | LinkedIn Insight Tag（合作伙伴 ID（Partner ID）+ 转化 ID（Conversion ID）） | （按页面） | ad_storage |

### 按事件参数映射（Per-Event Parameter Mapping）

| 事件 | 映射 |
|---|---|
| **购买（Purchase）** | 合作伙伴 ID（Partner ID）-> `{{Const - LinkedIn Partner ID}}`，转化 ID（Conversion ID）-> `{{Const - LI Conversion ID Purchase}}`，事件 ID（Event ID）-> `{{DLV - ecommerce.transaction_id}}` |
| **线索（Lead）** | 合作伙伴 ID（Partner ID）-> `{{Const - LinkedIn Partner ID}}`，转化 ID（Conversion ID）-> `{{Const - LI Conversion ID Lead}}`，事件 ID（Event ID）-> `{{DLV - form.submission_id}}` |
| **注册（Sign Up）** | 合作伙伴 ID（Partner ID）-> `{{Const - LinkedIn Partner ID}}`，转化 ID（Conversion ID）-> `{{Const - LI Conversion ID SignUp}}`，事件 ID（Event ID）-> `{{DLV - user.id}}`（如可用） |
| **加购（AddToCart）** | 合作伙伴 ID（Partner ID）-> `{{Const - LinkedIn Partner ID}}`，转化 ID（Conversion ID）-> `{{Const - LI Conversion ID AddToCart}}` |
| **关键页面浏览（KeyPageView）** | 合作伙伴 ID（Partner ID）-> `{{Const - LinkedIn Partner ID}}`，转化 ID（Conversion ID）-> `{{Const - LI Conversion ID KeyPageView}}` |

### 变量（Variables）

**常量变量（Constant variables）**：

| 变量名称 | 值 |
|---|---|
| `LinkedIn Partner ID` | （合作伙伴 ID（Partner ID），例如 `1234567`） |
| `LI Conversion ID Purchase` | （Purchase 规则的转化 ID（Conversion ID）） |
| `LI Conversion ID Lead` | （Lead 规则的转化 ID（Conversion ID）） |
| `LI Conversion ID SignUp` | （Sign-Up 规则的转化 ID（Conversion ID）） |
| `LI Conversion ID AddToCart` | （AddToCart 规则的转化 ID（Conversion ID）） |
| `LI Conversion ID KeyPageView` | （Key Page View 规则的转化 ID（Conversion ID）） |

**数据层变量（Data Layer variables）**：

| 变量名称 | 数据层变量名称 |
|---|---|
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - form.submission_id` | `form.submission_id` |

### 触发器（Triggers）

| 触发器名称 | 类型 | 条件 |
|---|---|---|
| All Pages | 页面浏览（Page View） | 所有页面（All pages） |
| CE - purchase | 自定义事件（Custom Event） | `purchase` |
| CE - generate_lead | 自定义事件（Custom Event） | `generate_lead` |
| CE - sign_up | 自定义事件（Custom Event） | `sign_up` |
| CE - add_to_cart | 自定义事件（Custom Event） | `add_to_cart` |

> 自定义事件名称**区分大小写**，必须与 dataLayer 的 `event` 值完全一致。

---

## 3. 转化跟踪（Conversion Tracking）

LinkedIn 支持三种方式，在 Campaign Manager > 衡量（Measurement）> 转化跟踪（Conversion tracking）中配置：

1. **全站 Insight Tag**——定义 URL 匹配。无需改动 JavaScript。
2. **事件特定 Insight Tag**——定义转化 ID（Conversion ID）；从 GTM 触发 `lintrk('track', { conversion_id: <ID> })`。
3. **转化 API（Conversions API / CAPI）**（服务端）——见第 4 节。

### 归因窗口（Attribution Windows）

| 设置 | 允许的值 | 默认值 |
|---|---|---|
| 点击后（Post-click） | 1、7、30、90（`SUBMIT_APPLICATION`、`PURCHASE`、`ADD_TO_CART`、`QUALIFIED_LEAD`、`LEAD` 可用 365） | 30 |
| 浏览后（View-through） | 1、7、30、90（同一批长窗口类型可用 365） | 7 |

### 计数行为（Counting Behavior）

- **多数类型**（Lead、Sign Up、关键页面浏览（Key Page View）、Download 等）：在转化窗口内每位成员只计一次转化。
- **Purchase 与 Add to Cart**：每个事件都可计数。

### 浏览器端去重（Browser-Side Deduplication）

避免对同一逻辑动作重复触发浏览器转化标签。用 GTM 触发器条件（"Once per page" / "Once per event"）。与转化 API（CAPI）冗余时，通过 `event_id` / `eventId` 去重。

---

## 4. 转化 API（Conversions API / CAPI）

LinkedIn 的转化 API（CAPI）是与 Insight Tag 搭配的推荐混合方案。使用官方服务端 GTM（sGTM）模板 **LinkedIn | CAPI Tag**（作者 `linkedin-developers`）——模式：GA4 网页标签（GA4 Web Tag）-> 服务端容器（server container）-> GA4 客户端（GA4 Client）-> LinkedIn CAPI 标签向 `/rest/conversionEvents` 发送请求。必填输入：API 访问令牌（API access token）+ 转化规则 ID（Conversion Rule ID）。每次请求固定 `Linkedin-Version: YYYYMM` 并每年迁移。

> **Insight Tag <-> CAPI 去重**：如果同一 `eventId` 从两端到达，**Insight Tag 事件优先**（与 Meta 相反）。两条规则必须作为独立转化规则存在，并关联到同一广告系列。

参见：https://learn.microsoft.com/en-us/linkedin/marketing/integrations/ads-reporting/conversions-api

---

## 5. 匹配受众（Matched Audiences，Remarketing）

Insight Tag 支持网站受众（Website Audiences，URL 规则），并与 Campaign Manager 中的联系人列表（Contact Lists，CSV/CRM）和公司列表（Company Lists）配合使用。激活前需要至少 **300 个已匹配成员**。Insight Tag 还驱动网站访客画像（Website Demographics）。相似受众（Lookalike Audiences）已于 2024 年 2 月 29 日停用——替代方案是**预测受众（Predictive Audiences）**与**受众扩展（Audience Expansion）**。

参见：https://www.linkedin.com/help/lms/answer/a420552

---

## 6. 同意与隐私（Consent & Privacy）

LinkedIn 没有发布原生的同意模式 SDK 信号。在 **GTM/CMP 层**执行同意（consent）：在每个 LinkedIn Insight 标签上添加 `ad_storage` 作为必需的额外同意检查（Additional Consent Check）。严格 GDPR 选择同意（opt-in）时，在获得同意前完全抑制基础标签。

### `li_fat_id`（第一方点击 ID（First-Party Click ID））

启用**增强转化跟踪（Enhanced conversion tracking）**后（Campaign Manager > 信号管理器（Signals manager）> Insight Tag > 设置（Settings）），LinkedIn 会在落地页 URL 后追加 `li_fat_id` 并将其作为第一方 Cookie 存储在广告主域名上。该值可作为 `idType: LINKEDIN_FIRST_PARTY_ADS_TRACKING_UUID` 发送给 CAPI 以实现高质量匹配。

### Cookie（关键集合）（Cookies (key set)）

| Cookie | 设置方 | 用途 | 生命周期 |
|---|---|---|---|
| `bcookie` | linkedin.com（第三方（third-party）） | LinkedIn 浏览器标识符 | 1 年 |
| `lidc` | linkedin.com | 路由 | 24 小时 |
| `li_gc` | linkedin.com | 存储访客 Cookie 同意 | 6 个月 |
| `UserMatchHistory` | linkedin.com | 广告 ID 同步 | 30 天 |
| `li_fat_id` | 第一方（广告主）（first-party (advertiser)） | LinkedIn 点击 ID | 30 天 |

Safari ITP 会屏蔽第三方 Cookie。要获得最大耐久性，请将 Insight Tag 与**转化 API（CAPI）**配合使用，发送 `SHA256_EMAIL` 与 `li_fat_id`。

---

## 7. dataLayer 映射（推荐模式）（dataLayer Mapping (Recommended Pattern)）

| GA4 dataLayer 事件 | LinkedIn 转化类型 | 推荐的 `event_id` |
|---|---|---|
| `purchase` | `PURCHASE` | `transaction_id` |
| `generate_lead` | `LEAD` | 表单提交 ID（form submission ID） |
| `sign_up` | `SIGN_UP` 或 `COMPLETE_SIGNUP` | 用户 ID（user ID） |
| `add_to_cart` | `ADD_TO_CART` | 购物车事件 ID（cart event ID） |
| `begin_checkout` | `START_CHECKOUT` | 会话 / 购物车 ID（session / cart ID） |
| `view_item` | `VIEW_CONTENT` 或 `KEY_PAGE_VIEW` | — |
| `subscribe` | `SUBSCRIBE` 或 `START_TRIAL` | 订阅 ID（subscription ID） |
| （自定义）预订演示（(custom) book demo） | `BOOK_APPOINTMENT` | 预订 ID（booking ID） |

当同一 `event_id` 同时发送给 Insight Tag 与 CAPI 时，使用**稳定标识符**（订单 ID、表单提交 ID）——绝不要用 `Date.now()` 或随机值。

---

## 8. 调试（Debugging）

- **GTM 预览模式（GTM Preview Mode）**：标签触发顺序（基础 -> 事件）、变量解析。
- **Campaign Manager > 信号管理器（Signals manager）> Insight Tag**：状态 `Active` / `Receiving traffic`（延迟最长 24 小时）。
- **开发者工具网络（DevTools Network）**：筛选 `px.ads.linkedin.com`。基础页面浏览（Base PageView）-> `/collect/`；事件（events）-> `/wa/?...`，带 `pid`、`conversionId`、`eventId`。

---

## 9. 最佳实践（Best Practices）

- **对高价值事件同时运行 Insight Tag + CAPI**，使用稳定的 `event_id`（交易 ID、表单 ID）。注意去重时 Insight Tag 优先。
- 在 CAPI 中**同时使用 `SHA256_EMAIL` + `LINKEDIN_FIRST_PARTY_ADS_TRACKING_UUID`** 以获得最佳匹配率。
- 在 Campaign Manager 中**启用增强转化跟踪（Enhanced conversion tracking）**（第一方 Cookie）。
- **使用 LinkedIn 官方发布的 GTM 模板**，而非旧的自定义 HTML（Custom HTML）。
- **每个事件类型使用独立的转化 ID（Conversion ID）**；不要在单个转化 ID（Conversion ID）下复用多个事件。
