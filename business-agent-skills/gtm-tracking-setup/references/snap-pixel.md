# Snap Pixel - GTM 实施手册

> 真实来源（Source of truth）：[Snap Conversions API — Introduction](https://developers.snap.com/api/marketing-api/Conversions-API/Introduction)——Getting Started、Parameters、Pixel integration、Offline Events、Verify Setup 和 Best Practices 文档的入口。

---

## 1. 事件与参数参考

### 标准事件

Snap 事件名称为大写蛇形命名（uppercase, snake_case）。Pixel 和 Conversions API 使用相同的名称。

| # | 事件名称 | 用例 |
|---|---|---|
| 1 | `PAGE_VIEW` | 页面访问基事件（每页触发） |
| 2 | `VIEW_CONTENT` | 商品 / 内容详情浏览 |
| 3 | `LIST_VIEW` | 类目 / 列表页浏览 |
| 4 | `SEARCH` | 站内搜索 |
| 5 | `ADD_CART` | 加入购物车 |
| 6 | `ADD_TO_WISHLIST` | 心愿单 / 收藏 |
| 7 | `START_CHECKOUT` | 开始结算 |
| 8 | `ADD_BILLING` | 账单 / 支付信息已添加 |
| 9 | `PURCHASE` | 购买完成（`currency` + `value` 必填） |
| 10 | `SIGN_UP` | 账号 / 服务注册 |
| 11 | `SUBSCRIBE` | 订阅行为 |
| 12 | `AD_CLICK` | 广告点击事件 |
| 13 | `AD_VIEW` | 广告展示事件 |
| 14 | `COMPLETE_TUTORIAL` | 教程完成 |
| 15 | `LEVEL_COMPLETE` | 游戏 / 应用关卡完成 |
| 16 | `INVITE` | 邀请已发送 |
| 17 | `LOGIN` | 用户登录 |
| 18 | `SHARE` | 分享行为 |
| 19 | `RESERVE` | 预约 |
| 20 | `ACHIEVEMENT_UNLOCKED` | 成就解锁 |
| 21 | `SPENT_CREDITS` | 积分消费 |
| 22 | `RATE` | 评分提交 |
| 23 | `START_TRIAL` | 试用开始 |
| 24 | `APP_INSTALL` | 应用安装 |
| 25 | `APP_OPEN` | 应用打开 |
| 26 | `SAVE` | 保存 / 收藏 |
| 27 | `CUSTOM_EVENT_1` – `CUSTOM_EVENT_5` | 业务专属的自定义槽位 |

> **没有 `LEAD` 标准事件。** 将 GA4 的 `generate_lead` 映射到 `SIGN_UP` 或某个 `CUSTOM_EVENT_*` 槽位。官方服务端 GTM 模板将继承的 GA4 `generate_lead` 映射到 `SIGN_UP`。

### Pixel 事件参数（浏览器端）

| 参数 | 类型 | 说明 |
|---|---|---|
| `currency` | String | ISO 4217 货币代码。 |
| `price` | Number | Pixel 风格负载中的订单 / 事件金额。 |
| `transaction_id` | String | 订单 ID；同时作为购买去重键。 |
| `item_ids` | Array[String] | 商品 / 内容 ID（动态广告必须与目录 feed 匹配）。 |
| `item_category` | String | 商品 / 内容类目。 |
| `number_items` | Integer | 商品总数量。 |
| `description` | String | 页面 / 商品描述。 |
| `search_string` | String | 搜索词（用于 `SEARCH`）。 |
| `sign_up_method` | String | 注册方式（用于 `SIGN_UP`）。 |
| `success` | Boolean | 流程类事件的成功标记。 |
| `payment_info_available` | Boolean | 账单步骤标记。 |
| `client_dedup_id` | String | Pixel 端去重 ID（非购买事件）。 |
| `level` | String | 游戏关卡（应用 / 游戏）。 |

### CAPI `custom_data` 字段

| 字段 | 说明 |
|---|---|
| `currency` | ISO 4217 货币代码。 |
| `value` | 数值型金额。 |
| `contents` | 商品对象数组（`id`、`quantity`、`item_price`、`delivery_category`）。 |
| `content_ids` | 商品 ID 数组（`contents` 的替代）。 |
| `content_category` | 商品类目。 |
| `num_items` | 商品总数量。 |
| `order_id` | 购买事件的订单 ID。 |
| `search_string` | 搜索词。 |

---

## 2. GTM 配置

### 前置条件

- 拥有 Events Manager 访问权限的 Snapchat Ads 账号。
- Snap Pixel ID（UUID 格式：`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`）。
- 每个页面都安装了 GTM 网页容器。
- 如使用 CAPI，需服务端 GTM 容器或后端集成。
- 已为目标地区定义同意设计（最低要求 `ad_storage`）。
- 如投放动态广告，商品详情页（PDP）、购物车和购买页需提供商品目录（Catalog）商品 ID。

### 安装方式

Snap 维护两个官方社区模板库（Community Template Gallery）模板：

| 模板 | 代码仓库 | 容器 |
|---|---|---|
| **Snap Pixel** | `Snapchat/snapchat-google-tag-manager` | Web GTM |
| **Snap ConversionAPI ServerSide** | `Snapchat/capi-google-tag-manager-serverside-tag` | Server-side GTM |

Snap Business Help 的 GTM 指南建议使用官方模板而非 Custom HTML。

### 网页模板关键字段

| 字段 | 说明 |
|---|---|
| **Pixel ID** | Snap Pixel ID（常量变量）。按 UUID 校验。 |
| **Event Type** | 标准事件名称下拉列表。 |
| **User Email / Hashed Email** | 高级匹配（Advanced Match）（SDK 在发送前对原始值做 SHA-256 哈希）。 |
| **Phone / Hashed Phone** | 高级匹配。 |
| **First Name / Last Name** | 高级匹配。 |
| **Mobile Ad ID / Hashed Mobile Ad ID** | 应用场景高级匹配。 |
| **Price / Currency / Number of Items** | 电商参数。 |
| **Item IDs / Item Category** | 商品元数据。 |
| **Transaction ID** | 订单 ID（购买去重键）。 |
| **Description / Level / Search String / Signup Method** | 事件专属参数。 |
| **Success / Payment Info Available** | 流程标记。 |
| **Client Deduplication ID** | 非购买的 Pixel 去重 ID。 |
| **CAPI Gateway Script URL** | 可选：通过 Conversions API Gateway 实现第一方服务。 |
| **Additional Initialization Data** | 自由格式的初始化负载。 |
| **Enable Console Logging (Debug Mode)** | 将 Pixel 调用输出到浏览器控制台。 |

### 推荐的网页标签

| 标签名称 | 模板 | 触发器 | 同意状态 |
|---|---|---|---|
| Snap - Pixel - PageView | Snap Pixel | All Pages | `ad_storage` |
| Snap - Pixel - ViewContent | Snap Pixel | CE - `view_item` | `ad_storage` |
| Snap - Pixel - ListView | Snap Pixel | CE - `view_item_list` | `ad_storage` |
| Snap - Pixel - Search | Snap Pixel | CE - `search` | `ad_storage` |
| Snap - Pixel - AddCart | Snap Pixel | CE - `add_to_cart` | `ad_storage` |
| Snap - Pixel - AddToWishlist | Snap Pixel | CE - `add_to_wishlist` | `ad_storage` |
| Snap - Pixel - StartCheckout | Snap Pixel | CE - `begin_checkout` | `ad_storage` |
| Snap - Pixel - AddBilling | Snap Pixel | CE - `add_payment_info` | `ad_storage` |
| Snap - Pixel - Purchase | Snap Pixel | CE - `purchase` | `ad_storage` |
| Snap - Pixel - SignUp | Snap Pixel | CE - `sign_up` | `ad_storage` |
| Snap - Pixel - Subscribe | Snap Pixel | CE - `subscribe` | `ad_storage` |

### 分事件参数映射

| 事件 | 映射 |
|---|---|
| **Purchase** | `transaction_id` → `{{DLV - ecommerce.transaction_id}}`，`price` → `{{DLV - ecommerce.value}}`，`currency` → `{{DLV - ecommerce.currency}}`，`item_ids` → `{{CJS - Snap Item IDs}}`，`number_items` → `{{CJS - Snap Number Items}}` |
| **AddCart** | `client_dedup_id` → `{{DLV - event_id}}`，`price` → `{{DLV - ecommerce.value}}`，`currency` → `{{DLV - ecommerce.currency}}`，`item_ids` → `{{CJS - Snap Item IDs}}` |
| **ViewContent** | `client_dedup_id` → `{{DLV - event_id}}`，`item_ids` → `{{CJS - Snap Item IDs}}`，`item_category` → category |
| **StartCheckout** | `client_dedup_id` → `{{DLV - event_id}}`，`price`，`currency`，`number_items`，`item_ids` |
| **Search** | `search_string` → `{{DLV - search_term}}` |
| **SignUp** | `client_dedup_id` → `{{DLV - form.submission_id}}`，`sign_up_method` → method |

### 变量

**常量（Constant）**：

| 变量 | 值 |
|---|---|
| `Const - Snap Pixel ID` | （Snap Pixel ID） |

**数据层（Data Layer）**：

| 变量 | 路径 |
|---|---|
| `DLV - ecommerce` | `ecommerce` |
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - ecommerce.items` | `ecommerce.items` |
| `DLV - event_id` | `event_id` |
| `DLV - snap_sc_click_id` | `snap_sc_click_id` |
| `DLV - snap_sc_cookie1` | `snap_sc_cookie1` |
| `DLV - form.submission_id` | `form.submission_id` |

**自定义 JS（Custom JS）**（GA4 items → Snap 字段）：

```javascript
// Variable name: CJS - Snap Item IDs
function() {
  var ecommerce = {{DLV - ecommerce}};
  var items = ecommerce && ecommerce.items;
  if (!items || !items.length) return undefined;
  return items.map(function(item) {
    return item.item_id || item.id || item.sku;
  }).filter(Boolean);
}
```

```javascript
// Variable name: CJS - Snap Number Items
function() {
  var ecommerce = {{DLV - ecommerce}};
  var items = ecommerce && ecommerce.items;
  if (!items || !items.length) return undefined;
  return items.reduce(function(total, item) {
    var quantity = Number(item.quantity || 1);
    return total + (isNaN(quantity) ? 1 : quantity);
  }, 0);
}
```

```javascript
// Variable name: CJS - Snap CAPI Contents
function() {
  var ecommerce = {{DLV - ecommerce}};
  var items = ecommerce && ecommerce.items;
  if (!items || !items.length) return undefined;
  return items.map(function(item) {
    var product = { id: item.item_id || item.id || item.sku };
    if (item.quantity != null) product.quantity = String(item.quantity);
    if (item.price != null) product.item_price = String(item.price);
    if (item.delivery_category) product.delivery_category = item.delivery_category;
    return product;
  }).filter(function(p) { return !!p.id; });
}
```

```javascript
// Variable name: CJS - Snap Event ID
function() {
  var ecommerce = {{DLV - ecommerce}};
  if (ecommerce && ecommerce.transaction_id) return ecommerce.transaction_id;
  var eventId = {{DLV - event_id}};
  if (eventId) return eventId;
  return 'snap-' + Date.now() + '-' + Math.random().toString(36).slice(2);
}
```

### 触发器

| 触发器 | 类型 | 条件 |
|---|---|---|
| All Pages | Page View | 所有页面 |
| CE - purchase | 自定义事件（Custom Event） | `purchase` |
| CE - add_to_cart | 自定义事件（Custom Event） | `add_to_cart` |
| CE - view_item | 自定义事件（Custom Event） | `view_item` |
| CE - view_item_list | 自定义事件（Custom Event） | `view_item_list` |
| CE - begin_checkout | 自定义事件（Custom Event） | `begin_checkout` |
| CE - add_payment_info | 自定义事件（Custom Event） | `add_payment_info` |
| CE - search | 自定义事件（Custom Event） | `search` |
| CE - sign_up | 自定义事件（Custom Event） | `sign_up` |
| CE - subscribe | 自定义事件（Custom Event） | `subscribe` |

> 自定义事件名称区分大小写，必须与 dataLayer 的 `event` 值完全一致。

---

## 3. Event ID 与去重（Deduplication）

Snap 对 48 小时窗口内具有相同 `event_id` 和时间戳的事件去重。为每个用户行为生成一个稳定的 ID，并通过两种路径发送相同的 ID。购买事件：将 CAPI 的 `event_id` 映射到 Pixel 的 `transaction_id`。非购买事件：将 CAPI 的 `event_id` 映射到 Pixel 的 `client_dedup_id`。

| 事件 | Pixel 去重字段 | CAPI 字段 | 推荐取值 |
|---|---|---|---|
| Purchase | `transaction_id` | `event_id` | 订单 ID |
| AddCart | `client_dedup_id` | `event_id` | 生成的加购行为 ID |
| ViewContent | `client_dedup_id` | `event_id` | 生成的内容浏览 ID |
| StartCheckout | `client_dedup_id` | `event_id` | 生成的结算 ID |
| SignUp | `client_dedup_id` | `event_id` | 表单提交 ID |

---

## 4. Conversions API（CAPI v3）

Snap 推荐的模式是 **Pixel + CAPI** 并通过 `event_id` 去重。使用官方 **Snap ConversionAPI ServerSide** GTM 模板，配合来自 Ads Manager > Business Details > Conversions API Tokens 的长期访问令牌（需要 Organization Admin 权限）。令牌仅存储在服务端——切勿嵌入网页容器。CAPI v2 已于 2025 年初弃用；新实现必须使用 v3。终端（endpoint）、schema、哈希与限流详情请参阅官方 Snap CAPI 文档。

---

## 5. 高级匹配（Advanced Match）/ 用户数据（User Data）

Snap CAPI 要求每个事件至少提供一个匹配键（match key）：`em`（哈希后的邮箱）、`ph`（哈希后的电话）、`client_ip_address` + `client_user_agent` 组合，或 `madid`（移动广告 ID，仅应用事件）。官方服务端 GTM 模板在值尚未经过 SHA-256 时自动对需哈希的用户数据字段做哈希，并自动处理 `_scid` / `_scclid` Cookie 捕获。

当用户点击 Snapchat 广告时，Snap 会在目标 URL 后追加 `ScCid`——将其持久化到第一方 Cookie，并在 CAPI 事件中作为 `user_data.sc_click_id` 传递。Pixel 处于活动状态时，将 `_scid` Cookie 作为 `user_data.sc_cookie1` 传递。完整字段列表与归一化规则请参阅官方 Snap CAPI Parameters 文档。

---

## 6. 隐私与同意（Privacy and Consent）

将所有 Snap Pixel 和 CAPI 标签置于 `ad_storage`（或等效的广告同意信号）之后。生成 `em`、`ph`、`fn`、`ln`、地址片段、`external_id`、`sc_click_id` 或 `sc_cookie1` 的变量，在同意被拒绝时应返回 `undefined`。CAPI 令牌和哈希逻辑保留在服务端。切勿将 PII 放入 URL、事件名称或自定义参数键中。

针对美国州级隐私和 iOS 14.5+ ATT opt-out，将 CAPI 的 `data_processing_options` 设为 `["LMU"]`。仅当项目有来自 CMP 或后端的明确隐私状态信号时才启用。

---

## 7. dataLayer 映射（GA4 兼容）

使用 GA4 风格的 ecommerce；仅在 GA4 未提供稳定值的地方补充 Snap 专属字段。

### 购买（Purchase）

```javascript
dataLayer.push({
  event: 'purchase',
  ecommerce: {
    transaction_id: 'ORDER-1001',
    currency: 'USD',
    value: 129.99,
    items: [
      { item_id: 'SKU-123', item_name: 'Example Product',
        item_category: 'Shoes', item_brand: 'Example Brand',
        price: 129.99, quantity: 1 }
    ]
  },
  snap_sc_click_id: '<optional stored ScCid>',
  snap_sc_cookie1: '<optional _scid cookie value>'
});
```

### 加入购物车（Add to Cart）

```javascript
dataLayer.push({
  event: 'add_to_cart',
  snap_event_id: 'cart-abc123',
  ecommerce: {
    currency: 'USD', value: 49.99,
    items: [{ item_id: 'SKU-123', item_name: 'Example Product',
              price: 49.99, quantity: 1 }]
  }
});
```

> 切勿将原始邮箱、电话、姓名或地址内容推入 dataLayer 供广告像素使用。对匹配标识符做哈希处理，并置于同意之后。

### GA4 → Snap 事件映射

| GA4 事件 | Snap 事件 |
|---|---|
| `page_view` | `PAGE_VIEW` |
| `view_item` | `VIEW_CONTENT` |
| `view_item_list` | `LIST_VIEW` |
| `search` | `SEARCH` |
| `add_to_cart` | `ADD_CART` |
| `add_to_wishlist` | `ADD_TO_WISHLIST` |
| `begin_checkout` | `START_CHECKOUT` |
| `add_payment_info` | `ADD_BILLING` |
| `purchase` | `PURCHASE` |
| `sign_up` | `SIGN_UP` |
| `subscribe` | `SUBSCRIBE` |
| `start_trial` | `START_TRIAL` |
| `login` | `LOGIN` |
| `share` | `SHARE` |
| `generate_lead` | `SIGN_UP` 或 `CUSTOM_EVENT_*`（没有 `LEAD` 标准事件） |

---

## 8. 调试（Debugging）

| 工具 | 用途 |
|---|---|
| GTM 预览模式（Preview Mode） | 标签触发顺序、变量、同意状态、事件数据。 |
| Snap Pixel Helper（Chrome） | Pixel 检测、已触发事件、参数。 |
| 测试事件（Test Events）/ CAPI 校验终端 | 实时检查 Pixel 和 CAPI 事件；`/v3/{asset_id}/events/validate` 对格式错误的事件返回 400 并附详情。 |

---

## 9. 最佳实践与常见陷阱

| 陷阱 | 影响 | 预防措施 |
|---|---|---|
| 将 `LEAD` 当作 Snap 标准事件 | CAPI 拒绝未知事件名称 | 使用 `SIGN_UP` 或某个 `CUSTOM_EVENT_*` 槽位。 |
| 向 CAPI 发送小写 / GA4 风格的事件名称 | 事件被丢弃或回退为自定义 | 使用大写的 Snap 名称（`PURCHASE`、`ADD_CART`……）。 |
| 购买事件缺失 `currency` 或 `value` | `PURCHASE` 被拒绝 | 购买事件务必同时发送两者。 |
| 浏览器 Pixel 与 CAPI 使用不同的去重 ID | 转化被重复计数 | Pixel 使用 `transaction_id`（购买）和 `client_dedup_id`（非购买）；CAPI 使用 `event_id`。共用同一个值。 |
| 将 CAPI 令牌嵌入网页容器 | 令牌泄露 | 令牌仅存储在服务端 GTM 或后端。 |
