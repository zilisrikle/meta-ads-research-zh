# Reddit Pixel - GTM 实施手册

> 真实来源（Source of truth）：[Reddit for Business Help Center](https://business.reddithelp.com/)——Pixel、Conversions API 和受众（Audiences）文档的入口。

---

## 1. 事件与参数参考

### 标准事件

官方 Reddit Pixel GTM 模板提供九种可选事件类型。浏览器端取值为 PascalCase；Reddit Conversions API 将每个事件映射为大写蛇形命名（upper-snake-case）的 `tracking_type`。

| # | 事件名称 | `tracking_type`（CAPI） | 用例 |
|---|---|---|---|
| 1 | PageVisit | `PAGE_VISIT` | 所有页面（基代码 / 全站标签） |
| 2 | ViewContent | `VIEW_CONTENT` | 商品详情或内容浏览 |
| 3 | Search | `SEARCH` | 站内搜索 |
| 4 | AddToCart | `ADD_TO_CART` | 商品加入购物车 |
| 5 | AddToWishlist | `ADD_TO_WISHLIST` | 商品加入心愿单 |
| 6 | Purchase | `PURCHASE` | 购买 / 订单完成 |
| 7 | Lead | `LEAD` | 线索获取 |
| 8 | SignUp | `SIGN_UP` | 注册 / 订阅 / 报名 |
| 9 | Custom | `CUSTOM` | 业务专属行为（需要 `customEventName`） |

Reddit 接受无限数量的自定义转化事件，但 Events Manager 中仅显示最近的 20 个自定义事件。只要行为能清晰对应，就使用标准事件。

### 事件参数

| 参数 | 类型 | 说明 |
|---|---|---|
| `currency` | String | ISO 4217 货币代码。适用于 AddToCart、AddToWishlist、Purchase、SignUp、Lead、Custom。 |
| `transactionValue` | Number | 以账号货币计的金额。与 `currency` 适用的事件相同。 |
| `itemCount` | Integer | 商品总数量。适用于 AddToCart、AddToWishlist、Purchase、Custom。 |
| `transactionId` | String | 订单 / 交易 ID。适用于 Purchase、SignUp、Lead、Custom。 |
| `conversionId` | String | 事件的唯一去重 ID（浏览器 + CAPI）。适用于所有事件。 |
| `products` | Array | 商品对象数组（schema 见下文）。 |

> **Conversion ID 是去重键（deduplication key）。** 对同一用户行为，通过 Reddit Pixel 和 Conversions API 发送相同的值。

### `products` 数组 schema

| 字段 | 类型 | 是否必填 | 说明 |
|---|---|---|---|
| `id` | String | 必填 | 目录商品 ID。动态商品广告（Dynamic Product Ads）**必须与目录 feed 匹配**。 |
| `category` | String | 必填 | 商品类目（如 Google product taxonomy）。 |
| `name` | String | 可选 | 商品展示名称。 |

启用模板的 "Use Ecommerce Product Data" 模式后，它会自动将 GA4 ecommerce items（`item_id`、`item_name`、`item_category` / `item_category2…5` 以 `Cat1 > Cat2 > …` 拼接）映射为 Reddit 商品对象，并将 `quantity` 加总为 `item_count`。

---

## 2. GTM 配置

### 前置条件

- 拥有 Events Manager 访问权限的 Reddit Ads 账号。
- 来自 Events Manager 的 Reddit **Pixel ID**。格式：`t2_xxxxx` 或 `a2_xxxxx`。
- 每个页面都安装了 GTM 网页容器（web container）。
- CAPI 需要：服务端 GTM 容器（server-side GTM container）和来自 Reddit Ads 的 Conversion Access Token（转化访问令牌）。
- 已为目标地区定义同意机制（Consent）设计（最低要求 `ad_storage`）。
- 如投放动态商品广告（Dynamic Product Ads），商品详情页（PDP）、购物车和购买页需提供商品目录（Catalog）商品 ID。

### 官方 GTM 模板

Reddit 在 `reddit` 所有者名下的 **GTM Community Template Gallery（GTM 社区模板库）** 中发布并维护这两个模板。

| 模板 | 容器类型 |
|---|---|
| **Reddit Pixel** | Web |
| **Reddit Conversions API** | Server |

优先使用这些模板而非 Custom HTML——它们处理加载器注入（loader injection）、初始化、Cookie 以及当前字段 schema。

### 网页模板关键字段

| 字段 | 说明 |
|---|---|
| **Pixel ID** | Reddit Pixel ID。根据 `(t2_|a2_)[a-z0-9]+` 校验。 |
| **Event to Fire** | `PageVisit`、`ViewContent`、`Search`、`AddToCart`、`AddToWishlist`、`Purchase`、`Lead`、`SignUp` 或 `Custom`。 |
| **CustomEventName** | 当 Event to Fire = `Custom` 时必填。 |
| **Currency** | ISO 4217（收入事件）。 |
| **Transaction Value** | 十进制数值（收入事件）。 |
| **Item Count** | 商品总数量。 |
| **Transaction ID** | 订单 / 交易 ID（purchase、signup、lead、custom）。 |
| **Conversion ID** | 唯一去重 ID。 |
| **Enable First Party Cookies** | `_rdt_uuid` 和点击 ID（click ID）存储所需。 |
| **Enable Advanced Matching** | 开关：Email、Phone、AAID、IDFA、External ID。 |
| **Add Data Processing Options** | 开关：LDU 的 `mode`、`country`、`region`。 |
| **Product Information** | 逐行商品录入或 JSON 负载（支持从 GA4 自动映射）。 |

### 服务端模板关键字段

| 字段 | 说明 |
|---|---|
| **Pixel ID** | Reddit Pixel ID。 |
| **Source of Event** | `WEBSITE`、`APP`、`PHYSICAL_STORE`、`OTHER` 之一。 |
| **Event to Fire** | 与网页模板相同的九个取值。 |
| **Conversion ID** | 去重 ID。 |
| **Test ID** | 测试模式标识符。**生产环境中禁用。** |
| **Conversion Access Token** | 在 Reddit Ads 中生成的 Bearer 令牌。 |
| **Advanced Matching Parameters** | `email`、`phone_number`、`aaid`、`idfa`、`externalId`。 |
| **Limited Data Usage Options** | `country`（ISO 3166-1 alpha-2）和 `region`。 |
| **Product Information** | 手动录入行或 JSON；回退使用 GA4 ecommerce items。 |

### 标签

| 标签名称 | 模板 | 触发器 | 同意状态 |
|---|---|---|---|
| Reddit - Pixel - Page Visit | Reddit Pixel | All Pages | `ad_storage` |
| Reddit - Pixel - View Content | Reddit Pixel | CE - view_item | `ad_storage` |
| Reddit - Pixel - Search | Reddit Pixel | CE - search | `ad_storage` |
| Reddit - Pixel - Add To Cart | Reddit Pixel | CE - add_to_cart | `ad_storage` |
| Reddit - Pixel - Add To Wishlist | Reddit Pixel | CE - add_to_wishlist | `ad_storage` |
| Reddit - Pixel - Purchase | Reddit Pixel | CE - purchase | `ad_storage` |
| Reddit - Pixel - Lead | Reddit Pixel | CE - generate_lead | `ad_storage` |
| Reddit - Pixel - Sign Up | Reddit Pixel | CE - sign_up | `ad_storage` |

Reddit 网页模板自身处理加载器注入，因此不需要单独的 Custom HTML 基代码。

### 分事件参数映射

| 事件 | 映射 |
|---|---|
| **Purchase** | Currency → `{{DLV - ecommerce.currency}}`，Transaction Value → `{{DLV - ecommerce.value}}`，Item Count → `{{CJS - Reddit Item Count}}`，Transaction ID → `{{DLV - ecommerce.transaction_id}}`，Conversion ID → `{{DLV - ecommerce.transaction_id}}`，Products → `{{CJS - Reddit Products}}` |
| **AddToCart** | Currency → `{{DLV - ecommerce.currency}}`，Transaction Value → `{{DLV - ecommerce.value}}`，Item Count → `{{CJS - Reddit Item Count}}`，Conversion ID → `{{DLV - event_id}}`，Products → `{{CJS - Reddit Products}}` |
| **ViewContent** | Conversion ID → `{{DLV - event_id}}`，Products → `{{CJS - Reddit Products}}` |
| **Search** | Conversion ID → `{{DLV - event_id}}`（避免发送敏感搜索词） |
| **AddToWishlist** | Currency → `{{DLV - ecommerce.currency}}`，Transaction Value → `{{DLV - ecommerce.value}}`，Conversion ID → `{{DLV - event_id}}` |
| **Lead** | Conversion ID → `{{DLV - form.submission_id}}` |
| **SignUp** | Conversion ID → `{{DLV - registration_id}}`，Transaction ID → 同上 |

### 变量

**常量（Constant）**：

| 变量 | 值 |
|---|---|
| `Const - Reddit Pixel ID` | （Reddit Pixel ID，如 `t2_abc123`） |

**数据层（Data Layer）**：

| 变量 | 数据层路径 |
|---|---|
| `DLV - ecommerce` | `ecommerce` |
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - ecommerce.items` | `ecommerce.items` |
| `DLV - event_id` | `event_id` |
| `DLV - form.submission_id` | `form.submission_id` |
| `DLV - registration_id` | `registration_id` |

**自定义 JS（Custom JS）**（GA4 items → Reddit `products`）：

```javascript
// Variable name: CJS - Reddit Products
function() {
  var ecommerce = {{DLV - ecommerce}};
  var items = ecommerce && ecommerce.items;
  if (!items || !items.length) return [];
  return items.map(function(item) {
    var cats = ['item_category','item_category2','item_category3','item_category4','item_category5']
      .map(function(k) { return item[k]; })
      .filter(function(v) { return v && String(v).trim(); })
      .join(' > ');
    var product = { id: String(item.item_id || '') };
    if (item.item_name) product.name = String(item.item_name);
    if (cats) product.category = cats;
    return product;
  }).filter(function(p) { return p.id; });
}
```

```javascript
// Variable name: CJS - Reddit Item Count
function() {
  var ecommerce = {{DLV - ecommerce}};
  var items = ecommerce && ecommerce.items;
  if (!items || !items.length) return undefined;
  return items.reduce(function(sum, item) {
    return sum + (Number(item.quantity) || 1);
  }, 0);
}
```

### 触发器

| 触发器 | 类型 | 条件 |
|---|---|---|
| All Pages | Page View | 所有页面 |
| CE - view_item | 自定义事件（Custom Event） | `view_item` |
| CE - search | 自定义事件（Custom Event） | `search` |
| CE - add_to_cart | 自定义事件（Custom Event） | `add_to_cart` |
| CE - add_to_wishlist | 自定义事件（Custom Event） | `add_to_wishlist` |
| CE - purchase | 自定义事件（Custom Event） | `purchase` |
| CE - generate_lead | 自定义事件（Custom Event） | `generate_lead` |
| CE - sign_up | 自定义事件（Custom Event） | `sign_up` |

> 自定义事件名称区分大小写，必须与 dataLayer 的 `event` 值完全一致。

---

## 3. Conversion ID 与去重（Deduplication）

Conversion ID（CAPI 中的 `conversion_id`）是广告主同时通过 Reddit Pixel 和 Conversions API 发送同一事件时的去重键。为每个真实用户行为生成一个稳定的 Conversion ID，并通过两种路径发送相同的值。购买事件如订单 / 交易 ID 唯一且稳定，可直接使用。对于其他事件，在浏览器或服务端触发之前生成 UUID。Reddit 通过 URL 参数 `rdt_cid` 归因广告点击，Pixel 和服务端模板将其持久化到第一方 Cookie `_rdt_cid`（90 天有效期），并在每次转化事件中转发。

---

## 4. Conversions API（Reddit CAPI）

使用官方 **Reddit Conversions API 服务端 GTM 模板**，配合来自 **Reddit Ads → Events Manager → Conversion Access Token** 的 Conversion Access Token。将令牌视为机密——切勿在网页容器或浏览器代码中暴露。该模板每次请求发送一个事件，并支持 Test ID 以便在 Events Manager 中验证。终端（endpoint）、负载 schema 与字段详情请参阅官方 Reddit Business Help Center。

---

## 5. 高级匹配（Advanced Matching）/ 用户数据（User Data）

网页模板支持高级匹配，字段包括 `email`、`phone_number`（或 `phoneNumber`）、`aaid`、`idfa`、`externalId`。服务端模板额外支持 `uuid`、`ip_address`、`user_agent` 和 `screen_dimensions`。所有 PII（email、phone、external_id、aaid、idfa）在传输前必须经过 SHA-256 哈希——Reddit 会将已哈希的值视为已哈希，不再重复哈希。如有条件，优先通过服务端 CAPI 传输标识符，而非浏览器端高级匹配，并将所有高级匹配置于明确同意之后。归一化规则请参阅官方 Reddit 文档。

---

## 6. 隐私与同意（Privacy and Consent）

将 Reddit 基标签和所有事件标签置于 `ad_storage` 之后。对于严格的 opt-in 地区，在获得同意之前完全阻止基标签触发——加载它会拉取 Reddit 脚本并写入 / 读取第一方标识符（`_rdt_uuid`、`_rdt_cid`、`_rdt_em`）。

针对美国州级隐私（LDU），官方模板提供 `mode`、`country`（ISO 3166-1 alpha-2）和 `region`（ISO 3166-2）。仅当项目有来自 CMP 或后端的明确隐私状态信号（privacy-state signal）时才启用 LDU。

---

## 7. dataLayer 映射（GA4 兼容）

### 购买（Purchase）

```javascript
dataLayer.push({
  event: 'purchase',
  ecommerce: {
    transaction_id: 'T12345',
    value: 99.99,
    currency: 'USD',
    items: [
      { item_id: 'SKU001', item_name: 'Parker Boots', item_category: 'Apparel',
        item_category2: 'Shoes', price: 99.99, quantity: 1 }
    ]
  }
});
```

| Reddit 字段 | 值 |
|---|---|
| Event to Fire | `Purchase` |
| Transaction ID | `{{DLV - ecommerce.transaction_id}}` |
| Conversion ID | `{{DLV - ecommerce.transaction_id}}` |
| Transaction Value | `{{DLV - ecommerce.value}}` |
| Currency | `{{DLV - ecommerce.currency}}` |
| Item Count | `{{CJS - Reddit Item Count}}` |
| Products | `{{CJS - Reddit Products}}` |

### 加入购物车（Add to Cart）

```javascript
dataLayer.push({
  event: 'add_to_cart',
  event_id: 'atc_1700000000_abc123',
  ecommerce: {
    value: 49.99,
    currency: 'USD',
    items: [{ item_id: 'SKU001', item_name: 'Parker Boots', price: 49.99, quantity: 1 }]
  }
});
```

### 线索（Lead）

```javascript
dataLayer.push({
  event: 'generate_lead',
  form: { submission_id: 'lead_12345', type: 'contact' }
});
```

| Reddit 字段 | 值 |
|---|---|
| Event to Fire | `Lead` |
| Conversion ID | `{{DLV - form.submission_id}}` |

> 切勿将原始邮箱、电话、姓名、地址或自由格式的消息内容推入 dataLayer 供广告像素使用。对匹配标识符在服务端做哈希处理，并置于同意之后。

### GA4 → Reddit 事件映射

| GA4 事件 | Reddit 事件 |
|---|---|
| `page_view` | PageVisit |
| `view_item` | ViewContent |
| `search` | Search |
| `add_to_cart` | AddToCart |
| `add_to_wishlist` | AddToWishlist |
| `purchase` | Purchase |
| `generate_lead` | Lead |
| `sign_up` | SignUp |

---

## 8. 调试（Debugging）

| 工具 | 用途 |
|---|---|
| GTM 预览模式（Preview Mode） | 标签触发顺序（Page Visit → event）、变量、Conversion ID 取值。 |
| Reddit Pixel Helper（Chrome） | Pixel 检测、事件负载。测试时启用第三方 Cookie。 |
| Events Manager / 事件测试（Event Testing） | 实时事件量、去重覆盖率、带 Test ID 标记的 CAPI 验证。 |

---

## 9. 最佳实践与常见陷阱

| 陷阱 | 影响 | 预防措施 |
|---|---|---|
| Pixel + CAPI 未共用 Conversion ID | 转化重复、报表虚高 | 每个行为一个稳定的 Conversion ID；两种路径发送相同值。 |
| DPA 事件缺失 `products[].id` | DPA / 目录匹配失效 | 在 ViewContent / AddToCart / Purchase 上发送 `id`（及 `category`）；与目录 ID 严格匹配。 |
| Purchase 缺失 currency / value | 优化信号减弱 | 在 Purchase 标签上映射 `ecommerce.value` 和 `ecommerce.currency`。 |
| Conversion Access Token 放在网页容器中 | 凭证泄露 | 仅存储在服务端 GTM 中。 |
| 生产环境启用 Test ID | 事件被限流并排除 | 发布前关闭 Test ID。 |
