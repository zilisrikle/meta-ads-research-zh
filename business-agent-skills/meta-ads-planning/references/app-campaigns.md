# Meta App Promotion（应用推广）系列

## 运行实践

App Promotion 系列优化安装量、安装后事件或价值。最常见的失败是：业务真正关心的是留存、购买、订阅或 LTV（用户生命周期价值, Lifetime Value），却永远只优化安装量。第二常见的失败是把 iOS 和 Android 当作同一个市场——衡量、选择加入率、成本和安装后行为都有实质差异。

### 最重要的事

- **选有稳定量级的最深事件，而不是选可用的最深事件。** 只有购买足够频繁时，对购买出价才有效。低于约每周 50 次购买，价值优化会饿死。往梯子上降一格。
- **素材是主要的可控杠杆。** Advantage+ App 每个系列允许最多 50 条素材。真正的多样性指不同的动机、钩子、UI 时刻和证明点——不是同一个创意的 50 个剪辑版本。
- **衡量就绪是二元的。** 没有 SDK / MMP / CAPI，iOS 上就没有可用的 App Promotion 系列。先规划技术栈，再规划花费。
- **iOS 和 Android 需要分开的系列。** 框架不同（iOS 上的 AAK / SKAN / AEM，Android 上的 Play Install Referrer + Meta Install Referrer）、选择加入现实不同、CPI（每次安装费用, Cost Per Install）区间不同。
- **再营销 ROAS 很容易被高估。** 回流用户可能本来就会回来，所以在把再营销 ROAS 当作因果结论之前，先用 holdout（留存对照组）或增量测试。
- **学习期重置很容易触发。** 出价变动 >20%、预算波动 >20%、整批更换素材，或中途切换优化事件，都会重置学习期。每个广告组每周 50 个事件后退出学习期。
- **不要假设旧的 8 事件排序工作流还在。** Web（网页）AEM 现在普遍被描述为自动化，而 app AEM 取决于 Meta / MMP / Apple 的设置细节。在给出关于事件上限或优先级排序的指令前，先核验选定的应用、MMP 和 Events Manager（事件管理工具）工作流。
- **Advantage+ App 是 2026 年的默认起点。** 手动应用系列是备选，只用于非常特定的资格 / 衡量边缘场景。

### 诊断哲学

三个框架必须分开推理：

1. **Apple AAK / SKAN** —— Apple 侧，隐私保护，iOS 上在人群层面接近真相，但慢（回传 24-48 小时以上）且有噪声（低于人群匿名阈值时返回空转化值）。
2. **Meta AEM** —— Meta 侧，接近实时，当 SDK / MMP 信号存在时，用于 iOS 14.5+ 的出价优化。
3. **MMP（AppsFlyer / Adjust / Singular / Branch / Kochava）** —— 跨渠道运营与去重；它们与 Meta 的集成同时打通了 AEM 和 AAK / SKAN。

Android 上还有：**Google Play Install Referrer**（同会话点击转化的确定性归因）和 **Meta Install Referrer**（Meta 侧加密的 referrer，补充 Meta 侧归因上下文）。

这些永远无法完美对齐。按各自的角色使用：
- AAK / SKAN：方向性的、隐私安全的真相
- AEM：出价信号
- MMP：运营控制台与跨渠道归因
- Meta 平台内数字：出价时刻的现实，即竞价看到的世界

---

## 系列类型速查表

| 目标 | 设置 | 优化事件 | 出价策略 | 每日最低预算指引 | 备注 |
|---|---|---|---|---|---|
| **新安装获客（默认）** | Advantage+ App | App install（应用安装） | Highest volume（最高投放量）或 Cost per result goal（单次成效费用目标） | tCPI（目标 CPI）指引 × 50 | 冷启动应用获客的默认项 |
| **高质量安装** | Advantage+ App 或手动 | App event（应用事件）（注册 / 教程 / 关键行为） | Cost per result goal | 每日预算支撑每周 >= 50 个事件 | 当留存代理指标与 LTV 相关时毕业 |
| **收入 / 购买优化** | Advantage+ App | App event = Purchase（购买） | Highest value（最高价值）或 ROAS goal | 足以跑出 50+ 购买 / 7 天 | 需要传递价值 + 货币映射 |
| **订阅 / 试用类应用** | Advantage+ App | App event = Subscribe（订阅）/ StartTrial（开始试用） | 先 Cost per result goal 再 ROAS goal | 试用量 >= 30 / 天起步 | 把 paid_start（付费开始）和 trial_start（试用开始）打干净标签 |
| **再营销（已有用户）** | App Promotion - Re-engagement（再营销） | 深链落地页中的应用事件 | Cost per result goal | 取决于受众规模 | 对增量持怀疑态度 |
| **游戏获客** | Advantage+ App + Playable | Tutorial complete（教程完成）或 level N（N 关） | Cost per result goal | 按教程量设门槛 | Playable 设计投入大，但对安装质量是正向的 |

---

## 1. 版位与基本特征

广告主能控制的是：(a) 素材资产，(b) 预算，(c) 出价目标，(d) 优化事件，(e) 受众提示（手动模式）。其余都是 Meta AI。

| 版位 | 展示位置 | 对应用的备注 |
|---|---|---|
| Facebook Feed | Meta 信息流 | 竖屏 / 方形视频最佳 |
| Instagram Feed | IG 主信息流 | 同上 |
| Instagram Reels | 全屏竖屏 | 2026 年的主要流量驱动 |
| Facebook Reels | 竖屏短视频 | 份额在增长 |
| Stories | 竖屏全屏 | 包括 playable 的前贴片视频 |
| Audience Network（激励视频 / 插屏） | 第三方应用 | 游戏获客量的主要来源；平均质量偏低 |
| In-stream video（信息流内视频） | 前贴 / 中贴 | 应用相关的量有限 |
| Marketplace、Right Column（右列） | 桌面端 / FB | 对应用是次要的 |

