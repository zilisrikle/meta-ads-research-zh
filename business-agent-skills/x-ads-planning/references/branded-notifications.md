# 品牌通知（Branded Notifications）

Branded Notifications 让用户在关键时刻订阅接收来自品牌的后续消息。

规格来源：X Branded Notifications 产品页面（https://business.x.com/en/advertising/branded-notifications）。最后核实：2026-04。

## 可用性

Branded Notifications 有资格门槛，运营负担比标准广告更重。

- 面向美国和加拿大的托管式广告主可用；与 X Next 合作的广告主全球可用。
- 需要公开且已验证的 X 账号。
- 通过 **Arrow**（IC Group 与 X Next 合作开发的第三方自动回复平台）配置和启动。
- 需要通过 X OAuth 授权 Arrow 访问品牌账号。
- 在围绕该功能构建计划之前，请先核实当前市场、账号、X Next 和审批要求。

## 适用场景

- 产品投放。
- 票务销售。
- 剧集发布。
- 活动提醒。
- 限时优惠。
- 连载式叙事。

## 流程

```text
Promoted CTA post
  -> User opts in by engaging
  -> Arrow listens for opt-ins via X APIs
  -> Automated @mention notification post
  -> Follow-up content or conversion destination
```

## 广告系列格式

| 格式 | 用途 |
|---|---|
| Scheduled Notifications | 即时订阅通知 + 一次计划通知 |
| Subscription Notifications | 即时订阅通知 + 多次计划通知 |
| Instant Notifications | 订阅后即时通知 |

## 规划输入

| 输入 | 为什么重要 |
|---|---|
| 触发时刻 | 通知必须有存在的理由 |
| 订阅消息 | 决定参与质量 |
| 后续内容 | 必须兑现承诺 |
| 时机 | 太早或太晚都会降低价值 |
| 衡量 | 追踪订阅、通知互动和下游行为 |

## 创意实践

- 让订阅价值明确。
- 文案简洁。
- 避免过度使用提醒。
- 将通知匹配到真实时刻：发布、揭晓、促销、直播、上线。

## 衡量

使用订阅率、通知互动、点击率、转化率，以及可能情况下的增量提升（incremental lift）。X Ads Manager 提供推广 CTA 帖的完整指标，但通知帖的报告可能有限。用户的设备和账号通知设置可能导致通知帖无法展示。

## 诊断

| 症状 | 可能原因 | 应对 |
|---|---|---|
| 订阅率低 | 提醒价值不明确 | 把时刻和回报说清楚 |
| 有订阅但后续跟进弱 | 通知时机或内容没有兑现承诺 | 收紧排期，提高落地页相关性 |
| 报告缺口 | 通知帖指标有限 | 提前规划 CTA 帖指标、下游 UTM 和提升/代理指标读取 |

## 运营节奏

- 在内部推销该想法之前，先确认 Arrow、OAuth、验证账号和 X Next / 客户团队的接入。
- 尽早锁定排期、文案和审批流程。
- 把通知表现与下游业务指标一起复盘，而不只看订阅数。
