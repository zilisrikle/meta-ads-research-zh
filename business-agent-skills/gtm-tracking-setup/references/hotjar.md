# Hotjar - GTM 实施手册

> 请始终在 [Hotjar 帮助中心](https://help.hotjar.com/) 核验最新规范。

---

## 1. 核心功能面

| 功能面 | 用途 |
|---------|---------|
| **跟踪代码（Tracking Code）** | 所有其他 Hotjar 功能所需的基础客户端脚本 |
| **会话回放（Recordings）** | 用户交互的会话回放（基于 DOM） |
| **热图（Heatmaps）** | 点击、鼠标移动、滚动热图 |
| **问卷 / 反馈（Surveys / Feedback）** | 站内问卷与反馈组件（Ask 产品） |
| **事件 API（Events API）** | 客户端 `hj('event', ...)`——用于筛选、分群、问卷定向 |
| **身份识别 API（Identify API）** | 客户端 `hj('identify', ...)`——用于用户 ID（User ID）与用户属性（User Attributes） |
| **抑制（Suppression）** | 用于屏蔽敏感内容的 HTML 属性 / 站点设置 |

Hotjar 的 GTM 文档明确：加载跟踪代码**不支持服务端打标（server-side tagging）**。GTM 实施必须是客户端（client-side）。

---

## 2. 跟踪代码设置

### Hotjar 站点 ID（Hotjar Site ID）

可在 Hotjar 的 Sites 页面查看；跟踪代码需要它来把数据关联到正确的站点。

### 单基础代码规则（Single-Base-Tag Rule）

**任何页面只应触发一个跟踪代码。**重复会导致数据损坏——常见原因：直接嵌入 + GTM 同时安装、多个站点 ID（Site ID），或单页应用（SPA）中的非页面浏览（Page View）触发器。

### 安装选项

| 选项 | 适用场景 | 说明 |
|--------|----------|-------|
| **Hotjar + GTM 集成** | 在 Hotjar 界面快速设置 | Hotjar 检测到 GTM，并在 Google 授权后创建标签 |
| **GTM 中的 Hotjar 跟踪代码（Hotjar Tracking Code）标签** | 首选的手动 / JSON 导出方案 | GTM 原生内置标签模板（`type: "hjtc"`）；填写站点 ID（Site ID），在所有页面（All Pages）触发 |
| **自定义 HTML 标签（Custom HTML tag）** | 需要时的备选方案 | 粘贴官方代码片段；固定指定 `hjsv` |

生成容器 JSON 时，优先使用 **GTM 原生的 Hotjar 跟踪代码（Hotjar Tracking Code）**标签（`type: "hjtc"`，单个 `siteId` 参数）。将站点 ID（Site ID）保存在 `Const - Hotjar Site ID` 中。

### 自定义 HTML 代码片段（备选）（Custom HTML Snippet (fallback)）

```html
<!-- Hotjar Tracking Code -->
<script>
  (function(h,o,t,j,a,r){
    h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
    h._hjSettings={hjid:{{Const - Hotjar Site ID}},hjsv:6};
    a=o.getElementsByTagName('head')[0];
    r=o.createElement('script');r.async=1;
    r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
    a.appendChild(r);
  })(window,document,'https://static.hotjar.com/c/hotjar-',".js?sv=");
</script>
```

> 重新从 Hotjar 复制代码片段，以获取当前的 `hjsv` 版本。

---

## 3. 事件 API（Events API）

事件可筛选会话回放（Recordings）/热图（Heatmaps）、构建回放分群（Recording Segments）、定向问卷（Surveys），并开启会话采集（session capture）。在 **Observe Plus/Business/Scale** 与 **Ask Plus/Business/Scale** 套餐中预先启用。

### JavaScript 写法（JavaScript Pattern）

```html
<script>
  window.hj = window.hj || function(){(hj.q = hj.q || []).push(arguments);};
  hj('event', 'generate_lead');
</script>
```

`window.hj` 队列垫片（queue shim）必须在调用前存在。始终将其包含在可能在基础标签加载完成前触发的自定义 HTML 标签内。

### 命名规则与限制（Naming Rules and Limits）

| 规则 | 值 |
|------|-------|
| 最大事件名称长度（Max event name length） | 250 个字符 |
| 允许的字符（Allowed characters） | 字母数字、下划线、连字符、空格、句点、冒号、竖线、斜线 |
| 每个站点的唯一事件数（可筛选）（Unique events per Site (filterable)） | 10,000 |
| 每个会话可搜索的唯一事件数（Unique events searchable per session） | 前 50 |
| 事件属性 / 参数（Event properties / parameters） | **不支持**——只传输事件名称 |

### 事件名称中不应包含的内容（What NOT to Put in Event Names）

事件名称必须是**非个人身份信息（non-PII）、低基数（low-cardinality）字符串**。不要包含电子邮箱、电话、姓名、地址、IP、9 位以上数字、带敏感查询参数的完整 URL、日期/时间戳、错误信息、SKU，或任何自由格式的输入。

使用与 dataLayer 对齐的稳定 snake_case 名称：

| dataLayer 事件 | Hotjar 事件 |
|-----------------|--------------|
| `generate_lead` | `generate_lead` |
| `purchase` | `purchase` |
| `sign_up` | `sign_up` |
| `add_to_cart` | `add_to_cart` |
| `begin_checkout` | `begin_checkout` |
| `form_error` | `form_error` |

---

## 4. 身份识别 API（Identify API）与用户属性（User Attributes）

将用户 ID（User ID）与任意键值属性（key/value attributes）附加到当前 Hotjar 用户（筛选回放、定向问卷、按 ID 支持数据删除）。适用于 **Observe Business/Scale** 与 **Ask Business/Scale** 套餐，必须在站点设置（Site settings）中启用。

在 `user_context_ready` 自定义事件（Custom Event）及单页应用（SPA）路由变化后，通过 GTM 自定义 HTML 标签（Custom HTML tag，带 `window.hj` 垫片）调用：

```html
<script>
  window.hj = window.hj || function(){(hj.q = hj.q || []).push(arguments);};
  hj('identify', '{{DLV - user_id}}', {
    plan: '{{DLV - user_plan}}',
    account_type: '{{DLV - account_type}}'
  });
</script>
```

`null` 允许作为匿名属性附加时的用户 ID（User ID）——但当用户 ID（User ID）为 `null` 时绝不能发送个人身份信息（PII）属性（无法完成查找/删除）。使用内部不透明 ID（opaque ID，而非电子邮箱）。电子邮箱（Email）仅在获得法律批准时才可放入保留的 `email` 键。若两者都为问卷定向（Survey targeting）提供数据，Identify 必须在同一页面生命周期内先于事件（Events）运行。

参见：https://help.hotjar.com/hc/en-us/articles/36820006120721-Identify-API-Reference

---

## 5. GTM 配置（GTM Configuration）

### 推荐标签（Recommended Tags）

| 标签名称 | 类型 | 触发器 | 用途 |
|----------|------|---------|---------|
| `Hotjar - Base` | GTM 原生 **Hotjar 跟踪代码（Hotjar Tracking Code）**（`hjtc`）——备选为自定义 HTML（Custom HTML） | 所有页面 / 页面浏览（All Pages / Page View） | 加载 Hotjar |
| `Hotjar - Event - generate_lead` | 自定义 HTML（Custom HTML） | `CE - generate_lead` | 线索会话打标（Lead session tagging） |
| `Hotjar - Event - purchase` | 自定义 HTML（Custom HTML） | `CE - purchase` | 购买会话打标（Purchase session tagging） |
| `Hotjar - Event - sign_up` | 自定义 HTML（Custom HTML） | `CE - sign_up` | 注册会话打标（Signup session tagging） |
| `Hotjar - Identify` | 自定义 HTML（Custom HTML） | `CE - user_context_ready`（以及单页应用（SPA）路由变化） | 用户 ID（User ID）+ 属性（attributes） |

> Hotjar 没有为事件（Events）或用户属性（User Attributes）发布官方 GTM 社区模板（Community Templates）。Knowit Experience 发布了非官方模板（`gtm-hotjar-event`、`gtm-hotjar-user-attributes`）；自定义 HTML（Custom HTML）仍是可接受的方案，特别是在 JSON 导出工作流中——其中 `cvt_*` ID 与 `galleryReference` 签名必须来自真实的 GTM 导出，而非编造。

### 变量（Variables）

| 变量名称 | 类型 | 示例 |
|---------------|------|---------|
| `Const - Hotjar Site ID` | 常量（Constant） | `1234567` |
| `DLV - user_id` | 数据层变量（Data Layer Variable） | `user_id` |
| `DLV - user_plan` | 数据层变量（Data Layer Variable） | `user_plan` |
| `DLV - account_type` | 数据层变量（Data Layer Variable） | `account_type` |
| `DLV - ab_test_variant` | 数据层变量（Data Layer Variable） | `ab_test_variant` |

### 触发器（Triggers）

| 触发器名称 | 类型 | 事件 |
|--------------|------|-------|
| `All Pages` | 页面浏览（Page View） | 内置（Built-in） |
| `CE - generate_lead` | 自定义事件（Custom Event） | `generate_lead` |
| `CE - purchase` | 自定义事件（Custom Event） | `purchase` |
| `CE - sign_up` | 自定义事件（Custom Event） | `sign_up` |
| `CE - user_context_ready` | 自定义事件（Custom Event） | `user_context_ready` |

### 文件夹（Folder）

为基础标签、事件标签、Identify 标签、站点 ID（Site ID）常量及 Hotjar 专用的数据层变量（DLV）创建一个 `Hotjar` 文件夹。共享的自定义事件触发器归入 `_Global`。

---

## 6. dataLayer 映射（dataLayer Mapping）

复用已有的 GA4 风格 dataLayer 事件。

### 线索（Lead）

```javascript
dataLayer.push({ event: 'generate_lead', form_id: 'contact', form_type: 'inquiry' });
```

```html
<script>
  window.hj = window.hj || function(){(hj.q = hj.q || []).push(arguments);};
  hj('event', 'generate_lead');
</script>
```

### 购买（Purchase）

```javascript
dataLayer.push({
  event: 'purchase',
  ecommerce: {
    transaction_id: 'T12345',
    value: 49.99,
    currency: 'USD',
    items: [{ item_id: 'SKU001', item_name: 'Product Name', price: 49.99, quantity: 1 }]
  }
});
```

```html
<script>
  window.hj = window.hj || function(){(hj.q = hj.q || []).push(arguments);};
  hj('event', 'purchase');
</script>
```

> 不要将交易 ID（transaction ID）、电子邮箱、电话、地址、购物车内容、商品名称或 SKU 代码传入 Hotjar 事件名称。Hotjar 事件不携带参数。

### 用户上下文（User Context）

```javascript
dataLayer.push({
  event: 'user_context_ready',
  user_id: 'u_123456',
  user_plan: 'pro',
  account_type: 'brand',
  ab_test_variant: 'pricing_v2_a'
});
```

```html
<script>
  window.hj = window.hj || function(){(hj.q = hj.q || []).push(arguments);};
  hj('identify', '{{DLV - user_id}}', {
    plan: '{{DLV - user_plan}}',
    account_type: '{{DLV - account_type}}',
    ab_test: '{{DLV - ab_test_variant}}'
  });
</script>
```

---

## 7. 隐私与同意（Privacy and Consent）

Hotjar 是行为分析（behavior analytics）工具，而非广告。在需要用户选择同意（opt-in）的司法管辖区，用 `analytics_storage` 同意状态（consent）来限制基础标签的触发。带个人上下文的 Identify 需要明确的用户数据同意（user-data consent）。Hotjar 遵守浏览器的**禁止跟踪（Do Not Track）**。

### 抑制（Suppression）

Hotjar 默认抑制用户输入。9 位以上的纯数字字符串始终被抑制。抑制规则的变更**不具有追溯效力**。

| 机制 | 效果 |
|-----------|--------|
| `data-hj-suppress` 属性或类 | 抑制元素及其子元素内的文本和图片/视频内容 |
| `data-hj-allow` 属性或类 | 允许在受支持的表单字段上采集按键（非递归） |
| 内联 SVG（Inline SVG） | 无法通过特定元素抑制来抑制 |

绝不要用 GTM 将原始表单字段值传入 Hotjar。将 Hotjar 抑制视为纵深防御（defense-in-depth）。

---

## 8. Cookie、存储、单页应用（SPA）、留存（Cookies, Storage, SPA, Retention）

**Cookie** 为第一方（first-party）。关键项：`_hjSessionUser_{site_id}`（365 天）、`_hjSession_{site_id}`（30 分钟，有活动时延长），以及 local storage 中的 `_hjUserAttributes` 和 session storage 中的 `hjViewportId`。禁用 Cookie 时 Hotjar 不会跟踪。参见：https://help.hotjar.com/hc/en-us/articles/36819973371409

**单页应用（SPA）**：`Hotjar - Base` 只在**所有页面 / 页面浏览（All Pages / Page View）**触发——绝不要用历史记录变化（History Change，Hotjar 自带单页应用（SPA）检测，会冲突）。URL 变化后发送 Identify；应用层 dataLayer 事件触发时发送事件（Events）。

**留存（Retention）**：回放（Recordings）365 天；热图（Heatmaps）365 天；问卷回复无限期保留直至删除；已识别用户 365 天无活动后删除；去标识化用户 3 个月后删除。

---

## 9. 调试（Debugging）

- 发布 GTM 后，在 Hotjar 的 Sites 页面执行**验证安装（Verify Installation）**（等待几分钟）。
- **开发者工具网络（DevTools Network）**：筛选 `hotjar`——确认 `hotjar-{site_id}` 已加载且只出现一个站点 ID（Site ID）。
- **调试模式（Debug Mode）**：在 URL 后追加 `?hjDebug=1`；控制台会记录基础加载、每次 `hj('event', ...)` 和每次 `hj('identify', ...)`。

---

## 10. 最佳实践与常见陷阱（Best Practices and Common Pitfalls）

| 陷阱 | 影响 | 预防 |
|---------|--------|------------|
| 通过服务端 GTM（server-side GTM）加载 Hotjar | 不支持 | 仅使用客户端 GTM（Client-side GTM） |
| 单页应用（SPA）中在历史记录变化（History Change）上触发基础标签 | 重复或缺失的脚本 | 仅页面浏览 / 所有页面（Page View / All Pages） |
| 重复安装 Hotjar（代码 + GTM） | 多个站点 ID（Site ID），跟踪损坏 | 删除直接嵌入；使用单一来源 |
| 事件在 `hj` 队列存在前触发 | 调用丢失 | 在每个事件标签中包含 `window.hj` 队列垫片（queue shim） |
| 在事件名称中发送个人身份信息（PII） | 隐私违规；事件没有参数 | 仅使用稳定的低基数（low-cardinality）名称 |