Advantage+ Placements 是推荐默认项；只有在有实测理由时才用手动版位（素材没为该版位制作、Audience Network 质量问题）。

---

## 2. 前置条件

| 条目 | 要求 |
|---|---|
| 应用已在 Meta App Dashboard（应用管理中心）注册 | 必需，带 Bundle ID / Package Name（包名） |
| Meta Business Manager（商务管理平台）权限 | 管理员角色 |
| 衡量 | 已集成 Meta SDK，**或** MMP 已启用 Meta 集成 |
| App Events（应用事件） | 至少 Install（安装）+ 一个安装后事件在上报 |
| iOS - AEM 资格 | 应用已注册、SDK / MMP 在转发信号、Advanced Data Sharing（高级数据共享）开启（MMP 路径） |
| iOS - SKAN / AAK | SKAdNetwork Identifier（标识符）已在 Meta 注册；AAK 回传 URL 已配置 |
| Android - Install Referrer | MMP / SDK 中已启用 Play Install Referrer；Meta Install Referrer 已启用 |
| Conversions API for App Events（可选但推荐） | 服务端事件上报到 Graph API 端点 |
| 每日预算 | 能跑出所选优化事件每周 50 个事件的量级 |

### 上线前检查清单

- [ ] 应用已在 Meta App Dashboard 注册，Bundle ID / Package Name 和 SKAdNetwork ID 列表正确
- [ ] Meta SDK 或 MMP 已接入 Meta，且 Advanced Data Sharing 开启
- [ ] 标准应用事件在上报：至少 `fb_mobile_first_install`，再加 `fb_mobile_complete_registration` / `fb_mobile_tutorial_completion` / `fb_mobile_purchase` 之一
- [ ] 收入事件上正确传递了货币和价值；口径已约定（毛值 vs 扣折扣、税、退款后的净值）
- [ ] iOS：AAK 回传 URL 已在 Meta 注册；SKAN 集成已通过 MMP 验证；ATT 弹窗文案和时机已审核
- [ ] iOS：所有设置完成后再开 AEM 开关；提前开启会让 Meta 以为应用已就绪——若实际未就绪，AEM 衡量会部分缺失/错误且无明显报错
- [ ] Android：MMP / SDK 同时捕获 Play Install Referrer 和 Meta Install Referrer
- [ ] 工程有余力则配置 Conversions API for App Events（提升匹配质量和信号覆盖）
- [ ] 深链（iOS 的 Universal Links、Android 的 App Links）已测试，可用于再营销
- [ ] 预算满足所选优化事件的事件量下限
- [ ] 素材集：至少 10 个资产，目标 20-50；视频优先、竖屏优先
- [ ] 用 App Ads Helper（应用广告助手）验证安装事件和深链能收到

---

## 3. 选择系列模式

### 3-1. Advantage+ App（默认）

Meta 官方 Advantage+ App 页面把该产品描述为用 Meta AI 优化**出价、受众和版位**，以驱动安装、安装后事件或价值。三个效果目标：

1. 最大化应用安装
2. 最大化安装后事件（如注册、教程、关卡、关键行为）
3. 最大化转化价值

关键点：
- 每个系列最多 50 个素材资产
- 适用时 AI 会把受众扩展到手动种子之外
- 推荐素材多样性（版式、钩子、动机）——「同一产品图的 50 个微小变体给系统的是冗余输入」
- 官方称平均 CAC（获客成本, Customer Acquisition Cost）降低 7%

何时用：2026 年几乎所有从零开始的应用获客工作。

何时回退到手动：
- 有 Advantage+ AI 反复覆盖的特定受众约束
- 资格被挡（少见；通常是设置问题）
- 测试隔离——需要干净地衡量受众或素材效果

### 3-2. 手动 App Promotion

手动模式给明确的受众控制。适用于：
- Holdout / 增量测试结构
- 特定的相似受众或自定义受众策略
- 再营销系列（应用事件受众要求手动）

### 3-3. 再营销系列

App Promotion 内的独立系列目标。定向已安装但不活跃、或未完成下游行为的用户。

资格：
- 有可衡量的安装基数
- 已实现深链（iOS 的 Universal Links、Android 的 App Links）
- MMP 和 Meta Active 集成中开启了 Re-engagement Attribution（再营销归因）（MMP 路径）
- iOS：AEM 开启再营销归因，才能在 IDFA 不可用时归因深链点击

常见场景：

| 场景 | 设置 | 注意事项 |
|---|---|---|
| 唤回流失用户（>=14 天未打开） | 流失用户受众 + 价值主张素材 | 最可能被自然回流蚕食 |
| 购物车 / 漏斗放弃 | 加购未购买受众 | 自然再转化偏差高 |
| 试用到期 / 降级 | 按生命周期阶段的自定义受众 | 生命周期文案比广告本身更重要 |
| 大促 / 活动推送 | 宽泛的已有用户受众 | 有时能测出真实 lift；要测试 |
| 交叉销售付费功能 | 免费层用户受众 | 注意收入归因多算 |

---

## 4. 优化事件梯子

梯子是应用系列中最重要的战略选择。

