# Microsoft Clarity - GTM 部署手册

> 请始终在 [Microsoft Learn - Clarity](https://learn.microsoft.com/en-us/clarity/) 核验最新规范。

---

## 1. 智能事件（Smart Events，自动检测）

代表性的自动检测事件（以界面为准）：

| 事件名 | 描述 |
|------------|-------------|
| **Purchase** | 购买完成 |
| **Add to Cart** | 加入购物车 |
| **Begin Checkout** | 结账开始 |
| **Contact Us** | 联系提交 |
| **Submit Form** | 表单提交 |
| **Request Quote** | 报价请求 |
| **Sign Up** | 新注册 |
| **Login** | 登录 |
| **Download** | 下载 |

Smart Event 类型：**Button Clicks** / **API events**（`clarity("event", name)`） / **Auto events** / **Page visits**。可以零代码创建自定义 Smart Event。

---

## 2. 上限与数据保留

| 项目 | 上限 / 保留期 |
|------|-------------------|
| 会话录制（Session recordings） | 每个项目每天最多 100,000 条（超量采样） |
| 热图（Heatmaps） | 每个热图最多 100,000 PV |
| 自定义标签（Custom Tags） | 每页最多 128 个标签，每个标签最多 255 字符 |
| GA4 集成 | 每个项目只能绑定一个 web property |
| 标准录制保留 | 30 天 |
| 收藏 / 加标签 / 抽样录制 | 最长 13 个月 |
| 热图数据 | 最长 13 个月 |

---

## 3. GTM 配置

### 部署步骤

1. 在 [Clarity](https://clarity.microsoft.com/) 创建项目
2. 获取 Project ID（URL：`https://clarity.microsoft.com/projects/view/{Project ID}/`）
3. 把 GTM 模板 JSON 中的 `{CLARITY_PROJECT_ID}` 替换为 Project ID
4. 将 JSON 导入 GTM

### 标签

| 标签名 | 模板 | 触发器 | 备注 |
|----------|----------|---------|-------|
| Clarity Tag | Microsoft Clarity - Official（Community Template，`cvt_MQDKZ`） | All Pages | 只需 Project ID |

### 模板配置字段

| 字段 | 描述 | 必填 |
|-------|-------------|----------|
| **Clarity Project ID** | 项目的唯一 ID | 是 |
| **Custom ID** | 唯一用户标识符 | 否 |
| **Custom Session ID** | 自定义会话 ID | 否 |
| **Custom Page ID** | 自定义页面 ID | 否 |
| **Friendly name** | 界面中显示的用户名 | 否 |
| **Custom tag Key** | 自定义标签键 | 否 |
| **Custom tag Value** | 自定义标签值（可多个） | 否 |

### 变量

| 变量名 | 类型 | 值 |
|---------------|------|-------|
| `Clarity Project ID` | Constant | （Project ID） |

### SPA 支持

Clarity 自动检测路由变化。检测不完整时，在 History Change 触发器上触发一个 Custom HTML 标签来附加路由：

```javascript
clarity("set", "route", location.pathname);
```

> `clarity("event", ...)` 只记录为事件，不拆分 URL——路由识别请使用 Custom Tag。

---

## 4. 客户端 API

### JavaScript API

| API | 语法 | 用途 |
|-----|--------|---------|
| **Custom Tags** | `clarity("set", key, value)` | 为会话附加自定义标签。key/value：字符串（最多 255 字符）；value 也可以是 string[]。每页最多 128 个标签 |
| **Identify** | `clarity("identify", customId, sessionId, pageId, friendlyName)` | 用户识别（跨设备跟踪）。只有 customId 必填 |
| **Event** | `clarity("event", eventName)` | 发送自定义事件 |
| **Upgrade** | `clarity("upgrade", reason)` | 优先录制该会话 |
| **ConsentV2** | `clarity("consentv2", {ad_Storage, analytics_Storage})` | 发送 cookie 同意状态（注意大写 S） |

### HTML 属性

| 属性 | 用途 |
|-----------|---------|
| `data-clarity-mask="true"` | 遮盖该元素及其子元素 |
| `data-clarity-unmask="true"` | 取消遮盖该元素及其子元素 |

> `data-clarity-mask="false"` 无效。用 `data-clarity-unmask="true"` 取消遮盖。

### PII 说明

不要直接将 PII（邮箱、电话、姓名）传给 Custom Tags、Event 或 Identify API。Clarity 会在 DOM 中遮盖 PII，但不会遮盖通过 API 发送的数据。Identify 请传入哈希化或内部 ID。

### clarity-events Community Template

GitHub：`mbaersch/clarity-events`。作为独立于基础 Clarity 标签的标签添加，从 GTM 界面配置 Event 发送、Custom Tags、Session Upgrade 和 Consent 管理。基础标签必须已安装。

---

## 5. 遮盖（Masking）

| 模式 | 行为 |
|------|----------|
| **Strict** | 遮盖所有内容（高隐私域名） |
| **Balanced**（默认） | 仅遮盖数字和邮箱地址 |
| **Relaxed** | 不遮盖（仅限公开站点） |

输入框和下拉框在**所有模式下始终被遮盖**。被遮盖的数据不会发送到 Clarity 服务器。设置变更只对新录制生效；生效最长需要 1 小时。

---

## 6. 同意管理

Clarity 根据 `analytics_storage` 信号控制 cookie 和功能：
- **Granted**：完整的带 cookie 分析。
- **Denied**：无 cookie 模式（无跨会话跟踪）。

如果已使用 Google Consent Mode（GCM），**无需额外配置**——Clarity 自动读取 `analytics_storage` / `ad_storage`。手动控制时，获得同意后从 Custom Event 标签发送 `clarity('consentv2', { ad_Storage: "granted", analytics_Storage: "granted" })`。在 EEA/UK/CH，Clarity 在收到同意信号前不会设置 cookie。

见：https://learn.microsoft.com/en-us/clarity/setup-and-installation/consent-mode

---

## 7. GA4 集成

自动集成：在 Clarity 的 Settings > Setup > Google Analytics integration 下选择一个 GA4 property。回传需要 24 小时以上。每个项目只能绑定一个 GA4 web property。集成范围（如 Playback URL Custom Dimension）随时间变化——以当前界面为准。

见：https://learn.microsoft.com/en-us/clarity/ga-integration/ga4-integration

---

## 8. 推荐设置

- 通过 GTM Community Template 安装（集中管理）
- 遮盖模式：**Balanced**
- 机器人检测：**ON**
- 同意：通过 GCM 集成或 ConsentV2 API 控制
- IP 屏蔽：屏蔽办公/开发 IP
- Smart Events：检查并自定义自动检测的事件

---

## 9. 调试

- **GTM Preview**：确认 Clarity 标签在 All Pages 上触发且 Project ID 正确。
- **DevTools Network**：筛选 `clarity.ms`，确认采集请求。
- **Clarity Dashboard**：实时会话数分钟内出现；录制约 2 小时内出现。
