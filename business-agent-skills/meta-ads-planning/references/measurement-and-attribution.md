# Meta 衡量、转化 API（CAPI）与归因

在敲定转化事件、Advantage+ 就绪度、出价策略或报告之前，使用这份参考文档。Meta 的投放质量高度依赖事件流和事件的业务含义。


## 核心原则

- 只用 Pixel 通常撑不起严肃的转化项目。当转化优化重要时，用 Pixel + 转化 API（CAPI, Conversions API）。
- 用相同的 `event_id` 对浏览器端和服务端事件去重。
- 用量级足够、延迟可接受的最深层可靠事件。
- 事件匹配质量（EMQ, Event Match Quality）是诊断工具，不是业务 KPI。可以改善它，但不要把分数当作因果提升的证据。
- 平台的每次转化费用/广告投入回报率是优化信号，不是财务真相。
- 有条件时，拆分点击转化、浏览转化、模型估算、新增客户、既有客户和增量视角。
- 自 Meta 2026 年 3 月的归因更新起，有条件时还要拆分**互动转化（engage-through）**。

---

## 信号栈

| 层 | 核验内容 | 为什么重要 |
|---|---|---|
| Pixel | 标准事件只触发一次，且在正确的页面/动作上触发 | 浏览器信号与事件覆盖 |
| 转化 API | 服务端事件低延迟发送 | 在浏览器/隐私损耗下信号更耐用 |
| 去重 | Pixel 与 CAPI 对同一事件共用 `event_id` | 防止重复计数或事件被丢弃 |
| 客户参数 | 在政策/同意允许时传邮箱、电话、外部 ID、`fbp`、`fbc`、IP、用户代理 | 提升匹配与归因质量 |
| 事件语义 | `Purchase`、`Lead`、价值、币种和自定义事件保持一致 | 防止学习跑偏和错误对比 |
| 业务回传 | CRM 阶段、线下销售、退款、毛利、生命周期价值（LTV, Lifetime Value） | 让优化对齐真实价值 |

---

## 事件选择

| 业务类型 | 首选事件路径 | 说明 |
|---|---|---|
| 电商 | 购买 -> 利润/价值感知的购买 | 加购（ATC, AddToCart）/发起结账（IC, InitiateCheckout）只作临时的已验证代理 |
| 潜客获取 | 潜在客户 -> 合格潜客 -> 商机/成交 | 原始潜客量可能有害 |
| SaaS | 注册/试用 -> 激活用户/产品合格潜客（PQL, Product Qualified Lead） -> 订阅/生命周期价值 | 激活与留存比注册数更重要 |
| 应用 | 安装 -> 应用内事件 -> 购买/价值 | 不要永远优化安装 |
| 本地业务 | 预约/合格来电 -> 完成工单/POS 销售 | 来电处理与服务区域质量很重要 |

### 最深层可靠事件规则

选择在价值、量级和延迟之间最平衡的事件：

```
Best business event
  ↓ if too sparse or too delayed
Validated proxy event
  ↓ if proxy quality is unproven
Higher-volume event for learning, with explicit quality risk
  ↓ once signal improves
Move back toward the deeper event
```

---

## 事件匹配质量（Event Match Quality）

通过发送干净、已获同意、格式正确的标识符来提升事件匹配质量：

- 有条件时传哈希邮箱和电话。
- 登录用户或 CRM 记录传 `external_id`。
- 浏览器标识如 `fbp` 和 `fbc`。
- 允许时为服务端事件传 IP 地址和用户代理。
- 姓名、电话格式、国家/州/邮编字段一致，邮箱小写归一化。

把 EMQ 当作健康目标。除非账户、实施方式和来源确实需要，否则避免"必须 8 分以上"这类硬性规则。

---

## 归因与报告

| 问题 | 建议立场 |
|---|---|
| 点击转化 | 在新归因模型下视为由链接点击驱动的转化；如果定义变过，与历史周期对比要谨慎 |
| 互动转化 | 与点击转化分开。它可能包含非链接互动和 5 秒视频观看后 1 天窗口内的转化 |
| 浏览转化 | 尽量保持可见但单独列示；浏览转化占比高可能夸大因果效应 |
| 再营销广告投入回报率 | 在增量测试之前保持怀疑 |
| Advantage+ 销售广告系列的广告投入回报率 | 对照新客占比、业务收入、毛利和增量来看 |
| 长决策周期 | 用更长的评估窗口，并与 CRM/真实来源对账 |

