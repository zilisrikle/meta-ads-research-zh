# Google Ads - GTM 实施手册

> 事实来源：[Set up Google Ads conversion tracking (Help Center)](https://support.google.com/google-ads/answer/6095821) —— 入口页面，内含增强型转化（Enhanced Conversions）、GTM/GA4 设置、转化价值、计数选项与故障排查的链接。

---

## 1. 转化设计（Conversion Design）

### 主要（Primary）vs 次要（Secondary）

| 类别 | 显示位置 | 出价优化 | 用例 |
|----------|-----------------|---------------------|----------|
| **Primary** | 「Conversions」列 | **可用** | 最终业务目标（购买、获客） |
| **Secondary** | 仅「All conversions」列 | 不可用 | 观察中间指标（加购、页面浏览等） |

- 转化是否用于出价优化，取决于"广告系列正在使用哪些转化目标"。即使次要转化，若加入自定义目标并关联到广告系列，也可以成为可参与出价的转化。
- 主要/次要设置在账户层级管理。

### 计数方式（Counting Method）

| 设置 | 行为 | 推荐场景 |
|---------|---------|---------------------|
| **Every** | 每次广告互动后都计每次转化 | 电商购买（每笔购买都产生收入） |
| **One** | 每次广告点击只计一次转化 | 线索获取（同一次点击的重复提交价值不大） |

### 转化窗口（Conversion Window）

| 窗口 | 描述 | 默认 | 范围 |
|--------|-------------|---------|-------|
| 点击转化 | 广告点击后的转化衡量窗口 | 30 天 | 1–90 天 |
| 浏览转化 | 广告展示（无点击）后的转化衡量窗口 | 1 天 | 1–30 天 |
| 互动浏览 | 仅视频广告（如观看 10 秒以上） | 3 天 | 1–30 天 |

### 转化价值（Conversion Value）

| 方式 | 描述 | 用例 |
|--------|-------------|----------|
| **静态（固定价值）** | 所有转化采用同一价值 | 表单提交 = USD 50 等 |
| **动态价值** | 从 dataLayer 获取每笔交易的价值 | 电商购买（订单金额） |

### 归因模型（Attribution Model）

| 模型 | 描述 |
|-------|-------------|
| **数据驱动（DDA）** | 通过机器学习分配各触点的贡献（**默认且推荐**） |
| **最终点击** | 100% 功劳归于最后一次点击。2023 年之前创建的转化操作可能仍默认此模型 |

> 首次点击、线性、时间衰减、基于位置等模型已于 2023 年弃用。

---

## 2. GTM 配置

### 前提条件

- 已在 Google Ads 中获取**转化 ID（Conversion ID）**（`AW-XXXXXXXXX`）和每个**转化标签（conversion label）**。
- **不要在 gtag.js 和 GTM 之间重复部署同用途的标签**（避免 GTM 管理的标签与硬编码标签共存导致重复触发）。

### 标签清单

| 标签名 | 标签类型 | 触发器 | 所需同意 |
|----------|---------|---------|-----------------|
| Google Ads - Conversion Linker | Conversion Linker | All Pages | ad_storage |
| Google Ads - Remarketing | Google Ads Remarketing | All Pages | ad_storage, ad_personalization |
| Google Ads - CV Purchase | Google Ads Conversion Tracking | CE - purchase | ad_storage |
| Google Ads - CV Lead | Google Ads Conversion Tracking | CE - generate_lead | ad_storage |
| Google Ads - EC Data (Purchase) | Google Ads User-Provided Data Event | CE - purchase | ad_storage, ad_user_data |

### Conversion Linker

该标签从 URL 中检测广告点击信息（GCLID）并存入第一方 Cookie。**原则上安装在 All Pages**。如果通过 GTM 在每个页面都加载了 Google 标签（如 GA4 配置标签），则可能不需要，因为 Google 标签已包含该功能。

**存储的 Cookie：**

| Cookie | 用途 |
|--------|---------|
| `_gcl_aw` | Google Ads 点击 ID |
| `_gcl_dc` | DoubleClick 点击 ID |
| `_gcl_gs` | Google Ads 会话信息 |

**配置：**
1. 标签类型："Conversion Linker"
2. 触发器：**All Pages**（必需）

**跨域配置：**勾选「Enable linking across domains」，以逗号分隔列表输入自动关联域名，可选勾选「Enable linking in form action URLs」。

### 转化标签（Conversion Tag）

1. 标签类型："Google Ads Conversion Tracking"
2. 输入 Conversion ID 和 label
3. 通过 dataLayer 变量设置转化价值、货币代码和交易 ID

### 触发器模式

| 模式 | 触发器类型 | 示例条件 | 备注 |
|---------|-------------|-------------------|-------|
| 感谢页 | Page View | Page URL 包含 `/thank-you` | 最简单 |
| 表单提交 | Form Submission | Form ID 等于 `contact-form` | 建议启用「Wait for Tags」和「Check Validation」 |
| **dataLayer 事件（推荐）** | Custom Event | Event name = `purchase` 等 | 最灵活、最可靠 |

### 变量

**常量变量**（集中管理 ID/label）：

| 变量名 | 值 |
|---------------|-------|
| `Google Ads - Conversion ID` | （Conversion ID） |
| `Google Ads - CV Label Purchase` | （Conversion label） |
| `Google Ads - CV Label Lead` | （Conversion label） |

**dataLayer 变量：**

| 变量名 | dataLayer 变量名 |
|---------------|------------------------|
| `DLV - ecommerce.value` | `ecommerce.value` |
| `DLV - ecommerce.currency` | `ecommerce.currency` |
| `DLV - ecommerce.transaction_id` | `ecommerce.transaction_id` |
| `DLV - ecommerce.items` | `ecommerce.items` |
| `DLV - enhanced_conversion_data` | `enhanced_conversion_data` |

### dataLayer 实现示例（购买完成）

```javascript
dataLayer.push({ ecommerce: null });
dataLayer.push({
  'event': 'purchase',
  'ecommerce': {
    'transaction_id': 'T-20260221-001',
    'value': 99.97,
    'currency': 'USD',
    'items': [
      { 'item_id': 'SKU-001', 'item_name': 'Product A', 'price': 29.99, 'quantity': 2 },
      { 'item_id': 'SKU-002', 'item_name': 'Product B', 'price': 39.99, 'quantity': 1 }
    ]
  },
  'enhanced_conversion_data': {
    'email': 'user@example.com',
    'phone_number': '+12025550123'
  }
});
```

> 推送前先清空 `ecommerce: null` 是必需的（防止与之前的电商数据混淆）。

**购买标签的必需参数：** `value`、`currency`、`transaction_id`（用于去重）。

---

## 3. 增强型转化（Enhanced Conversions）

将用户提供的第一方数据（如邮箱）与 Google 账户匹配，补充不依赖 Cookie 的转化衡量。

### GTM 配置

**前提：** Google Ads > Goals > Conversions > Settings > 开启 Enhanced Conversions > 方法：选择「GTM」

**方法 A：Google Ads User-Provided Data Event 标签（推荐）** —— 标签类型「Google Ads User-Provided Data Event」，输入 Conversion ID，在 User-provided data 部分设置 dataLayer 变量，用与转化标签相同的事件触发。

**方法 B：**在现有转化标签上勾选「Include user-provided data」并配置用户提供数据变量。

GTM 模板字段名：`user_provided_data`（子字段 `email`、`phone_number`、`address`）。让标签处理哈希 —— 通过 dataLayer 传递明文；除非使用显式的 `sha256_*` 字段，否则不要预先哈希。

完整字段参考与规范化规则见 Enhanced conversions for web (Google Ads Help)。

### PII 处理注意事项

**放在 dataLayer 上的 PII（个人身份信息）对 GTM 容器中的所有标签都可见**（GA4、Meta Pixel 等）。缓解措施：
- 将增强型转化变量设计为仅供 Google Ads 相关标签使用
- 注意 PII 可能出现在 GTM 预览模式和监控工具中
- 仅在获得同意后发送数据
- 条件允许时优先在服务端（sGTM）做哈希/发送

---

## 4. GA4 集成

### 关联设置

GA4 Admin > Product Links > Google Ads Links > 选择账户 > 开启 Personalized Advertising。

| 平台 | 所需权限 |
|----------|--------------------|
| GA4 | 资源层级「Editor」或以上 |
| Google Ads | 账户「Admin」权限 |

### 原生转化 vs GA4 导入转化

| 对比 | 原生（推荐） | GA4 导入 |
|-----------|---------------------|-----------|
| 浏览转化 | 可衡量 | **不支持** |
| 跨设备转化 | 可衡量 | **受限** |
| 数据新鲜度 | 数小时内 | 24–48 小时 |
| 增强型转化 | 完整支持 | 受限 |
| 出价优化 | 最优 | 处于劣势 |
| 归因范围 | 仅 Google Ads 内 | 跨所有渠道 |

### 推荐配置：混合方案

```
[Primary - bidding]   Google Ads 原生转化标签（经由 GTM），开启增强型转化
[Secondary - observe] GA4 关键事件导入（跨渠道分析；不用于出价）
* 不要在两侧将同一事件都设为「primary」
```

### 数据差异

Google Ads 与 GA4 之间可能出现约 20–30% 的差异。主要原因：归因模型差异、参照日期（GA4 = 转化日期，Ads = 点击日期）、浏览转化（仅 Ads）、数据处理时机、无效流量过滤。

---

## 5. 同意模式（Consent Mode）

### 对 Google Ads 重要的同意类型

| 同意类型 | 范围 |
|--------------|-------|
| `ad_storage` | 广告 Cookie |
| `ad_user_data` | 为广告目的发送用户数据 |
| `ad_personalization` | 再营销等广告个性化 |

### Basic vs Advanced

| 模式 | 行为 | 数据收集 |
|------|---------|----------------|
| **Basic** | 获得同意前阻止所有标签 | 拒绝时不收集数据 |
| **Advanced（推荐）** | 无论同意状态如何都加载标签，发送无 Cookie 的 ping | 拒绝时仍发送无 Cookie ping；可建模 |

### GTM 触发顺序

1. `Consent Initialization - All Pages`（默认同意设置）
2. `Initialization - All Pages`（初始化标签）
3. `All Pages`（常规页面浏览标签）

欧洲经济区/英国必须使用 Consent Mode。其他地区也推荐使用，以保证数据质量并符合当地隐私法规（CCPA/CPRA、LGPD、APPI 等）。

---

## 6. 服务端 GTM（Server-Side GTM）

在 sGTM 容器内使用官方 **「Google Ads Conversion Tracking」** 和 **「Google Ads Remarketing」** 标签。两条路径：(A) 浏览器 → sGTM → GA4（经由 GA4 传给 Ads），或 (B) sGTM 直接触发 Ads 转化标签。好处：HttpOnly Cookie（缓解 ITP）、减少广告拦截器影响、服务端管理凭证。

sGTM 容器设置与 CAPI 参数映射见 Google Ads Conversions (server-side)。

---

## 7. 线下转化（Offline Conversions）

当广告点击带来线下结果（如成交）时，将转化数据导入 Google Ads。在落地时捕获 **GCLID**（存于 `_gcl_aw`，90 天有效期），通过 Google Ads Data Manager（推荐）、Ads 界面或 Google Ads API 上传。相关标识：GCLID（网页）、GBRAID（iOS app）、WBRAID（iOS 网页）。**Enhanced Conversions for Leads** 通过匹配哈希后的用户数据改善 OCI（中位数约提升 10%）。

见 About offline conversion imports。

---

## 8. 自动出价与转化数据

智能出价（Smart Bidding）策略（Maximize Conversions、tCPA、Maximize Conversion Value、tROAS）需要充足的转化量 —— 通常按策略需要每 30 天 15–50+ 次转化。用**宏观转化**（购买、获客）作为 primary、**微观转化**（add_to_cart、begin_checkout）作为 secondary；仅当宏观转化太稀疏无法学习（<50/月）或冷启动期，才把微观转化提升为 primary。

提升数据质量：开启增强型转化、设置准确的动态价值、使用原生标签、保持衡量一致性。

见 About bidding strategies。

---

## 9. 调试（Debugging）

| 工具 | 检查内容 |
|------|--------------|
| **GTM 预览模式** | 标签触发顺序、dataLayer 状态、变量值、同意状态 |
| **Tag Assistant** (tagassistant.google.com) | 标签检测与诊断 |
| **DevTools Network** | 发往 `googleads.g.doubleclick.net/pagead/conversion/` 的请求（cv = label、value、oid = transaction ID、gclaw = GCLID） |

### 常见问题

| 症状 | 检查内容 |
|---------|--------------|
| 转化未记录 | Conversion Linker 是否在 All Pages？ID/label 是否正确？触发器？容器是否发布？是否被 Consent Mode 阻止？ |
| 转化价值为 0 / 为空 | dataLayer 推送是否在标签触发前执行？DLV 变量名是否正确？ |
| GCLID 未存储 | Linker 是否在 All Pages？GCLID 是否在重定向链中丢失？跨域配置？ |
| EC 匹配率低 | 优先发送邮箱；发送多个数据点；核对规范化 |
| 转化重复 | 用 `transaction_id` 去重（在同一转化操作内有效） |

---

## 10. 最佳实践与常见坑

- **Conversion Linker 部署在 All Pages** 是基础 —— 没有它，GCLID/Cookie 捕获就会断裂。
- **gtag.js 和 GTM 之间不要重复标签** —— 选一个唯一事实来源，避免重复触发。
- 购买务必发送 **`value`、`currency`、`transaction_id`**；transaction ID 是去重键。
- **使用 DDA 归因**，有策略地划分 primary/secondary；不要把每个事件都标为 primary。
- **尽量在服务端哈希 PII**，否则让标签从明文 dataLayer 字段自动哈希 —— 绝不要双重哈希。

---

## 11. 实施检查清单

- [ ] GTM 容器安装在每个页面；无 gtag.js/GTM 重复
- [ ] GA4 ↔ Google Ads 已关联；已启用自动标记
- [ ] Conversion Linker 已配置并随 All Pages 发布
- [ ] 原生转化标签已配置；计数（Every/One）、窗口、归因（DDA）、primary/secondary 正确
- [ ] 转化价值已设置；无重复 primary；GA4 关键事件作为 secondary 导入
- [ ] Google Ads 中已开启 Enhanced Conversions + GTM；至少发送邮箱；规范化正确
- [ ] 已实施 Consent Mode v2 并接入 CMP；优先 Advanced 模式
- [ ] 定期检查状态、EC 匹配率、Ads 与 GA4 差异、出价学习状态
