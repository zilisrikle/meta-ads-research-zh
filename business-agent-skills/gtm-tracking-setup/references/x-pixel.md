# X Pixel - GTM 实施手册

> 真实来源（Source of truth）：[X for Business Help Center](https://business.x.com/en/help)——Pixel、Conversion API 和广告系列衡量（campaign measurement）文档的入口。

---

## 1. 事件与参数参考

### 标准事件

| 事件名称 | 示例用例 |
|-----------|-------|
| **Page View** | 所有页面（Base Code；在 Events Manager 中创建事件以用作转化） |
| **Purchase** | 购买完成 |
| **Lead** | 咨询完成、资料索取 |
| **Sign Up** | 账号创建（在 Events Manager 中验证） |
| **Add to Cart** | 加入购物车 |
| **Add to Wishlist** | 加入心愿单 |
| **Checkout Initiated** | 进入结算页 |
| **Content View** | 商品详情页 |
| **Added Payment Info** | 支付信息已填写 |
| **Search** | 站内搜索 |
| **Subscribe** | 订阅 / 邮件订阅注册 |
| **Start Trial** | 免费试用注册 |
| **Download** | 文件 / 应用下载 |
| **Product Customization** | 商品选项选择 |
| **Custom** | 其他行为 |

### 事件参数

| 参数 | 类型 | 说明 |
|-----------|------|------|
| `value` | Number | 转化价值 |
| `currency` | String | ISO 4217 货币代码（如 `'USD'`） |
| `conversion_id` | String | 唯一去重 ID（如订单 ID） |
| `search_string` | String | 站内搜索词 |
| `description` | String | 附加事件描述 |
| `status` | String | `'started'` 或 `'completed'` |
| `contents` | Array | 商品详情数组（见下文） |
| `num_items` | Number | 顶层商品总数量 |
| `email_address` | String | 邮箱（X 会应用 SHA256。**推荐使用 CAPI**） |
| `phone_number` | String | E.164 格式的电话（**推荐使用 CAPI**） |
| `restricted_data_use` | String | `'restrict_optimization'` 或 `'off'` |

### contents 数组

| 参数 | 类型 | 说明 |
|-----------|------|------|
| `content_type` | String | 商品类目（Google taxonomy） |
| `content_id` | String | SKU 或 GTIN |
| `content_name` | String | 商品名称 |
| `content_price` | Number | 单价 |
| `num_items` | Number | 数量 |
| `content_group_id` | String | 变体组 ID |

---

## 2. GTM 配置

### 前置条件

- 来自 X Ads Events Manager 的 **Pixel ID** 和每个事件的 **Event ID**（`tw-XXXXX-YYYYY`）
- 使用 GTM 社区模板 **Twitter Base Pixel** 和 **Twitter Event Pixel**

### 标签

| 标签名称 | 模板 | 触发器 | 同意状态 |
|--------|------------|---------|------|
| X Pixel - Base | Twitter Base Pixel | All Pages | ad_storage |
| X Pixel - Purchase | Twitter Event Pixel | CE - purchase | ad_storage |
| X Pixel - Lead | Twitter Event Pixel | CE - generate_lead | ad_storage |
| X Pixel - SignUp | Twitter Event Pixel | CE - sign_up | ad_storage |
| X Pixel - Add to Cart | Twitter Event Pixel | CE - add_to_cart | ad_storage |
| X Pixel - Content View | Twitter Event Pixel | CE - view_item | ad_storage |

为所有事件标签配置**标签排序（tag sequencing）**：先触发 `X Pixel - Base`。

### 分事件参数映射

| 事件 | 映射 |
|---------|----------|
| **Purchase** | value → `{{DLV - ecommerce.value}}`，currency → `{{DLV - ecommerce.currency}}`，conversion_id → `{{DLV - ecommerce.transaction_id}}`，contents → `{{cjs - X Pixel Contents}}` |
| **Lead** | conversion_id → `{{DLV - form.submission_id}}` |
| **SignUp** | status → `completed` |
| **Add to Cart** | contents → `{{cjs - X Pixel Contents}}` |
| **Content View** | contents → `{{cjs - X Pixel Contents}}` |

> **购买前事件（Pre-purchase events）**：对于 Add to Cart / Checkout Initiated，**不要映射** `value` / `currency` / `conversion_id`（防止意外的收入归因）。仅发送 `contents`。

### 变量

**常量变量（Constant variables）**：

| 变量名称 | 值 |
|--------|---|
| `X Pixel ID` | （Pixel ID） |
| `X Event ID - Purchase` | （Event ID） |
| `X Event ID - Lead` | （Event ID） |
| `X Event ID - SignUp` | （Event ID） |
| `X Event ID - AddToCart` | （Event ID） |
| `X Event ID - ContentView` | （Event ID） |

**数据层变量（Data Layer Variables）**：

| 变量名称 | 数据层变量名 |
|--------|-------------------|
| `DLV - ecommerce` | `ecommerce` |
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - ecommerce.items` | `ecommerce.items` |
| `DLV - form.submission_id` | `form.submission_id` |

**自定义 JS 变量（Custom JS Variable）**（GA4 items → X Pixel contents）：

```javascript
// Variable name: cjs - X Pixel Contents
function() {
  var ecommerce = {{DLV - ecommerce}};
  if (!ecommerce || !ecommerce.items) return [];
  return ecommerce.items.map(function(item) {
    return {
      content_id: item.item_id || '',
      content_name: item.item_name || '',
      content_price: item.price || 0,
      num_items: item.quantity || 1,
      content_type: item.item_category || 'product'
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

> 自定义事件名称**区分大小写**。必须与 dataLayer 的 `event` 值完全一致。

---

## 3. X Pixel 专属注意事项

### conversion_id 与去重（Deduplication）

使用**稳定 ID**（如订单 ID；不要用 `Date.now()` 或随机值）。只有 Page View 类事件会被 X 自动去重（~30 分钟）。对于 Purchase/Lead 等，`conversion_id` 是 **Pixel 与 CAPI 之间的主要去重机制**——从两者发送相同的值。

### 同意管理（Consent Management）

没有原生同意模式。在所有 X Pixel 标签上添加 `ad_storage` 作为附加同意检查；直到 `ad_storage: granted` 才触发。

### PII（邮箱 / 电话）

客户端 dataLayer 提交有泄露给其他标签的风险。**强烈推荐通过 CAPI 走服务端。**

### SPA 环境

Base Code 在初始加载时运行一次。通过 GTM History Change 触发器触发路由变化事件（Content View 等）。

### DPA 要求

需要 Page View / Content View / Add to Cart / Purchase。所有事件都需要 `contents`；Purchase 另需 `value` / `currency`。仅限实物商品。

---

## 4. Conversion API（CAPI）

使用服务端自定义标签（Stape 或自托管 sGTM）与 Pixel 并行发送服务端事件，通过 `conversion_id` 去重。认证使用来自 X Developer Portal 的 OAuth 1.0a（CAPI 访问可能需要 X 销售团队批准）。不支持实时测试模式（12–24 小时生效）。参见 [Conversion API 参考（Conversion API reference）](https://developer.x.com/en/docs/x-ads-api/measurement/web-conversions/conversion-api)。

---

## 5. 调试（Debugging）

| 工具 | 验证内容 |
|--------|---------|
| **GTM 预览模式（Preview Mode）** | 标签触发顺序、变量取值 |
| **X Pixel Helper** | Pixel 检测、参数、错误 |
| **Events Manager** | 状态（最长 24 小时生效） |