| API/报告变更 | 在依赖某个窗口或指标之前，先核验当前 Ads Manager/API 的行为 |

归因窗口的可用性可能因 Ads Manager 界面、API 版本、目标、事件类型和报告工具而异。在断言 7 天/28 天浏览或 28 天点击窗口可用或已移除之前，先核验当前 Ads Manager/API 的行为。

### 互动转化（Engage-through）归因

Meta 2026 年 3 月的归因更新把点击转化归因收窄到链接点击，并为社交原生互动引入/更名了**互动转化（engage-through）归因**。把它当作重要的报告拆分：

- 包括点赞、表情回应、评论、分享、收藏、主页/非链接互动，以及有条件时的有效视频观看。
- 使用 1 天窗口。
- 视频有效观看阈值按 5 秒报告；视频短于 5 秒时为视频时长的 97%。
- 它有助于解释 Meta、GA4 与后端系统之间的差异，但它不等同于基于点击的访问。
- 对转化类广告系列，在账户/工具支持时，把点击转化、互动转化和浏览转化分开报告。

---

## 增量（Incrementality）

预算和量级允许时，用增量方法：

| 方法 | 适合 | 注意 |
|---|---|---|
| Meta 实验/A-B 测试 | 战术性设置或创意测试 | 仍是平台内运行 |
| 转化提升（Conversion Lift） | 对 Meta 影响的因果解读 | 需要足够量级和准入资格 |
| 地域对照组（Geo holdout） | 渠道/账户级增量 | 需要稳定的地域市场和足够花费 |
| 客户/CRM 对照组 | 既有客户或生命周期广告系列 | 需要干净的客户名单 |
| 媒介组合模型（MMM, Marketing Mix Modeling） | 更大体量的项目与跨渠道规划 | 需要历史数据和谨慎建模 |
| 财务对账 | 常开的合理性检查 | 本身不是因果证据 |

外部增量基准只作参考。不要把任何平均结果当作单个账户的通用预测。

---

## 潜客质量闭环

对潜客获取，定义：

| 阶段 | 示例 | 用途 |
|---|---|---|
| 原始潜客 | 即时表单提交 | 量级与前端每次潜在客户费用（CPL, Cost Per Lead） |
| 合格潜客 | 销售认可、MQL、SQL | 更好的优化/报告目标 |
| 商机 | 商机创建 | 业务质量 |
| 成交 | 收入 | 真实来源 |

运营检查：

- 高价值潜客用高意向（Higher Intent）表单。
- 质量重要时加 1-3 个筛选问题。
- 量级和延迟允许时，把 CRM 阶段回传给 Meta。
- 监控首响速度；跟进慢会让好潜客看起来差。

---

## 常见衡量误区

| 误区 | 修复 |
|---|---|
| 严肃的转化花费只用 Pixel | 加 CAPI 和去重 |
| 永远优化原始潜客 | 导入合格结果或用更好的表单摩擦 |
| 中途改 `Purchase` 的价值语义 | 保持价值定义稳定并记录变更 |
| 把 Meta 广告投入回报率当财务真相报告 | 与订单、CRM、POS、毛利、生命周期价值对账 |
| 忽视既有客户 | 定义并监控客户分群 |
| 转化延迟还没过就下结论 | 用合适的窗口和稳定期 |

## 归因与 Ads Insights API 变更

### Ads Insights API 归因窗口变更

| 字段 | 值 |
|---|---|
| 生效日期 | 2026-01-12 |
| 变更 | `7d_view` 和 `28d_view` 不再通过 Ads Insights API 的 `action_attribution_windows` 参数返回数据 |
| 失效模式 | 静默：API 返回空数据集，而不是错误码。报告集成可能显示"0"而不是真相。 |
| 保留的窗口 | `1d_view`、`1d_click`、`7d_click`、`28d_click`、`1d_engaged-view`（2026 年 3 月后更名为互动转化）。Ads Manager 默认报告设置：7 天点击 + 1 天互动转化 + 1 天浏览。 |
| 历史数据保留 | 去重计数字段和按小时细分只保留 13 个月 |


