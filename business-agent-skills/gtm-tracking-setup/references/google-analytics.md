# GA4 + GTM - 设计与实施手册

> 侧重：面向 GA4 的 GTM 容器设计。后台管理/分析侧主题仅做摘要并指向官方文档。
> 唯一事实来源：[GA4 Help Center](https://support.google.com/analytics)。

---

## 1. 资源结构（Property Structure）

**Account > Property > Data Stream。**1 个 property = 1 个站点（+ app）；避免过度拆分。就 GTM 设计而言，通常针对单个 Web 数据流的衡量 ID（Measurement ID）（`G-XXXXX`）。限额与设置细节见 GA4 property setup。

---

## 2. 事件分类（Event Taxonomy）

### 事件类别

| 类别 | 描述 | 实现方式 |
|---------|------|------|
| **自动收集** | 默认收集（`first_visit`、`session_start`、`page_view` 等） | 无需 |
| **增强型衡量** | 在 GA4 界面开启（`scroll`、`click`、`file_download`、`video_start/progress/complete`、`form_start/submit`） | 界面开关 |
| **推荐事件（Recommended events）** | Google 定义的名称与参数。启用特定报告功能（变现报告等） | GTM/代码 |
| **自定义事件** | 用户自定义。上述未覆盖的全部 | GTM/代码 |

**优先级**：推荐事件 > 自定义事件。自动生成变现报告和 ML 功能需要推荐事件。

### 事件命名规则

- **区分大小写**（`my_event` ≠ `My_Event`）
- 必须以字母开头。仅允许字母、数字和下划线。**少于 40 个字符**
- 使用**小写 snake_case**（如 `contact_form_submit`）。事件名不能使用非 ASCII 字符
- 本地化或非 ASCII 文本可用于参数**值**

**保留前缀**（禁用）：`_`、`firebase_`、`ga_`、`google_`、`gtag.`

**保留事件名**（禁用）：`ad_activeview`、`ad_click`、`ad_exposure`、`ad_query`、`ad_reward`、`adunit_exposure`、`app_clear_data`、`app_exception`、`app_install`、`app_remove`、`app_store_refund`、`app_store_subscription_cancel`、`app_store_subscription_convert`、`app_store_subscription_renew`、`app_update`、`app_upgrade`、`dynamic_link_app_open`、`dynamic_link_app_update`、`dynamic_link_first_open`、`error`、`first_open`、`first_visit`、`in_app_purchase`、`notification_dismiss`、`notification_foreground`、`notification_open`、`notification_receive`、`os_update`、`screen_view`、`session_start`、`user_engagement`

### 事件设计原则：用参数而非事件名区分

事件名保持通用，用参数表达细节。这样可以控制唯一事件名的增长，保留分析灵活性。

**反例**：`cta_click_top_banner`、`cta_click_bottom_button`、`cta_click_sidebar_link`

**正例**：
```
Event: cta_click
Parameters:
  cta_id: cta_bnr_top / cta_btn_btm / cta_link_side
  click_url: https://...
```

参数值前缀模式：`type_position`（如 `cta_bnr_top`、`cta_btn_footer`）

> 运营指南：自定义事件保持在约 15–25 个。Web 端无官方上限，但 App 端硬上限为 500。

### 参数/限额规则（GTM 设计关键限额）

| 项目 | Standard | 360 |
|------|------|-----|
| 每个事件的参数数 | 25 | 25 |
| 参数名 | 40 字符（字母数字+下划线，必须以字母开头） | 同左 |
| 参数值 | 100 字符 | 500 字符 |
| 用户属性名/值 | 24 / 36 字符 | 同左 |
| 唯一事件名（App 硬上限） | 500 | 500 |
| 每个事件的电商商品数 | 200 | 200 |
| 关键事件 | 30 | 50 |

完整配额参考：GA4 limits。

---

## 3. 推荐事件参考

### 所有资源通用

| 事件 | 用途 |
|---------|------|
| `login` | 登录 |
| `sign_up` | 注册 |
| `search` | 站内搜索 |
| `select_content` | 内容选择 |
| `share` | 内容分享 |

### 电商

> 「规定/强烈推荐的参数」= 没有它们事件仍会触发，但这些参数对变现报告、优化和 ROAS 衡量很重要。

| 事件 | 用途 | 规定/强烈推荐的参数 |
|---------|------|---------------|
| `view_item_list` | 查看商品列表 | `items` |
| `select_item` | 选择商品 | `items` |
| `view_item` | 查看商品详情 | `currency`、`value`、`items` |
| `add_to_cart` | 加入购物车 | `currency`、`value`、`items` |
| `remove_from_cart` | 从购物车移除 | `currency`、`value`、`items` |
| `add_to_wishlist` | 加入心愿单 | `currency`、`value`、`items` |
| `view_cart` | 查看购物车 | `currency`、`value`、`items` |
| `begin_checkout` | 开始结账 | `currency`、`value`、`items` |
| `add_shipping_info` | 提交配送信息 | `currency`、`value`、`items` |
| `add_payment_info` | 提交支付信息 | `currency`、`value`、`items` |
| `purchase` | 购买完成 | `currency`、`value`、`transaction_id`、`items` |
| `refund` | 退款 | `currency`、`value`、`transaction_id` |

### 线索获取（Lead Generation）

| 事件 | 用途 | 规定/强烈推荐的参数 |
|---------|------|---------------|
| `generate_lead` | 线索生成（表单提交） | `currency`、`value` |
| `qualify_lead` | 线索合格 | `currency`、`value` |
| `disqualify_lead` | 线索不合格 | `currency`、`value` |
| `close_convert_lead` | 线索成交 | `currency`、`value` |
| `close_unconvert_lead` | 未成交 | `currency`、`value` |

### SaaS 自定义事件示例

```
# Onboarding
onboarding_start / onboarding_step_complete (step_name, step_number) / onboarding_complete

# Feature usage
feature_used (feature_name, feature_category) / feature_discovered (feature_name)

# Subscription
plan_viewed (plan_name, plan_price) / trial_started / trial_expired
subscription_started (plan_name, billing_cycle, value)
subscription_upgraded / subscription_downgraded (from_plan, to_plan)
subscription_canceled (cancel_reason)

# Support
help_article_viewed (article_id) / support_ticket_created / feedback_submitted (feedback_type, rating)
```

### items 数组结构

- 每个事件最多 **200 个商品**，items 数组内最多 **27 个自定义参数**
- 标准字段：`item_id`、`item_name`、`price`、`quantity`、`item_brand`、`item_category`（最多 5 层）、`item_variant`、`affiliation`、`discount`、`coupon`、`location_id`、`index`

---

## 4. 关键事件（Key Events，即转化）

在 GA4 中，转化称为**关键事件**。任何事件都可标记为关键事件；只标记业务关键动作。计数方式（每次事件 vs 每次会话）、转化窗口（默认 30/90 天）、归因模型细节（数据驱动 vs 付费与自然最终点击）见 GA4 key events 与 Attribution in GA4。

**GTM 设计实务要点**：
- 触发器粒度很重要：线索类事件在 GTM 中选择"每次会话一次"语义（如触发器组/阻止），购买用"每次事件一次"。
- 不同报告使用不同归因（流量获取 = 最终非直接、用户获取 = 首次触点、转化 = 所选模型）。这是数字对不上的常见原因。
- Google Ads 导入：GA4 关键事件默认为 **Secondary**；用于出价时切换为 **Primary**。CV 同步最长 24 小时。

---

## 5. 自定义维度/指标（Custom Dimensions / Metrics）

配额（standard / 360）：事件级 50/125、用户级 25/100、商品级 10/25、自定义指标 50/125。删除后等待 48 小时再重建。完整参考：Custom dimensions and metrics。

**GTM 设计影响**：
- 先检查默认维度，再把参数注册为自定义维度（避免重复）。
- 高基数参数不要做成维度（事件 ID / 会话 ID / 用户 ID）—— 会被归入「(other)」。
- 使用内置 **User-ID** 功能，不要用自定义维度。
- 数值 → 自定义指标，不要用维度。
- 通过 GTM 推送自定义参数后，及时在 GA4 后台注册 —— 未注册的参数不会出现在界面报告中。

---

## 6. GTM 集成设计

### Google 标签（GA4 配置）

- Google 标签 = 建立与 GA4 连接的基础标签。通过 **Initialization 触发器**在所有页面触发
- GA4 事件标签在各自的触发器上触发（旧的「Configuration tag」下拉已弃用）
- 使用**配置变量**在标签间共享公共参数。**把 Google 标签配置变量作为 Measurement ID、服务端容器 URL、公共参数和同意状态的唯一事实来源，事件标签只引用该变量**（防止重复定义）
- **不要双重打标**：同时使用 gtag.js 和基于 GTM 的 GA4 标签会导致数据重复收集。二选一

### GTM 命名规范

**标签**：`[Platform] [type] - [name]`
- 示例：`GA4 event - generate_lead`、`GA4 config`、`Meta - Purchase`

**触发器**：`[Type] - [Description]`
- 示例：`click - blog - register button`、`CE - purchase`、`timer - 30s engagement`

**变量**：`[prefix] - [description]`

| 前缀 | 类型 |
|---------------|------|
| `dlv` | Data Layer Variable |
| `cookie` | First-party Cookie |
| `js` | JavaScript Variable |
| `cjs` | Custom JavaScript |
| `url` | URL Variable |
| `aev` | Auto-Event Variable |
| `regex` | Regular Expression |
| `const` | Constant |

示例：`dlv - ecommerce.value`、`cjs - X Pixel Contents`、`const - GA4 Measurement ID`

### DataLayer 设计

**初始化**：在 GTM 容器代码段**之前**声明

```javascript
window.dataLayer = window.dataLayer || [];
```

**设计规则**：

- 推送事件时包含 `event` 键
- 变量名用 camelCase（GTM 区分大小写）
- 用描述性名称（`page_category` ✓，`cat` ✗）
- 用显式的 dataLayer 推送，不要抓取 HTML

```javascript
dataLayer.push({
  event: 'form_submit',
  form_type: 'contact',
  form_location: 'footer'
});
```

**持久性**：dataLayer 变量只在当前页面内有效。页面跳转后必须重新推送或用 cookie/localStorage。

### SPA 支持

SPA 中常见 `page_view` 事件重复计数和事件漏发。**提前**决定方案。

**方案 1（推荐）**：开启增强型衡量的「基于浏览器历史记录事件的页面变化」
- 检测 `history.pushState` / `popstate` 并自动发送 `page_view`
- 兼容 React Router、Vue Router、Next.js 等

**方案 2**：在增强型衡量中**关闭**自动 `page_view`，通过 GTM 手动发送
- 显式设置 `page_location` 和 `page_title`

**重复计数检查**：在 DebugView 中务必验证每次页面导航 `page_view` 只触发**一次**。

### 服务端 GTM（sGTM）

- 浏览器 → sGTM 服务器（1 次 HTTP 请求）→ 分发到各供应商（GA4、Google Ads、Meta 等）
- 客户端 HTTP 请求减少 → 页面性能提升
- **Google 标签和 GA4 事件标签**都要设置 `server_container_url`
- **注意**：首次命中可能绕过 sGTM。验证路由是否正确

---

## 7. 电商跟踪

### dataLayer 模式

**必需**：每次推送电商事件前**先清空** ecommerce 对象（防止残留数据污染）。

```javascript
dataLayer.push({ ecommerce: null }); // Clear previous ecommerce data
dataLayer.push({
  event: 'add_to_cart',
  ecommerce: {
    currency: 'USD',
    value: 29.99,
    items: [{
      item_id: 'SKU_12345',
      item_name: 'Product Name',
      price: 29.99,
      quantity: 1
    }]
  }
});
```

### GTM「Send Ecommerce data」复选框

在 GA4 事件标签上启用后，电商数据会从 dataLayer **自动读取**。无需逐个配置变量。

### 常见错误

1. 未执行 `dataLayer.push({ ecommerce: null })` → 残留数据污染后续事件
2. 缺 `currency` / `value` → 变现报告中没有数值
3. `transaction_id` 用随机值 → 去重失败。用稳定的订单 ID
4. 连续事件（如快速点击数量按钮）未做节流/防抖

### GTM vs gtag.js

| 方案 | 推荐场景 |
|------|-----------|
| **gtag.js** | 只在 GA4 中衡量电商时 |
| **GTM** | 多平台共享同一 dataLayer 时（GA4 + Google Ads + Meta 等） |

---

## 8. 数据质量（内部流量、引荐排除、跨域）

在 **GA4 Admin > Data Streams / Data Filters** 中配置，不在 GTM 中。见 GA4 data filters 与 Cross-domain measurement。

**GTM 相关坑**：
- 内部流量过滤器默认为 **「Testing」**（未生效）。记得切换为「Active」。
- 把支付处理商（PayPal、Stripe）加入引荐排除列表。
- 跨域：所有域名使用**相同的 Measurement ID**；客户端 ID 通过 `_gl` 参数传递。去掉查询参数的服务端重定向会破坏归因 —— 验证 `_gl` 在所有重定向中存活。表单装饰与链接装饰一样是官方支持的。

---

## 9. User-ID 与报告身份（Reporting Identity）

登录时发送稳定、非 PII 的标识（≤256 字符）；未登录时不发送；登出时发送 `null`。**不要**把 User-ID 同时注册为自定义维度 —— 用内置功能。报告身份选项（Blended / Observed / Device-based）：**推荐 Blended**。见 User-ID for the web。

---

## 10. 同意模式与隐私（Consent Mode and Privacy）

### 同意类型与模式

七种同意信号（`analytics_storage`、`ad_storage`、`ad_user_data`、`ad_personalization`、`functionality_storage`、`personalization_storage`、`security_storage`）。两种实现模式：**Basic**（同意前阻止标签）和 **Advanced**（推荐；默认「denied」+ 无 Cookie ping，可做广告主级 CV 建模）。默认同意按显示横幅的地区限定范围。完整参考：Consent Mode。

行为建模大约需要每天 1,000 个事件持续 7 天（`denied`），以及 28 天中有 7 天每天 1,000 个已授权用户；达标是必要非充分条件。

数据保留：创建资源后立即改为 **14 个月**（标准版最长）。保留期只影响探索报告；标准汇总报告不受影响。见 Data retention。

### PII 禁令

不要向 GA4 发送邮箱、电话、姓名、地址、信用卡号或政府签发 ID（违反服务条款，有封号风险）。

**常见污染路径**：
- URL 查询参数中含邮箱 → 被记入 `page_location`
- 表单输入值被误作参数发送
- 用邮箱/用户名做 User-ID（用哈希后的内部 ID）

**缓解措施**：
- 在 Data Streams > Tag settings > 「Configure your query parameters further」中，把含 PII 的查询参数加入排除列表
- 设计应用 URL 时避免在查询参数中包含 PII

---

## 11. 报告与受众（Reports and Audiences）

探索技术（自由表格、漏斗、路径、细分重叠、用户分层、用户生命周期、同类群组）和受众（默认 30 天成员资格，最长 540 天；standard 上限 100 / 360 上限 400；开始累积需 24–48 小时，不追溯）在 GA4 界面配置。见 Explorations 与 Audiences。

GTM 相关：确保受众依赖的事件和参数（如 `engagement_time`、购物车放弃者的加购事件、「功能探索者」的页面路径匹配）实际在触发，必要时注册为自定义维度。

---

## 12. Google Ads / Search Console 关联

在 GA4 Admin 中完成（不在 GTM）。GA4 需要 Editor + Google Ads 需要 admin / Search Console 需要已验证所有者。开启 Google Ads 自动标记，验证 GCLID 在重定向中存活。见 Product links。

---

## 13. 配置限额

GTM 设计用到的关键限额已汇总在 §2。完整的资源级配额参考（受众、关键事件、探索抽样、会话超时等）及 360 差异见 [GA4 limits](https://support.google.com/analytics/answer/9267744)。

---

## 14. 测试与验证

- **GTM 预览模式**自动附加 `debug_mode: true` —— 在 GA4 **DebugView** 中查看事件的最简单方式。
- `debug_mode` 只标记调试显示；事件仍会进入生产报告，除非过滤。启用**开发者流量数据过滤器**排除。
- 用 GTM Tag Assistant + DevTools Network 交叉检查。用无痕模式验证避免缓存。电商：确认事件名、唯一的 `transaction_id`、参数语法。
- 标准报告延迟 24–48 小时。见 [DebugView](https://support.google.com/analytics/answer/7201382)。

---

## 15. 初始设置检查清单

1. 创建 GA4 资源和 Web 数据流；通过 GTM 在 **Initialization 触发器**上安装 Google 标签。
2. 数据保留设为 **14 个月**；逐个检查增强型衡量开关。
3. 定义内部流量 IP 并创建过滤器（「Testing」→ 验证 → 「Active」）；启用开发者流量过滤器。
4. 实施推荐事件；把业务关键的标记为关键事件。
5. 配置跨域（如需）、User-ID（如有登录）、同意模式（尤其欧盟）。
6. 为推送的自定义参数注册自定义维度/指标。
7. 用 DebugView + Realtime 端到端验证，并记录衡量方案（事件、参数、触发器、注册状态）。