| 阶段 | 事件示例（Meta 标准） | 用途 | 毕业触发 |
|---|---|---|---|
| 1. 安装 | `fb_mobile_first_install` | 冷启动、新应用、无安装后量级 | 安装投放稳定 >=2 周，但留存或收入弱 |
| 2. 引导 | `fb_mobile_complete_registration`、`fb_mobile_tutorial_completion` | 当注册与留存相关时，作为质量代理 | 阶段 2 事件 >= 50 / 周/广告组，业务验证了与 D7 留存的相关性 |
| 3. 关键行为 | `fb_mobile_add_to_cart`、`fb_mobile_search`、`fb_mobile_achievement_unlocked`、`fb_mobile_level_achieved` | 更强的质量代理；接受安装到行为的延迟 | 阶段 3 事件 >= 50 / 周，可预测变现 |
| 4. 变现 | `fb_mobile_purchase`、`Subscribe`、`StartTrial` | 直接收入信号 | 购买量稳定 >= 50 / 周 |
| 5. 价值 / ROAS | 带价值的 `fb_mobile_purchase` | 成熟优化，LTV 导向 | 价值管道干净，货币和退款处理已定 |

### 毕业规则

- **一次只上一格。** 跳格（安装 -> 购买）几乎一定会因量级不足而饿死。
- **验证代理指标。** 不能预测 D7 留存的教程完成率，作为优化事件还不如安装。
- **测算漏斗。** 安装 : 阶段 2 : 阶段 3 : 阶段 4 的比例大致应为 100 : 30-60 : 10-30 : 2-10。若阶段 3 < 安装的 5%，说明太稀疏，不能拿来出价。
- **按广告组计数，不按系列。** Meta 每周 50 个事件的阈值是按广告组、按周算的。

### 各阶段量级门槛

| 优化阶段 | 考虑采用的最低事件/周/广告组 | 舒适区 |
|---|---|---|
| 安装 | 50 | 100+ |
| 引导事件 | 50 | 100+ |
| 关键行为 | 50 | 100+ |
| 购买（CPA 模式） | 50 | 100+ |
| 价值 / ROAS 目标 | 7 天内至少 50 个不同的购买 | 100+ |

### 价值优化细节

跑 Value（Highest value / ROAS goal，最高价值 / ROAS 目标）需要：
- 广告组层级过去 7 天 50+ 次购买
- 每个收入事件都传递价值参数，且货币统一
- 退款处理已定（跳过、发负值、单独事件）
- 区分试用和付费（订阅类应用）：量级允许时默认优化 `paid_start`；付费量不足时才退到 `trial_start`

对于应用内广告收入类应用（带激励广告的游戏），解锁「In-app ad impression」（应用内广告展示）作为优化目标，需要过去 28 天 >= 15 个带不同价值的归因 AdImpression 事件。

---

## 5. iOS 衡量：三层结构

2026 年的 iOS 应用衡量是一个栈，不是单一数据源。把任何一层当作真相，是最常见的诊断错误。

### 5-1. Apple AdAttributionKit（AAK）

Apple 的前瞻框架，基于 SKAN 的基础但能力更强。Apple 建议今后使用 AAK。

| 属性 | 值 |
|---|---|
| 基础支持 | iOS / iPadOS 17.4+ |
| 再营销支持 | iOS / iPadOS 18+ |
| Web AdAttributionKit | iOS 14.5+、Safari 15.4+ |
| 点击归因窗口 | 30 天 |
| 浏览归因窗口 | 24 小时 |
| 回传时机 | 安装 / 再营销后 24-48 小时 |
| 转化值 | 最多 64 个不同信号（粗 + 细 schema） |
| 每个事件的回传数 | 最多 3 次（iOS 18+） |
| ATT 要求 | 不需要 |

#### iOS 版本更新

- **再营销的重叠转化窗口**——通过 conversion tags（转化标签）同时跑多个再营销系列，各自有独立的转化路径
- **可配置的归因窗口和冷却期**——按转化类型设冷却期（如安装 6 小时、再营销 1 小时），阻止竞争性归因
- **Geo 层级的回传数据**——回传中原生带国家代码
- **改进的测试 / 开发者模式**
- **Web-to-app 再营销流程**——深链把用户带进应用，生成再营销转化，通过 Universal Links 把转化标签拼到 URL 上

对技能建议的影响：
- iOS 18+ 是 OS 层面再营销归因的真实资格门槛
- 跑多个并发再营销系列前，MMP / Meta 集成必须支持重叠窗口——按渠道检查 MMP 支持情况

### 5-2. SKAdNetwork（SKAN）——并行

仍为兼容性保留。原始框架。SKAN 4.0 要点：
- 最多 3 次回传（0-48 小时、3-7 天、8-35 天）
- 64 个转化值（0-63）
- Source identifier（来源标识符）4 位（10,000 种组合）

隐私阈值：安装量不足时，SKAN 回传返回空转化值。Meta 公布的 SKAN 过阈推荐下限是每个系列每天约 88+ 安装。

2026 年 SKAN 和 AAK 并行运行。大多数账户仍能看到可观的 SKAN 量。AAK 是未来；迁移是渐进的、由 MMP 驱动的。

### 5-3. Meta App AEM（应用的 Aggregated Event Measurement，汇总事件衡量）

Meta 侧的隐私保护协议，让 Meta 在 iOS 14.5+ 设备上即使 ATT 被拒绝也能衡量网页和应用事件。用于 **Meta 侧出价优化和报表**。

关键事实：
- 适用于 iOS 14.5+ 设备（不像 AAK 需要 iOS 17.4）
- 报表接近实时，而 SKAN 回传要 24-48 小时以上
- AEO（应用事件优化, App Event Optimization）和 VO（价值优化, Value Optimization）系列支持 1 天点击和 7 天点击归因
- 开启展示设备匹配后可做浏览转化报表
- AEM 和 SKAN 可在同一系列上同时运行