规划影响：

- 任何硬编码了 `7d_view` 或 `28d_view` 的报告管道都在静默丢转化。该 skill 必须问：你的仪表盘请求的是哪个归因窗口，2026-01-12 之后迁移过吗？
- 跨越 2026-01-12 的同比对比必须标注这一变更。
- 浏览转化主导的品类（认知/视频/品牌）会完全失去可见的长尾浏览转化。

### 点击转化与互动转化的重新分类

| 字段 | 值 |
|---|---|
| 生效日期 | 2026 年 3 月（逐步 rollout，不是单日切换） |
| 点击转化重新定义 | 现在要求真实的站外链接点击（到网站、应用、潜客表单等） |
| 划归互动转化 | 点赞、分享、收藏、评论、主页点击、图片展开、短视频观看 |
| 互动转化窗口 | 1 天 |
| 互动转化视频阈值 | 5 秒（此前"有效观看"为 10 秒） |
| 默认归因设置（2026 年 3 月后） | 7 天点击 + 1 天互动转化 + 1 天浏览 |
| 计费影响 | 无。计费不变；只有分类和报告不同。 |


规划影响：

- 2026 年 3 月前 vs 3 月后，Ads Manager 中的转化数不可比。从 3 月中旬起建立新基准。
- Meta 与 GA4 的点击差异应该缩小，因为 Meta 的点击数现在更接近"站外链接点击"的语义。
- 互动转化不是"站内访问"；它是"社交原生互动后在同一窗口内的转化"。不要把它说成流量。

### 当前归因窗口

| 窗口 | API 名称 | Ads Manager 对比中可用 | Insights API 中可用 | 默认？ |
|---|---|---|---|---|
| 1 天点击 | `1d_click` | 是 | 是 | 对比的一部分 |
| 7 天点击 | `7d_click` | 是 | 是 | 默认点击窗口 |
| 28 天点击 | `28d_click` | 是 | 是 | 仅对比 |
| 1 天浏览 | `1d_view` | 是 | 是 | 默认浏览窗口 |
| 7 天浏览 | `7d_view` | 核验当前 API/UI 支持 | 核验当前 API/UI 支持 | 不要假定可用 |
| 28 天浏览 | `28d_view` | 核验当前 API/UI 支持 | 核验当前 API/UI 支持 | 不要假定可用 |
| 1 天互动转化 | `1d_engaged_view` 或当前等效值 | 核验当前 API/UI 支持 | 核验当前 API/UI 支持 | 与站外链接点击归因分开 |


注意：互动转化的 API 参数名可能滞后于界面命名。实际调用 API 并检查 `action_attribution_windows` 的回显来核验。

### 归因设置 vs 对比窗口 vs 默认行为归因窗口

这三个术语经常被混淆；规划 skill 必须把它们分清。

| 术语 | 位置 | 控制什么 | 效果 |
|---|---|---|---|
| 归因设置（Attribution Setting） | 广告组层级（单结果成本目标 > 更多选项） | 同时控制投放优化和报告 | 决定哪些转化计入出价优化器的学习，以及默认展示哪些列 |
| 对比窗口（Compare Attribution Settings） | Ads Manager 报告视图（列下拉菜单） | 仅报告 | 让用户并排查看 1d_click / 7d_click / 28d_click / 1d_view / 1d_engaged-view |
| 默认行为归因窗口（Default Action Attribution Window） | 账户级报告默认 | 仅报告 | 未设置覆盖时默认展示的归因列；可在账户层级配置 |


Skill 立场：读数时永远说明用的是三者中的哪一个。没有"7 天点击 + 1 天浏览"这个前提，"ROAS = 3.4" 没有意义。

### 互动转化归因：精确定义

| 项目 | 值 |
|---|---|
| 触发互动 | 点赞、分享、收藏、评论、主页点击、图片展开、≥5 秒的视频观看 |
| 排除 | 站外链接点击（那是点击转化） |
| 窗口 | 1 天 |
| 视频阈值 | 5 秒，低于旧的 10 秒"有效观看" |
| 报告形式 | 与点击转化、浏览转化分开的列（开启对比时） |

### 浏览转化（`1d_view`）

