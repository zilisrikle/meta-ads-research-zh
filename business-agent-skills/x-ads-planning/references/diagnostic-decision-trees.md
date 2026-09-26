# 诊断决策树（Diagnostic Decision Trees）

已有 X Ads 账号出现效果问题时，使用本参考文档。

## 快速分流

从可见症状出发，再分支到第一个可能瓶颈：

```text
Delivery low?
  -> Check billing / dates / disapproval / eligibility
  -> If clean: check bid, budget, audience size, and Optimized Targeting

Traffic weak?
  -> If impressions low: delivery or targeting problem
  -> If impressions high and CTR low: creative / message / context problem

Conversions weak?
  -> If clicks low quality: targeting or intent mismatch
  -> If clicks good but CVR low: destination, event, offer, or trust problem

Reported performance volatile?
  -> If event volume low: budget / fragmentation / conversion delay
  -> If event volume healthy: creative fatigue, audience mix, or measurement issue
```

不要一次改所有层。先修第一个确认的瓶颈，记录变更，等到信号足够再进入下一层。

## 花费低或投放受限

可能原因：

- 预算、出价或目标过于严格。
- 在遵守约束和排除后，受众太窄。
- 需要规模时未开启优化定向（Optimized Targeting）。
- 最高出价（Maximum bid）或目标成本（Target cost）低于竞价能成交的水平。
- 创意被拒审或质量低。
- 账号、市场或格式资格问题。
- 优质/托管式产品在该账号实际不可用。

行动：

- 先检查投放状态、拒审、账单、资金来源和广告系列日期。
- 放宽弹性定向输入，或在合适处开启优化定向。
- 减少不必要的广告组碎片化。
- 确认目标、出价类型和优化目标与期望结果一致。
- 如果广告系列投放不到 24 小时，先区分正常爬坡和真正的无投放，再重建。

## CTR 低

可能原因：

- 第一行、第一帧或视觉对比弱。
- 信息与受众、关键词或对话语境不匹配。
- 通用创意复用，没有 X 原生框定。
- 话题标签/URL/emoji 杂乱或美学质量差。

行动：

- 用差异化概念替换装饰性变体。
- 文案匹配关键词/搜索意图或粉丝相似假设。
- 提高视觉清晰度，去掉杂乱。
- 测试更强的钩子、证据点、offer 或产品演示。
- 阅读回复和引用帖语境；低质量或敌意回复常常解释了下游行为弱的原因。

## 点击便宜但转化弱

可能原因：

- 业务需要转化，目标却优化向流量或互动。
- 落地页信息不匹配、加载慢、表单摩擦或信任低。
- 转化事件太浅。
- 受众信号宽但不精准。

行动：

- 在追踪和量级支持时，转到 Website conversions / Sales。
- 审计落地页速度、offer、证据、表单长度和移动端体验。
- 按拉新 vs 再营销、关键词/受众主题拆分报告。
- 用 CRM 或订单质量判断便宜点击是否有用。

## CPA 或 ROAS 波动

可能原因：

- 转化量低或转化报告延迟。
- 同时改动太多。
- 预算相对广告组/创意数量太小。
- 再营销和拉新混在一个读数里。
- Pixel/CAPI/目录事件问题。
- 把展示后或互动后归因当作最终收入真相来读。

行动：

- 改策略前先检查事件健康度和转化延迟。
- 合并结构。
- 尽可能把再营销和拉新分开。
- 批量变更，等待有意义的观察窗口。
- 用 3-7 天滚动读数和外部真相来源报告，再做重大变更。

## 线索质量低

可能原因：

- 优化向原始表单填写，而不是合格线索。
- offer 吸引了低意图用户。
- 落地页过度承诺或筛选不足。
- 定向太宽，创意筛选不够。

行动：

- 把 CRM 质量反馈回报告或线下分析。
- 在落地页/表单加筛选。
- 用文案既吸引合适用户，也劝退不合适用户。
- 追踪合格线索率，而不只看 CPL。

## DPA 效果弱

可能原因：

- 事件与目录之间的产品 ID 不匹配。
- Feed 的标题、价格、可用性或图片有问题。
- 再营销窗口相对购买周期太宽或太窄。
- 产品集包含低利润或无货产品。

行动：

- 审计 X Shopping Manager 的 feed 健康度和事件匹配。
- 复盘产品层级表现和库存。
- 拆分拉新、再营销和交叉销售/加售的角色。
- 当收入 ROAS 误导时，优化向利润或边际贡献。

## 应用广告系列结果与 MMP 不一致

可能原因：

- X 与 MMP 的归因窗口不同。
- SKAdNetwork 或隐私约束影响 iOS 报告。
- 事件没有正确映射进 X Events Manager。
- 报告尚未定稿。

行动：

- 对齐 X 与 MMP 的归因窗口。
- 在 Events Manager 中检查事件状态。
- 与应用分析和 LTV 对账。
- 在相关处等待 24-48 小时让报告定稿。

## 关键词广告系列效果差

可能原因：

- 关键词太宽、歧义，或混了不同意图层级。
- 缺少否定关键词。
- 创意没有回应搜索/帖子语境。
- 优化定向或额外版位让关键词读数不纯。
- 在积累足够搜索/关键词数据前就暂停了广告系列。

行动：

- 拆分品牌/品类/问题/竞品/事件关键词。
- 为无关含义和不安全语境加排除关键词。
- 重写文案，在第一行回应关键词意图。
- 复盘关键词明细，从优胜者扩展。
- 用 UTM 和落地页分析判断质量，而不只看 CTR。
