# 度量与信号质量（Measurement and Signal Quality）

在最终确定 X Ads 目标、出价和报告前使用本参考。

## 度量栈

| 用途 | 必需 / 推荐配置 |
|---|---|
| 网站流量 | UTM、落地页分析、点击质量、跳出/CVR 检查 |
| 网站转化 / 销售 | X Pixel 和/或 Conversion API / Conversions API、事件管理器（Events Manager）、事件定义、相关场景下的去重 |
| 线索收集 | 表单事件、CRM 阶段、合格线索回传、线下对账 |
| 电商 / DPA | X Shopping Manager、产品目录、产品 ID、页面浏览（Page View）、内容浏览（Content View）、加购（Add to Cart）、购买（Purchase）、价值（value） |
| 应用安装 / 再互动 | 已获批的 MMP：Adjust、AppsFlyer、Branch、Kochava 或 Singular |
| 品牌 / 高端 | 触达、频次、视频观看质量、提升研究（lift study）或代理需求指标 |

X 文档在不同界面分别使用 **Conversion API** 和 **Conversions API**。将其视为同一服务端转化共享概念，但在撰写实施说明时保留当前界面的标签。

对于规模化转化系列，仅用 Pixel 度量较为脆弱。可行时优先使用 Pixel + 服务端 Conversion API / Conversions API，再与业务数据对账。

## 事件设计

选择可达到足够量级的最深层可靠事件：

| 业务 | 主事件 | 临时回退 |
|---|---|---|
| 电商 | 带价值的购买（Purchase） | 购买量过低时才用加购或内容浏览 |
| 线索收集 | 合格线索 / CRM 阶段 | 质量回传未就绪时用表单提交 |
| SaaS | 合格注册、试用激活、演示预约 | 注册或定价页访问 |
| 应用 | 应用内事件或留存价值 | 安装 |
| 活动 | 注册、购票、出席 | 落地页浏览或提醒订阅 |

若弱微转化与业务价值不相关，不要长期优化到这些事件。

## 归因与增量

- 将 X 报告的转化与业务真相来源（收入、CRM 或应用分析）分开。
- 在报告中明确展示互动后（post-engagement）与浏览后（post-view）归因窗口。
- 当浏览后归因未经校准时，内部效果读数使用纯点击或折扣后的浏览归因报告。
- 对于应用系列，将 X 归因窗口与 MMP 后台对齐以减少差异。
- 在量级和预算允许时使用对照组（holdouts）、地域测试、前后对比、提升研究或受众排除。
- 尽可能将再营销与拓新分开报告。
- 统一使用 UTM。成熟账户可交叉验证 X Ads Manager、GA4/数仓、CRM/订单、购买后问卷、MMP，以及可用的 MMM/地域提升。

## 应用度量

X 应用安装出价要求配置一个已获批并接入 X Ads 账户的 MMP。X 还支持 SKAdNetwork，并面向符合条件的广告主提供高级移动度量（Advanced Mobile Measurement）计划。

MMP 文档可能将 X 描述为高级自归因渠道（Advanced self-attributing network）。将其视为移动归因集成背景，而非替代与应用分析及 LTV 的对账。

## 目录与 DPA 信号质量

推荐动态产品广告（Dynamic Product Ads）前先核验：

- 已实施 X Pixel 或 Conversion API / Conversions API，除非该系列仅做链接点击优化。
- 产品事件包含与目录匹配的产品标识符。
- DPA 事件包含必需的产品/事件参数，如产品 ID、价值、货币，以及适用的购买/内容动作。
- X Shopping Manager 目录已上传、获批并定期更新。
- 产品 Feed 包含准确的标题、价格、库存、图片、URL、分类，以及相关的促销价。
- 产品集按有意义的分类、毛利、库存或系列目标组织。

对于服务端事件，确认实施在可用时采集 `twclid`、正确哈希用户标识符，并在 Pixel 与 CAPI 同时上报同一事件时使用稳定的 `conversion_id` 去重。

## 与 Meta 的度量差距

不要默认 X 具备 Meta 式的工具：

- 不应默认存在 Meta 式 CAPI Gateway 对应物。
- 不应默认存在聚合事件度量（Aggregated Event Measurement）对应物。
- 不应默认存在事件匹配质量（Event Match Quality）评分对应物。

信号质量弱时采取更保守的规划。