| 项目 | 值 |
|---|---|
| 定义 | 看到广告后 1 天内转化，且无互动 |
| 保留的窗口 | 只有 1 天。7 天和 28 天已于 2026-01-12 移除。 |
| 适用场景 | 曝光即计划机制的认知/视频/品牌广告系列 |
| 风险 | iOS 侧重度建模；不要当作确定性数据解读 |

### 模型估算转化与 Meta 的 DDA

Meta 不像 Google Ads 那样把自己的归因模型叫"DDA"，而是用统计建模、AEM 和 CAPI 的组合来填补隐私导致的缺口。

| 机制 | 作用 | 出现位置 |
|---|---|---|
| 聚合事件衡量（AEM, Aggregated Event Measurement） | 聚合 iOS 拒授权用户和其他受限场景的网页事件 | 网页 + 应用，2025-06 起自动生效（无需手动配置） |
| 模型估算转化 | 无法直接匹配时的统计估计；Meta 基于可观测用户的模式填补缺口 | 与观测转化一起在 Ads Manager 中报告 |
| AEM-for-app | 同样思路用于 iOS 应用转化；配合 SKAdNetwork/AdAttributionKit | 应用的聚合事件报告 |
| Meta 的归因模型 | 所选窗口内的末次触达，2026 年 3 月后互动转化与浏览转化作为独立渠道 | Ads Manager 默认模型 |


Skill 立场：Ads Manager 的数字混合了观测转化和模型估算转化，混合比例不公开。永远与后端收入/潜客对账，尤其在归因变更之后。

### 跨域衡量与 iOS 14.5+

| 项目 | 值 |
|---|---|
| iOS ATT | IDFA 需要用户主动授权；全球授权率约 25-30% |
| SKAdNetwork（SKAN） | 应用安装的确定性但聚合回传；转化值分桶 |
| AdAttributionKit | WWDC 2024 公布的 SKAN 后继/扩展；支持再互动和第三方应用商店 |
| Meta AEM（网页） | 对拒授权和受限用户的网页事件做隐私保护聚合 |
| Meta AEM-for-app | iOS 应用广告系列中的网页-应用跨端衡量与浏览转化报告 |
| 跨域网页衡量 | 单个 Pixel/CAPI 数据集可覆盖子域名；跨根域名需要关联独立数据集并做域名验证 |


### 聚合事件衡量（AEM）现状

| 字段 | 值 |
|---|---|
| 事件上限/优先级行为 | 在假定需要手动设置事件上限或优先级之前，先核验当前界面/文档 |
| 手动优先级 | 已移除；Events Manager 中的 AEM 标签页已消失 |
| 价值优化 | AEM 对 iOS 拒授权用户的所有合规事件的价值总和建模 |
| Pixel + CAPI 关系 | AEM 以两者为输入；对 Pixel 无法可靠观测的事件（购买后、服务端确认），CAPI 必不可少 |


Skill 立场：任何还在写"选择 8 个优先事件"的规划文档都已过时。

### 域名验证

| 字段 | 值 |
|---|---|
| 2026 年 AEM 网页的硬性要求 | 否（AEM 自动聚合；手动优先级已取消） |
| 强烈建议 | 是，为了拥有转化域名、干净地使用自定义转化，以及在商务管理平台（Business Manager）下认领主页/Pixel |
| 方法 | DNS TXT 记录、HTML 文件上传、meta 标签 |


Skill 立场：即使 AEM 不再硬性要求，也永远为主要转化域名做域名验证。验证还是部分商务管理平台控制项的前提。

---

## 转化 API（Conversions API）细节

### 服务端事件参数

每个服务端事件的必填项：

| 参数 | 是否必填 | 说明 |
|---|---|---|
| `event_name` | 必填 | 标准事件名称（Purchase、Lead、AddToCart 等）或自定义 |
| `event_time` | 必填 | Unix 时间戳（秒）。网页/应用事件：最多回传 7 天。线下 `physical_store`：最多 62 天。 |
| `action_source` | 必填 | 以下之一：`website`、`app`、`phone_call`、`chat`、`email`、`physical_store`、`system_generated`、`business_messaging`、`other` |
| `user_data` | 必填 | 哈希后的个人身份信息和标识符，用于把事件匹配到用户 |
| `event_source_url` | 当 `action_source = website` 时必填 | 事件发生的完整 URL |
| `event_id` | 强烈建议 | 用于浏览器端/服务端去重 |
| `client_user_agent` | 当 `action_source = website` 时必填 | 浏览器的 UA 字符串 |