#### 2025-2026 年变化

- **2025 年 web AEM 变化**：Web AEM 现在普遍被描述为不再需要旧的手动 8 事件优先级排序工作流。未经核查选定的 MMP / Meta 应用设置前，不要把这句话自动套用到 app AEM。
- App AEM 行为必须在当前应用和 MMP 工作流中核验
- 资格检查发生在广告组创建时；若应用符合 AEM 资格，默认选中 AEM

#### 设置坑点

必须在**完成所有设置步骤之后**再开 AEM 开关。提前开启等于告诉 Meta 应用已就绪——若实际未就绪，AEM 衡量会部分缺失/错误且无明显报错。这是单个最常见的设置错误。

#### AEM 数据何时可用

| iOS 版本 | AAK | SKAN | AEM |
|---|---|---|---|
| iOS 14.5 - 16.x | 不支持 | 支持 | 支持 |
| iOS 17.4 - 17.x | 支持（安装） | 支持 | 支持 |
| iOS 18+ | 支持（安装 + 再营销） | 支持 | 支持 |

### 5-4. 三层矩阵

| 层 | 归属 | 延迟 | 用途 |
|---|---|---|---|
| AAK / SKAN | Apple | 24-48 小时以上 | 隐私安全的方向性真相、合规记录 |
| AEM | Meta | 接近实时 | 出价优化、Meta 内报表、接近实时的信号 |
| MMP / SDK | 厂商 | 实时（受 ATT 限制） | 跨渠道运营、自定义事件、更深的 LTV 分析 |
| CAPI for App Events | Meta 直连 | 实时 | 增强 SDK / MMP 信号、自定义参数、亚分钟级延迟 |

对账规则：**永远不要指望它们对得上。** 调查大的差距，不纠结小的差距。出价决策看 AEM，合规/隐私叙事看 AAK / SKAN，跨渠道运营看 MMP。

---

## 6. iOS ATT（App Tracking Transparency，应用跟踪透明度）

ATT 是绕不开的背景。把基准当作弱输入；驱动规划的应该是账户自己的选择加入率、iOS 占比和安装后事件覆盖。

| 指标 | 规划含义 |
|---|---|
| 全球平均选择加入率 | 因应用品类、国家、弹窗时机、品牌信任度差异很大；以自己应用的数据为准 |
| 优化弹窗的上限 | 好的弹窗时机能实质提升选择加入率，但不要用通用基准做规划 |
| Meta 失去的行为数据 | 把 iOS 用户级数据视为实质不完整；依赖 AEM、AAK / SKAN、MMP 报表、相关的 CAPI 和增量测试 |

#### 弹窗时机最佳实践

- 在用户体验到应用价值后展示（教程后、第一次关键行为后）——比冷启动弹窗实质提升选择加入率
- 前置教育页解释跟踪的作用（如更好的推荐）可进一步提升
- Apple 允许范围内本地化 ATT 系统弹窗文案（purpose string，用途说明）

ATT 对 Meta App 系列的影响：
- 大多数转化数据变成建模数据
- iOS 报表最多需要 5 天才能完全落地（SKAN 回传延迟）
- 优化精度低于 Android

---

## 7. Android 衡量

Android 相对 iOS 简单，但两条 referrer 路径都要。

| 来源 | 覆盖内容 |
|---|---|
| Google Play Install Referrer | 到 Play Store 的同会话点击转化的确定性归因 |
| Meta Install Referrer | 把 Meta 广告元数据加密进 Play Store referrer；覆盖点击（同会话 + 非同会话）和浏览转化 |
| MMP SDK | 两者都捕获、去重、归因 |

Meta Install Referrer **不能**替代 Play Install Referrer——它是补充。没开 Meta Install Referrer，Meta 在 Android 上的浏览转化安装就丢了。

大多数账户现在通过 MMP SDK 自动处理，两条都有了。

---

## 8. MMP 集成

MMP 对认真做应用花费不是可选项。它们处理跨渠道归因、与 Meta 的去重，以及 iOS 上的 AEM / AAK / SKAN 管道。Meta 自己的 SDK 也能单独跑应用系列，但多渠道的应用投放需要 MMP。

### 厂商矩阵

| MMP | Meta AEM 支持 | AAK 状态 | 典型优势 |
|---|---|---|---|
| AppsFlyer | 通过 Meta 集成和高级共享设置支持 Meta AEM | 在选定账户中核验当前 AAK 支持 | 渠道覆盖广、企业级应用运营 |
| Adjust | 支持 Meta 应用衡量集成 | 在选定账户中核验当前 AAK 支持 | 游戏获客和反作弊工作流 |
| Singular | 支持 Meta 应用衡量集成 | 在选定账户中核验当前 AAK 支持 | 成本聚合和营销数据仓库工作流 |
| Branch | 支持 Meta 应用衡量集成和深链报表 | 在选定账户中核验当前 AAK 支持 | 深链和 web-to-app 工作流 |
| Kochava | 支持 Meta 应用衡量集成 | 在选定账户中核验当前 AAK 支持 | 反作弊和衡量运营 |

### 每个 MMP 的设置检查

- MMP 仪表盘中启用了 Meta 集成
- 应用已在 Meta Business Manager 注册并与 MMP App ID 匹配
- Advanced Data Sharing / AEM 开关开启（AppsFlyer / Adjust）
- 跑再营销系列则开启 Re-engagement Attribution
- 应用设置中 IP masking（IP 掩码）关闭（AppsFlyer 特定——高优先级）
- 标准事件映射已审核（如 AppsFlyer `af_revenue` -> Meta `_valueToSum`）
- 需要浏览转化报表则开启浏览展示设备匹配
- AAK：回传 URL 转发已配置

