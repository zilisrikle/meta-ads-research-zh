# 转化衡量与增量性（Measurement and incrementality）

本参考用于设计转化操作（conversion actions）、决定出价目标、判断上报数据是否可信，或规划增量性测试。本参考是*决策层*——不覆盖 GTM 代码部署实现、dataLayer 设计或像素触发逻辑。相关内容请使用 `gtm-tracking-setup` skill。

## 范围与本参考不覆盖的内容

| 范围内 | 范围外（用其他 skill / 参考） |
|---|---|
| 出价目标的选择、估值方法、评估方式 | 标签触发逻辑、GTM 容器 JSON、dataLayer 结构 → `gtm-tracking-setup` |
| 同意模式 v2（Consent Mode v2）策略与影响 | CMP 横幅用户体验、按地理 IP 的规则、法务审核 |
| 增强型转化（Enhanced Conversions）与线下转化导入（Offline Conversion Import）作为决策工具 | 服务器端哈希代码、API 客户端实现 |
| 建模转化解读、VTC 政策、归因选择 | 受众构建 / 细分策略 → playbooks |
| 增量性方法、适用条件、统计解读 | 具体的 MMM 模型拟合 → 供应商或分析团队 |
| iOS / 应用衡量的规划层内容 | SDK 安装与事件映射 → Firebase 文档 |

---

## 运营原则

- **转化操作即策略。**智能出价（Smart Bidding）和 PMax / Demand Gen 会激进地朝着被标记为主要转化（Primary）的目标优化。选择有可操作量级和延迟的最深层可靠信号。把弱代理指标设为主要转化，未来几年都会买到弱业务结果。
- **平台指标是方向盘，不是财务真相。**上报的广告支出回报率（ROAS, Return on Ad Spend）和每次转化费用（CPA, Cost Per Action）包含建模、浏览转化（VTC, View-through conversions）、品牌流量和再营销捡漏。在把任何平台数字当作基准真相之前，先用营收、销售管道（pipeline）、CRM、应用 LTV 或边际贡献（contribution margin）对账。
- **实测（Observed）> 建模（modeled）。**提升同意采集率、增强型转化覆盖率和线下导入量，会把转化从建模转为实测，收紧智能出价。这通常胜过任何广告系列设置调整。
- **默认把 VTC 当作独立信号。**在标题 CPA / ROAS 里合并点击转化和浏览转化会掩盖因果，尤其是在 PMax、Demand Gen、展示广告和视频广告系列中。
- **在 Google Ads 内部，提升研究衡量的是 Google 视角下的 Google。**它们捕捉不到跨平台泄漏。做重大决策时，用地理对照（geo holdouts）、MMM 或业务数据对账做三角验证。
- **不能对账的就不要优化。**如果你的 Google Ads 转化数、GA4 转化数和 CRM/POS 营收无法在可解释的差异内达成一致，先修好数据管道再扩大花费。

---

## 1. 衡量栈速览

一套可用的 Google Ads 衡量栈有四层。每层有不同的故障模式和修复方式。

```
┌──────────────────────────────────────────────────────────────────┐
│  Layer 4: Reconciliation                                         │
│    CRM / POS / DWH / finance source of truth                     │
│    → Reconciles platform CV with business outcome                │
└─────────────────▲────────────────────────────────────────────────┘
                  │  Offline imports, API joins, blended dashboards
┌─────────────────┴────────────────────────────────────────────────┐
│  Layer 3: Google Ads conversion actions                          │
│    Primary / Secondary CV, attribution model, VTC windows        │
│    → Drives Smart Bidding and reporting                          │
└─────────────────▲────────────────────────────────────────────────┘
                  │  Tag fires, gclid join, EC match, OCI upload
┌─────────────────┴────────────────────────────────────────────────┐
│  Layer 2: Signal collection                                      │
│    Google tag, GTM, sGTM / Tag Gateway, Enhanced Conversions     │
│    → Where most accuracy is won or lost                          │
└─────────────────▲────────────────────────────────────────────────┘
                  │  Cookies, identifiers, hashed PII, click IDs
┌─────────────────┴────────────────────────────────────────────────┐
│  Layer 1: Consent and privacy                                    │
│    Consent Mode v2, ITP/ETP, CMP, regional rules                 │
│    → Determines what Layer 2 is allowed to see                   │
└──────────────────────────────────────────────────────────────────┘
```

当某个数字看起来不对时，自上而下诊断：这是对账问题（第 4 层）、转化操作问题（第 3 层）、标签 / 标识符问题（第 2 层），还是同意问题（第 1 层）？在错误的层级行动会浪费几周时间。

---

## 2. 转化操作设计

### 主要 / 次要 / 微型决策

| 类型 | 用途 | 示例 | 默认计数规则 |
|---|---|---|---|
| **主要（Primary）** | 出价优化（智能出价以此为目标） | 购买、合格线索（qualified lead）、预约、付费注册、应用内营收事件 | 「计入转化（Include in Conversions）」开启 |
| **次要（Secondary）** | 仅监控——不参与出价 | Newsletter 订阅、内容下载、无付款的账户创建 | 「计入转化」关闭 |
| **微型（Micro）** | 互动信号；除非代理有效性已验证，否则永不设为主要 | 滚动深度、停留时长、定价页浏览、视频播放 50% | 「计入转化」关闭 |

**选择主要转化。**用有可操作量级和延迟的最深层可靠信号。翻译成实操：

| 每月量级（真实主要转化） | 可出的价 |
|---|---|
| 0–10 | 手动 CPC / 争取点击（Maximize Clicks）。不要在这个量级上跑智能出价；信号太薄。 |
| 10–30 | 单一主要转化跑争取转化（Maximize Conversions）。不跑多广告系列 tCPA。 |
| 30–50 | CPA 稳定且主要转化明确时可上 tCPA。 |
| 50–100 | 价值传递干净时，价值出价变得可行。 |
| 100+ | 多广告系列组合策略、价值规则、带导入的 PMax 线索广告。 |

如果最深层可靠信号做不到每月 30+，**要么深化创意 / 报价 / 落地页把量级做上去**，**要么把主要转化放宽到稍浅但仍有意义的动作**（例如用合格线索代替已成交），**要么在量级起来之前保持手动出价**。不要靠提拔一个微型转化来伪造「主要转化」。

### 价值传递

| 层级 | 传递内容 | 适用场景 |
|---|---|---|
| 固定价值 | 每个转化一个静态值 | 经济模型稳定的线索广告；B2B 表单填写的默认选择 |
| 动态营收 | 订单营收或按阶段加权的线索价值 | 电商默认；SaaS 付费注册 |
| 毛利感知价值 | 营收 × 毛利率（或边际贡献） | 跨产品毛利差异显著时的电商 |
| 线索评分 | CRM 派生的评分（0–100、哈希分桶或阶段价值） | 线索评分可靠的 B2B；企业 SaaS |

电商用固定价值的转化几乎总是错的——智能出价分不清 20 美元的订单和 2,000 美元的订单。B2B 线索广告里，固定价值是务实的默认选择，除非有导入的线索评分。