`user_data` 常用字段（除 `client_ip_address`、`client_user_agent`、`fbp`、`fbc`、`external_id` 外，其余均为哈希）：

| 字段 | 描述 |
|---|---|
| `em` | SHA-256 小写邮箱 |
| `ph` | SHA-256 归一化电话（无 `+`，纯数字，带国家码） |
| `fn`、`ln` | 哈希的名/姓，小写，ASCII 归一化 |
| `ge` | 哈希的性别（`m`/`f`） |
| `db` | 哈希的出生日期，YYYYMMDD |
| `ct`、`st`、`zp`、`country` | 哈希的城市、州、邮编、国家 |
| `external_id` | 第一方用户 ID（建议哈希） |
| `client_ip_address` | 明文 IP |
| `client_user_agent` | 明文 UA |
| `fbp` | Facebook 浏览器 Pixel cookie |
| `fbc` | Facebook 点击 ID，格式 `fb.subdomain_index.timestamp.fbclid` |

`custom_data` 常用字段：`currency`、`value`、`content_type`、`content_ids`、`contents`（`{id, quantity, item_price}` 数组）、`num_items`、`order_id`、`predicted_ltv`、`status`、`delivery_category`。

关于优化资格的说明：除 `physical_store`（仅用于衡量）外，所有 `action_source` 值都支持优化。


### 浏览器端/服务端去重

| 项目 | 值 |
|---|---|
| 去重时间窗口 | 48 小时 |
| 主要去重键 | `event_name` + `event_id` |
| 浏览器优先 | 当两者在约 5 分钟内都到达时，Meta 优先采用浏览器端事件 |
| 备选去重 | `fbp` + `external_id` 匹配存在但不太可靠；不是推荐方法 |
| 去重核验 | 用 Events Manager > 测试事件（Test Events）和诊断（Diagnostics）标签页确认去重正在生效 |
| 失效模式 | `event_name` 大小写不同（`Purchase` vs `purchase`）；每端各自重新生成 `event_id`；`event_id` 只在一端发送 |


#### 去重失效模式

| 失效 | 症状 | 修复 |
|---|---|---|
| 浏览器端与服务端各自生成 `event_id` | 转化重复计数；平台广告投入回报率虚高 | 在真实来源（服务端或共享客户端缓存）生成一次 ID，传给 Pixel 和 CAPI 两端 |
| `event_name` 大小写不一致 | 去重遗漏；重复计数 | 统一用 Meta 的精确大小写（如 `Purchase`）；在代码评审中拒绝自定义大小写漂移 |
| 服务端事件晚于浏览器端 > 48 小时 | 去重遗漏；重复计数 | 服务端事件在浏览器事件后数秒内发送；绝不批量延迟数小时 |
| 服务端事件早于浏览器端 > 5 分钟且没有 `event_id` | 浏览器优先逻辑不适用 | 即使浏览器先触发，也永远带上 `event_id` |
| 浏览器端与服务端价值/币种不同 | 报告漂移；优化不稳定 | 以服务端为真实来源；可选择在浏览器端压住价值不上报 |
| 用 external_id 作去重键而没有 `event_id` | 跨事件冲突 | 以 `event_id`+`event_name` 为主；external_id 只当用户数据，不当去重键 |
| 网站服务端事件缺 `event_source_url` | 事件被拒绝或降权 | 当 `action_source=website` 时永远带上 `event_source_url` |
| 网页服务端事件缺 `client_ip_address` 和 `client_user_agent` | 匹配质量更低，EMQ 更低 | 从浏览器请求中捕获两者并转发到服务端 |

### 经 CAPI 的线下事件

| 项目 | 值 |
|---|---|
| 方法 | CAPI 是 Meta 推荐的线下/门店事件路径 |
| 数据集 | 线下事件必须关联一个数据集；数据集可包含网页、应用、门店、商业消息、旧版线下转化、MMP |
| `action_source` | 门店交易用 `physical_store`；非网页非应用的用 `phone_call`、`email`、`chat`、`system_generated` |
| `event_time` 窗口 | `physical_store` 事件最多回传 62 天；其他事件类型 7 天 |
| 去重窗口 | 线下事件 7 天 |
| 去重字段组合 | `dataset_id` + `event_time` + `event_name` + `item_number`，再加 `order_id` 或用户字段 |
| 优化支持 | `physical_store` 仅用于衡量；其他操作来源支持优化 |
| 上传频率 | 建议实时；每天一次可接受 |