### Meta SDK + MMP 共存

Meta SDK 和 MMP SDK 可以同时装，但事件来源要小心。通用规则：**用于出价的事件只用一个来源。** 同一个购买事件同时从 Meta SDK 和 MMP 导入会导致重复计数，破坏出价。

| 决策 | 最常见的模式 |
|---|---|
| 安装归因 | MMP（跨渠道） |
| 给 Meta 出价的应用内事件 | MMP 转发给 Meta（推荐）或 Meta SDK 直连（二选一） |
| 跨渠道报表 | MMP |
| iOS AEM 信号 | MMP 通过 Advanced Data Sharing 转发 |

### Conversions API for App Events（应用事件的 CAPI）

直连 Meta Graph API 端点的服务端到服务端管道。完全绕过设备层限制。与 SDK 和 MMP 共存。

适用场景：
- SDK / MMP 不支持的自定义参数（如预测 LTV、订阅层级）
- 需要亚分钟级延迟
- 后端有信号但客户端 SDK 没有（服务端购买确认）
- 在 ATT 屏蔽设备导致客户端数据减少的地方，增强 SDK / MMP 信号覆盖

注意：Meta 网页版「一键」CAPI 设置**不**延伸到应用场景。App CAPI 只能直接集成——需要工程工作。

去重：SDK 和 CAPI 之间保持 `event_name` 和 `event_id` 一致；48 小时内匹配的事件会被去重；若浏览器/应用事件和服务端事件在约 5 分钟内到达，Meta 优先采用浏览器/应用事件。

---

## 9. 应用出价策略

| 策略 | 作用 | 应用系列用途 | 量级需求 |
|---|---|---|---|
| **Highest volume（无目标）** | 在预算内最大化结果数 | 冷启动获客、安装量的默认项 | 最低 |
| **Cost per result goal** | 以 CPA / CPI 为目标，但按竞价浮动 | 应用事件优化最常用 | 需要每个广告组每周 50 个事件才能学习 |
| **Bid cap（出价上限）** | 每次竞价出价的硬上限 | 应用中少见；激进的成本控制 | 需要有成本上限的经验 |
| **Highest value（无目标）/ VO** | 在预算内最大化总价值 | 成熟的变现应用 | 每个广告组 7 天 50+ 个价值事件 |
| **ROAS goal（最低 ROAS）** | 仅当预测回报 >= 目标时展示广告 | 价值稳定的成熟应用 | 同 VO + 对价值数据有信心 |

### 各自的适用场景（当前运营判断）

| 阶段 | 推荐出价 |
|---|---|
| 全新应用，无事件 | Highest volume 跑安装 |
| 上线 2-4 周，安装量稳定，引导事件在上报 | Cost per result goal 跑引导事件 |
| 引导稳定，关键行为稳定 | Cost per result goal 跑关键行为 |
| 购买量 >= 50 / 周 | Cost per result goal 跑购买 |
| 购买 + 价值干净且 >= 50 / 周 | Highest value / ROAS goal |

### 量级阈值表

| 出价策略 | 量级下限 | 舒适区 |
|---|---|---|
| Highest volume（安装） | 50 安装 / 周 / 广告组 | 200+ |
| Cost per result goal（事件） | 50 事件 / 周 / 广告组 | 100+ |
| Bid cap | 50+ | 100+ |
| VO / ROAS goal | 50 个价值事件 / 7 天 / 广告组 | 100+ |
---

## 10. 预算设计

应用系列的学习对预算-出价比敏感。预算不足的系列竞价探索不够，无法走出学习期。

### 实用下限公式

每日预算 >=（目标 CPA 或 CPI）×（每周走出学习期每天需要的事件数）

每周 50 个事件，每天需要约 7-8 个。所以：

| 优化事件 | 目标成本 | 每日预算下限（约） |
|---|---|---|
| 安装，CPI $2 | 50 / 周 | $14-16 / 天 / 广告组 |
| 注册，CPA $5 | 50 / 周 | $35-40 |
| 购买，CPA $20 | 50 / 周 | $140-160 |
| 购买，CPA $50 | 50 / 周 | $350-400 |

从业者经验法则：低于该下限 2 倍的系列会报「Learning Limited」（学习受限）或无限期停留在学习期。

### 预算推理的归因窗口

| 窗口 | 备注 |
|---|---|
| 1-day click（1 天点击） | 更严格；适合短决策/冲动型应用 |
| 7-day click（7 天点击） | 2026 年大多数应用广告主的默认项 |
| 1-day view（1 天浏览） | 大多数流程默认包含；iOS 安装后事件**不支持** 1-day view |
| 7-day view、28-day view | 2026 年 1 月已移除 |
| 28-day click（28 天点击） | 安装事件可用，安装后事件不可用 |

2026 年 1 月浏览窗口移除后，许多账户上报转化一夜掉了 15-40%。按新窗口规划目标 CPA。

### 学习期规则

- 7-10 天内不要改出价策略 / 优化事件
- 24-48 小时内预算变动不要超过 20%
- 不要一次性换掉整个素材集
- iOS 表现不要用 <3-5 天的数据下结论（建模数据还没落地）

---

## 11. 再营销 playbook

再营销是应用营销中最容易自我欺骗的领域。

### 设置

1. 实现深链——iOS 的 Universal Links 和 Android 的 App Links。自定义 scheme 只是备选。
2. 受众策略：
   - **流失用户**（N 天未打开应用）
   - **漏斗放弃者**（加购、看过付费墙、开始试用——未转化）
   - **生命周期阶段**（免费用户、流失订阅者）
