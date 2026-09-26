# TikTok Pixel - GTM 实施手册

> 真实来源（Source of truth）：[TikTok for Business Developers Portal](https://business-api.tiktok.com/portal/docs)——Pixel、Events API 和高级匹配（Advanced Matching）文档的入口。

---

## 1. 事件与参数参考

### 标准事件

> **PageView**：独立于标准事件之外。通过 Base Code 中的 `ttq.page()` 自动触发（由基标签处理，All Pages 触发器）。

| 事件名称 | 示例用例 |
|-----------|-------|
| **ViewContent** | 商品详情页、文章页 |
| **Search** | 站内搜索 |
| **AddToCart** | 加入购物车 |
| **AddToWishlist** | 加入心愿单 |
| **InitiateCheckout** | 进入结算页 |
| **AddPaymentInfo** | 支付信息填写完成 |
| **Purchase** | 购买完成（旧名称：CompletePayment / PlaceAnOrder） |
| **Lead** | 线索获取 / 表单提交（旧名称：SubmitForm） |
| **CompleteRegistration** | 注册完成 |
| **Contact** | 咨询（含电话点击） |
| **Download** | 文件下载 |
| **Subscribe** | 订阅 / 邮件订阅注册 |
| **StartTrial** | 试用开始 |
| **Schedule** | 预约确认 |
| **SubmitApplication** | 申请表单提交 |
| **ApplicationApproval** | 申请获批 |
| **FindLocation** | 门店搜索 |
| **CustomizeProduct** | 商品定制 |

> **事件名称迁移**：CompletePayment→Purchase、SubmitForm→Lead（旧名称仍会自动转换）。ClickButton、PlaceAnOrder 为软弃用（soft-deprecated）至 2027 年。新实现请使用新名称。
> **Login**：在作为标准网页事件使用前，请在当前 TikTok Ads Manager / 开发者文档中确认；它不在当前公开的支持标准事件表中。

### 事件参数

| 参数 | 类型 | 说明 |
|-----------|------|------|
| `value` | Number | 订单总金额（务必与 `currency` 一同发送。ROAS / VBO 必需） |
| `currency` | String | ISO 4217 货币代码（如 `'USD'`） |
| `content_id` | String | 商品 ID（必须与目录 SKU 匹配；DPA 必需） |
| `content_ids` | Array | 多个商品 ID |
| `content_type` | String | `'product'` 或 `'product_group'`（DPA 必需） |
| `content_name` | String | 商品 / 页面名称 |
| `content_category` | String | 商品类目 |
| `contents` | Array | 商品详情数组（见下文） |
| `price` | Number | 单价（`value` = 总价，`price` = 单价） |
| `quantity` | Integer | 数量 |
| `search_string` | String | 站内搜索词 |
| `description` | String | 附加描述 |
| `status` | String | 状态 |
| `order_id` | String | 订单 ID |
| `num_items` | Integer | 商品数量 |

> **event_id**：位于**配置对象（configuration object，第三个参数）**中的唯一标识符，不在事件参数中。与 Events API 配合使用时去重必需。通过 GTM 模板的专用字段设置。

### contents 数组

| 参数 | 类型 | 说明 |
|-----------|------|------|
| `content_id` | String | 商品 ID（必须与目录 SKU 匹配，区分大小写） |
| `content_type` | String | `'product'` 或 `'product_group'` |
| `content_name` | String | 商品名称 |
| `content_category` | String | 类目 |
| `price` | Number | 单价 |
| `quantity` | Integer | 数量 |
| `brand` | String | 品牌名称 |

---

## 2. GTM 配置

### 前置条件

- 从 TikTok Ads Manager > Events Manager 获取的 **Pixel ID**
- **推荐**：从 Events Manager > Settings > Partner Platform > Google Tag Manager 自动设置
- **手动**：将 Base Code（`ttq.load()` + `ttq.page()`）放入 Custom HTML 标签，并使用 GTM 社区模板 "**TikTok Pixel**" 处理事件

### 标签

| 标签名称 | 模板 | 触发器 | 同意状态 |
|--------|------------|---------|------|
| TikTok Pixel - Base | Custom HTML（Base Code）或自动设置 | All Pages | ad_storage |
| TikTok Pixel - Purchase | TikTok Pixel | CE - purchase | ad_storage |
| TikTok Pixel - Lead | TikTok Pixel | CE - generate_lead | ad_storage |
| TikTok Pixel - CompleteRegistration | TikTok Pixel | CE - sign_up | ad_storage |
| TikTok Pixel - AddToCart | TikTok Pixel | CE - add_to_cart | ad_storage |
| TikTok Pixel - ViewContent | TikTok Pixel | CE - view_item | ad_storage |
| TikTok Pixel - InitiateCheckout | TikTok Pixel | CE - begin_checkout | ad_storage |

为所有事件标签配置**标签排序（tag sequencing）**：先触发 `TikTok Pixel - Base`。

### 分事件参数映射

| 事件 | 映射 |
|---------|----------|
| **Purchase** | value → `{{DLV - ecommerce.value}}`，currency → `{{DLV - ecommerce.currency}}`，event_id → `{{DLV - ecommerce.transaction_id}}`，contents → `{{cjs - TikTok Pixel Contents}}` |
| **Lead** | event_id → `{{DLV - form.submission_id}}` |
| **CompleteRegistration** | status → `completed` |
| **AddToCart** | contents → `{{cjs - TikTok Pixel Contents}}` |
| **ViewContent** | contents → `{{cjs - TikTok Pixel Contents}}` |
| **InitiateCheckout** | contents → `{{cjs - TikTok Pixel Contents}}`，value → `{{DLV - ecommerce.value}}`，currency → `{{DLV - ecommerce.currency}}` |

### 变量

**常量变量（Constant Variables）**：

| 变量名称 | 值 |
|--------|---|
| `TikTok Pixel ID` | （Pixel ID） |

**数据层变量（Data Layer Variables）**：

| 变量名称 | 数据层变量名 |
|--------|-------------------|
| `DLV - ecommerce` | `ecommerce` |
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - ecommerce.items` | `ecommerce.items` |
| `DLV - form.submission_id` | `form.submission_id` |

**自定义 JavaScript 变量（Custom JavaScript Variable）**（GA4 items → TikTok contents）：

```javascript
// Variable name: cjs - TikTok Pixel Contents
function() {
  var ecommerce = {{DLV - ecommerce}};
  if (!ecommerce || !ecommerce.items) return [];
  return ecommerce.items.map(function(item) {
    return {
      content_id: item.item_id || '',
      content_type: 'product',
      content_name: item.item_name || '',
      content_category: item.item_category || '',
      price: item.price || 0,
      quantity: item.quantity || 1
    };
  });
}
```

### 触发器

| 触发器名称 | 类型 | 条件 |
|-----------|------|------|
| All Pages | Page View | 所有页面 |
| CE - purchase | 自定义事件（Custom Event） | `purchase` |
| CE - generate_lead | 自定义事件（Custom Event） | `generate_lead` |
| CE - sign_up | 自定义事件（Custom Event） | `sign_up` |
| CE - add_to_cart | 自定义事件（Custom Event） | `add_to_cart` |
| CE - view_item | 自定义事件（Custom Event） | `view_item` |
| CE - begin_checkout | 自定义事件（Custom Event） | `begin_checkout` |

> 自定义事件名称**区分大小写**。必须与 dataLayer 的 `event` 值完全一致。

---

## 3. TikTok Pixel 专属注意事项

### event_id 与去重（Deduplication）

去重依据 ~48 小时内的 **event_source_id**（Pixel ID）+ **event** + **event_id**。使用**稳定 ID**（如订单 ID），并从 Pixel 和 Events API 发送**相同的 `event_id`**。

### 同意管理（Consent Management）

TikTok 没有原生同意模式（consent mode）。使用 GTM 同意设置：
- 在所有 TikTok 标签（含 Base）上添加 `ad_storage` 作为附加同意检查
- 对于严格的 GDPR opt-in，在获得同意前阻止 Base 标签触发（因为 `ttq.load()` 会拉取脚本并设置 Cookie）

### 高级匹配（Advanced Matching，PII）

哈希后的 PII（email、phone、external_id）通过 `ttq.identify()` 或 GTM 模板的专用字段发送。哈希参数名称使用 `sha256_*` 前缀（如 `sha256_email`、`sha256_phone_number`、`sha256_external_id`）。参见[高级匹配文档（Advanced Matching docs）](https://business-api.tiktok.com/portal/docs?id=1739585702090754)。

> **PII 警告**：客户端原始 PII 有泄露给其他标签的风险。发送预哈希的值，或**使用服务端 Events API**。

### EMQ（事件匹配质量，Event Match Quality）

Events Manager 中的评分 0–10。目标 6.0+。通过哈希邮箱 + 电话 + external_id + ttclid + _ttp + Events API 提升。

### 自定义事件（Custom Events）

用于标准事件之外的追踪（仅用于报表）。**优化请使用标准事件。**

### SPA 环境

SPA 内 URL 变化的自动检测因环境而异。若不可靠，通过 GTM History Change 触发器触发路由变化事件（如 ViewContent）。**注意防止重复触发。**

### 目录（Catalog）/ DPA 要求

需要 ViewContent、AddToCart、Purchase。全部需要 `content_id`（与目录 SKU 匹配，区分大小写）+ `content_type`。Purchase 另需 `value` + `currency`。在 `contents` 之外同时包含顶层 `content_ids` / `content_type` 可提升购物广告兼容性。

### Cookie

Pixel 设置 `_ttp` 第一方 Cookie 并从 URL 捕获 `?ttclid=`。在转化时将两者都转发给服务端 Events API，以提升匹配质量。

---

## 4. Events API

使用 GTM 服务端模板 [tiktok/gtm-template-eapi](https://github.com/tiktok/gtm-template-eapi)（sGTM），与 Pixel 并行发送服务端事件，通过 `event_id` 去重。认证使用来自 Events Manager 的 Access Token。参见 [Events API 文档（Events API docs）](https://business-api.tiktok.com/portal/docs?id=1741601162187777)。

---

## 5. 调试（Debugging）

| 工具 | 验证内容 |
|--------|---------|
| **GTM 预览模式（Preview Mode）** | 标签触发顺序、变量取值 |
| **TikTok Pixel Helper** | Pixel 检测、参数、错误 |
| **测试事件（Test Events）**（Events Manager） | 实时事件接收（Events API 使用 `test_event_code`） |