### 转化型潜在客户（Conversion Leads）CRM 集成

| 项目 | 值 |
|---|---|
| 适用场景 | 让 Meta 潜客投放向 CRM 阶段质量优化，而不是原始表单填写 |
| 事件来源 | Facebook/Instagram 潜客广告（即时表单）和合格的网站表单 |
| 必需映射 | 15-16 位 Meta 潜客 ID 映射进 CRM |
| 阶段时限 | 阶段事件必须发生在潜客创建后 28 天内 |
| 阶段转化率 | 应在 1% 到 40% 之间 |
| 实际规划下限 | Meta 开发者指引列出：每月至少 200 个潜客，才期待稳定的优化 |
| 上传频率 | 每天至少一次；接近实时优先 |
| 见效时间 | 预计约 3 个月；实施 1 个月到数月 |


Skill 立场：不要对每月少于约 200 个潜客、或没有每日 CRM 上传管道的品牌承诺转化型潜客优化。平台侧的投放质量杠杆是真实的，但无法一夜之间补回。

### 事件匹配质量（EMQ, Event Match Quality）

| 项目 | 值 |
|---|---|
| 分数范围 | 0-10（或标注 Poor / OK / Good / Great） |
| 健康匹配的实际下限 | 一般 6+；8+ 为理想 |
| 输入 | `user_data` 中的客户参数（哈希邮箱、电话、姓名、地址、external_id、fbp、fbc、IP、UA）及其匹配率 |
| 计算窗口 | 过去 48 小时的事件 |
| 界面可用性 | 网页事件；线下/应用/潜客需要 Meta 指引和 Integration Quality API beta 权限 |
| 误用 | EMQ 是诊断工具，不是 KPI；高 EMQ 不证明有提升 |

参数层级（对匹配的重要性）：

| 层级 | 参数 |
|---|---|
| 最高 | `em`、`ph`、`external_id` |
| 中 | `fbp`、`fbc` |
| 低 | `client_ip_address`、`client_user_agent` |


### Integration Quality API（beta）

| 项目 | 值 |
|---|---|
| 状态 | Beta；需要 Meta 代表开通权限 |
| 能力 | 以程序化方式访问逐事件诊断、EMQ、去重状态、缺失参数 |
| 覆盖 | 网页事件为一等公民；线下/应用/潜客/alpha/beta 集成需要 Meta 指引 |
| 适用场景 | 用持续的集成健康监控替代手动的 Events Manager 抽查 |


### CAPI 网关/云连接器 vs 服务端直连

| 方法 | 适合 | 优点 | 缺点 |
|---|---|---|---|
| CAPI 网关（Meta 托管） | 营销人员/小团队，无内部数据工程 | 无代码、Meta 托管、部署快 | 对事件载荷控制有限、难以做数据增强、有月度托管成本 |
| 服务端 GTM（sGTM） | 有 GTM 经验的中型团队 | 浏览器端共存、多平台分发、灵活性中等 | 有托管成本、GTM 复杂度 |
| 服务端直连 | 工程主导的 DTC、企业 | 灵活性最高、无中间托管、可从内部系统增强数据 | 开发成本最高、需要维护 CAPI 版本 |
| MMP/合作伙伴集成 | 应用为主的广告主 | 与归因栈打包 | 受 MMP 路线图牵制、有额外的去重考量 |


Skill 立场：按团队能力选，而不是按 Meta 的"推荐"徽章选。无论哪种方法，都要在 Events Manager 里验证去重。

### Pixel + CAPI 冗余

| 项目 | 值 |
|---|---|
| 严肃账户是否必需 | 是，自约 2021 年 Meta 官方指引起，2024-2026 年进一步强化 |
| 为什么要冗余 | Pixel 覆盖服务端看不到的（浏览器端微事件）；CAPI 覆盖 Pixel 无法可靠发送的（结账后服务端确认、广告拦截器、ITP） |
| 没有冗余 | EMQ 退化、AEM 信号更少、优化更嘈杂 |
| 有冗余 + 去重 | 全量可见且不重复计数 |


