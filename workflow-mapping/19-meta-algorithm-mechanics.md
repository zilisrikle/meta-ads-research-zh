# Meta 广告算法到底是怎么工作的

> **研究日期：** 2026 年 5 月
> **可信度：** 核心机制为高（直接来自 Meta 工程博客和官方帮助中心）；内部 ML 细节为中（从业者测试和 Meta 公告推断）
> **来源：** 文末 25+ 条引用

---

## 目录

1. [广告竞价系统](#1-广告竞价系统)
2. [学习期](#2-学习期)
3. [Advantage+ 广告系列](#3-advantage-广告系列)
4. [广告系列预算优化（CBO）vs 广告组预算（ABO）](#4-广告系列预算优化cbo-vs-广告组预算abo)
5. [定向机制](#5-定向机制)
6. [Pixel 与 Conversions API](#6-pixel-与-conversions-api)
7. [归因](#7-归因)
8. [质量与相关性](#8-质量与相关性)
9. [投放与优化](#9-投放与优化)
10. [iOS 14.5+ 的影响](#10-ios-145-的影响)

---

## 1. 广告竞价系统

### 竞价如何工作

每次用户打开 Facebook 或 Instagram、滑到一个广告位，Meta 就在数千万条在投广告之间跑一次实时竞价。整个流程约 **200 毫秒**，分四步：

**第 1 步——召回（Andromeda）**
Meta 的 Andromeda 召回引擎扫描数千万条在投广告候选，缩小到几千条相关候选。自 Andromeda 更新（2025 年 10 月全球部署）起，这一阶段会评估素材元素（钩子、版式、出镜人物、文案、落地页）来判断相关性——而不只是看受众定向参数。Andromeda 用深度神经网络，模型容量是前代的 **10,000 倍**，跑在 NVIDIA Grace Hopper Superchips 上。[来源 1、3]

**第 2 步——轻排序（Light Ranking）**
快速过滤器，把几千条候选砍到几百条，剔掉明显不匹配的。这是计算成本很低的一遍，淘汰一眼就不行的。[来源 2]

**第 3 步——重排序（Heavy Ranking）**
剩下的几百条广告用总价值（Total Value）公式打分。得分最高的晋级。真正的计算发生在这里。[来源 2]

**第 4 步——竞价**
从重排序池里选出最终赢家。赢家拿到这个展示；输家要么被分到差版位，要么拿不到展示。[来源 2]

### 总价值公式

决定每次竞价赢家的公式：

```
总价值 =（广告主出价 x 预估行动率）+ 广告质量/用户价值
```

转化类广告系列进一步展开：

```
预估行动率 = 预估 CTR x 预估点击到转化率
```

**赢的是总价值最高的广告——不是出价最高的。** 这是根本原则。素材强、相关性高的小广告主，能持续打败素材烂的大品牌。[来源 1、2、4]

#### 要素 1：广告主出价

这是你愿意为一个结果付多少钱。由出价策略控制（最低费用、费用上限、竞价上限或 ROAS 目标）。出价可以是：
- **自动**（Meta 定）：最低费用下，Meta 想怎么出就怎么出，在预算内拿最多结果
- **半控制**（费用上限，Cost Cap）：Meta 以平均 CPA 不高于目标为目标
- **硬上限**（竞价上限，Bid Cap）：Meta 在单次竞价里绝不出超你设的上限
- **价值优化**（ROAS 目标）：Meta 优化广告支出回报率，不只优化转化量 [来源 5、6]

#### 要素 2：预估行动率

这是 Meta 预测特定用户看到广告后采取期望行动的概率。不只是历史 CTR。算法考虑：

- 该用户基于完整行为历史采取期望行动的可能性
- 时段和设备相关的转化概率
- 当前浏览会话和近期购买行为的上下文信号
- Instagram、Facebook、Messenger、Audience Network 的跨平台行为模式
- 用户在购买旅程中的位置（认知、考虑、转化），通过序列学习判断 [来源 1、7]

**具体例子：** 出价 $5.00、预估行动率 2.3%、质量分 0.85 的广告主，总价值 $0.098。但如果 Meta 检测到目标受众晚上 8 点在移动端更活跃，预估行动率可能升到 3.1%，总价值涨到 $0.132——不用改出价，这个时段就更有竞争力了。[来源 1]

#### 要素 3：广告质量 / 用户价值

Meta 对广告质量和用户体验价值的评估。这是在保护平台——Meta 要用户一直滑，烂广告伤的是它自己的生意。质量信号包括：

- 素材互动指标（评论、分享、收藏——正反馈）
- 用户反馈（隐藏、举报误导、举报垃圾——负反馈）
- 落地页体验和加载速度
- 与单个用户的相关性
- 品牌安全与政策合规
- 避开诱导互动和标题党 [来源 1、4、8]

### 花费节奏（Pacing）如何工作

Pacing 是防止 Meta 在一天头几个小时烧光预算的机制。两部分协同工作：

**预算 Pacing：** Meta 把日预算分摊到全天（或整个广告系列周期），找最好的机会。日预算下，Meta 以平均值为目标——单日最多可花到日预算的 **75% 以上**，但每周（周日到周六）总花费不超过日预算的 7 倍。[来源 9]

**出价 Pacing：** Meta 按全天竞争水平调整出价激进度。早上机会贵（某些时段竞争高），就收一收，等后面便宜的展示。[来源 10]

**关键 pacing 行为：**
- Meta 算法倾向在清晨竞争低、库存便宜时前置花钱——哪怕你的受众那个时段不转化 [来源 11]
- 一天中途改预算，Meta 按比例分摊：中午把 $100 预算提到 $200，Meta 的目标是剩下半天花约 $100 [来源 12]
- 改预算常触发短暂的花费 spike，算法在重新校准 [来源 12]
- 学习期算法可能花得更激进，为了更快收集转化数据 [来源 12]

### 出价策略——各用在什么时候

| 策略 | 如何工作 | 适合 | 风险 |
|----------|-------------|----------|------|
| **最低费用（Lowest Cost）**（默认） | Meta 想怎么出就怎么出，在预算内拿最多结果。无费用约束。 | 新广告系列、测试、出学习期、小预算 | CPA 可能不可预测地 spike；扩量表现差（预算翻倍常让 CPA 涨 50%） |
| **费用上限（Cost Cap）** | Meta 以平均 CPA 不高于你设的金额为目标。单个结果可能超上限，但平均值应该守住。 | 有 50-100+ 基准转化的成熟广告系列；需要可预测 CPL 的线索型 | 上限太紧会花不出去；上限设在当前平均 CPA 上 10-20% |
| **竞价上限（Bid Cap）** | 单次竞价的硬性最高出价。Meta 拒绝参与要更高出价的竞价。 | 严格的单均经济模型（affiliate、固定 payout）；要求每次获客都达标的业务 | 严重限制投放、拉长学习期；设太低可能一分钱花不出去 |
| **ROAS 目标** | 优化广告支出回报率，不只优化转化量。优先高价值购买而非数量。 | 商品价值差异大的电商；目录销售；重收入不重转化数的业务 | 需要准确的购买价值追踪；稳定下来更慢 |

[来源 5、6、13]

**ROAS 目标的关键 nuance：** 最低费用/费用上限/竞价上限只优化两个变量：广告花费和购买数（CAC = 花费 / 购买数）。ROAS 目标加了第三个变量——每次购买的**价值**。所以 ROAS 目标广告系列可能跳过转化概率 3%、购物车 $100 的用户，去要转化概率 2%、购物车 $200 的用户。[来源 14]

### 竞价重叠（Auction Overlap）

多个广告组定向相似受众时，你在跟自己竞价。Meta 的处理：

- **竞价重叠检测：** Meta 识别自己的广告组在争同一个展示
- **内部去重：** 每个展示只让自己表现最好的广告进竞价
- **但：** 广告组重叠太多还是会切碎数据和预算，拖慢学习、降低效率
- **解法：** 合并受众相似的广告组，集中数据信号 [来源 4]

---

## 2. 学习期

### 学习期技术上在发生什么

新建广告组或做重大修改时，Meta 投放系统进入学习期。这段时间算法在**探索**——测试不同受众细分、版位、时段、素材组合，给你的广告系列建预测模型。

**多数广告主忽略的关键洞察：** 学习期算法不是在浪费你的预算。它是在用早期预算做探索投资，好让后面花得更有效。那些看似浪费的、打给不转化的冷受众的展示，是在教系统：学习完成后要**避开**谁。[来源 15]

**学习期算法在做：**
1. 把广告推给多样化的受众细分，找谁转化
2. 测试不同版位（信息流、快拍、Reels、Audience Network），找最优投放
3. 调出价 pacing，摸清全天竞争怎么变
4. 给你的广告系列建转化预测模型
5. 给你的产品/offer 精调预估行动率

这阶段效果波动大——CPA 一天一个样。正常，预期内。[来源 16、17]

### 50 次转化规则

Meta 算法要求每个广告组 **7 天内约 50 次优化事件** 才能出学习期。这不是拍脑袋——是建可靠预测模型的统计下限。

**关键数学：**

```
最低日预算 =（目标 CPA x 50）/ 7
```

例子：
- 目标 CPA $40：每个广告组最低 $286/天
- 目标 CPA $60：每个广告组最低 $429/天
- 目标 CPA $100：每个广告组最低 $714/天

预算在数学上 7 天产不出 50 次转化，就会永远卡在"学习受限"。[来源 15、16、17]

**重要 caveat：**
- "50 次转化"指的是你选的优化事件（购买、线索、加购等）
- 7 天 50 次购买不现实，考虑优化更高漏斗事件（比如用加购代替购买），发生频率更高
- 具体阈值因广告系列目标和转化类型而异——有的 30 次就出，有的要更多
- 一个广告组 10 条广告，Meta 要测完 10 条，实际需要约 500 次转化（10 x 50）才能知道哪条最好 [来源 18]

### 什么会重置学习期

重大修改让算法重启探索。包括：

| 修改类型 | 重置学习期？ |
|------------|-----------------|
| 单次改预算超 20% | 是 |
| 改定向（受众、地域、年龄、性别） | 是 |
| 广告组里增删广告 | 是 |
| 改优化事件 | 是 |
| 改出价策略或出价金额 | 是 |
| 暂停 7 天+ 再开 | 是 |
| 小幅改预算（< 20%） | 通常不 |
| 文案微改（改一个词） | 通常不 |
| 给**别的**广告组加新广告 | 不（不影响现有广告组） |

**关键：** 广告组层级加预算超 20% 重置学习。但 CBO 下，加**广告系列层级**预算不会触发同样的重置——CBO 激进扩量稳定得多。[来源 15、17、19]

### 学习受限——什么意思、怎么修

"学习受限（Learning Limited）"和普通的"学习中"不一样。它是 Meta 算出的诊断结论：**当前条件下**你的广告组 7 天内**到不了** 50 次优化事件。这不是临时状态。

**常见原因：**
1. **预算相对 CPA 太低：** 日预算 $40、CPA $30，一天 1.3 次转化 = 38 天才出学习期（7 天内数学上不可能）
2. **受众太窄：** 小受众限制了算法找转化者的能力
3. **切太碎：** 广告组太多分预算，没有一个能到阈值
4. **出价/费用控制太紧：** 费用上限或竞价上限太死，竞价参与不够
5. **转化率低：** 落地页烂或 offer 弱，预算受众都够还是转化不够 [来源 15、20]

**修复方法：**
1. **加预算**，让日花费现实地做到每天 7+ 次转化（50/7）
2. **合并广告组**——把相似受众合并，集中预算和数据
3. **扩大受众**——Meta 建议受众最少 200 万效果最好
4. **优化事件上移**（购买量不够就用加购代替购买）
5. **放宽或去掉费用/竞价上限**，学习期别卡太死
6. **用 CBO** 代替 ABO，把转化信号汇总到广告组之间
7. **提高落地页转化率**，每块钱换更多事件 [来源 15、20、21]

### 小改动为什么会累积搞崩学习期

每次重置学习的修改都重启 7 天时钟。广告主每 2-3 天改一次（调预算、加广告、改定向），算法永远攒不够出学习期的数据。形成死亡螺旋：

1. 学习期效果差/波动，广告主看着难受
2. 改点东西"修一下"
3. 修改重置学习
4. 效果继续波动
5. 循环

需要的纪律：**上线或大改后至少 7 天别碰广告系列。** 第 3 天看着像拉胯，可能是算法在探索它以后会降权的细分。[来源 15、16、17]

---

## 3. Advantage+ 广告系列

### Advantage+ 购物广告系列（ASC）底层如何工作

Advantage+ 购物广告系列（现叫 Advantage+ 销售广告系列）是 Meta 自动化程度最高的广告系列类型。不用手动选受众、版位、出价策略，你给 Meta：

1. 预算
2. 产品目录
3. 素材资产（每个广告系列最多 150 条广告）
4. 国家定向

**其余一切——年龄、性别、兴趣、版位、出价——Meta 的 AI 包办。**[来源 22、23]

**底层 ASC 用 Meta 的 Andromeda 召回引擎**，在对的时间把对的产品匹配给对的人。它把拉新（找新客）和再营销（召回老客）合成一个广告系列，不用分开建漏斗。[来源 22]

**关键技术细节：**
- ASC 每个广告系列部署最多 **150 种素材组合**
- 出价优化、版位分配、受众选择全自动
- 可以设"老客预算上限"（比如再营销最多占 20% 预算）——这是少数可用的控制项之一
- 没有人口统计定向（不能选年龄、性别、兴趣）
- 每个广告账户最多同时跑 **8 个 ASC 广告系列** [来源 22、23、24]

### Advantage+ 受众

开启后（ASC 内或手动广告系列的设置项），Advantage+ 受众让 Meta 算法替你找受众，而不是你定义。算法用：

- 你的素材信号（广告讲什么、谁出镜）
- 你的 Pixel/CAPI 数据（以前谁转化过）
- Andromeda 召回引擎匹配可能转化的人
- 你给的"受众建议"只当起点，不当约束——Meta 会扩到建议之外

**Andromeda 更新后，兴趣定向和类似受众基本不带动效果了。** 素材就是定向。Meta 读钩子、版式、出镜人物、文案、落地页来决定推给谁。[来源 2、25]

### Advantage+ 版位

Meta 把广告分发到所有可用版位：
- Facebook 信息流、快拍、Reels、Marketplace、视频信息流、右侧栏
- Instagram 信息流、快拍、Reels、Explore
- Messenger 收件箱、快拍
- Audience Network

算法按"哪预测效果最好、成本最低"决定版位分配。转化类广告系列一般建议开。**警告：** 优化链接点击或落地页浏览时去掉 Audience Network（误点/点击欺诈），优化 ThruPlay 时去掉 Audience Network 激励视频（激励观看）。[来源 26]

### Advantage+ 素材

Meta 自动优化的素材元素包括：
- 文案变体（不同标题和正文组合）
- 图片背景扩展
- 视觉修饰和增强
- 版式适配（不同版位的宽高比调整）
- Reels 版位加音乐

Meta 称用 Advantage+ 素材工具的广告主 **ROAS 提升 22%**。[来源 3、25]

### ASC 什么时候打败手动（什么时候不行）

**ASC 最有效时：**
- Pixel 成熟，有大量转化历史
- 预算 $5,000+/月
- 素材库强（10+ 个不同概念）
- 想简化账户结构
- 通过目录卖货

**ASC 拉胯时：**
- Pixel 新，数据少
- 预算小（算法要量级学习）
- 需要精确控制受众细分
- 在验证非常具体的假设
- 需要定向算法可能忽略的特定人口/兴趣 [来源 22、23、27]

**效果基准：** ASC 广告系列通常比手动 CPA 低 10-20%，管理 overhead 小得多。有案例把多个广告系列合成 ASC 后 CPA 从 $52 降到 $35（降 33%）。[来源 22]

---

## 4. 广告系列预算优化（CBO）vs 广告组预算（ABO）

### CBO 如何分配预算

CBO（现叫 Advantage 广告系列预算）在广告系列层级设一个预算。Meta 算法实时按以下分给广告组：

- 每个广告组的预测转化概率
- 当前 CPM 和竞争水平
- 历史效果数据
- 实时竞价动态

**例子：** 5 个广告组、日预算 $250 的广告系列，可能给广告组 A 花 $150、B 花 $60、C 花 $30、D 和 E 几乎不花——因为算法预测 A 效果最好。[来源 28]

**CBO 关键行为：**
- 预算转移持续发生，经常按小时
- CBO 把所有广告组的转化信号汇总，把广告系列当一个学习单元
- 所以 CBO 比 ABO 更快攒够 50 次转化阈值（ABO 每个广告组独立学习）
- 广告系列层级加预算**不**重置学习期（ABO 下广告组层级加预算会）
- 可以给广告组设最低/最高花费限制，防止某个广告组被饿死 [来源 28、29、30]

### CBO 什么时候好、ABO 什么时候好

| 场景 | 最佳选择 | 为什么 |
|----------|------------|-----|
| 扩量已验证的广告组（3-5 个已盈利） | CBO | 算法自动找到最高效的分配 |
| 测新受众或新素材 | ABO | 保证每个测试变量分到等量预算 |
| 3+ 广告组、日预算 > $100 | CBO | 数据够算法做有意义的优化 |
| 需要精确控制每个受众的花费 | ABO | 每个广告组预算有保障 |
| 全漏斗策略（拉新 + 再营销） | CBO | 算法按效果在漏斗阶段间调预算 |
| 测特定变量的 A/B 测试 | ABO | 防止 Meta 饿死测试组 |

[来源 28、29]

**效果数据：** 测试/探索期 ABO 拉新平均 ROAS 94%，CBO 81%。但扩量期 CBO 持续胜出，因为实时优化对受众行为的微变化反应比人工快。Meta 2025 年 4 月内部数据：切到 CBO 后 **6 周内 ROAS 涨 17%**。[来源 28、29]

### CBO 的最低/最高花费限制

可以用广告组级限制约束 CBO 分配：
- **最低日花费：** 保证广告组至少拿到 X（测试期防饿死有用）
- **最高日花费：** 防止一个广告组吃掉整个广告系列预算

**最佳实践：** 日预算 $450、3 个广告组，每个设 $100 最低。保证所有广告组都出学习期，还剩 $150 给 Meta 动态优化。[来源 29]

---

## 5. 定向机制

### 宽泛定向到底怎么工作

Andromeda 更新后，Meta 的定向范式从**受众优先**根本转向**素材优先**。算法不再主要依赖兴趣分类或人口过滤。而是：

1. **Andromeda 扫描你的素材**——分析视觉、文案、钩子、出镜人物、产品类型、落地页内容
2. **预测谁会感兴趣**，把素材信号与用户行为模式匹配
3. **在宽泛人群里试投放**，从互动和转化信号中学习
4. **数据积累后收窄到高概率转化者**

所以素材就是定向。健身产品广告里 30 多岁女性在锻炼，广告自然会推给这个年龄段爱健身的女性——不是因为你选了这些兴趣，而是 Andromeda 读懂了素材。[来源 2、25、31]

### 兴趣定向——Meta 如何给用户分类

兴趣分类来自：
- 用户关注和互动的页面
- 互动的内容（帖子、视频、文章）
- 用的 App
- 点过或转化过的广告
- 加入的群组
- 参加过的活动
- 站外行为（Pixel 和 CAPI 数据， where available）

**后 Andromeda 现实：** 兴趣定向效果明显下滑。多数高效果账户现在只跑宽泛定向（只定国家），让素材信号驱动受众选择。紧的兴趣微受众常被宽泛设置打败，因为限制了算法在你的假设之外找转化者的能力。[来源 25、31]

### 自定义受众——匹配如何工作

自定义受众上传第一方数据（邮箱、电话等），Meta 与用户库匹配：

1. 上传客户标识（邮箱、电话、姓名等）
2. Meta 用 SHA-256 加密哈希后再匹配
3. 哈希标识与 Meta 用户库比对
4. 匹配上的用户组成自定义受众（匹配率通常 40-70%，看数据质量）
5. 没匹配上的数据丢弃

**匹配质量取决于：** 标识数量（邮箱+电话+姓名 > 只有邮箱）、数据新鲜度、标识是否与用户在 Meta 的资料一致。[来源 32]

### 类似受众——Meta 如何找相似用户

类似受众分析你的源受众（自定义受众或基于 Pixel），找特征相似的新用户：

1. Meta 分析源受众的数百个属性
2. 识别模式：人口统计、兴趣、行为、设备使用、内容消费
3. 给平台所有用户打相似分
4. 你选百分比（1% = 最像，10% = 更宽但没那么像）

**后 Andromeda 现实：** 和兴趣定向一样，类似受众作为主力策略效果下降了。Meta 自己的算法用宽泛定向常能同样甚至更高效地找到这些人。类似受众在 Advantage+ 广告系列里当受众建议还有用。[来源 25、31]

### "预估受众规模"到底什么意思

Ads Manager 里的受众规模条是**估算，不是保证**。它显示符合你定向条件的账户（不是人）的大概数量，基于近期平台活跃。它**不**预测：
- 多少人真会看到你的广告
- 多少人在活跃用平台
- 你的预算能触达多少

---

## 6. Pixel 与 Conversions API

### Meta Pixel 如何工作

Meta Pixel 是放在网站上的一段 JavaScript，用户做特定动作时触发：

1. 用户与 Meta 广告互动（或被展示）后访问你的网站
2. Pixel JavaScript 在用户浏览器加载
3. 追踪的事件发生（页面浏览、加购、购买）时 Pixel 触发，向 Meta 服务器发 HTTP 请求
4. 请求包含：事件名、事件价值、时间戳、用户标识（cookie）、页面 URL
5. Meta 把事件匹配到用户 Meta 档案和带来访问的广告

**浏览器端 Pixel 的局限：**
- 被广告拦截器屏蔽（影响 25-40% 桌面流量）
- 被浏览器隐私功能屏蔽（Safari ITP、Firefox ETP）
- 受 iOS 14.5 ATT 退出的影响
- Cookie 限制影响跨站追踪
- 纯 Pixel 配置**丢 20-40% 数据**
- 纯 Pixel 广告主的移动端上报转化最多**掉 61-72%** [来源 32、33、34]

### Conversions API（CAPI）如何工作

CAPI 走**服务器到服务器**——从你的 web 服务器直接发事件数据到 Meta 服务器，完全绕开浏览器：

1. 用户在网站完成动作
2. 你的服务器捕获事件数据
3. 服务器通过 Graph API 直接发给 Meta
4. 请求包含：事件名、价值、时间戳、哈希用户标识（邮箱、电话、IP、user agent）
5. Meta 把事件匹配到用户档案

**CAPI 相对 Pixel 的优势：**
- 不受广告拦截器影响
- 不受浏览器隐私功能影响
- 不受 Cookie 限制影响
- 能带 enrichment 数据（线下转化、CRM 数据、电话订单）
- 事件准确率**80-95%+**，Pixel 可靠性持续下滑
- 能带更多标识（邮箱、电话、external ID），匹配更好 [来源 32、33、34]

### Pixel 与 CAPI 的数据去重

Pixel + CAPI 双跑（推荐配置）时，同一事件可能发两次——浏览器一次、服务器一次。Meta 用两个字段去重：

1. **event_name：** 两边必须完全一致（比如"Purchase"）
2. **event_id：** 每个事件实例的唯一标识

Meta 在约 **5 分钟窗口**内收到 event_name 和 event_id 都匹配的两个事件，保留一个、丢弃重复。[来源 32、33、35]

**关键实施细节：** event_id 两边对不上，或命名规范不一致，去重就崩。结果要么转化重复计数（虚高效果），要么漏事件（低估）。两种都污染算法的优化信号。[来源 33]

### 事件匹配质量（EMQ, Event Match Quality）分

EMQ 是 Meta 给你的服务端事件数据打的分（0-10），衡量能多好地匹配到 Meta 用户档案。EMQ 越高 = 优化能力越强。

- **8-10（"Great"）：** 最优效果。广告系列效率、受众质量、ROAS 都强
- **7-8（"Good"）：** 扎实，还有提升空间
- **6 以下：** 归因精度明显损失。算法优化受损

**提高 EMQ：**
- 传更多标识：邮箱、电话、fbp（Facebook 浏览器 ID）、fbc（点击 ID）、IP、user agent
- 在 Events Manager 验证参数格式
- CAPI payload 别缺值、别畸形
- **EMQ 提高 2-3 分** 能 measurable 地降 CPM、提转化率 [来源 34、36]

---

## 7. 归因

### 归因窗口

Meta 默认归因设置是 **7 天点击、1 天互动、1 天浏览**。意思是：

| 窗口 | 追踪什么 |
|--------|---------------|
| **7 天点击** | 点击广告内链接后 7 天内的转化 |
| **1 天互动** | 与广告互动（点赞、分享、收藏、评论）但**没点**链接后 1 天内的转化 |
| **1 天浏览** | 被展示广告（没互动）后 1 天内的转化 |

**可选点击窗口：** 1 天、7 天、28 天（28 天只能在归因对比工具里看，不能当广告系列设置）

**最近变化（2026 年 3 月）：** Meta 把点击归因收窄到只算**链接点击**，社交互动（点赞、分享、收藏）不算了。以前用户点赞广告后购买算点击转化，现在算互动转化。[来源 37、38、39]

### Meta 归因和 GA4 有什么不同

Meta 和 Google Analytics 4 的衡量逻辑根本不同：

- **Meta：** 事件归因；统计归因到广告曝光（点击和浏览）的转化
- **GA4：** 会话归因；统计归因到带来网站访问的会话的转化
- **浏览归因：** Meta 默认含 1 天浏览；GA4 默认不含浏览归因
- **跨设备：** Meta 靠登录跨设备追踪用户；GA4 靠 cookie/用户 ID
- **报告延迟：** iOS 用户数据 Meta 最多延迟 **72 小时**（Apple 隐私变化）；GA4 接近实时 [来源 37]

**结果：** Meta 报的转化几乎永远比 GA4 多。不代表 Meta 在撒谎——它统计了 GA4 看不到的转化（浏览、跨设备、多日旅程）。但也意味着 Meta 可能把本来就会发生的转化算到自己头上。[来源 37、38]

### 浏览归因争议

1 天浏览归因有争议。Meta 算法聪明到能预测谁要买了，然后拼命给他展示广告，把购买归因拿走。以下情况尤其严重：
- 回头客多的
- 购买模式可预测的
- 宽泛定向，Meta 反正能找到可能转化的人

**但：** 归因设置里去掉 1 天浏览可能伤效果，因为 Meta 用浏览信号优化投放。算法有浏览数据学得更快、定向更准，哪怕部分浏览转化是虚高的。[来源 37、40]

### 增量归因（2025 年新出）

Meta 现在提供"增量（Incremental）"作为替代归因模型，用统计模型估算广告曝光**带来**的转化提升，哪怕超出标准归因窗口。它比标准归因保守，但是模型估算（概率性），不是直接测量的。[来源 37、38]

### 汇总事件衡量（AEM, Aggregated Event Measurement）

AEM 是 Meta 给 iOS 14.5+ 退出追踪用户的隐私保护归因系统。关键机制：

- **2025 年 6 月更新：** Meta 取消了之前的 8 事件上限。所有符合条件的标准和自定义事件自动处理，不用手动排序配置
- AEM 不再要求域名验证（只在链接归属或 iOS App 配置时要）
- Events Manager 里的 AEM 配置页签已移除
- 改完不再等 72 小时（不用手动配置了）
- Pixel 和 CAPI 配置依然关键——配好后 Meta 自动处理 AEM 事件 [来源 41]

---

## 8. 质量与相关性

### 广告相关性诊断

Meta 在 2019 年用三个独立诊断取代了单一的相关性分数（1-10 分）。广告过 **500 次展示** 后开始衡量，与定向同一受众的其他广告对比：

#### 1. 质量排名（Quality Ranking）

衡量感知广告质量，基于：
- 正负用户反馈（隐藏、举报垃圾 vs 互动）
- 高质量图片/视频
- 语法正确、可读性好
- 避开诱导互动/标题党
- 落地页体验与广告承诺一致

**可能值：**
- 高于平均（前 55%+）
- 平均（35-55 分位）
- 低于平均（后 35%）
- 低于平均（后 20%）
- 低于平均（后 10%）

#### 2. 互动率排名（Engagement Rate Ranking）

预估互动（点赞、评论、分享、收藏、点击）与同类广告的对比。这是相对值——竞品同受众拿 500 赞时你的 200 赞没意义。反映素材的情绪感染力。

#### 3. 转化率排名（Conversion Rate Ranking）

预估转化率与同优化目标、同类受众广告的对比。反映落地页质量、offer 强度，以及广告是否为点击后的动作设对了预期。

[来源 42、43、44]

### 质量如何影响 CPM 和投放

低质量排名直接推高成本、压低投放：
- **总价值越高 = 实际 CPM 越低。** 质量分强的广告用更低出价赢竞价，展示更便宜
- **三项全是低于平均（后 20%）** 说明有根本问题，素材/定向/落地页要立刻修
- **从低到平均的提升 > 从平均到高于平均的提升**——先修最差的诊断项 [来源 42、44]

### 素材疲劳——Meta 如何检测

素材疲劳是受众看同一条广告太多次。Meta 的指标：

| 信号 | 阈值（拉新） | 阈值（再营销） |
|--------|------------------------|------------------------|
| 频次 | > 2.5 | > 6-8 |
| CTR 相对峰值下跌 | > 20% | > 30% |
| CPM 上涨 | > 50% | > 40% |
| 首次展示占比 | < 50% | 不适用 |

**"3-2-1 上新框架"：**
- 上线期每 **3 天** 看一次效果
- **2 个**预警信号同时出现（频次 > 2.5 且 CTR 跌 20%）
- 最多 **1 周** 内上新素材，否则效果明显恶化

**按花费的上新频率：**
- 高花费账户（$100K+/月）：每 2-3 周上新
- 标准账户：高频次每 7-14 天，低频次每 4-6 周
- 再营销广告系列：每 4-6 周上新

**Meta 广告平均寿命：** 活跃投放下 3-5 天出现初期疲劳信号。第 7 天 CTR 通常比峰值跌 20-40%。[来源 45、46、47]

---

## 9. 投放与优化

### Meta 的 ML 模型如何预测谁会转化

Meta 的预测系统用多种神经网络架构协同：

1. **卷积神经网络（CNN）：** 分析素材资产——图片内容、视频帧、视觉元素
2. **循环神经网络（RNN）：** 建模序列行为模式——用户看广告前后做了什么
3. **Transformer 模型：** 处理用户兴趣、产品、广告内容之间的上下文关系

系统每天处理**数十亿次用户互动**，综合信号：
- 站内行为（在 Facebook/Instagram 上互动了什么）
- 跨平台信号（Meta 系 App 的行为）
- 广告主网站的 Pixel/CAPI 转化数据
- 上下文信号（时段、设备、网速、浏览上下文）
- 序列学习（用户在购买旅程的位置）[来源 1、7]

### Andromeda + GEM：双系统架构

截至 2025 年底，Meta 广告投放跑在两个 AI 系统协同上：

**Andromeda（召回——2024 年底）**
- 决定哪些广告**有资格**被展示（召回/过滤阶段）
- 深度神经网络，NVIDIA Grace Hopper Superchips 上 10,000 倍模型容量
- 召回率 +6%、广告质量 +8%
- 处理素材内容、行为历史、互动模式的信号
- 和老系统反着来：先评估素材，再找匹配用户 [来源 3、31]

**GEM（生成式广告推荐模型，Generative Ads Recommendation Model——2025 年中）**
- 平台广告预测的**中央智能**
- 按大语言模型规模搭建
- 识别自然互动和广告序列中的模式
- 综合互动、行为、转化数据
- 把预测喂回 Andromeda 改进召回决策
- 2025 年 Q3 架构改进后，加数据加算力的效果翻倍 [来源 25、48]

### 规模化转化优化

选"转化"为优化目标时，Meta：

1. 识别最可能完成你选的转化事件的用户
2. 计算每次潜在展示的转化概率
3. 把概率代入总价值公式（作为预估行动率）
4. 按真实转化数据持续更新预测
5. 把投放转向转化概率最高的用户细分

**自我强化循环：** 转化数据越多 = 预测越准 = 转化率越高 = 数据更多。所以初始转化信号强的广告系列越跑越好，起手就差的常常一直差——算法缺优化信号。[来源 7、26]

### 落地页体验信号

Meta 算广告质量时会看落地页质量。信号包括：
- 页面加载速度（慢的被罚）
- 内容相关性（页面和广告承诺对得上吗？）
- 落地后用户行为（跳出率、停留时长、滚动深度）
- 移动端优化
- 弹窗密度和 UX 质量

这些信号既进质量排名诊断，也进总价值公式的广告质量项。[来源 1、4]

### 频次与覆盖优化

优化覆盖时，Meta 在预算内把广告推给尽量多的独立用户。关键机制：
- 可手动设频次上限
- 优化覆盖时算法优先最便宜的版位（可能不是最有效的）
- 覆盖为目标时，考虑把版位限在最有效的（信息流、快拍、Reels），别全开 [来源 26]

### 预算为什么花不均匀

日花费不均匀的几个原因：

1. **周平均：** 日预算是平均值——单日最多可花到 75% 以上
2. **机会型 pacing：** Meta 识别转化"好日子""坏日子"，调花费
3. **竞价竞争波动：** CPM 按小时、星期、季节差很多
4. **学习期花费：** 新广告系列早期常激进花钱收数据
5. **小受众饱和：** 受众有限时，Meta 可能快速花完再没优质展示
6. **Q4 竞争：** 节假日 CPM 翻倍甚至更多，同预算展示少得多 [来源 9、11、12]

---

## 10. iOS 14.5+ 的影响

### Apple 的 ATT 框架改变了什么

2021 年 4 月，Apple 的 App 跟踪透明度（ATT, App Tracking Transparency）框架要求 App 跨 App/跨网站跟踪前必须弹框要明确许可。对 Meta 的影响：

- **ATT 前：** Meta 靠 cookie 和 IDFA（广告商标识符）跨网跟踪用户，建详细的跨站行为画像
- **ATT 后：** 多数用户拒绝（拒绝率约 75-85%）。Meta 丢了大量跨 App、跨站追踪数据
- **结果：** 定向精度降、转化延迟/漏报、优化信号变差 [来源 49、50]

### 广告主的具体变化

| 领域 | iOS 14.5 前 | iOS 14.5 后 |
|------|-----------------|----------------|
| 归因窗口 | 默认 28 天点击、7 天浏览 | 收窄到 7 天点击、1 天浏览 |
| 转化报告 | 接近实时 | iOS 用户最多延迟 72 小时 |
| 事件追踪 | 每个域名无限事件 | 之前限 8 个优先事件（已取消——2025 年 6 月） |
| 受众定向 | 详细的跨站行为画像 | 第三方数据少，更依赖第一方和站内信号 |
| 优化信号 | 丰富的跨平台数据 | 更依赖建模/估算转化 |
| 报告粒度 | 完整人口统计拆解 | 退出用户拆解有限 |

[来源 49、50、51]

### 代理商如何绕开数据限制

**1. 第一方数据策略：**
- 通过自有渠道建邮箱/电话名单
- CRM 对接自定义受众，提高匹配率
- 用购买历史和客户分群做定向
- 上传线下转化数据到 Meta

**2. 服务端追踪（CAPI）：**
- 完全绕开浏览器端追踪限制
- 带 Pixel 拿不到的标识，做数据 enrichment
- Triple Whale 报告（2025 年 4 月）：用 CAPI 的品牌数据质量分和广告系列效果都更好

**3. 宽泛定向 + 素材主导策略：**
- 少依赖行为定向（信号丢了）
- 让 Meta 算法靠素材信号找转化者
- 测多个素材角度，不测多个受众细分

**4. 增强衡量：**
- 退出用户用汇总事件衡量（AEM）
- 转化建模（Meta 用统计模型估算漏报的转化）
- 多触点归因工具（Triple Whale、Northbeam、Hyros）交叉验证 Meta 上报数据
- GA4 + 后端数据对比验证转化准确性

**5. Conversions API 实施：**
- 服务端追踪补浏览器限制漏掉的事件
- 更多用户标识提高匹配质量
- 与 Pixel 实时去重 [来源 49、50、51、52]

### App 广告系列的 SKAdNetwork

Apple 的 SKAdNetwork 给 App 安装广告系列提供有限的聚合转化数据：
- 报告延迟（24-48 小时）
- 转化价值信息有限
- 无用户级归因
- 只有广告系列级数据（无素材级拆解）
- Meta 把 SKAdNetwork 数据整合进 App 广告系列报告，但粒度比 ATT 前少得多 [来源 49]

---

## 值得深挖的线索

1. **GEM 对素材测试的影响：** GEM 越来越强，可能根本改变素材测试——模型能在花大钱实测前预测出赢家素材。

2. **Andromeda 的序列学习：** 系统理解用户在购买旅程的位置并按序推广告的能力还在进化。这可能让传统漏斗式广告系列结构（TOFU/MOFU/BOFU）过时。

3. **增量归因的成熟度：** Meta 的增量归因模型还新，相对 lift study 的精度有待规模化验证。

4. **"死水坑"现象：** 有记录的模式——新广告系列初期靠新鲜受众细分效果好，然后算法过度 targeting 同一个坑直到榨干，效果下滑。

5. **建模转化的精度：** Meta 越来越靠统计建模估算漏报转化（ATT 退出的锅），上报和实际的 gap 越来越让人担心。

---

## 来源引用

1. Madgicx -- "How Machine Learning Facebook Ads Work: 2025 Algorithm Guide" -- https://madgicx.com/blog/machine-learning-facebook-ads
2. Lucid Media -- "How the Meta Ads Algorithm Actually Works in 2026" -- https://lucidmedia.co.nz/blog/meta-ads-algorithm-explained-2026
3. Meta Engineering Blog -- "Meta Andromeda: Supercharging Advantage+ automation" (Dec 2024) -- https://engineering.fb.com/2024/12/02/production-engineering/meta-andromeda-advantage-automation-next-gen-personalized-ads-retrieval-engine/
4. LeadEnforce -- "How the Meta Ad Auction Works and Why It Impacts CPC and ROAS" -- https://leadenforce.com/blog/how-the-meta-ad-auction-works-and-why-it-impacts-cpc-and-roas
5. Benly -- "Meta Ads Bidding Strategies: Cost Cap vs Bid Cap vs ROAS Target 2026" -- https://benly.ai/learn/meta-ads/bidding-strategies-guide
6. Jon Loomer -- "Facebook Ads Bid Strategies: Lowest Cost, Cost Cap, Bid Cap" -- https://www.jonloomer.com/facebook-ads-bid-strategies/
7. mr.Booster -- "Meta Targeting in 2025: What's Changed and How to Stay Ahead" -- https://mrbooster.com/meta-targeting-in-2025-whats-changed-and-how-to-stay-ahead/
8. 1ClickReport -- "Meta Value Rules 2026: Setup Guide" -- https://www.1clickreport.com/blog/meta-value-rules-2025-guide
9. Meta Business Help Center -- "About Daily Budgets" -- https://www.facebook.com/business/help/190490051321426
10. Meta Business Help Center -- "About Pacing" -- https://www.facebook.com/business/help/1754368491258883
11. Wevion/Adrow -- "Budget Pacing Facebook Ads Complete Guide 2026" -- https://adrow.ai/en/blog/budget-pacing-facebook-ads-guide/
12. LeadEnforce -- "Meta Daily Budgets Explained" -- https://leadenforce.com/blog/meta-daily-budgets-explained-why-facebook-ads-spend-more-than-expected
13. Spinta Digital -- "Meta Ads Bidding Strategies 2026" -- https://spintadigital.com/blog/meta-ads-bidding-strategies-2026/
14. Andrew Faris (LinkedIn) -- "Meta Ads Bidding Nuance: tROAS" -- https://www.linkedin.com/posts/andrew-faris-980b84108_meta-ads-bidding-nuance-that-most-people-activity-7345486027374870530-jGKw
15. AdStellar -- "Meta Ads Learning Phase Struggles: Complete Guide 2026" -- https://www.adstellar.ai/blog/meta-ads-learning-phase-struggles
16. Meta Business Help Center -- "About the Learning Phase" -- https://www.facebook.com/business/help/112167992830700
17. AdAmigo -- "Meta Ads Learning Phase: Manage Volatility" -- https://www.adamigo.ai/blog/meta-ads-learning-phase-manage-volatility
18. Lebesgue -- "About the Facebook Ads Learning Phase [2025 Update]" -- https://lebesgue.io/facebook-ads/facebook-ads-learning-phase-what-you-need-to-know-2024-update
19. Vaizle Insights -- "ABO vs CBO in Meta Ads" -- https://insights.vaizle.com/abo-vs-cbo-meta-ads/
20. Meta Business Help Center -- "About Learning Limited" -- https://www.facebook.com/business/help/269269737396981
21. Modern Marketing Institute -- "How to Exit the Meta Ads Learning Phase Fast" -- https://www.modernmarketinginstitute.com/blog/how-to-exit-the-meta-ads-learning-phase-fast-and-start-scaling-profitably-in-2026
22. Adverge Media -- "Meta Advantage+ Shopping Campaigns: Complete Setup Guide 2026" -- https://advergemedia.com/blog/meta-advantage-plus-shopping-campaigns/
23. Birch -- "Understanding Meta's Advantage+ Sales Campaigns [2025 Guide]" -- https://bir.ch/blog/advantage-plus-sales-campaigns-guide
24. Lunio -- "Meta Advantage+ Shopping Campaigns: Everything You Need to Know" -- https://www.lunio.ai/blog/meta-advantage-plus-shopping-campaigns
25. iMarkInfotech -- "How Meta Andromeda Is Transforming Ad Targeting" -- https://www.imarkinfotech.com/how-meta-andromeda-is-transforming-ad-targeting/
26. Jon Loomer -- "A Guide to Meta Ads Optimization for Delivery" -- https://www.jonloomer.com/a-guide-to-meta-ads-optimization-for-delivery/
27. Needle AI -- "A Founder's Guide to Meta Advantage Plus Shopping Campaigns" -- https://www.askneedle.com/blog/meta-advantage-plus
28. Adswize -- "Facebook Ads Budget: CBO vs ABO, Which Wins in 2025?" -- https://adswize.app/blog/facebook-ads-budget-cbo-vs-abo
29. AdAmigo -- "CBO Best Practices for Meta Ads 2025" -- https://www.adamigo.ai/blog/cbo-best-practices-meta-ads
30. AdsUploader -- "ABO vs CBO: Which Budget Strategy Actually Works in 2026" -- https://adsuploader.com/blog/abo-vs-cbo
31. Search Engine Land -- "Inside Meta's AI-driven advertising system: How Andromeda and GEM work together" -- https://searchengineland.com/meta-ai-driven-advertising-system-andromeda-gem-468020
32. Tracklution -- "Conversions API vs Meta Pixel: The Difference & How to Decide" -- https://www.tracklution.com/learn/conversion-tracking/conversions-api-vs-meta-pixel/
33. Marketer.com -- "Meta Pixel vs CAPI: What's the Difference?" -- https://www.marketer.com/blog/meta-pixel-vs-capi-what-s-the-difference-and-do-you-really-need-both
34. WeTracked -- "Meta Ads CAPI Explained (2026)" -- https://www.wetracked.io/post/what-is-capi-meta-facebook-conversion-api
35. Conversios -- "What Is Facebook Event Deduplication & Why It Matters" -- https://www.conversios.io/blog/facebook-event-deduplication-pixel-capi/
36. Tomaque -- "The Ultimate Guide to Event Match Quality" -- https://www.tomaque.com/the-ultimate-guide-to-event-match-quality-facebook-pixel-meta-capi-and-conversion-api-in-2025/
37. Aimerce Newsletter -- "Understanding Facebook Attribution in 2025" -- https://newsletter.aimerce.ai/p/understanding-facebook-attribution-in-2025
38. Jon Loomer -- "How Meta Ads Attribution Works in 2026" -- https://www.jonloomer.com/meta-ads-attribution-2026/
39. Media Performance -- "Meta Engage-Through Attribution Explained" -- https://www.mediaperformance.co.uk/meta-engage-through-attribution/
40. Ben Gould -- "Why You Shouldn't Remove 1-Day View Attribution" -- https://beng501.wordpress.com/2024/10/11/why-you-shouldnt-remove-the-1-day-view-attribution-from-your-meta-ads/
41. Conversios -- "Meta Aggregated Event Measurement (AEM) Explained 2025" -- https://www.conversios.io/blog/meta-aggregated-event-measurement/
42. AdStellar -- "Meta Ads Performance Metrics Explained: 2026 Guide" -- https://www.adstellar.ai/blog/meta-ads-performance-metrics-explained
43. KlientBoost -- "26 Tips To Get a Better Facebook Ad Quality Ranking" -- https://www.klientboost.com/facebook/facebook-ad-quality-ranking/
44. LeadEnforce -- "How to Interpret Facebook's Quality Ranking, Engagement, and Conversion Rates" -- https://leadenforce.com/blog/how-to-interpret-facebooks-quality-ranking-engagement-and-conversion-rates
45. AdAmigo -- "Meta Ads Frequency Benchmarks (When Ads Start Fatiguing)" -- https://www.adamigo.ai/blog/meta-ads-frequency-benchmarks-when-ads-start-fatiguing
46. BestEver -- "Creative Fatigue: What It Is and How to Avoid It in 2025" -- https://www.bestever.ai/post/creative-fatigue
47. Flighted -- "How to Identify and Fix Meta Ad Fatigue in 2026" -- https://www.flighted.co/blog/how-to-identify-and-fix-meta-ad-fatigue
48. ALM Corp -- "Meta Andromeda & GEM: Complete AI Advertising System Guide 2026" -- https://almcorp.com/blog/meta-andromeda-gem-ai-advertising-system-guide/
49. AdNabu -- "iOS 14 Impact on Facebook Ads: Key Changes & Solutions 2026" -- https://blog.adnabu.com/shopify/ios-14-impact-on-facebook-ads/
50. Does Infotech -- "Meta Ads Work with iOS Updates & Privacy Changes" -- https://doesinfotech.com/how-meta-ads-work-with-the-latest-ios-updates-and-privacy-changes/
51. Conquerra Digital -- "Meta Ads Performance Post-iOS Privacy Changes" -- https://conquerradigital.ae/meta-ads-performance-post-ios-privacy-changes-whats-working-now/
52. iamattila -- "Meta Ads Performance Shift: Trends & Strategies (April 2025)" -- https://iamattila.com/meta-ads/