### 按业务模式的默认配置（与 [business-model-playbooks.md](business-model-playbooks.md) 联动）

| 业务模式 | 默认主要转化 | 常见升级路径 |
|---|---|---|
| 电商 | 购买，动态营收价值 | Feed 有毛利标签后升级为毛利感知价值 |
| B2B 线索 | CRM 导入的合格线索（ECfL 或 OCI） | 增加阶段价值（MQL / SQL / Opportunity） |
| 本地服务 | 时长达标的电话 **或** 表单提交 | 通过 OCI 增加「预约完成」 |
| 高客单 / 长周期 | 咨询预约 | 量级允许时通过 OCI 增加已成交（closed-won） |
| 应用 | tCPI 安装起步 → 应用内事件 tCPA → tROAS | 成熟阶梯，见第 11 节 |
| 到店 | 到店（Store visit，Google 建模） | 符合条件时通过 POS 导入到店销售（Store sale） |

### 按广告类型速查

| 广告系列类型 | 是否基于点击转化出价 | 非点击转化处理 | 说明 |
|---|---|---|---|
| 搜索 | 是 | 否（VTC 会上报但不计入转化列） | 标准点击驱动 |
| 展示 | 是 | EVC 可用于视频资源；标准 VTC 仅上报 | 把 VTC 当辅助证据，不当作主 CPA 故事 |
| 购物 | 是 | 否 | 点击驱动 |
| 视频行动（Video Action） | 点击 + 互动观看（Engaged-View） | EVC 计入转化；标准 VTC 单独上报 | EVC 是直接响应视频的非点击信号 |
| 应用 | 点击 + 基于观看 | VTC 优化可让符合条件的 VTC 参与出价，尤其 Android 安装 | iOS：SKAN + ICM 信号混合，见第 11 节 |
| **Performance Max** | 点击 + 互动观看 | EVC 计入转化；标准 VTC 仅 PMax 到店目标（Store Goals）计入转化 | 按季度审计广告事件类型和渠道组合 |
| **Demand Gen** | 点击 + 互动观看 | EVC 计入转化；VTC 优化可对 YouTube 资源开启，默认关闭 | 平台对比列（Platform Comparable columns）可仅上报用计入 VTC |