3. MMP 和 Meta 集成中开启 Re-engagement Attribution。
4. iOS：确保 AEM 开启再营销（AAK 再营销要 iOS 18+，AEM 在 iOS 14.5+ 可用）。
5. 在 iOS 18.4+ 上跑多个重叠再营销流程时设置 conversion tag。

### 素材侧重

| 受众 | 素材角度 |
|---|---|
| 泛流失用户 | 价值提醒 + 上次访问后的新功能/改进 |
| 购物车放弃者 | 具体商品 + 激励（免邮、折扣） |
| 试用到期 | 试用期间获得的结果 + 付费价值主张 |
| 免费层 | 升级才解锁的具体付费功能 |

### 增量

最重要的问题只有一个：记在再营销账上的转化里，有多少本来就会发生？

- Holdout 测试（「ghost ads」，幽灵广告）：随机扣留一部分对照组不做再营销，对比唤回率
- 「胜利」中可观的份额可能是自然发生的；再营销花费可观时要用 holdout
- 地理 holdout：在一个地区停掉再营销 2-4 周，对比 LTV / 留存
- 保守报表：按增量系数给平台上报的再营销 ROAS 打折

如果跑不了 holdout，就不要只凭平台 ROAS 激进地放大再营销预算。

---

## 12. Playable（可玩广告）设计

Playable 广告是交互式 HTML5 迷你体验，让用户在安装前先试玩。最适合游戏，以及任何能在 30-60 秒内演示核心交互循环的应用。

### 规格

| 元素 | 规格 |
|---|---|
| HTML5 包文件格式 | ZIP |
| 包总大小上限 | 5 MB |
| `index.html` 大小 | <= 2 MB |
| 包内文件数上限 | 100 |
| 必需入口 | 根目录的 `index.html` |
| Lead-in video（前贴片视频） | 必需，支持所有长宽比 |
| 前贴片版位 | 仅 Facebook Feed、Instagram Feed |
| Playable 版位 | Feed（FB / IG）、Stories、Audience Network（激励视频、插屏） |
| Fallback video（兜底视频） | playable 无法渲染的版位必需 |

### 三段式结构

1. **Lead-in video**——吸引注意力；Feed 中下方有「Try Now」（立即试玩）CTA
2. **Interactive demo（交互演示）**——<= 2 步展示核心玩法；全屏
3. **End card（结束卡）**——安装 CTA，带 App Store / Play Store 深链

### 设计规则

- **教程 2 步最理想。** 超过 2 步完成率实质下降
- **演示全程展示 CTA，** 不要只放在结尾
- **前贴片视频必须和演示内容一致。** 不一致导致流失和质量下降
- **一个 playable 搭配多版广告文案测试**
- **用户失败/完成时循环 playable**——不要死胡同
- **默认静音；** 靠视觉/触觉反馈
- **发布前在 App Ads Helper 中测试**

**不**用 playable 的场景：
- 没有易于预览的核心交互的应用（工具类、内容类、SaaS）
- 制作产能不足（一个可用的 playable 演示是实打实的工程）
- 品牌故事主导的系列

---

## 13. 素材制作规则

应用素材原则：

| 规则 | 原因 |
|---|---|
| **前 1-3 秒必须钩子 + 展示应用 UI** | 信息流滑动速度快；受 ATT 影响的用户照样看广告，注意力是唯一的信号 |
| **展示应用本身，而不是泛泛拿着手机的演员** | 泛泛的手机 mockup（样机）素材全品类都跑得差 |
| **竖屏视频优先** | 2026 年 Reels + Stories 主导版位价值 |
| **全部加字幕** | 大多数用户静音观看 |
| **静态图文字密度 <= 图片面积的 20-25%** | 文字密度更高会降投放（legacy 20% 规则已放宽，但仍有参考意义） |
| **多样性 = 不同的动机，不是不同的剪辑** | 算法需要不同的钩子、受众、证明类型 |
| **制作节奏：每月 20-50 条新素材** | 匹配 Advantage+ App 的胃口 |
| **每 2-4 周刷新** | 放量时视频疲劳来得最快 |

### 应用的素材版式角色

| 版式 | 角色 |
|---|---|
| 竖屏视频（Reels 原生） | 主要放量驱动 |
| 方形视频（1:1） | Feed 原生备选 |
| 静态图 | 便宜的多样性，支撑再营销 |
| 轮播 | 多功能 / 多截图应用 |
| Playable | 游戏获客的质量助推器、安装预筛选器 |
| UGC 风格 | 信任 + 亲近感；测评 / 前后对比 |
| Stories 全屏 | 品牌时刻 + 清晰 CTA |

### 应用的 ABCD 式框架

- **Attention（注意）**——1-3 秒内用惊艳的 UI 时刻或痛点抓注意力
- **Branding（品牌）**——应用名 / logo 尽早出现，但不是第一帧
- **Connection（连接）**——用户的动机明确（省时间、赢游戏、省钱）
- **Direction（行动）**——安装 CTA + 清晰的下一步画面

---

## 14. 衡量栈设计（完整矩阵）

| 层 | iOS 角色 | Android 角色 | 由谁搭建 |
|---|---|---|---|
| 应用内 Meta SDK | 直接上报应用事件到 Meta | 直接上报应用事件到 Meta | 应用工程师 |
| 应用内 MMP SDK | 捕获 + 转发事件；管理 SKAN / AAK / AEM | 捕获 Play Install Referrer + Meta Install Referrer | 应用工程师 + MMP 设置 |
| AAK 回传 URL | Apple 签名的回传到 Meta | 不适用 | MMP / Meta |
| SKAN 配置 | 转化值 schema | 不适用 | MMP / Meta |
| AEM 开关 | Meta 侧聚合，设置完成后开启 | 不适用（次要） | Meta + MMP 开关 |
| ATT 弹窗 | 驱动客户端数据质量 | 不适用 | 应用团队 |
| Conversions API for App Events | 服务端增强 | 服务端增强 | 后端工程师 |
| 深链（Universal / App Links） | 再营销归因 + 体验 | 再营销归因 + 体验 | 应用工程师 |
| Meta App Dashboard 注册 | AEM 和 SKAN 必需 | 必需 | Meta 管理员 |

