# Microsoft Advertising / UET - GTM 部署手册

> 权威来源：[Universal Event Tracking (Microsoft Learn)](https://learn.microsoft.com/en-us/advertising/guides/universal-event-tracking) —— UET 标签部署、Conversion Goals、Audiences 的入口，以及 Microsoft Advertising Help Center 相关部署文档的链接。

---

## 1. 概述

Universal Event Tracking（UET，通用事件跟踪）是 Microsoft Advertising 的跟踪框架。在全站部署一个 UET 标签即可驱动：

- **转化跟踪（Conversion tracking）**：跟踪购买、线索、注册、下载、关键页面浏览、网站停留时长、每次访问页面数。
- **再营销名单 / 受众定向（Remarketing lists / audience targeting）**。
- **自动出价（Automated bidding）** 优化。
- **动态再营销（Dynamic remarketing）**（商品受众），发送零售/垂直参数时启用。
- **Conversions API（CAPI，服务端转化接口）**，使用同一套 UET 数据模型。

**单标签规则**：一个 UET 标签可以与账户下所有的转化目标（conversion goals）和受众配合使用。只有当账户结构、所有权归属或特定的服务端设计要求时，才需要多个 UET 标签。

标准部署模式：

1. 在 Microsoft Advertising 中创建一个 UET 标签。
2. 将 UET 标签加到每个页面（GTM：Base 标签，触发器为 All Pages）。
3. 在界面中创建转化目标（conversion goals）和/或再营销名单（remarketing lists）。
4. 针对动作型目标（action-specific goals），从页面或服务端发送自定义事件（custom events）。

---

## 2. UET 标签部署

### UET Tag ID

UET 标签的数字型 Microsoft Advertising 标识符。它是 GTM Base 标签唯一必填的标识符。在 Microsoft Advertising 界面的 conversion tracking / UET tag 下查找。

### GTM 模板 / Custom HTML 选项

| 选项 | 适用场景 |
|---|---|
| **Microsoft Advertising UET GTM Community Template** | 目标 GTM 容器可安装社区模板时首选 |
| **Custom HTML** | 社区模板无法通过审批时，或跨环境导出 GTM 容器 JSON 时的回退方案（模板 ID 是环境特定的） |
| **Microsoft Advertising in-product GTM integration** | 部分账户配置下可从 UET 标签页面使用 |

使用 Custom HTML 时，标准模式为初始化 `uetq` 队列并加载 UET JavaScript：

```html
<script>
(function(w,d,t,r,u){
  var f,n,i;
  w[u]=w[u]||[],f=function(){
    var o={ti:"{{Const - UET Tag ID}}", enableAutoSpaTracking: true};
    o.q=w[u], w[u]=new UET(o);
  },
  n=d.createElement(t), n.src=r, n.async=1,
  n.onload=n.onreadystatechange=function(){
    var s=this.readyState;
    s&&s!=="loaded"&&s!=="complete"||(f(),n.onload=n.onreadystatechange=null);
  },
  i=d.getElementsByTagName(t)[0], i.parentNode.insertBefore(n,i);
})(window,document,"script","//bat.bing.com/bat.js","uetq");
</script>
```

### 标签

| 标签名 | 类型 | 触发器 | 同意（Consent） |
|---|---|---|---|
| Microsoft Ads - UET Base | Microsoft UET（模板）或 Custom HTML | All Pages | ad_storage |
| Microsoft Ads - Event - purchase | Microsoft UET event | CE - purchase | ad_storage |
| Microsoft Ads - Event - generate_lead | Microsoft UET event | CE - generate_lead | ad_storage |
| Microsoft Ads - Event - sign_up | Microsoft UET event | CE - sign_up | ad_storage |
| Microsoft Ads - Event - add_to_cart | Microsoft UET event | CE - add_to_cart | ad_storage |
| Microsoft Ads - Event - begin_checkout | Microsoft UET event | CE - begin_checkout | ad_storage |
| Microsoft Ads - Event - view_item | Microsoft UET event | CE - view_item | ad_storage |

配置标签排序（tag sequencing），确保 Base 标签在任何事件标签之前触发。`uetq` 数组会在 `bat.js` 加载完成前缓存推送（push），因此排序更多是保险起见。

---

## 3. 转化目标（Conversion Goals）

UET 收集页面加载和自定义事件；目标（goals）对这些数据进行评估。转化目标在 Microsoft Advertising 界面或 Campaign Management API 中定义。

### 目标类型

| 目标类型 | API 对象 | 适用场景 | GTM 要求 |
|---|---|---|---|
| URL / 目标网址（Destination URL） | `UrlGoal` | 感谢页 / 确认页 / 关键页面 | 仅 Base 标签 |
| 事件（Event） | `EventGoal` | 表单提交、购买、下载、自定义交互 | Base 标签 + 自定义事件 |
| 停留时长（Duration） | `DurationGoal` | 网站停留时长阈值 | 仅 Base 标签 |
| 每次访问页面数（Pages viewed per visit） | `PagesViewedPerVisitGoal` | 互动次数 | 仅 Base 标签 |
| 线下转化（Offline conversion） | `OfflineConversionGoal` | 网页点击后线下成交的线索 | UET + msclkid 采集 + 线下上传/API |
| App 安装 | `AppInstallGoal` | App 安装归因 | 专用方案；很少用网页 GTM |
| 线下交易（In-store transaction） | `InStoreTransactionGoal` | 线下门店购买 | 专用方案；很少用网页 GTM |

### `GoalCategory`（EventGoal）——自 2021 年 6 月起必填

支持的枚举值：

```
AddToCart, BeginCheckout, BookAppointment, Contact, GetDirections, Other,
OutboundClick, PageView, Purchase, RequestQuote, Signup, SubmitLeadForm, Subscribe
```

GA4 → Microsoft 映射：

| 站点事件 | `GoalCategory` |
|---|---|
| `purchase` | `Purchase` |
| `generate_lead` / 联系表单 | `SubmitLeadForm` 或 `Contact` |
| `sign_up` | `Signup` |
| `subscribe` | `Subscribe` |
| `add_to_cart` | `AddToCart` |
| `begin_checkout` | `BeginCheckout` |
| 报价请求 | `RequestQuote` |
| 站外链接 | `OutboundClick` |
| `book_appointment` | `BookAppointment` |
| 门店路线 | `GetDirections` |
| 关键页面浏览 | `PageView` |
| 其他 | `Other` |

### 计数方式（Counting，`CountType`）

| 值 | 行为 | 推荐场景 |
|---|---|---|
| `All`（默认） | 每次点击后的每次转化都计数 | 销售、可重复的收入事件 |
| `Unique` | 每次点击只计一次转化 | 线索表单、注册、报价请求 |

### 转化窗口（Conversion Windows）

| 字段 | 范围 | 默认值 |
|---|---|---|
| `ConversionWindowInMinutes`（点击后） | 1 分钟到 129,600 分钟（90 天） | 43,200（30 天） |
| `ViewThroughConversionWindowInMinutes` | 1 分钟到 43,200 分钟（30 天） | 默认不返回 |

展示转化跟踪还要求账户属性 `IncludeViewThroughConversions` 为 true。

### `ExcludeFromBidding`

为 `true` 时，该目标不计入 `Conversions`/`ConversionRate`/`CostPerConversion`/`ReturnOnAdSpend`/`RevenuePerConversion`/`Revenue` 列，也不参与自动出价计算。`All*` 列仍会包含它。用于跟踪微转化（`view_item`、`add_to_cart`）做观察/受众积累，而不污染出价。

### 增强型转化（Enhanced conversions）

每个目标都暴露 `IsEnhancedConversionsEnabled`。启用后，通过 UET 或 CAPI 发送的哈希化第一方标识符（`em`、`ph`）可提高匹配率。当前字段要求和归一化规则见 [Microsoft Advertising Help Center](https://help.ads.microsoft.com/)（Microsoft 的邮箱/电话哈希规则与 Google 和 Meta 不同）。

---

## 4. 自定义事件跟踪（Custom Event Tracking，客户端）

### 语法

```javascript
window.uetq = window.uetq || [];
window.uetq.push('event', 'purchase', {
  event_category: 'ecommerce',
  event_label: 'order_complete',
  event_value: 1,
  revenue_value: 99.97,
  currency: 'USD',
  event_id: 'order-12345'
});
```

要点：

- **先初始化队列**（`window.uetq = window.uetq || []`），使 `bat.js` 加载完成前执行的推送被缓存。
- 第二个位置参数是**事件动作（event action）**；EventGoal 的 `ActionExpression` 匹配的就是它。
- `event_category` / `event_label` / `event_value` 是 EventGoal 可匹配的附加维度。
- `event_id` 是客户端 UET 与 CAPI 同时触发同一逻辑事件时的**去重键（deduplication key）**。两侧发送相同的值。
- **收入事件的必填参数：**`revenue_value` 和 `currency`。

### 参数命名

新的 GTM 部署使用长形式（`event_category`、`event_label`、`event_value`、`event_id`），便于可读。短形式（`ec`、`ea`、`el`、`ev`）出现在旧版代码片段中。

### GA4 dataLayer 事件 → UET 动作映射

| GA4 事件 | UET 动作 | 建议类别 | 备注 |
|---|---|---|---|
| `purchase` | `purchase` | `ecommerce` | 发送 `revenue_value` + `currency`；用 `transaction_id` 作为 `event_id` 以便 CAPI 去重 |
| `generate_lead` | `generate_lead` | `lead_generation` | 用表单提交 ID 作为 `event_id` |
| `sign_up` | `sign_up` | `account` | |
| `subscribe` | `subscribe` | `subscription` | |
| `add_to_cart` | `add_to_cart` | `ecommerce` | |
| `begin_checkout` | `begin_checkout` | `ecommerce` | |
| `view_item` | `view_item` | `ecommerce` | 适用于商品受众 |
| `view_item_list` | `view_item_list` | `ecommerce` | |
| `view_cart` | `view_cart` | `ecommerce` | |
| `search` | `search` | `ecommerce` | |
| `contact`（电话/邮箱点击） | `contact_click` | `contact` | |

### GTM Custom HTML 示例（Purchase）

```html
<script>
window.uetq = window.uetq || [];
window.uetq.push('event', 'purchase', {
  event_category: 'ecommerce',
  event_label: 'order_complete',
  event_value: 1,
  revenue_value: {{DLV - ecommerce.value}},
  currency: '{{DLV - ecommerce.currency}}',
  event_id: '{{DLV - ecommerce.transaction_id}}'
});
</script>
```

> 需要以数字形式发送的 GTM 数字变量不要加引号。注入 Custom HTML 的任何用户可控字符串都要做清洗/转义。

---

## 5. dataLayer 映射（GA4 兼容）

使用 GA4 风格的电子商务对象，使同一套 `dataLayer` 驱动 Microsoft Ads、Google Ads、Meta、TikTok 和 LinkedIn。

```javascript
dataLayer.push({ ecommerce: null });
dataLayer.push({
  event: 'purchase',
  ecommerce: {
    transaction_id: 'T-20260426-001',
    value: 99.97,
    currency: 'USD',
    items: [
      { item_id: 'SKU-001', item_name: 'Product A', price: 29.99, quantity: 2 }
    ]
  }
});
```

```javascript
dataLayer.push({
  event: 'generate_lead',
  form: {
    name: 'contact',
    submission_id: 'lead-20260426-001',
    lead_type: 'contact'
  }
});
```

GTM 变量：

| 变量名 | Data Layer 键 |
|---|---|
| `DLV - ecommerce` | `ecommerce` |
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - ecommerce.items` | `ecommerce.items` |
| `DLV - form.name` | `form.name` |
| `DLV - form.submission_id` | `form.submission_id` |

> **不要把原始邮箱、电话、姓名或地址推送到广泛共享的 `dataLayer`**——容器中的每个标签都能读取。PII 处理放在服务端，用于增强型转化 / CAPI。

---

## 6. Conversions API（CAPI）- 服务端

CAPI 是 Microsoft 的服务器到服务器 UET 数据通道。推荐与 UET JavaScript 并行运行（recommended），使用稳定的共享标识符（如 `transaction_id`）作为 `event_id`（客户端）/ `eventId`（CAPI）进行去重。CAPI 通常从 sGTM 或后端服务调用；切勿将 bearer token 存放在客户端。

**关键命名陷阱：** CAPI 要求使用 camelCase 且字段名与客户端 UET 不同——例如 `eventCategory`（而非 `event_category`）、`value`（而非 `revenue_value`）、`eventId`（而非 `event_id`）。Microsoft 的 `em`/`ph` SHA-256 归一化规则（移除邮箱用户名部分的点、剥离 `+alias`、转小写）也与 Google 和 Meta 不同。

完整的 CAPI schema、端点、鉴权、批量限制和字段参考，见 [Microsoft Advertising Help Center](https://help.ads.microsoft.com/)。

---

## 7. Microsoft 点击 ID（`msclkid`）

点击 Microsoft 广告后，`msclkid` 会被自动标记（auto-tagged）到落地页上。为每个用户持久化**最新**的值约 90 天（第一方 cookie、local storage 或服务端存储），并在该用户的每次 CAPI 事件中携带——没有 `msclkid`，转化归因会不可靠。添加/更新任意 UET 目标时，自动标记会自动启用。

见 [Microsoft Advertising Help Center](https://help.ads.microsoft.com/)。

---

## 8. 线下转化（Offline Conversions）

线索在线下才完成定级（CRM 线索 → 成交）的线索型业务使用 `OfflineConversionGoal`。提交时从落地页 URL 采集 `msclkid`，与时间戳/同意状态一并存入 CRM；定级后再通过 `ApplyOfflineConversions` 上传，使用原始 `msclkid`，转化时间戳须在配置的窗口内（默认 30 天，最长 90 天）。支持增强型转化。

见 [Microsoft Advertising Help Center](https://help.ads.microsoft.com/)。

---

## 9. 同意（Consent）

### GTM 同意类型

| 同意类型 | 何时需要 |
|---|---|
| `ad_storage` | 所有 UET 标签 |
| `ad_user_data` | 发送哈希化邮箱/电话时（增强型转化、CAPI userData） |
| `ad_personalization` | 账户使用再营销 / 个性化受众时 |

### CAPI `adStorageConsent`

| 值 | 含义 |
|---|---|
| `G` | 已授权（Granted，未填写时默认） |
| `D` | 已拒绝（Denied）——`D` 的事件不用于广告或归因 |

对于严格 opt-in 司法辖区，获得同意前应完全屏蔽 UET 基础标签（不加载 `bat.js`），而不是只依赖同意信号。

---

## 10. 调试

| 工具 | 验证内容 |
|---|---|
| **GTM Preview Mode** | 标签触发顺序（Base → Event）、变量值、UET Tag ID 和事件参数解析是否正确 |
| **UET Tag Helper**（Edge/Chrome 扩展） | UET 标签检测、页面加载和自定义事件载荷、校验错误 |
| **Microsoft Advertising UET diagnostics** | 标签状态（`Active`/`Receiving traffic`）、最近事件、转化计数 |
| **DevTools Network** | 筛选 `bat.bing.com`（客户端）或 `capi.uet.microsoft.com`（CAPI）；检查载荷和 HTTP 状态 |

> 报表可能延迟数小时——不要用 30 分钟窗口的转化计数排查问题。CAPI：`400` 响应会包含 `error.details[]`；除非设置 `continueOnValidationError: true`，单个无效事件会拒绝整个批次。

---

## 11. 最佳实践与常见陷阱

- 高价值事件**同时运行 UET（浏览器）+ CAPI（服务端）**，用共享的 `event_id`/`eventId`（如交易 ID）去重。
- **每个站点一个 UET 标签**——单标签规则。多个标签应是例外。
- **`msclkid` 保留 90 天**，用最新值覆盖，并在每次 CAPI 事件中携带。
- **不要把客户端 snake_case 和 CAPI camelCase 混淆**——`event_category`/`revenue_value` 只在浏览器有效；CAPI 要求 `eventCategory`/`value`。
- **切勿将 CAPI bearer token 存放在客户端**——它只属于服务端环境或 sGTM。切勿将原始 PII 推送到 `dataLayer`。
