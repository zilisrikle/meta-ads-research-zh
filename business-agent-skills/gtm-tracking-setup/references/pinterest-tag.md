# Pinterest Tag - GTM 部署手册

> 权威来源：[Install the Pinterest tag (Pinterest Help Center)](https://help.pinterest.com/en/business/article/install-the-pinterest-tag) —— 基础代码部署、事件代码和 Pinterest Conversions API 的入口，附相关链接。

---

## 1. 事件与参数参考

### 标准事件（Standard Events）

Pinterest Help 列出了 20 种事件类型。`pintrk('track', '<name>', {...})` 的值是小写。

| # | 事件名 | `pintrk` 值 | 适用场景 |
|---|---|---|---|
| 1 | Checkout | `checkout` | 完成交易 / 购买 |
| 2 | AddToCart | `addtocart` | 商品加入购物车 |
| 3 | PageVisit | `pagevisit` | 主要页面浏览、商品页、文章页 |
| 4 | Signup | `signup` | 用户注册 |
| 5 | WatchVideo | `watchvideo` | 视频观看 |
| 6 | Lead | `lead` | 线索或意向行为 |
| 7 | Search | `search` | 站内搜索 |
| 8 | ViewCategory | `viewcategory` | 类目 / 列表页浏览 |
| 9 | AddPaymentInfo | `addpaymentinfo` | 添加支付信息 |
| 10 | AddToWishList | `addtowishlist` | 加入心愿单 |
| 11 | InitiateCheckout | `initiatecheckout` | 结账开始 |
| 12 | Subscribe | `subscribe` | 付费订阅行为 |
| 13 | ViewContent | `viewcontent` | 网页 / 商品 / 落地页浏览 |
| 14 | Contact | `contact` | 通过电话、邮箱、聊天等方式联系 |
| 15 | Schedule | `schedule` | 预约已安排 |
| 16 | FindLocation | `findlocation` | 门店 / 地点查找 |
| 17 | CustomizeProduct | `customizeproduct` | 商品定制 |
| 18 | SubmitApplication | `submitapplication` | 申请已提交 |
| 19 | StartTrial | `starttrial` | 免费试用开始 |
| 20 | Custom | （自定义名称） | 业务特定行为 |

> Pinterest 使用 **Checkout**（而非 Purchase）作为标准购买事件。GA4 的 `purchase` → Pinterest 的 `checkout`。

### 事件参数

| 参数 | 类型 | 描述 |
|---|---|---|
| `value` | Number | 订单 / 事件总金额。用于付费和自然转化报告。 |
| `currency` | String | ISO 货币代码（如 `USD`、`EUR`、`JPY`）。 |
| `order_id` | String | 转化分析报告必填。 |
| `order_quantity` | Integer | 商品数量。用于转化报告。 |
| `event_id` | String | 去重用的唯一事件标识符。 |
| `promo_code` | String | 促销码。 |
| `property` | String | 资产 / 品牌上下文。 |
| `search_query` | String | 搜索词（用于 `search`）。避免敏感搜索内容。 |
| `video_title` | String | 视频标题（用于 `watchvideo`）。 |
| `lead_type` | String | 线索类别（用于 `lead`）。 |
| `line_items` | Array | 商品详情对象数组（见下）。 |

Pinterest 接受以下任意大小写敏感的字段名作为事件 ID：`eventID`、`event_id`、`eid`。新部署统一使用 `event_id`。

### `line_items` Schema

| 字段 | 类型 | 描述 |
|---|---|---|
| `product_id` | String | 商品 ID。动态再营销时**必须与目录 feed 一致**。 |
| `product_name` | String | 商品名称。 |
| `product_category` | String | 商品类目。 |
| `product_variant_id` | String | 变体 ID。 |
| `product_variant` | String | 变体名称 / 值。 |
| `product_price` | Number | 单价。 |
| `product_quantity` | Integer | 数量。 |
| `product_brand` | String | 品牌。 |

单个商品时，`addtocart`、`checkout`、`pagevisit` 的 `product_id` 可以直接放在顶层，不必用 `line_items`。

---

## 2. GTM 配置

### 前置条件

- 拥有 Conversions 权限的 Pinterest Business 账户。
- Pinterest **Tag ID**（Pinterest Ads Manager > Conversions 中的 10 位数字 ID）。
- 每个页面都安装了 GTM 容器。
- 已定义目标区域的同意设计（至少 `ad_storage`）。
- 如跑动态再营销，PDP、购物车、购买页上有目录商品 ID。

### 安装

GTM 内置 **Pinterest Tag** 模板。优先用模板而非 Custom HTML——模板内部处理加载器和 `pintrk('page')` 调用。

### 关键模板字段

| 字段 | 描述 |
|---|---|
| **Tag ID** | Pinterest Tag ID（常量变量）。 |
| **Event to Fire** | 标准事件名（`pagevisit`、`checkout` 等）或自定义。 |
| **Event Data** / **Custom Parameters** | 事件参数（`value`、`currency`、`order_id`、`line_items` 等）。 |
| **Event ID** | 唯一的去重 ID。 |
| **Hashed Email** | 增强匹配（Enhanced Match）字段。 |
| **Opt Out Information** | 有限数据处理（Limited Data Processing）标志。 |

### 标签

| 标签名 | 模板 | 触发器 | 同意 |
|---|---|---|---|
| Pinterest - Base | Pinterest Tag | All Pages | `ad_storage` |
| Pinterest - Checkout | Pinterest Tag | CE - purchase | `ad_storage` |
| Pinterest - AddToCart | Pinterest Tag | CE - add_to_cart | `ad_storage` |
| Pinterest - PageVisit | Pinterest Tag | CE - view_item (PDP) | `ad_storage` |
| Pinterest - ViewCategory | Pinterest Tag | CE - view_item_list | `ad_storage` |
| Pinterest - InitiateCheckout | Pinterest Tag | CE - begin_checkout | `ad_storage` |
| Pinterest - Search | Pinterest Tag | CE - search | `ad_storage` |
| Pinterest - Lead | Pinterest Tag | CE - generate_lead | `ad_storage` |
| Pinterest - Signup | Pinterest Tag | CE - sign_up | `ad_storage` |

每个事件标签都配置**标签排序（Tag Sequencing）**：先触发 `Pinterest - Base`。

### 各事件参数映射

| 事件 | 映射 |
|---|---|
| **Checkout** | `event_id` → `{{DLV - ecommerce.transaction_id}}`，`order_id` → 同上，`value` → `{{DLV - ecommerce.value}}`，`currency` → `{{DLV - ecommerce.currency}}`，`order_quantity` → `{{CJS - Pinterest Order Quantity}}`，`line_items` → `{{CJS - Pinterest Line Items}}` |
| **AddToCart** | `event_id` → `{{DLV - event_id}}`，`value` → `{{DLV - ecommerce.value}}`，`currency` → `{{DLV - ecommerce.currency}}`，`line_items` → `{{CJS - Pinterest Line Items}}` |
| **PageVisit** | `line_items` → `{{CJS - Pinterest Line Items}}`（PDP 上） |
| **InitiateCheckout** | `value`、`currency`、`line_items` |
| **Search** | `search_query` → `{{DLV - search_term}}` |
| **Lead** | `event_id` → `{{DLV - form.submission_id}}`，`lead_type` → `{{DLV - form.type}}` |

### 变量

**常量（Constant）**：

| 变量 | 值 |
|---|---|
| `Const - Pinterest Tag ID` | （Pinterest Tag ID） |

**Data Layer**：

| 变量 | Data Layer 路径 |
|---|---|
| `DLV - ecommerce` | `ecommerce` |
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - ecommerce.items` | `ecommerce.items` |
| `DLV - event_id` | `event_id` |
| `DLV - form.submission_id` | `form.submission_id` |
| `DLV - form.type` | `form.type` |

**Custom JS**（GA4 items → Pinterest `line_items`）：

```javascript
// Variable name: CJS - Pinterest Line Items
function() {
  var ecommerce = {{DLV - ecommerce}};
  var items = ecommerce && ecommerce.items;
  if (!items || !items.length) return [];
  return items.map(function(item) {
    return {
      product_id: item.item_id || '',
      product_name: item.item_name || '',
      product_category: item.item_category || '',
      product_variant: item.item_variant || '',
      product_price: Number(item.price || 0),
      product_quantity: item.quantity || 1,
      product_brand: item.item_brand || ''
    };
  }).filter(function(x) { return x.product_id; });
}
```

```javascript
// Variable name: CJS - Pinterest Order Quantity
function() {
  var ecommerce = {{DLV - ecommerce}};
  var items = ecommerce && ecommerce.items;
  if (!items || !items.length) return 0;
  return items.reduce(function(sum, item) {
    return sum + (Number(item.quantity) || 0);
  }, 0);
}
```

### 触发器

| 触发器 | 类型 | 条件 |
|---|---|---|
| All Pages | Page View | All pages |
| CE - purchase | Custom Event | `purchase` |
| CE - add_to_cart | Custom Event | `add_to_cart` |
| CE - view_item | Custom Event | `view_item` |
| CE - view_item_list | Custom Event | `view_item_list` |
| CE - begin_checkout | Custom Event | `begin_checkout` |
| CE - search | Custom Event | `search` |
| CE - generate_lead | Custom Event | `generate_lead` |
| CE - sign_up | Custom Event | `sign_up` |

> 自定义事件名大小写敏感，必须与 dataLayer 的 `event` 值完全一致。

---

## 3. 事件 ID 与去重（Event ID and Deduplication）

`event_id` 是去重键。Pinterest 在 Pinterest Tag 和 Conversions API 两侧匹配相同的 event ID 来抑制重复。每个用户动作为其生成一个稳定的 `event_id`，同一事件的浏览器和服务端两条路径发送相同的值。购买事件中，如果 `order_id` 唯一且稳定，可以复用为 `event_id`。

---

## 4. Conversions API

Pinterest 的 CAPI 以服务器到服务器的方式发送转化，支持服务端 GTM 双发（Pixel + CAPI），以 `event_id` 去重。使用**服务端 GTM Pinterest CAPI 标签模板**，access token 在 Pinterest Ads Manager > Conversions > Set Up API 中生成。token 只保留在服务端。端点、载荷 schema 和哈希规则见官方 Pinterest Conversions 文档。

---

## 5. 增强匹配（Enhanced Match）

增强匹配（Enhanced Match）在标签加载时传递哈希化邮箱，以改善跨设备匹配。GTM 中，用同意后才填充的 data layer 变量设置 Pinterest Tag 模板的 **Hashed Email** 字段。Automatic Enhanced Match 会自动检测表单字段并提交哈希化客户数据——受监管站点请在 Ads Manager 中关闭，依赖同意控制的 Manual Enhanced Match。字段级归一化和哈希细节见官方 Pinterest 文档。

---

## 6. 隐私与同意（Privacy and Consent）

Pinterest 基础标签和所有事件标签都用 `ad_storage` 门控。严格 opt-in 区域，获得同意前完全屏蔽基础标签——加载它会拉取 Pinterest 脚本并设置标识符。生成哈希化标识符的变量在同意被拒绝时必须返回 `undefined`。

美国州隐私合规方面，GTM 模板暴露了 Limited Data Processing 字段（Opt Out Type = `LDP`，外加哈希化的州和国家）。只有项目有明确的隐私状态信号时才启用 LDP——不要在 GTM 中推断位置。

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
      { item_id: 'SKU001', item_name: 'Parker Boots', item_category: 'Shoes',
        item_brand: 'Parker', price: 99.99, quantity: 1 }
    ]
  }
});
```

| Pinterest 字段 | 值 |
|---|---|
| 事件 | `checkout` |
| `event_id` | `{{DLV - ecommerce.transaction_id}}` |
| `order_id` | `{{DLV - ecommerce.transaction_id}}` |
| `value` | `{{DLV - ecommerce.value}}` |
| `currency` | `{{DLV - ecommerce.currency}}` |
| `order_quantity` | `{{CJS - Pinterest Order Quantity}}` |
| `line_items` | `{{CJS - Pinterest Line Items}}` |

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

| Pinterest 字段 | 值 |
|---|---|
| 事件 | `lead` |
| `event_id` | `{{DLV - form.submission_id}}` |
| `lead_type` | `{{DLV - form.type}}` |

> 切勿将原始邮箱、电话、姓名或自由格式的消息内容推送到给广告像素用的 dataLayer。匹配标识符要哈希化，并用同意门控。

### GA4 → Pinterest 事件映射

| GA4 事件 | Pinterest 事件 |
|---|---|
| `purchase` | Checkout |
| `add_to_cart` | AddToCart |
| `view_item` | PageVisit（PDP）或 ViewContent |
| `view_item_list` | ViewCategory |
| `begin_checkout` | InitiateCheckout |
| `add_payment_info` | AddPaymentInfo |
| `add_to_wishlist` | AddToWishList |
| `search` | Search |
| `sign_up` | Signup |
| `generate_lead` | Lead |
| `subscribe` | Subscribe |
| `contact` | Contact |
| `schedule` | Schedule |
| `start_trial` | StartTrial |

---

## 8. 调试

| 工具 | 用途 |
|---|---|
| GTM Preview Mode | 验证标签触发顺序（Base → Event）、变量、事件数据。 |
| Pinterest Tag Helper（Chrome） | 像素检测、事件载荷、Enhanced Match 值。 |
| Test Events（Ads Manager > Conversions） | Tag 和 CAPI 的实时检查（`?test=true`）。 |

---

## 9. 最佳实践与常见陷阱

| 陷阱 | 影响 | 预防 |
|---|---|---|
| 事件标签在基础标签之前触发 | 事件丢失或归因失败 | 标签排序（Tag Sequencing）：每个事件标签先触发 `Pinterest - Base`。 |
| 把 `purchase` 映射到自定义事件 | 失去基于 Checkout 的优化信号 | GA4 `purchase` 始终映射到 Pinterest `checkout`。 |
| `product_id` 与目录不一致 | 动态再营销和目录销售失效 | 使用目录 feed 的精确 ID。 |
| Tag + CAPI 双发缺少 `event_id` | 重复计数、转化虚高 | 每个动作一个稳定 ID，两条路径都发送。 |
| 事件参数中含原始 PII | 隐私违规 | 切勿发送邮箱、电话、姓名、地址或消息内容作为事件数据。 |