### 按成熟度的推荐栈

| 成熟度 | 栈 |
|---|---|
| 早期（MVP，低花费） | 仅 Meta SDK；安装 + 1 个安装后事件 |
| 成长（多渠道、真实花费） | MMP（AppsFlyer / Adjust / Singular / Branch / Kochava）+ Meta 集成 + AEM 开启 |
| 成熟（LTV 导向、多广告渠道） | MMP + CAPI for App Events + AEM + 自定义事件 |
| 老练 | 以上全部 + holdout / 增量测试计划 + 价值优化 |

---

## 15. iOS vs Android 放量矩阵

| 维度 | iOS | Android |
|---|---|---|
| 衡量可靠性 | 建模，多层（AAK / SKAN / AEM） | 确定性（Install Referrers） |
| 报表延迟 | 完整画面最多 5 天 | 接近实时 |
| 选择加入 / 数据质量 | ATT 选择加入率 13-25% | 高（无等效弹窗） |
| 典型 CPI 溢价 | 更高（同一市场常为 Android 的 1.5-3 倍）——有差异 | 基准 |
| LTV / ARPU（每用户平均收入） | 单用户常更高 | 单用户更低，量更大 |
| 再营销归因 | 完整画面需要 AAK / AEM 且 iOS 18+ | 标准 |
| 学习稳定性 | 波动更大 | 更稳定 |
| 决策数据下限 | 3-5 天 | 1-2 天可接受 |
| 运营上限 | 实用指引：iOS 系列数量保持少，避免信号碎片化 | 不那么关键 |

含义：**绝不要用混合数字同时放量 iOS 和 Android。** 按平台拆系列；按平台分配预算；按平台评估 LTV。

---

## 16. 按月量级的决策矩阵

量级指上线后每个广告组每月的优化事件量。

| 月事件量 | 策略 |
|---|---|
| < 200 | 优化安装（或有量级的最浅事件）；不跑 ROAS goal；不要拆成很多广告组 |
| 200 - 1000 | 优化引导 / 关键行为；试 Cost per result goal；考虑 2-3 个广告组 |
| 1000 - 5000 | 优化购买或深层事件；价值数据干净则引入 VO；3-5 个广告组 |
| 5000+ | 成熟 ROAS goal；多广告组素材测试；iOS / Android 分系列；再营销计划可行 |

与归因窗口默认项交叉参考：在 7 天点击下，同一系列上报的转化比 1 天点击多——所以「200 量级」规则必须按所选归因窗口理解。

---

## 17. Advantage+ App vs 手动决策树

```
冷启动获客，默认情况 ........................... Advantage+ App
+ 有重要的自定义受众约束 ....................... 手动（或两个都测）
+ 需要 Holdout / 增量测试 ...................... 手动
+ 再营销系列 ................................... App Promotion - Re-engagement（手动模式）
+ 资格被挡 ..................................... 查设置；手动备选
+ 特定的素材-受众配对 ......................... 手动，或 A+ 按受众配素材
```

2026 年倾向用 Advantage+。手动越来越是战术工具，不是默认项。

---

## 18. 诊断决策树

| 症状 | 先查 | 可能的动作 |
|---|---|---|
| 安装便宜，留存差 | 优化事件深度、安装后漏斗映射、应用引导流程 | 把优化往深处移（安装 -> 引导 -> 关键行为），审计引导流程 |
| iOS 报表低 / 不稳定 | AEM 开关顺序（是不是开太早了？）、SKAN schema、AAK 回传 URL、MMP 集成 | 改系列前先核验设置；阈值过之前预期有部分空白 |
| iOS / Android 数字分化严重 | 预期内；方法论差异 | 不要对账——按平台运营 |
| CPI 高，留存好 | 素材质量、商店页转化率、出价设置 | 提升素材 + 商店页；在质量信号上谨慎放量 |
| 应用内事件好但购买低 | 变现路径、付费墙、代理事件质量 | 检查优化事件下方的漏斗；考虑更深的事件 |
| 再营销 ROAS 高，LTV 没 lift | 自然回流偏差 | 跑 holdout；按增量系数给平台 ROAS 打折 |
| 卡在学习期 | 每个广告组每周的事件量 | 换更浅的事件或合并广告组 |
| 花不出去 | 预算-出价比、资格、AEM 开关、受众规模 | 检查设置；提预算或放宽受众 |
| 2026 年 1 月转化骤降 | 浏览窗口被移除 | 重新定基线；这是方法论变化，不是表现问题 |
| 素材疲劳 | 素材年龄、频次、花费集中度 | 加新概念（不同动机），不是新剪辑 |
| Playable 不起量 | 包大小、兜底视频、前贴片视频、版位支持 | 核验规格；兜底视频必需 |
| AEM 数据稀疏 | 设置顺序、MMP「Advanced Data Sharing」开关、IP masking 开启、应用注册 | 重跑设置检查清单；AEM 对配错没有报错界面 |
| ATT 选择加入率 <10% | 弹窗时机、文案、前置教育 | 把弹窗移到价值体验之后 |
| Android 浏览转化缺失 | Meta Install Referrer 未启用 | 在 MMP + SDK 中启用 |
| iOS 安装后事件不支持 1-day view | —— | 记住：1-day view 不支持 iOS 安装后事件 |
| 2026 年 1 月转化下跌 | —— | 重新定基线；这是方法论变化，不是表现问题 |
| ATT 弹窗在应用冷启动时展示 | —— | 把弹窗移到价值体验之后 |
| Playable 超过 2 步才展示价值 | —— | 2 步内展示价值 |
| 前贴片视频和 playable 演示不一致 | —— | 保持两者一致 |
| 同一个购买事件同时从 Meta SDK 和 MMP 导入 | —— | 只保留一个事件来源；双导入会重复计数并破坏出价 |
| 用 <3 天的数据决策 iOS 表现 | —— | 至少等 3-5 天数据落地 |
| 学习期预算变动 >20% | —— | 预算变动控制在 20% 以内 |
| 单个广告组覆盖 LTV 差异很大的所有地区 | —— | 按地区拆分广告组，避免信号混杂 |
| 没有深链 | —— | 实现深链；否则再营销系列能跑但体验崩 |
| 用过时的网页工作流手动给 AEM 事件排序 | —— | 先核验当前应用 / MMP 设置 |
| 把 CPI 和品类基准对比 | —— | 先按 ATT 影响 / 衡量框架归一化再比 |