参考：[Google Ads Help](https://support.google.com/google-ads/answer/16542520)。

---
## 3. 同意模式 v2（Consent Mode v2）

同意模式 v2（CMv2）控制 Google 标签可以收集和使用哪些数据（[Consent mode reference](https://support.google.com/google-ads/answer/13802165?hl=en)）。它是**基础设施**，不是设置项——配置错误的 CMv2 会在受影响流量上悄无声息地削弱再营销、转化跟踪、建模和出价。

### 基础版 vs 高级版

| 模式 | 用户同意前的标签行为 | 用户拒绝时是否发送 ping | 运营影响 |
|---|---|---|---|
| **基础版（Basic）** | 用户同意前标签不加载 | 无——拒绝即完全静默 | 法务姿态更简单；丢失所有非同意者信号，包括建模燃料 |
| **高级版（Advanced）** | 横幅弹出前标签已加载；cookie / 标识符暂缓直到同意 | 无 cookie ping（无 ID，只有上下文：时间戳、页面、浏览器、地理区域） | 建模可以填补部分缺口；无 cookie ping 在部分欧盟成员国的 ePrivacy 下是灰色地带 |

实操默认：针对 EEA / UK 的大多数 Google Ads 账户用**高级版**，因为建模能实质性缩小同意拒绝带来的缺口。只有法务审核明确要求时才切到基础版。

### 区域要求

Google Ads 规划中主要的运营门槛是 EEA / UK / 瑞士流量。同意模式 v2 在这些地区对个性化广告和受众功能是强制的（[Google Ads Help](https://support.google.com/google-ads/answer/13695607?hl=en)）。

| 区域 | 规划层面的影响 |
|---|---|
| **EEA + UK + 瑞士** | 缺少必需同意信号时，再营销受众、个性化广告、转化建模、EC 和受众功能可能降级或停用。丢失的信号无法回填。 |
| 其他区域 | 与法务 / 隐私负责人另行确认本地隐私与同意要求。不要假设 CMv2 能替代本地合规工作。 |

### 建模门槛

Google Ads 只在**国家 × 域名**同时满足以下两个条件时，才对非同意流量建模转化：

| 门槛 | 值 |
|---|---|
| Google Ads 转化建模 | 每 7 天滚动窗口内 **700 次广告点击**，按国家 × 域名（[Google Ads Help](https://support.google.com/google-ads/answer/10548233?hl=en)） |
| GA4 行为建模 | 最近 28 天中有 ≥ 7 天日活 ≥ 1,000 且 `analytics_storage='granted'`，**且** ≥ 7 天日事件 ≥ 1,000 且 `analytics_storage='denied'`；报告身份 = **混合（Blended）** |

同一域名运营多国时，小国家可能永远达不到每周 700 点击，那里就不会启用建模。实操解法：按区域汇总报告；接受小市场没有建模；或减少转化操作碎片化。

### 信号定义

| 信号 | 层级 | 控制内容 |
|---|---|---|
| `ad_storage` | 上游（存储） | 广告 cookie / 标识符是否可写 / 可读 |
| `analytics_storage` | 上游（存储） | 分析 cookie（`_ga`）是否可写 / 可读 |
| `ad_user_data` | 下游（传输） | 用户数据是否可发送给 Google 用于广告——**增强型转化和基于标签的转化跟踪必需** |
| `ad_personalization` | 下游（传输） | 数据是否可用于**再营销 / 个性化广告 / 受众功能** |

没有 `ad_personalization='granted'`，就无法用 EEA 用户构建再营销受众。

### EEA 执法状态

对不合规的 EEA / UK / CH 账户，同意信号缺口可能削弱衡量、广告个性化和再营销能力（[Google Ads Help](https://support.google.com/google-ads/answer/13695607?hl=en)）。规划时把丢失的信号视为不可恢复。

- 再营销受众不再填充（丢失期间不补建）
- 受影响流量的个性化广告停用
- 这些用户的转化跟踪、人口统计报告、EC、受众功能停止工作
- **丢失的数据不可恢复**——无法回填
- 账户层面的 GDPR / DMA 罚款风险独立于功能降级

### 同意模式信号架构变更（2026 年 6 月 15 日）

**2026 年 6 月 15 日**起，Google Signals 不再控制跨设备广告数据；同意模式 v2 成为跨设备广告 ID 的唯一控制点。确保同意用户有 `ad_storage='granted'` 和 `ad_personalization='granted'` 在流动——否则此日期后跨设备再营销和客户匹配（Customer Match）会悄无声息地降级。Google Signals 在 GA4 内保留较窄的行为 / 人口统计角色。

---

## 4. 增强型转化（Enhanced Conversions, EC）

增强型转化（EC）在转化事件旁发送哈希化的一方数据（邮箱、电话、姓名、地址）（[EC for web Help](https://support.google.com/google-ads/answer/15712870?hl=en)、[EC for leads Help](https://support.google.com/google-ads/answer/14274408?hl=en)）。Google 用它匹配自己见过的登录用户，找回因 cookie 限制、ITP、广告拦截或未同意会话而丢失的转化。**EC 是现代 Google Ads 衡量中单位努力杠杆最高的手段。**

### 两个产品，两种活

| 产品 | 职责 | 匹配数据采集时点 | 是否后续上传线下事件？ |
|---|---|---|---|
| **EC 网页版（ECfW）** | 找回线上转化（电商购买、在线表单填写） | 转化页面（结账成功、感谢页） | 否——匹配发生在转化时 |
| **EC 线索版（ECfL）** | 把*线下* CRM 事件归因回点击 | 线索表单提交 | 是——后续线下结果（合格、商机、已成交）通过 Data Manager / API / Zapier 上传，基于哈希 PII 匹配（不需要 gclid） |

### 设置方式

| 方式 | 可靠性 | 适用场景 |
|---|---|---|
| Google 标签——自动检测 | 低–中 | 快速起步；依赖标准表单字段结构；SPA 和定制结账会失效 |
| Google 标签——手动（CSS 选择器 / JS 变量） | 高 | 大多数生产账户 |
| GTM（同 gtag 三种模式） | 高 | 已有 GTM 时的默认选择 |
| **Data Manager / Google Ads API** | 最高 | 服务器端、批处理友好，ECfL 线下部分必需 |

（2026 年 6 月简化：EC 成为每个转化操作的**单一开关**；多源摄入——标签 + Data Manager + API 同时——成为默认。方法选择器 UI 被移除。）

### 哈希要求

- SHA-256，十六进制编码，**小写**
- 邮箱归一化：小写 + 去空格 +（仅 Gmail）去掉点和 `+后缀`
- 电话用 E.164 格式（`+15551234567`），无空格或标点
- 哈希发生在**传输前**——Google 永远收不到原始 PII
- 除了邮箱 / 电话，还可以传名 / 姓 / 邮编 / 国家以提高匹配率

### 覆盖率 / 匹配率

盯的诊断指标是**覆盖率**：附带有效 EC 数据的转化占比。目标 ≥ 70%。低于 50% 意味着相当一部分转化没享受到 EC，限制因素通常是缺字段或选择器映射错。

### 报告的提升

| 来源 | 声称 | 备注 |
|---|---|---|
| Google（产品营销） | 搜索 +5%、YouTube +17% | Google 案例研究的最佳情况平均值 |
| 独立从业者报告 | 衡量转化 +5–10% | 业内更常见的水平 |
| 与同意模式 v2 建模叠加 | 混合 +15–25% | 大部分「提升」是 EC + 建模共同贡献 |

提升数据在 Google Ads 界面「转化 → 诊断 → 转化提升（Conversion Uplift）」下展示，但**仅在 EC 激活后前 30 天**可见。之后 EC 找回的转化会并入转化总数，不再单独成列。30 天窗口内截屏留档。

### 与同意模式的兼容

EC 需要 `ad_user_data='granted'`。没有它，即使 `ad_storage='granted'`，EC 也不会触发。CMv2 和 EC 互补：CMv2 管同意，EC 在同意已授予时提供确定性一方信号，提高转化的实测占比，降低对建模的依赖。

---
## 5. 线下转化导入（Offline Conversion Import, OCI）

OCI 把发生在网站之外（CRM 更新、销售电话、到店、成交）的点击后事件带回 Google Ads，归因到最初的广告点击（[Google Ads Help](https://support.google.com/google-ads/answer/2998031?hl=en-EN)）。**对线索质量可变的线索广告，OCI 或 ECfL 没得选**——没有它，智能出价会买更多便宜表单，不管它们成不成单。

### 点击标识符

| 标识符 | 用例 | OCI 上传有效期 | 粒度 |
|---|---|---|---|
| `gclid` | 标准网页广告点击 → 网页 / 线下 CRM 转化 | 点击后 **90 天** | 用户级（确定性） |
| `gbraid` | 网页广告点击 → 应用转化（ATT 受限的 iOS） | 聚合匹配窗口 | 粗粒度 / 聚合 |
| `wbraid` | 应用广告点击 → 网页转化（ATT 受限的 iOS） | 聚合匹配窗口 | 粗粒度 / 聚合 |

（规范记忆：**g**braid = **g**oing to app（去应用），**w**braid = **w**eb destination（去网页）。少数老第三方文章写反了；以 Google 规范为准。）

**采集机制**（规划层——实现在 `gtm-tracking-setup`）：

1. 在落地页从 URL 读取点击 ID
2. 存入一方 cookie + 隐藏表单字段
3. 表单提交时把点击 ID 写到 CRM 线索记录
4. CRM 阶段推进时，把 `{click_id, conversion_time, value, conversion_action}` 通过 API / Data Manager / 原生 CRM 集成上传回 Google Ads

### 调整窗口

| 窗口 | 行为 |
|---|---|
| **转化后 7 天内** | 调整会被智能出价读取——出价会适应你导入的质量信号 |
| 7–55 天 | 调整记录用于上报，但智能出价**忽略** |
| > 55 天 | 硬拒绝——不接受调整 |

实操规则：**至少每天导入**，理想是 API 按小时。按周批量会丢失大部分出价影响力。

### ECfL vs OCI——用哪个

| 场景 | 用 |
|---|---|
| 有原生 CRM 集成（HubSpot Marketing Hub Pro+、Salesforce 连接） | **ECfL 走集成** |
| 线索表单在自家网站、定制 CRM | 能可靠采集邮箱 / 电话就用 **ECfL**；OCI 只做备选 |
| 老账户已跑 gclid + OCI，CRM 无 PII | OCI 可以继续，但要规划升级——Google 自 2024 年起的明确方向是经 Data Manager 的 ECfL |
| 无表单的电话（纯来电） | 转接号码 + 按通话时长 OCI |
| 隐私受限的 iOS 应用转化 | gbraid / wbraid 处理——仅聚合 |

ECfL 比 OCI 更耐用：不需要存储的点击 ID、支持跨设备匹配，是新构建的规范路径。

上传时效因产品而异：标准 OCI 导入可用保留最长 90 天的 GCLID；增强型线索转化的线下事件如果在关联最后一次点击后超过 63 天才上传，则不予导入。

### 常见拒绝 / 失败模式

| 症状 | 原因 | 修复 |
|---|---|---|
| "Click ID outside 90-day window" | gclid 超过 90 天 | 更早上传；销售周期很长时，导入更近的合格阶段代理指标，或用 ECfL（用户数据在早期就采集） |
| "Enhanced conversions for leads import too old" | ECfL 线下事件在关联最后一次点击后超过 63 天才上传 | 每天上传，或导入更近的漏斗事件；不要围绕 >63 天的上传延迟设计 ECfL |
| "Conversion time before click time" | 时区配置错误 | 确认 CRM 和 Google Ads 用同一时区；加 1–2 天缓冲；不要对时间戳四舍五入 |
| "Click not yet processed" | 点击后约 6 小时内就导入转化 | 延迟 6+ 小时再导入；按小时批量没问题 |
| "No 'Import from clicks' conversion action" | 转化操作建于点击之后 | 无法回填；转化操作设置要与上线日期对齐 |
| 哈希格式错误 | 需要 SHA-256 十六进制小写 | 哈希前归一化邮箱（小写、去空格、Gmail 去点） |
| ECfL 匹配率太低 | 邮箱 / 电话字段缺失或传错字段 | 检查选择器 / 变量；看覆盖率诊断 |

### `conversion_environment` 参数

Google 2025 年 6 月宣布线下上传支持 `conversion_environment` 参数（值：`UNSPECIFIED` / `UNKNOWN` / `APP` / `WEB`）。**2025 年 9 月 30 日的硬性截止日已撤回**——不带它的上传仍可处理。实操立场：**新集成要加上**，因为缺了它应用 / 网页归因和智能出价信号质量会降级；但老上传不带该参数也不会被拒绝。

---

## 6. 服务器端打标与 Google Tag Gateway

### 决策树

```
Is the account on EEA/UK traffic OR > $50k/month spend OR Safari-heavy audience?
├── No → Default Google tag (client-side) is enough. Stop here.
├── Yes, but < $250k/month spend
│   └── Google Tag Gateway for Advertisers (GTG)
│       Cost: ~free; replaces ad-hoc 1P-domain hacks
│       Wins: ITP cookie persistence, ad-blocker resilience, ~13% claimed conv. signal recovery
└── Yes, > $250k/month spend OR multi-platform CAPI (Meta, TikTok in parallel) OR CRM data enrichment
    └── Full sGTM (Stape / GCP / Tag Pilot) + GTG
        Realistic cost: $1,000+/month all-in
        Wins above GTG: payload enrichment, server-side EC, multi-channel CAPI, PII hygiene
```

### Google Tag Gateway 广告版（GTG）

- 2025 年起 GA（正式发布）；由「一方模式（first-party mode）」更名
- **2025 年 4 月 10 日**起，带 Google Ads / Floodlight 标签的 GTM 容器先以 `Set-Cookie` 语义加载 Google 标签，击破大多数 ITP / 扩展级 cookie 限制
- 报告的提升：转化信号 **~13%**（Google 数据，Merkle / Brainlabs 呼应；未被大规模独立审计）
- 它**不是** sGTM 的替代品；预算允许时推荐 **CDN + sGTM** 组合

### 服务器端 GTM（sGTM）

真实成本现状：

| 托管 | 月成本下限 | 备注 |
|---|---|---|
| GCP 自建（Cloud Run，≥3 台服务器） | ~$90 | 另加日志、出口流量、存储；真实成本更高 |
| Stape（托管） | $20+（高流量 $200–400） | 10k 请求 / 月以下有免费档 |
| Tag Pilot | $15+ | 无事件上限 |
| **总计（含搭建 50–120 小时 @ ~$120/小时，日常维护 10–20 小时/月）** | **~$1,000+/月** | 可行性讨论就用这个数字 |

**sGTM 解决什么：**
- Safari ITP cookie 持久化（服务器端设置的一方 cookie 绕过 JS 7 天 / `gclid` 24 小时上限）
- 广告拦截器韧性（广告拦截器看不到请求）
- 负载增强（发送前合并 CRM 数据、服务器端 EC、利润值）
- 多平台 CAPI 并行（Meta、TikTok 等共用同一套 sGTM 基础设施）
- PII 卫生（数据出境前先哈希 / 剥离）

**sGTM 不解决什么：**
- 页面加载速度（现代客户端 GTM 本来就是异步）
- 靠它实现「GDPR 合规」——同意义务与标签在哪跑无关
- 缺少同意导致的归因问题——无论服务器端还是客户端，同意管一切

**采用经验法则：**月花费约 $250k 或同时跑两个以上平台的 CAPI，是 sGTM 靠 5%+ 的归因回收覆盖总成本的门槛。低于这个，单用 GTG 通常是正确答案。

---

## 7. 建模转化

Google Ads 在确定性归因不可用时（拒绝同意、跨设备旅程、cookie 过期、应用 ↔ 网页），会在实测转化旁上报**建模转化**。置信度足够高时，建模值填入标准「转化」列。

### 转化何时被建模

| 触发 | 原因 |
|---|---|
| 用户拒绝同意（高级版 CMv2） | 只有无 cookie ping 可用；Google 据此建模 |
| 跨设备旅程 | 用户在手机点击、桌面转化，未登录 |
| Safari / iOS cookie 过期 | ITP 把 gclid cookie 压到 7 天（URL 带 `gclid` 参数时 24 小时） |
| 应用 ↔ 网页桥接 | iOS ATT 受限；gbraid / wbraid 部分归因 |

### 建模转化在界面如何呈现

- 「转化（Conversions）」列在置信度足够高时**包含**建模值
- **转化提升诊断（Conversion Uplift diagnostic）**展示建模占比提升，但**仅在建模按转化操作激活后前 30 天**——留档用
- 建模值最多 **5 天**稳定，可能被追溯上调

### 「正常」建模占比

| 建模占总转化 | 解读 |
|---|---|
| **EEA 流量中 < 10%** | **警告**：CMv2 可能配错了——横幅在标签后触发、默认不是「拒绝」、或未达国家 × 域名每周 700 点击门槛 |
| 10–30% | 同意率 50–70% 的健康 CMv2 正常范围 |
| 30–35% | 值得关注；检查同意率下降或横幅用户体验变更 |
| **> 35%** | **排查**：同意横幅可能过度压制，或域名切分太窄达不到建模门槛 |

（业内经验法则；Google 未发布官方基准。）

### 实操解读规则

- 把建模转化当**方向性**信号，不当可入账的。它告诉你「大概还有这么多转化发生了」；不告诉你哪次点击带来哪个转化。
- 建模占比高且 PMax / Demand Gen ROAS 看起来很美时，**单独算一遍纯点击 ROAS**做合理性检查。
- EC + GTG 上线后，建模占比通常下降——这是好事；实测占比上升，智能出价的信号更锐。
- CMv2 设置、EC 启用、归因模型变更前后，不要直接对比同比转化总数，加注脚。

---
## 8. 归因模型

### 可用模型

转化操作只剩**两个**可选模型：

| 模型 | 行为 | 何时用 |
|---|---|---|
| **数据驱动归因（DDA, Data-Driven Attribution）** | 贝叶斯 / 机器学习模型基于账户内实测转化路径，把功劳分配到各触点 | **默认**；几乎所有账户都推荐 |
| **最终点击（Last Click）** | 100% 功劳给转化前最后一次点击 | DDA 不可信时（量级极低、刚上线账户，或你明确需要一个保守参照） |

四个规则模型（首次点击、线性、时间衰减、位置模型）已不再支持，自动迁移到 DDA（[Google Ads Help](https://support.google.com/google-ads/answer/6259715?hl=en)）。

### DDA 资格

- 大多数网页 / 线索转化操作**无最低门槛**——DDA 是新建操作的默认
- 部分应用和到店操作仍有最低门槛：30 天内 ≥ 3,000 广告互动且 ≥ 300 转化才能启用；≥ 2,000 / ≥ 200 才能保持资格
- 稳定建模推荐：30 天内 ≥ 200 转化且 ≥ 2,000 广告互动

### 范围与限制

DDA 评估手机 / 桌面 / 平板全路径，覆盖搜索、展示、购物、YouTube、Discover、Gmail 和 Performance Max——包括网页 → 应用 → 应用内旅程。

**关键限制：**DDA 只给 **Google 系渠道**记功。Meta、TikTok、自然流量、邮件等非 Google 触点不可见。这是平台 ROAS 系统性高估 Google 真实贡献的结构性原因。

### 归因窗口

| 窗口类型 | 范围 | 默认 |
|---|---|---|
| 点击归因 | 1–90 天 | **30 天**（大多数转化操作） |
| 浏览归因 | 1–30 天 | **1 天** |
| 互动观看归因 | 1–30 天 | 网页转化 / 线下导入和到店 / 到店销售**3 天**；视频行动 / Demand Gen / P-MAX 和 Web to App Connect 应用内操作**1 天**；ACi **2 天**；ACe **1 天** |

按转化延迟选窗口，不按虚荣心。窗口过长会虚增计数、拖慢学习；窗口过短会漏计高考虑度购买。

### 跨账户归因

MCC / 经理账户层面的跨账户转化跟踪会**覆盖**子账户归因设置。开启时报告要在经理层级查看。多账户架构的归因模型和转化操作设计在经理层级规划。

---

## 9. 浏览转化（VTC, View-through conversions）与互动观看转化（EVC, Engaged-view conversions）

VTC 衡量看了广告没点击、之后转化的用户。适用于所有有展示型资源的地方：展示、视频、PMax、Demand Gen、应用。**全账户对 VTC 一视同仁**，不要让政策随广告系列类型漂移。

### 按广告系列类型的默认窗口与计入规则

| 广告系列 | 默认非点击窗口 | 默认是否计入「转化」列 |
|---|---|---|
| 搜索 | n/a（点击驱动） | n/a |
| 展示 | 1 天 | **否**——只在「浏览转化」列上报 |
| 购物 | 1 天 | **否** |
| 视频行动 | 1 天互动观看 | **EVC 计入，标准 VTC 不计** |
| 应用——安装 | 2 天安装 EVC / 24 小时 VTC（开启 VTC 优化时） | **符合条件的优化后浏览信号计入** |
| 应用——互动 | 1 天互动 EVC / 24 小时 VTC（开启 VTC 优化时） | **符合条件的优化后浏览信号计入** |
| **Performance Max（标准）** | 1 天互动观看 | **EVC 计入；标准 VTC 不计** |
| **Performance Max 到店目标（Store Goals）** | 1 天 | **计入** |
| **Demand Gen** | 1 天互动观看；开启 VTC 优化时建议 1 天 VTC | **EVC 计入；VTC 仅在开启 VTC 优化时计入。平台对比列可仅上报用计入 VTC** |

参考：[View-through conversions](https://support.google.com/google-ads/answer/16542520)、[Engaged-view conversions](https://support.google.com/google-ads/answer/10048752)、[Demand Gen VTC optimization](https://support.google.com/google-ads/answer/16399666)、[Demand Gen Platform Comparable columns](https://support.google.com/google-ads/answer/15299024)。

也就是说，标准 PMax、展示、购物和搜索**通常**不把标准 VTC 计入主要转化列。但视频互动符合条件时仍可计入 EVC。Demand Gen 要格外小心：普通转化可含 EVC；VTC 优化开启后 YouTube 的 VTC 可参与出价；平台对比列计入 VTC 只为跨平台上报，不影响出价。

### 互动观看转化（EVC）

- **可跳过插播（Skippable in-stream）**：观众观看 ≥ 10 秒未点击，之后在窗口内转化
- **信息流 / Shorts**：观众观看 ≥ 5 秒未点击，之后在窗口内转化
- 适用于视频、应用、展示视频资源、Demand Gen 和 Performance Max
- EVC 窗口默认：视频行动 / Demand Gen / P-MAX 为 1 天；到店 / 到店销售为 3 天
- EVC 流入所支持广告系列类型的**转化**和**所有转化**列

### VTC 处理政策（本 skill 默认）

| 原则 | 做法 |
|---|---|
| **默认 = 先监控** | 单独跟踪标准 VTC；不把纯展示功劳当主要证据，除非广告系列明确启用了 VTC 优化，或是 PMax 到店目标 / 符合条件的应用 |
| **窗口从短** | VTC 默认 1 天；窗口越长因果越模糊 |
| **不要盲混点击 + EVC + VTC** | 做重大决策时，把点击、EVC、含 VTC 的 CPA / ROAS 分开算 |
| **盯 VTC 占比** | 某广告系列 VTC > 总转化 50%，其直接效果很可能被高估 |
| **按季度审计 Demand Gen VTC 优化和 PMax 到店目标** | 这些会让 VTC 参与出价；用业务结果重新校准 |

### VTC 红旗

| 症状 | 可能原因 | 动作 |
|---|---|---|
| PMax 到店目标或 VTC 占比高的报告中，VTC > 总上报转化 50% | 展示 / YouTube 投放过重、低质版位、频次过高 | 加品牌排除、审计版位报告、用渠道报告 / 广告事件类型，并用门店 / POS 或后端数据对账 |
| 开启 VTC 优化后 Demand Gen VTC > 70% | 受众太窄 → 频次过度曝光或展示功劳虚增 | 放宽受众信号、加新创意、拆分拓客 / 再营销，看点击 + EVC 表现 |
| 应用 VTC > 60% | iOS 归因噪声或再营销占比过高 | 审计 ATT 选择率，拆分拓客与再互动 |
| 上线展示后 ROAS 飙升 | 新增 VTC 捡漏，不是新增营收 | 验证业务营收动了没有；没动就让 VTC 可见但重新加权 |

---

## 10. GA4 与 Google Ads——谁是真相来源

Google Ads 转化数、GA4 转化数、CRM / POS 营收**永远不会完全一致**。这很正常，正确的反应是知道哪个数字驱动哪个决策——而不是「修」到它们一致。

### 数字为什么对不上

| 差异 | 原因 |
|---|---|
| Google Ads 转化数 > GA4 转化数 | Google Ads 计数建模转化；GA4 只计直接归因事件。EC 找回的转化 Google Ads 能看到、GA4 看不到 |
| GA4 会话数对不上 Google Ads 点击数 | 机器人过滤、重定向中 gclid 丢失、单页导航差异、会话超时 vs 点击事件 |
| GA4 把某转化归到 "google / cpc"；Google Ads 没看到 | 基于 cookie 的 gclid 过期或被剥离；用户在窗口期后回来 |
| Google Ads 转化时间 vs GA4 转化时间不同 | Google Ads 按**点击日期**归档；GA4 按**事件日期**归档 |

### 真相来源规则

| 决策 | 真相来源 |
|---|---|
| **出价决策**（智能出价、目标 CPA / ROAS、Google 广告系列内预算分配） | **Google Ads** 转化数据——这是智能出价唯一能吃的数据 |
| **跨渠道绩效上报**（Google vs Meta vs 自然 vs 邮件） | **GA4**（UTM 卫生做好）或摄入全渠道的 DWH / MMP |
| **业务上报与财务** | **CRM / POS / DWH / 财务真相来源**——永远不要跨渠道加总平台上报营收 |
| **诊断标签 / 跟踪问题** | **GA4 DebugView + Google 标签诊断**并行 |

### 不要加总平台上报营收

如果 Google Ads 上报 $100k 营收、Meta 上报 $80k 营收，你并没有 $180k 的广告驱动营收。每个平台都会认领涉及对方的旅程。业务上报用单一真相来源（CRM、归因一致的 GA4、或 MMM）；平台数字只用于平台内优化。

---
## 11. iOS 与应用衡量

iOS 衡量是一套与网页根本不同的栈。单独规划。

### ATT（应用跟踪透明度，App Tracking Transparency）规划参考范围

| 类别 | 选择率（Opt-in rate） |
|---|---|
| 行业平均 | ~38% |
| 游戏整体 | ~39%（体育 50%、超休闲 43%、动作 40%） |
| 教育 | ~14%（从 2023 年的 7% 上升而来） |
| 出版 | ~26%（一年前 18% 上升而来） |
| 金融 / 电商 / 工具 | 20–30% |

这些是方向性规划参考；应用类别、地区、提示设计都会实质性影响实际值。

运营含义：仅靠 ATT 可归因信号太薄，撑不起 iOS 的 tCPA / tROAS。Google 用多种信号混合补偿：(1) ATT 同意的 IDFA 转化，(2) 设备端衡量（ODM, on-device measurement），(3) SKAN 聚合，外加 (4) 2025 年 11 月起的集成转化衡量（ICM, Integrated Conversion Measurement）。

### SKAdNetwork 4.0

SKAN 4 是本规划指南的生产基线。SKAN 5 在 WWDC 2023 讨论过，后被并入 AdAttributionKit（AAK）；苹果已从开发者文档删除 SKAN 5 引用。

| 功能 | 细节 |
|---|---|
| 回传（Postbacks） | 每次安装最多 3 次，窗口 0–2 / 3–7 / 8–35 天 |
| 转化值 | 回传 1 带 64 值细粒度；回传 2、3 为粗粒度（低 / 中 / 高） |
| 来源标识符（Source identifier） | 4 位层级，带人群匿名分层 |
| 网页到应用（Web-to-app） | 支持 Safari 广告驱动的 App Store 打开 |
| Google Ads + SKAN | 2024 年 3 月 19 日起上线；经 Google Ads API 的聚合数据，延迟约 72 小时；界面 SKAdNetwork 转化报告可见 |

**转化值结构设计**：把你的**营收分层**映射到回传 1 的 64 个细粒度槽位。不要把细值浪费在漏斗阶段事件上——粗桶（回传 2 / 3）足够做阶段跟踪。

### AdAttributionKit（AAK）

iOS 17.4 引入；WWDC 2025 / iOS 18.4 大幅升级（重叠转化窗口、可配置冷却期、地理级回传数据）。**采用率仍然低**——Meta、Google、Snap 主要还在 SKAN 上。为 AAK 迁移做准备，但**不要现在就依赖它**；它不是 Google 应用广告系列的主要衡量框架。

### 集成转化衡量（ICM, Integrated Conversion Measurement）

Google 自有的苹果框架替代方案。iOS 版**2025 年 11 月 12 日起公开测试 GA**。通过 MMP 集成（Singular、Branch、Adjust）加 Firebase ODM 的去标识化事件级信号，实质性增加无双重 ATT 同意用户的可观测 iOS 转化。自助开启。

**从业者默认：任何有意义的应用广告系列都开启 ICM iOS。**这是目前 iOS 应用广告最大的单项衡量升级。

### Firebase + Google Ads 管道

```
GA for Firebase SDK ──→ GA4 property ──→ Google Ads account
       (≥ 11.14.0)         (linked)        (linked)
```

- **SDK 版本要求**：≥ 11.14.0（2025 年 6 月）才能完整集成 iOS ICM + ODM
- 2025 年 12 月 3 日起，GA4 新上报转化的自动导入成为默认
- ODM（设备端衡量）在 EEA、英国和瑞士**禁用**
- GA4 事件标记为**关键事件（Key Events）**，再在 Google Ads 中指定为转化

### 应用广告系列出价阶梯

| 阶段 | 出价类型 | 升级门槛 |
|---|---|---|
| 新应用，无安装信号 | **ACi tCPI** | 日预算 ≥ 50× 目标 CPI |
| 有安装，想要质量 | 目标应用内事件的 **ACi tCPA** | 目标事件日转化 ≥ 10；日预算 ≥ 10× 目标 CPA |
| 成熟，在变现 | **ACi tROAS** | 日营收转化 30–50+；转化窗口 < 30 天且覆盖约 90% 营收 |
| 再互动 | **ACe tCPA** | 应用安装量 ≥ 50,000；深链 + 受众列表；预算 ≥ 15× 目标 CPA |
| 预注册（Android） | **ACpre** | 预算 ≥ 50× 目标 CPI |

把这些当作规划门槛，上线前在界面确认当前应用广告系列要求。

### 2026 年 iOS 从业者要点

1. **开启 ICM iOS**——当前 iOS 最大的单项衡量升级。
2. **Firebase iOS SDK 升级到 ≥ 11.14.0**——完整集成必需。
3. **在价值时刻规划 ATT 提示**——新手引导后 / 首次胜利后，本地化预提示。2025–2026 年选择率提升 5–10 个点的类别（出版、教育）都改了时机 / 话术。
4. **不要过早升级出价类型**——尊重 10 / 30–50 的日量级门槛；tROAS 下信号太薄会导致花费剧烈波动。
5. **SKAN 回传 1 映射营收分层，不是事件**——细值是你唯一的 iOS 高分辨率信号。
6. **把同意模式 v2 当作跨设备的关键基础设施**（2026 年 6 月 15 日 Google Signals 降级后，CMv2 成为跨设备 ID 的唯一控制）。
7. **为 AAK 做准备但不依赖**——在 Google 切换之前，保持 SKAN 4 + ICM 栈。

---

## 12. 增量性测试

在 Google Ads 内部，提升研究衡量的是 Google 视角下的 Google。捕捉不到跨平台泄漏、自然搜索替代或 Meta 再营销重叠。用它们，但要三角验证。

### 方法对比

| 要回答的问题 | 方法 | 最低花费 / 数据 | 解读耗时 | 2026 年自助可用？ |
|---|---|---|---|---|
| YouTube 带动品牌指标？ | **品牌提升（Brand Lift）** | $5–10k / 4 周 | 4 周 | **否**——需要 Google 客户代表 |
| YouTube 带动品牌搜索？ | **搜索提升（Search Lift）** | $10k / 4 周 | 4 周 | 自助（视频、Demand Gen） |
| Demand Gen / 视频转化增量？ | **用户转化提升（User Conversion Lift）** | $5k / ~1k 转化 | 2–4 周 | 自助（视频、Discovery、Demand Gen）；搜索 / 展示 / 购物 / PMax 需要客户代表 |
| PMax 在搜索 / 购物之上增量？ | **PMax 提升实验（PMax Uplift Experiment）** | $5k+ | 4–6 周 | **自助** |
| 多渠道广告系列全国增量？ | **地理对照（Geo holdout）**（GeoLift / CausalImpact） | 6 个月以上地理数据，≥ 20 个地理单元 | 测试 4–8 周 + 分析 | DIY / 代理商 |
| 邮件 / CRM 推送带来净新增营收？ | **客户匹配对照（Customer Match holdout）** | 列表 ≥ 50k 成员 | 2–4 周 | DIY |
| 明年渠道预算怎么分？ | **MMM** | 2 年周度数据 | 初始建模 6–12 周 | 分析团队 / 供应商 |
| 打开 X 之后营收变了吗？ | **CausalImpact 前后对比** | 3 个月以上前测期、对照序列 | 后测 2–4 周 | DIY |

### 转化提升（Conversion Lift，Google Ads 原生）

- **2025 年 11 月 11 日变更**：切换到贝叶斯推断后，最低花费从约 $100k 降到 **$5,000**。对中小企业是重大民主化。
- **上报**：贝叶斯可信区间，不是频率学派置信度。读法如「该广告系列提升转化的概率为 90%」。默认上报带是 80% 可信区间；90% 是头条确定性目标。
- **自助**：视频、Discovery、Demand Gen——完全自助。尽管 $5k 门槛适用，**搜索、展示、购物、Performance Max 在很多账户仍需客户代表协调**。
- **方法**：用户提升（User Lift，按 cookie / 用户随机，需登录量）和地理提升（Geo Lift，地理对照，抗信号丢失更强）。
- **2026 年界面**：提升研究在**实验（Experiments）**标签页下。

### 品牌提升（Brand Lift）

- **最低门槛因国家、账户、衡量指标和购买路径而异。**承诺品牌提升研究前，先在界面或与 Google 客户代表确认资格、花费门槛和搭建路径。
- **推荐的规划姿态**：展示和预算要足够检出所选指标；不要给触达很小的广告系列承诺读数。
- **衡量**：广告回忆、知名度、考虑度、好感度、品牌兴趣、购买意向、品牌联想。
- **方法**：产品内问卷，曝光组 vs 对照组用户。
- **自助**：大多数账户不支持自助——需要 Google 客户代表。

### PMax 提升实验

- 衡量在现有搜索 + 购物 + 展示之上*新增* PMax 的增量转化
- 2025 年起在实验标签页自助；2024–2025 年广泛推出
- 两种相关实验类型：
  - **升级实验（Upgrade Experiments）**——模拟把标准购物 / DSA / GDA 预算转到 PMax
  - **优化实验（Optimization Experiments, Beta）**——在 PMax 广告系列内 A/B 测试素材变体
- 从业者验证过的测试模式：PMax 排除品牌搜索 vs 包含；蚕食症状交叉参考 [diagnostic-decision-trees.md](diagnostic-decision-trees.md)

### 地理对照 / DIY（跨平台问题最站得住脚）

| 方法 | 严谨度 | 工具 |
|---|---|---|
| **合成控制（Synthetic control）**(Abadie) | 高 | 手搓 R / Python、[GeoLift](https://github.com/facebookincubator/GeoLift) |
| **CausalImpact**（贝叶斯结构时间序列） | 高 | [google/CausalImpact](https://github.com/google/CausalImpact) R 包、`tfp-causalimpact` Python |
| **双重差分（Difference-in-differences）** | 中 | 普通回归 |
| 粗配对市场（肉眼配对） | 低 | 避免——站不住脚 |

**GeoLift 最低数据**：≥ 6 个月日度或 ≥ 1 年周度地理层级数据；≥ 20 个不同地理单元；清晰的 KPI 时间序列。

**常见坑**：
- 相邻市场 / 区域间的溢出污染对照组
- 处理组地理单元里还有其他广告系列在跑
- 前测期太短，对照拟合不稳定
- 把单次测试当结论——地理测试置信区间宽；要复现

### 客户匹配对照

**功效所需样本量**（公式：`n = (Z₁₋α/₂ + Z₁₋β)² × 2σ² / Δ²`）：

- 2% 基准转化率、20% 相对最小可检效应、95% 置信、80% 功效 → 每组约 **25,000**（列表总计至少 50k）
- B2B 在 2–3% 转化率 → 每组 12,700–19,200
- **实操下限**：列表 50,000+ 成员；低于 10k 是演戏，不是衡量

**跨平台泄漏**是结构性弱点。Google 压不住对照组用户在 Meta、TikTok、自然流量或邮件里被触达。干净的版本是同时在所有付费渠道协调对照；否则你衡量的只是「叠加在其他一切之上的 Google」。

### MMM 作为高级附录话题

MMM 在跨渠道预算分配比广告系列内优化更重要时有用，通常需要至少 2 年周度数据。工具选择交给分析团队；本规划 skill 的重要建议是：

- 用 MMM 做自上而下的渠道分配和饱和问题。
- 用提升研究或地理对照对具体广告系列做更快的因果解读。
- 平台归因只做投放中的方向盘，不做最终业务真相。
- MMM、提升、平台数据三角验证，不要把任何一个模型当作定论。

### 诚实提醒

1. 尽管 Google 宣传「人人 $5k」，搜索 / 购物 / PMax 的自助转化提升在很多账户仍需客户代表。
2. 贝叶斯可信区间依赖 Google 的先验——不完全透明。
3. 围墙花园内的提升会高估纯 Google 增量（Meta + 自然泄漏）。
4. 地理测试在国家层面受样本量限制；剔除主要市场后，可比的测试 / 对照区域数可能太少，检不出温和提升。
5. **PMax 对搜索的蚕食仍是最大的未解决衡量问题。**Optmyzr（2025 年 2 月，n=503）和 Adalysis（3,300 个 PMax 广告系列，120 万搜索词）都记录了严重重叠；标准报告把双方都归到 PMax。
6. MMM 的质量取决于吃进去的数据——短或噪声大的历史会产出自信的错误答案。
7. 客户匹配 50–70% 的匹配率意味着你的对照只有部分是「真」的。
8. 隐私 / 信号丢失持续削弱基于用户的方法。2025 年从业者共识已转向以地理和 MMM 为主、用户级提升为辅。

---
## 13. 衡量健康检查

### 每日

| 检查 | 看什么 |
|---|---|
| 花费异常 | CPC 突然飙升、日预算消耗偏离 > 30% |
| 转化量下跌 | 转化数低于 7 天滚动均值减 2σ |
| 标签触发 | 标签诊断——有无「近期无触发」告警 |
| 拒登 / 政策 | 拒登广告、Feed 拒登（购物 / PMax） |

### 每周

| 检查 | 看什么 |
|---|---|
| 建模转化占比 | EEA 流量在 10–35% 预期带内；超出 = 排查 |
| EC 覆盖率 | 附带有效 EC 数据的转化 ≥ 70% |
| 各广告系列 VTC 占比 | 看趋势；跨 50% 的标红 |
| 搜索词 / 版位 | 不相关查询 / 版位的浪费花费 |
| GA4 ↔ Google Ads 转化数差异 | 方向稳定；突然分叉要标红 |

### 每月

| 检查 | 看什么 |
|---|---|
| 同意率趋势 | 横幅用户体验变更、区域变化、选择率停滞 |
| 建模 vs 实测占比 | 看趋势；实测占比恶化 = 栈退化 |
| OCI / ECfL 上传健康 | 匹配率、拒绝原因、延迟 |
| 归因窗口是否合适 | 转化延迟分布仍在所选窗口内 |

### 每季度

| 检查 | 看什么 |
|---|---|
| 转化操作重设计 | 主要转化还是最深层可靠信号吗？ |
| 增量性复核 | 对重要广告系列跑提升研究或地理对照 |
| 品牌 vs 非品牌上报 | 品牌仍被隔离，没有掩盖非品牌表现 |
| 跨平台对账 | Google Ads + GA4 + CRM / POS 仍绑在一起 |

---

## 14. 常见坑与修复

| 症状 | 可能原因 | 修复 |
|---|---|---|
| 转化数突然翻倍 | 感谢页转化标签每次页面加载都触发（SPA 路由变更，或标签重复） | 改为基于事件触发；用 GTM 预览 + Tag Assistant 验证；`transaction_id` 去重 |
| 建模转化占比突然上升 | 同意率下降，或横幅变更 | 审计 CMP 变更、横幅版本历史、同意接受率 |
| EEA 建模占比 < 10% | CMv2 配错（横幅在标签后触发、默认不是「拒绝」、或未达每周 700 点击门槛） | 检查默认同意状态、横幅加载顺序、国家 × 域名量级 |
| GA4 和 Google Ads 转化剧烈分叉 | 重定向中 gclid 丢失、归因窗口差异、建模、EC 覆盖率 | 审计重定向、点击 ID 采集、EC 覆盖率；接受一定差异 |
| ROAS 飙升但利润没动 | 建模转化虚增，或 VTC 捡漏，或品牌占比上升 | 分解：建模 vs 实测、点击 vs VTC、品牌 vs 非品牌 |
| 线索 CPA 看着好但 SQL 率掉了 | 出价策略在学低质量线索 | 主要转化切到 ECfL / OCI 的合格线索；重跑 2–3 周 |
| 线下导入导致出价波动 | 学习期不规则大批量上传 | 改为每天或每小时导入；学习期避免大回填 |
| Demand Gen CPA 看着差 | 最终点击 CPA 可能低估漏斗中部价值，而平台对比或 VTC 优化视角可能高估 | 分开看点击 vs EVC vs 展示；考虑对业务结果的混合 CPA |
| PMax 营收很好，业务营收没动 | 品牌捡漏、再营销偏置、EVC / VTC 解读、低毛利产品组合 | 品牌排除、NCA 模式、按广告事件类型和毛利分段（见 [pmax.md §7](pmax.md)） |
| iOS 应用 tROAS 不稳 | 量级低于日 30–50 营收转化；SKAN 信号稀疏 | 退回 ACi tCPA、深化 ATT 提示用户体验、开启 ICM |
| EEA / UK / CH 广告系列悄悄丢了再营销 | 受管制区域用户的 CMv2 / 必需同意信号缺失 | 正确重配 CMv2；丢失的期间不可恢复 |
| 转化调整不影响出价 | 调整在转化后 > 7 天才到 | 改为 7 天内导入节奏；周期很长的切主要转化为更近的代理指标 |

---

## 附录 A：按账户规模的最小可用衡量配置

「够用」的栈随花费规模走。账户没到那个量级别过度工程；到了别欠工程。

### 月花费 < $5,000

- Google 标签（客户端）经 GTM
- 自动检测的增强型转化
- 单一主要转化操作；次要做监控
- 大多数网页 / GA4 转化操作默认 DDA；最终点击只做有意的保守参照
- 有 EEA / UK / CH 流量才上 CMv2（那里强制）
- 不需要 sGTM、GTG
- 跳过增量性测试——量级太薄

### 月花费 $5,000–$50,000

- Google 标签 + GTM + 手动增强型转化（CSS 选择器 / GTM 变量）
- **Google Tag Gateway** 开启
- 有 EEA / UK / CH 触达就上 CMv2 高级版
- B2B 线索经 Data Manager / Zapier 做 ECfL
- 符合条件时在 Demand Gen / 视频上做转化提升（$5k 门槛）
- 每季度 PMax 提升实验

### 月花费 $50,000–$250,000

- 以上全部，外加：
- 可行时服务器端哈希 EC 负载
- CRM 驱动业务经 API 做 OCI / ECfL
- 留存 / 再互动做客户匹配对照
- 重要广告系列每季度转化提升
- 有 ≥ 2 年数据时每年或每半年做一次 MMM 解读

### 月花费 > $250,000

- 全套 sGTM（Stape / GCP / Tag Pilot）+ GTG 组合
- 多平台 CAPI（经 sGTM 并行 Meta、TikTok）
- DWH 锚定的归因与对账
- 持续增量性计划：滚动提升研究 + 地理对照 + MMM 三角验证
- 专职衡量负责人（内部或合作）

---

## 交叉参考

- 转化设计与主要 / 次要政策在 SKILL 层 → [SKILL.md §3 — 转化设计](../SKILL.md#conversion-design)
- 按广告类型的 VTC / EVC 处理政策 → [SKILL.md](../SKILL.md)
- 预算 vs 信号可行性 → [budget-planning.md](budget-planning.md)
- 故障账户的诊断决策树 → [diagnostic-decision-trees.md](diagnostic-decision-trees.md)
- PMax 专属衡量细节（素材组报告、品牌排除、基于点击的自定义转化） → [pmax.md](pmax.md)
- Demand Gen 专属衡量（渠道报告、NCA 模式） → [demand-gen.md](demand-gen.md)
- 搜索专属衡量（品牌 vs 非品牌隔离） → [search-ads.md](search-ads.md)
- 应用专属衡量细节（事件深度、SKAN 结构设计） → [app-campaigns.md](app-campaigns.md)