### 浏览器端 CAPI（CAPI for Browsers）

浏览器端 CAPI（CAPI for Browsers，曾称"server-side Pixel"/"browser CAPI"）把浏览器事件经由服务端端点路由，但仍驻留在浏览器端。除非当前 Meta 材料在用户账户中把它作为独立产品呈现，否则把它当作更广泛的 Pixel + CAPI 实施讨论的一部分。

Skill 立场：不要在 skill 里把"CAPI for Browsers"编码为独立策略。把它编码为浏览器来源事件的实施选项之一。

---

## CAPI 实施检查清单

在宣布 CAPI"做完"之前，用它做审计。

### 基础

- 每个应触发事件的页面都装了 Pixel
- CAPI 已通过以下之一部署：服务端直连、sGTM、CAPI 网关、MMP
- 数据集已在 Events Manager 配置并关联到广告账户
- 主要转化域名已做域名验证
- 用测试事件（Test Events）确认每个事件 Pixel 和 CAPI 都能触发

### 逐事件载荷

- `event_name` 与 Meta 标准名称完全一致（区分大小写）
- `event_time` 在转化发生的时刻发送，绝不批量延迟超过 1 小时
- `action_source` 取正确的值（`website`/`app`/`physical_store` 等）
- `website` 事件带上 `event_source_url`
- `client_user_agent` 和 `client_ip_address` 从原始请求转发
- `event_id` 每个事件生成一次，发给浏览器端和服务端两端
- 浏览器端与服务端的 `value` 和 `currency` 一致
- 购买类事件发送 `order_id`
- `content_ids` 和 `contents` 与目录 SKU ID 对齐

### user_data 卫生

- 同意允许时传哈希邮箱和哈希电话
- 填充 `external_id`（第一方用户 ID）
- 从 cookie/点击捕获 `fbp` 和 `fbc` 并转发
- 同意允许时传哈希的姓名、地址字段
- 哈希字段中没有意外混入的明文个人身份信息（核验哈希格式）

### 去重核验

- 浏览器端与服务端共用 `event_name` 和 `event_id`
- 在 Events Manager > 诊断中检查去重状态
- 生产事件没有"仅 Pixel"或"仅服务端"警告
- 定期（每周）抽查，去重率 ≥ 90%

### 线下/CRM

- 线下事件每天至少上传一次
- `physical_store` 的 `event_time` 在 62 天内
- 如果目标是潜客且 CRM 阶段可用，做转化型潜客（Conversion Leads）集成
- 15-16 位 Meta 潜客 ID 从潜客广告映射到 CRM

### 质量

- 每周按事件跟踪 EMQ（目标 6+，争取 8+）
- 有权限时监控 Integration Quality API
- 每月把 `value`/`currency` 与后端收入对账

### 文档

- 逐事件规格存入仓库：必填字段、真实来源、去重键策略
- 针对"Pixel 触发但 CAPI 没触发"/"CAPI 触发但 Pixel 没触发"/"EMQ 下降"的应急手册（Runbook）

---

## 易变衡量项检查

在对 API 窗口、CAPI 载荷、去重、线下事件或转化型潜客准入资格做硬性建议之前，先查下面的官方来源。

- Meta 转化 API 服务端事件：https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/server-event
- Meta CAPI 去重：https://developers.facebook.com/docs/marketing-api/conversions-api/deduplicate-pixel-and-server-events
- 经 CAPI 的 Meta 线下事件：https://developers.facebook.com/docs/marketing-api/conversions-api/offline-events
- Meta 转化型潜客 CRM 集成：https://developers.facebook.com/docs/marketing-api/conversions-api/conversion-leads-integration
- Meta Integration Quality API / EMQ：https://developers.facebook.com/docs/marketing-api/conversions-api/integration-quality-api

## CAPI 载荷示例

### 网页购买（服务端，action_source=website）