---

## 19. 常见坑

- 业务真正要的是留存或收入，却永远优化安装量
- 把 Meta 上报的再营销 ROAS 当作增量
- 为图「简单」把 iOS 和 Android 混在一个系列里
- 设置检查清单没走完就开 AEM 开关
- 同一系列中途切换优化事件（强制学习期重置）
- 同一个创意做 50 个剪辑就自称多样性
- 7 天/广告组购买不足 50 就跑 Value / ROAS goal
- 订阅类应用把 trial_start 当付费事件计数
- Android 上忽略 Meta Install Referrer（丢浏览转化）
- 忘记 1-day view 不支持 iOS 安装后事件
- 以为 2026 年 1 月的转化下跌是表现问题（实际是浏览窗口移除）
- ATT 弹窗在应用冷启动时展示
- 做的 playable 超过 2 步才展示价值
- 前贴片视频和 playable 演示不一致
- 同一个购买事件同时从 Meta SDK 和 MMP 导入（重复计数；破坏出价）
- 用 <3 天的数据决策 iOS 表现
- 学习期预算变动 >20%
- 单个广告组覆盖 LTV 差异很大的所有地区（信号混杂）
- 没有深链——再营销系列能跑但体验崩
- 用过时的网页工作流手动给 AEM 事件排序，未核验当前应用/MMP 设置
- 把 CPI 和品类基准对比，未按 ATT 影响/衡量框架归一化

---

## 20. 易变检查（给客户建议前重新核验）

iOS 应用衡量是效果营销中变化最快的领域。引用数字前重查这些：

| 条目 | 为何易变 | 重查频率 |
|---|---|---|
| AAK iOS 版本要求（17.4 / 18 / 18.4 / 未来） | Apple 每个大版本都发新功能 | 每季度或 WWDC 后 |
| Apple 的推荐框架（AAK vs SKAN 对等） | Apple 在逐步把应用迁到 AAK | 每半年 |
| Meta AEM 事件上限 / 优先级 | 2025 年 web AEM 优先级变了；app AEM 取决于当前 Meta / MMP 支持 | 每季度 |
| Meta 归因窗口 | 2026 年 1 月移除了 7 天/28 天浏览——未来还可能变 | 按 Meta 公告 |
| MMP AEM / AAK 功能对等 | 各 MMP 上线支持的速度不同 | 检查所选 MMP 的当前设置界面 |
| Meta Advantage+ App 资格 / 行为 | 默认开启的变化经常发生 | 每季度 |
| Playable 规格限制 | 偶尔更新 | 每年 |
| ATT 选择加入率基准 | 缓慢漂移 | 每年 |
| ROAS goal / VO 量级阈值 | 有文档，但 Meta 历史上微调过阈值 | 每年 |
| 优化目标的移除/新增 | Meta 会下线目标（如 Highest value、ROAS goal 的细节） | 按 Meta 发版 |
| Android 的 iOS Install Referrer 对等 | 采用率和平台支持可能变化 | 每年 |

易变应用条目的当前官方核查点：

- Meta Advantage+ App campaigns: https://www.facebook.com/business/ads/meta-advantage-plus/app-campaigns
- Meta App Events: https://developers.facebook.com/docs/app-events/
- Meta CAPI for App Events: https://developers.facebook.com/docs/marketing-api/conversions-api/app-events/
- Meta playable ads: https://www.facebook.com/business/ads/playable-ad-format
- Apple AdAttributionKit: https://developer.apple.com/app-store/ad-attribution/

拿不准时，除了上面的官方文档，还要看当前 Meta、Apple 和所选 MMP 的设置界面，以实际账户为准。

---

## 21. 运营节奏

| 节奏 | 事项 |
|---|---|
| 每日 | 花费 pacing（投放节奏）、学习期状态、异常检查（尤其 iOS） |
| 每 3-4 天 | 素材表现复盘（iOS 数据落地前不要早于 3-5 天行动） |
| 每周 | 素材刷新决策、AEM / AAK / MMP 对账复盘 |
| 每两周 | 出价微调（<= 20%）、新广告组测试 |
| 每月 | cohort LTV 复盘、再营销的 holdout / 增量测试（如在跑）、ATT 弹窗表现、结构审计 |
| 每季度 | iOS 衡量框架核验（AAK / SKAN / AEM 更新）、MMP 功能对等、归因窗口变化 |

---
