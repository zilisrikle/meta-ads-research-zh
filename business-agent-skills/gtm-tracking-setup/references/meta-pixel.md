# Meta Pixel - GTM 实施手册

> 真实信息来源：[Meta Pixel 开发者文档](https://developers.facebook.com/docs/meta-pixel/)——标准事件（Standard Events）、高级匹配（Advanced Matching）、转化 API（CAPI）与同意文档的入口。

---

## 1. 事件与参数参考（Event and Parameter Reference）

### 标准事件（Standard Events）

| # | 事件名称 | 适用场景 | 必需参数 |
|---|-----------|-------|-------------|
| 1 | **PageView** | 所有页面（模板中作为基础标签在每个页面触发） | 无（None） |
| 2 | **ViewContent** | 商品详情、内容浏览 | 推荐：`content_ids`、`value`、`currency` |
| 3 | **Search** | 站内搜索 | 推荐：`search_string` |
| 4 | **AddToCart** | 加入购物车 | 推荐：`content_ids`、`value`、`currency` |
| 5 | **AddToWishlist** | 加入心愿单 | 无（None） |
| 6 | **InitiateCheckout** | 开始结账 | 推荐：`value`、`currency`、`num_items` |
| 7 | **AddPaymentInfo** | 输入支付信息 | 无（None） |
| 8 | **Purchase** | 购买完成 | **`value`（必填）、`currency`（必填）**（用于优化、ROAS 衡量与 DPA。缺失值会导致事件无法用于优化。） |
| 9 | **Lead** | 线索获取 | 推荐：`value`、`currency` |
| 10 | **CompleteRegistration** | 注册完成 | 无（None） |
| 11 | **Contact** | 咨询 / 联系 | 无（None） |
| 12 | **CustomizeProduct** | 商品定制 | 无（None） |
| 13 | **Donate** | 捐赠 | 无（None） |
| 14 | **FindLocation** | 门店 / 地点搜索 | 无（None） |
| 15 | **Schedule** | 预约 / 订座 | 无（None） |
| 16 | **StartTrial** | 免费试用开始 | 推荐：`value`、`currency` |
| 17 | **SubmitApplication** | 申请提交 | 无（None） |
| 18 | **Subscribe** | 付费订阅开始 | 推荐：`value`、`currency` |

### 事件参数（Event Parameters）

| 参数 | 类型 | 说明 |
|-----------|------|------|
| `value` | 浮点数（Float） | 货币金额（**必须以数字发送；不允许字符串**。始终与 `currency` 配对。） |
| `currency` | 字符串（String） | ISO 4217 货币代码（例如 `'USD'`） |
| `content_name` | 字符串（String） | 商品或内容名称 |
| `content_category` | 字符串（String） | 类别 |
| `content_ids` | 数组[字符串]（Array[String]） | 商品 ID 数组（必须与目录商品 ID 一致；DPA 必需） |
| `content_type` | 字符串（String） | `'product'`（按 SKU）或 `'product_group'`（含变体的组） |
| `contents` | 数组[对象]（Array[Object]） | 商品详情对象数组（见下表） |
| `num_items` | 整数（Integer） | 商品数量 |
| `predicted_ltv` | 浮点数（Float） | 预测 LTV（用于 Subscribe 与 StartTrial） |
| `search_string` | 字符串（String） | 搜索关键词（用于 Search 事件） |
| `status` | 布尔值（Boolean） | 注册状态等（`true` / `false`） |

> **eventID**：在**选项对象（第 4 个参数）**中指定的唯一标识符，而非事件参数（第 3 个参数）：`fbq('track', 'Purchase', {params}, {eventID: '...'})`。与转化 API（CAPI）联用时，去重必填。在 GTM 模板中通过 "Event ID" 字段设置。

### contents 数组（contents array）

| 参数 | 类型 | 说明 |
|-----------|------|------|
| `id` | 字符串（String） | 商品 ID（必须与目录 ID 一致；DPA 必需） |
| `quantity` | 整数（Integer） | 数量 |
| `item_price` | 浮点数（Float） | 单价 |

---

## 2. GTM 配置（GTM Configuration）

### 前置条件（Prerequisites）

- 已在 Meta 事件管理工具（Meta Events Manager）中获取 **Pixel ID**
- 使用 GTM 社区模板（Community Template）"**Facebook Pixel**"（模板名称在模板库中可能变化）
- 由于模板内部处理 `fbevents.js` 的加载与初始化（`fbq('init', ...)`），**不需要自定义 HTML 基础代码**

### 关键模板配置选项（Key Template Configuration Options）

| 设置 | 说明 |
|---|---|
| **Pixel ID** | Meta Pixel ID（多个 ID 可用逗号分隔） |
| **事件名称（Event Name）** | 标准事件名称或自定义事件名称（也可使用变量） |
| **对象属性（Object Properties）** | 事件参数（表格或 JS 变量） |
| **事件 ID（Event ID）** | 去重用唯一标识符（与转化 API（CAPI）联用时必填） |
| **高级匹配（Advanced Matching）** | 启用用户数据（电子邮箱、电话等）的发送 |
| **同意已授予（Consent Granted）** | 为 `false` 时，只加载 SDK 并停止发送数据 |
| **增强型电商（Enhanced Ecommerce）** | 自动映射 DataLayer 的 ecommerce 对象 |
| **禁用自动配置（Disable Automatic Configuration）** | 禁用按钮点击与元数据的自动采集 |

### 标签（Tags）

| 标签名称 | 模板 | 触发器 | 同意 |
|--------|------------|---------|------|
| Meta Pixel - PageView | Facebook Pixel | 所有页面（All Pages） | ad_storage |
| Meta Pixel - Purchase | Facebook Pixel | CE - purchase | ad_storage |
| Meta Pixel - Lead | Facebook Pixel | CE - generate_lead | ad_storage |
| Meta Pixel - CompleteRegistration | Facebook Pixel | CE - sign_up | ad_storage |
| Meta Pixel - AddToCart | Facebook Pixel | CE - add_to_cart | ad_storage |
| Meta Pixel - ViewContent | Facebook Pixel | CE - view_item | ad_storage |
| Meta Pixel - InitiateCheckout | Facebook Pixel | CE - begin_checkout | ad_storage |

为所有事件标签配置**标签排序（tag sequencing）**：先触发 `Meta Pixel - PageView` 作为兜底，确保 Pixel 可靠初始化。

### 按事件参数映射（Per-Event Parameter Mapping）

| 事件 | 映射 |
|---------|----------|
| **Purchase** | value -> `{{DLV - ecommerce.value}}`、currency -> `{{DLV - ecommerce.currency}}`、事件 ID（Event ID）-> `{{DLV - ecommerce.transaction_id}}`、content_ids -> `{{cjs - Meta Pixel Content IDs}}`、content_type -> `product`、contents -> `{{cjs - Meta Pixel Contents}}` |
| **Lead** | 事件 ID（Event ID）-> `{{DLV - form.submission_id}}` |
| **CompleteRegistration** | status -> `true` |
| **AddToCart** | content_ids -> `{{cjs - Meta Pixel Content IDs}}`、content_type -> `product`、contents -> `{{cjs - Meta Pixel Contents}}` |
| **ViewContent** | content_ids -> `{{cjs - Meta Pixel Content IDs}}`、content_type -> `product`、contents -> `{{cjs - Meta Pixel Contents}}` |
| **InitiateCheckout** | content_ids -> `{{cjs - Meta Pixel Content IDs}}`、content_type -> `product`、contents -> `{{cjs - Meta Pixel Contents}}`、value -> `{{DLV - ecommerce.value}}`、currency -> `{{DLV - ecommerce.currency}}` |

### 变量（Variables）

**常量变量（Constant variables）**：

| 变量名称 | 值 |
|--------|---|
| `Meta Pixel ID` | （Pixel ID） |

**数据层变量（Data Layer variables）**：

| 变量名称 | 数据层变量名称 |
|--------|-------------------|
| `DLV - ecommerce` | `ecommerce` |
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - form.submission_id` | `form.submission_id` |

**自定义 JS 变量（Custom JS variables）**（GA4 items -> Meta Pixel 转换）：

```javascript
// Variable name: cjs - Meta Pixel Contents
function() {
  var ecommerce = {{DLV - ecommerce}};
  if (!ecommerce || !ecommerce.items) return [];
  return ecommerce.items.map(function(item) {
    return {
      id: item.item_id || '',
      quantity: item.quantity || 1,
      item_price: Number(item.price || 0)
    };
  }).filter(function(x) { return x.id; });
}
```

```javascript
// Variable name: cjs - Meta Pixel Content IDs
function() {
  var ecommerce = {{DLV - ecommerce}};
  if (!ecommerce || !ecommerce.items) return [];
  return ecommerce.items.map(function(item) {
    return item.item_id || '';
  }).filter(Boolean);
}
```

### 触发器（Triggers）

| 触发器名称 | 类型 | 条件 |
|-----------|------|------|
| All Pages | 页面浏览（Page View） | 所有页面（All pages） |
| CE - purchase | 自定义事件（Custom Event） | `purchase` |
| CE - generate_lead | 自定义事件（Custom Event） | `generate_lead` |
| CE - sign_up | 自定义事件（Custom Event） | `sign_up` |
| CE - add_to_cart | 自定义事件（Custom Event） | `add_to_cart` |
| CE - view_item | 自定义事件（Custom Event） | `view_item` |
| CE - begin_checkout | 自定义事件（Custom Event） | `begin_checkout` |

> 自定义事件名称**区分大小写**，必须与 dataLayer 的 `event` 值完全一致。

---

## 3. Meta Pixel 特定注意事项（Meta Pixel-Specific Considerations）

### eventID 与去重（eventID and Deduplication）

去重在约 48 小时内匹配**事件名称（event_name）** + **eventID**。使用**稳定 ID**（例如订单 ID），并从 Pixel（`eventID`，驼峰命名，选项参数）与转化 API（CAPI）（`event_id`，下划线命名，JSON 字段）发送相同的值。DataLayer 字段名任意（例如 `meta_event_id`）；各层名称不同，但值必须一致。

### AEM（聚合事件衡量（Aggregated Event Measurement））

iOS 14.5+ ATT 合规协议。规范频繁变化——以当前事件管理工具（Events Manager）界面为准，确认是否需要 AEM 标签页（AEM tab）、事件优先级（event prioritization）与域名验证（domain verification）。参见 [docs](https://developers.facebook.com/docs/marketing-api/aggregated-event-measurement)。

### 域名验证（Domain Verification）

在商务管理平台（Business Manager）> 商务设置（Business Settings）> 品牌安全（Brand Safety）> 域名（Domains）下配置：DNS TXT（推荐）或 HTML meta 标签。参见 [docs](https://developers.facebook.com/docs/sharing/domain-verification)。

### 同意管理（Consent Management）

- 在每个 Meta Pixel 标签上添加 `ad_storage` 作为额外同意检查（Additional Consent Check）。在 `ad_storage: granted` 前标签不会触发。
- 模板的 "Consent Granted" 设置与 Meta 同意模式（Meta Consent Mode）集成。
- 严格 GDPR 选择同意（opt-in）时，在获得同意前不要加载 `fbevents.js` 本身。

### 高级匹配（Advanced Matching）

发送哈希处理后的用户数据（电子邮箱、电话等）以提高匹配率。在 GTM 模板的**高级匹配（Advanced Matching）**部分配置。Pixel 自动应用 SHA-256 哈希。完整参数列表（em、ph、fn、ln、ge、db、ct、st、zp、country、external_id）参见 [docs](https://developers.facebook.com/docs/meta-pixel/advanced/advanced-matching)。

> **PII 警告**：同意（consent）前不要将原始电子邮箱/电话推入 DataLayer（其他标签可能造成泄露风险）。**强烈建议使用服务端转化 API（CAPI）发送。**

### EMQ（事件匹配质量（Event Match Quality））

事件管理工具（Events Manager）中按事件评分 0-10（每 48 小时更新）。目标 8+。电子邮箱是影响最大的标识符；电话、`_fbp`、`_fbc` 与 `external_id` 也是高优先级。参见 [docs](https://www.facebook.com/business/help/765081237991954)。

### 自定义事件与自定义转化（Custom Events and Custom Conversions）

`fbq('trackCustom', 'EventName', {...})` 用于自定义事件；优化时**优先使用标准事件**。自定义转化（Custom conversions）是事件管理工具（Events Manager）中定义的基于规则（URL 或事件筛选），无需改动代码。

### 单页应用（SPA）环境（SPA Environments）

初始加载时加载 `fbevents.js` 一次。路由变化事件（如 ViewContent、额外 PageView）通过 GTM 历史记录变化（History Change）触发器触发。

### Cookie（Cookies）

| Cookie 名称 | 生命周期 | 用途 |
|-----------|---------|------|
| `_fbp` | 约 3 个月（~3 months） | 第一方浏览器标识符 |
| `_fbc` | 约 3 个月（~3 months） | 存储广告点击的 `fbclid` |

第一方 Cookie；在限制第三方 Cookie 的环境中仍有效。使用转化 API（CAPI）时，将 `_fbp`/`_fbc` 转发到服务端以提高匹配质量。

### 动态广告（Dynamic Ads (DPA)）/ 目录（Catalog）

ViewContent、AddToCart、Purchase 必需，全部带 `content_ids`（匹配目录）与 `content_type`；Purchase 还需要 `value`/`currency`。参见 [docs](https://www.facebook.com/business/help/1275400645914358)。

---

## 4. 转化 API（Conversions API / CAPI）

使用 GTM 服务端模板（sGTM）"**Facebook Conversions API**"将服务端事件与 Pixel 并行发送，通过 `event_id` 去重。将客户端事件转发到服务端（例如通过 GA4 客户端（GA4 Client）），并映射到使用与 Pixel 相同 `eventID` 值的 CAPI 标签。参见 [Conversions API 文档](https://developers.facebook.com/docs/marketing-api/conversions-api)。

---

## 5. 调试（Debugging）

| 工具 | 检查项 |
|--------|---------|
| **GTM 预览模式（GTM Preview Mode）** | 标签触发顺序、变量值 |
| **Meta Pixel Helper** | Pixel 事件 / 参数 / 错误（仅浏览器） |
| **测试事件（Test Events）**（事件管理工具（Events Manager）） | Pixel + CAPI 实时接收 |