```json
{
  "data": [
    {
      "event_name": "Purchase",
      "event_time": 1745971200,
      "action_source": "website",
      "event_id": "ord_12345_a1b2c3",
      "user_data": {
        "em": ["<sha256(lowercase(email))>"],
        "ph": ["<sha256(normalized_phone)>"],
        "external_id": ["<sha256(user_id)>"],
        "fbp": "fb.1.1745971000123.1234567890",
        "fbc": "fb.1.1745970000456.PAAaBbCc",
        "client_ip_address": "203.0.113.42",
        "client_user_agent": "Mozilla/5.0 ..."
      },
      "custom_data": {
        "currency": "USD",
        "value": 129.99,
        "order_id": "ord_12345_a1b2c3",
        "content_type": "product",
        "content_ids": ["SKU-1234"],
        "contents": [{"id": "SKU-1234", "quantity": 1, "item_price": 129.99}],
        "num_items": 1
      }
    }
  ]
}
```

### 线下/门店购买（action_source=physical_store）

```json
{
  "data": [
    {
      "event_name": "Purchase",
      "event_time": 1745885000,
      "action_source": "physical_store",
      "event_id": "pos_store23_txn_889721",
      "user_data": {
        "em": ["<sha256(email)>"],
        "ph": ["<sha256(phone)>"],
        "fn": ["<sha256(firstname)>"],
        "ln": ["<sha256(lastname)>"],
        "ct": ["<sha256(city)>"],
        "st": ["<sha256(state)>"],
        "zp": ["<sha256(zip)>"],
        "country": ["<sha256(country)>"]
      },
      "custom_data": {
        "currency": "USD",
        "value": 245.00,
        "order_id": "pos_store23_txn_889721"
      }
    }
  ]
}
```

### CRM 转化型潜客（action_source=system_generated）

```json
{
  "data": [
    {
      "event_name": "Lead",
      "event_time": 1745899000,
      "action_source": "system_generated",
      "event_id": "crm_lead_47781",
      "user_data": {
        "em": ["<sha256(email)>"],
        "ph": ["<sha256(phone)>"],
        "lead_id": "1234567890123456"
      },
      "custom_data": {
        "lead_event_source": "CRM Salesforce",
        "event_source": "crm",
        "lead_status_stage": "qualified"
      }
    }
  ]
}
```

说明：

- `event_id` 必须能从你的真实来源 ID 确定性推导；绝不在每端随机生成

---

## 归因对账工作表

当后端收入与 Meta 归因收入分歧时，先走完这份清单，再下"Meta 坏了"的结论。

| 序号 | 问题 | 去哪看 | 不一致时的可能原因 |
|---|---|---|---|
| 1 | 广告系列用的是什么归因设置？ | 广告组设置 | 2026 年 3 月默认值变了 |
| 2 | 仪表盘请求的是哪个归因列？ | API 代码/Looker/Triple Whale 配置 | 仪表盘还在请求 `7d_view`（自 2026-01-12 起返回空） |
| 3 | 互动转化启用并计入了吗？ | 对比归因设置 | 2026 年 3 月后点击/互动/浏览的拆分 |
| 4 | 相关事件的 EMQ 是多少？ | Events Manager | EMQ 低 = 匹配不足 |
| 5 | 去重率是多少？ | Events Manager > 诊断 | 低则重复计数 |
| 6 | 浏览器端与服务端的价值/币种一致吗？ | 诊断 | 漂移会虚增广告投入回报率 |
| 7 | 线下/CRM 事件在回补吗？ | 数据集上传 | 超出窗口的事件被静默丢弃 |
| 8 | 模型估算转化占比多少？ | 不确定性公开 | iOS 曝光重 = 更多建模 |
| 9 | AEM 自动聚合健康吗？ | Events Manager | 很少出问题；2025-06 后基本自动 |
| 10 | 目录 Feed 干净吗？ | Catalog Manager | 缺货/被拒商品对投放不可见 |
| 11 | Safari/Firefox 上有缺失的浏览器事件吗？ | 网页 QA | ITP/广告拦截器；CAPI 应能覆盖 |
| 12 | 落地页捕获 `fbc` 了吗？ | 落地页 QA | 缺少 fbclid → 丢失点击归因 |
| 13 | 转化域名验证了吗？ | 商务设置（Business Settings） | 缺验证会降低归因质量 |
| 14 | 同比周期跨越 2026-01-12 或 2026 年 3 月吗？ | 仪表盘周期 | 不可比；做标注 |
