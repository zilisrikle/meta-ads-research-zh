---
name: gtm-tracking-setup
description: 设计分析追踪并输出可导入的 GTM 容器 JSON，覆盖 GA4、Google Ads、Microsoft Ads、Meta、TikTok、X、Pinterest、Snap、Reddit、LinkedIn、Microsoft Clarity 与 Hotjar。当用户想要搭建或扩展追踪时触发——转化追踪、事件追踪、dataLayer 设计、像素/代码安装、会话录制或热图——包括随口一提的需求如 "wire up Google Analytics"、"track form submissions"、"add a Meta pixel"、"set up purchase tracking" 或 "I need a tracking plan"。即使用户没有明确提到 GTM 也使用本 skill，因为浏览器端追踪几乎总是经由 GTM 落地。不触及容器的纯 GA4 管理后台配置（自定义维度、受众、报告）可跳过。
---

# GTM 追踪搭建

为 GA4、GTM 及各广告平台设计追踪配置，并输出可导入的 GTM 容器 JSON 文件。

## 工作流

```
0. 阅读应用代码 → 1. 收集需求 → 2. 查阅参考手册 → 3. 参照示例 → 4. 输出 JSON → 5. 落地指南
```

---

## 0. 阅读应用代码

如果可以读到被追踪站点/应用的源代码（工作区的任意位置，或用户提供的路径），**先阅读并理解代码再开始设计。**阅读真实代码能显著提升 dataLayer 设计、触发器条件和事件定义的准确性——靠猜表单 ID 或路由规则，只会造出上线后默默失效的触发器。

仅在无法读取代码时跳过此步骤（例如用户在对接一个他读不到的第三方站点）。

**需要检查的点：**
- 页面结构与路由（SPA/MPA 判断、主要页面识别）
- 表单实现（表单 ID、提交处理、校验）
- 电商功能（购物车、结算、购买完成流程）
- 已有的 `dataLayer.push` 调用或分析相关代码
- CTA、电话链接、站外链接的实现模式
- HTML `data-*` 属性和类名（可用于触发器条件的元素）

**流程：**
1. 询问用户应用源代码在哪里（或在工作区中显而易见时与用户确认）
2. 先探索结构（Glob/Grep），再阅读单个文件
3. 阅读主要页面与组件的源码
4. 识别已有的追踪实现，避免重复埋点

---

## 1. 收集需求

向用户收集以下信息，遇到不清楚的要提问澄清。

| # | 项目 | 示例 | 必填 |
|---|------|-----|------|
| 1 | 应用路径（代码可读时） | （用户提供的路径） | 源代码可读时 |
| 2 | 站点类型 | 企业站 / 落地页 / 电商 / 线索收集 | ✅ |
| 3 | 使用的平台 | GA4、Google Ads、Meta、TikTok、X、Clarity | ✅ |
| 4 | 主要转化 | 购买、表单提交、电话拨打等 | ✅ |
| 5 | SPA / MPA | MPA | 用于决定 page_view 控制方式 |
| 6 | GTM 账号 ID | `123456` | 输出 JSON 时 |
| 7 | GTM 容器 ID | `789012` | 输出 JSON 时 |
| 8 | 容器公开 ID | `GTM-XXXXXXX` | 输出 JSON 时 |
| 9 | 站点域名 | `example.com` | 输出 JSON 时 |
| 10 | 各平台 ID | GA4 衡量 ID、Pixel ID 等 | 输出 JSON 时 |

**访谈技巧：**
- 知道站点类型和使用的平台之后就可以开始设计
- 各平台 ID 只需要在输出 JSON 前确认（设计阶段可用占位符）
- 电商站点确认电商漏斗（view_item → add_to_cart → begin_checkout → purchase）是否适用

---

## 2. 参考手册

各平台的详细规格在 `references/` 目录下的手册中。**只阅读用户所选平台对应的手册。**

| 手册 | 内容 | 阅读时机 |
|---|---|---|
| `references/google-tag-manager.md` | GTM 容器设计、命名规范、JSON 规范 | **必读**（JSON 输出的基础） |
| `references/google-analytics.md` | GA4 媒体资源设计、事件体系、推荐事件、电商 | 使用 GA4 时 |
| `references/google-ads.md` | 转化设计、增强型转化、动态再营销 | 使用 Google Ads 时 |
| `references/microsoft-ads.md` | UET 代码、转化目标、自定义事件、dataLayer 映射 | 使用 Microsoft Ads / Bing 时 |
| `references/meta-pixel.md` | 标准事件、自定义事件、转化 API（Conversions API） | 使用 Meta 时 |
| `references/tiktok-pixel.md` | 标准事件、Events API、高级匹配（Advanced Matching） | 使用 TikTok 时 |
| `references/x-pixel.md` | 标准事件、转化 API（Conversion API） | 使用 X 时 |
| `references/pinterest-tag.md` | 标准事件、转化 API（Conversions API）、增强匹配（Enhanced Match） | 使用 Pinterest 时 |
| `references/snap-pixel.md` | 标准事件、Conversions API v3、高级匹配、去重（dedup） | 使用 Snap 时 |
| `references/reddit-pixel.md` | 标准事件、转化 API（Conversions API）、高级匹配（Advanced Matching）、动态产品广告（DPA） | 使用 Reddit Ads 时 |
| `references/linkedin-insight-tag.md` | Insight Tag、转化、Matched Audiences、转化 API（Conversions API） | 使用 LinkedIn 时 |
| `references/microsoft-clarity.md` | 热图、会话录制、自定义标签 | 使用 Clarity 时 |
| `references/hotjar.md` | 追踪代码、Events API、Identify API、屏蔽（suppression） | 使用 Hotjar 时 |

---

## 3. 参照示例

`examples/` 目录下有常见站点/平台组合的示例 GTM 容器 JSON 文件。**把它们当作参考，而非直接复制的文件。**阅读与用户情况最接近的示例，理解结构模式（文件夹布局、标签/触发器/变量命名、排序、跨平台的触发器共享），然后根据用户的实际需求重新搭建一个定制容器。

| 示例 | 平台 | 适用场景 |
|---|---|---|
| `examples/corporate-site.json` | GA4 + Clarity | 企业站、首页 |
| `examples/landing-page-with-clarity.json` | GA4 + Clarity + Google Ads | Google Ads 落地页，线索收集型转化 |
| `examples/landing-page-with-hotjar.json` | GA4 + Google Ads + Hotjar | 用 Hotjar（Recordings、Heatmaps、Events）替代 Clarity 的落地页，线索收集型转化 |
| `examples/paid-search.json` | GA4 + Clarity + Google Ads + Microsoft Ads | Google 与 Bing 的付费搜索（purchase + generate_lead 转化） |
| `examples/ecommerce.json` | GA4 + Clarity + Google Ads | 基础电商站（电商漏斗） |
| `examples/lead-generation.json` | GA4 + Clarity + Google Ads + Meta | 线索收集站点，多广告平台 |
| `examples/b2b-lead-generation.json` | GA4 + Clarity + Google Ads + LinkedIn | B2B 线索收集（用 LinkedIn Insight Tag 做 B2B 定向） |
| `examples/saas-lead-generation.json` | GA4 + Clarity + Google Ads + LinkedIn + Reddit | SaaS / 开发者工具线索收集（LinkedIn 做决策者定向 + Reddit 做技术社区定向；generate_lead + sign_up 转化） |
| `examples/social-ecommerce.json` | GA4 + Clarity + Google Ads + Meta + Pinterest | 视觉/社交电商（Meta 与 Pinterest 全量电商漏斗） |
| `examples/gen-z-ecommerce.json` | GA4 + Clarity + Google Ads + Snap + TikTok | 面向年轻人的移动优先电商（Snap 与 TikTok 全量电商漏斗） |
| `examples/ecommerce-multi-platform.json` | GA4 + Clarity + Google Ads + Meta + TikTok + X | 全平台电商 |

**示例的使用方法：**
1. 阅读与用户情况最接近的示例，理解模式
2. 搭建仅包含用户实际需要的平台、转化和触发器的定制容器——不要把示例中用不到的内容带进来
3. 在用户提供真实 ID 之前，使用占位符（`{ACCOUNT_ID}`、`{GA4_MEASUREMENT_ID}` 等）
4. 对于社区模板（`cvt_*`），向用户说明 `cvt_*` ID 必须替换为导入目标容器中分配的模板 ID

**没有示例完全匹配时：**
- 综合多个示例的结构模式，依据 `google-tag-manager.md` 中的 JSON 规范和各平台手册从零设计
- 始终遵循命名规范（`google-tag-manager.md` 第 3 节）

---

## 4. JSON 输出

### 文件命名规范

输出文件遵循以下命名规范：

```
gtm-{type}-v{number}-{date}.json
```

| 元素 | 说明 | 示例 |
|------|------|-----|
| `gtm` | 表示 GTM 容器（固定） | `gtm` |
| `{type}` | 站点类型或项目名 | `lp`、`ec`、`corporate`、`lead-gen` |
| `v{number}` | 顺序版本号（从 1 开始） | `v1`、`v2`、`v3` |
| `{date}` | 生成日期（YYYYMMDD） | `20260304` |

示例：`gtm-lp-v1-20260304.json`、`gtm-ec-v2-20260310.json`

**操作规则：**
- 版本号最大的文件是最新版
- 不删除历史版本（GTM 导入出问题时可用于回滚）
- 为同一类型生成新文件时，检查已有文件的版本号并分配下一个顺序号

### 搭建流程

1. **参照相关示例** → 内化结构模式（文件夹布局、命名、触发器共享）
2. **定义标签：**
   - 只包含用户实际使用的平台对应的标签
   - 为需求中确认的转化添加标签和触发器
   - 标签 ID 顺序分配（导入时会被重新编号）
3. **定义触发器：**
   - 自定义事件名与用户的 dataLayer 设计对齐
   - 把条件相同的共享触发器合并为一个（避免重复）
4. **定义变量：**
   - 常量变量使用用户提供的 ID（尚未提供时用占位符）
   - 添加标签所需的数据层变量
5. **组织文件夹：**
   - 只为在用的平台创建文件夹
   - 使用示例中的平台文件夹规范

### 输出 JSON 时的检查项

- [ ] 顶层存在 `exportFormatVersion: 2`
- [ ] 所有标签都设置了 `firingTriggerId`
- [ ] 内置触发器 ID（`2147479553`、`2147479573`、`2147479583`）使用正确
- [ ] 所有标签、触发器、变量都设置了 `parentFolderId`
- [ ] 遵循命名规范（标签：`[平台] - [类型] - [详情]`）
- [ ] 参数中不包含 PII（电话号码、邮箱等）
- [ ] 有 GA4 事件标签通过 `tagReference` 引用 GA4 Config 标签时，引用名称正确
- [ ] 使用社区模板（`cvt_*`）的地方，给用户附上在导入目标替换 ID 的说明

---

## 5. 落地指南

输出 JSON 后，引导用户完成以下步骤。

### GTM 导入流程
1. GTM 后台 → 管理（Admin）→ 导入容器（Import Container）
2. 选择生成的 JSON 文件
3. 新容器选“覆盖”（Overwrite）；已有容器选“合并”（Merge）
4. 如果有社区模板标签，先从模板库安装

### dataLayer 埋点
- 根据设计好的追踪需求，给出应用侧需要添加的 `dataLayer.push` 代码
- 用表格记录事件名、参数名、类型、取值的规范
- 电商站点还要给出 ecommerce 对象的结构（items 数组的 schema）

### 验证流程
1. 在 GTM 预览模式下验证所有标签正常触发
2. 在 GA4 DebugView 中确认事件接收
3. 用各平台的辅助工具验证（Meta Pixel Helper 等）
4. 确认事件中不包含 PII
5. 没问题后填写版本名称和描述并发布

---

## 给已有容器打补丁

如果用户已有 GTM 容器，只想增改几个标签而不是从头重建：

1. **让用户从 GTM 导出当前容器**（管理（Admin）→ 导出容器（Export Container）），提供 JSON。这样能得到已有的标签/触发器/变量 ID 和文件夹结构。
2. **阅读导出文件**，了解现状——实际使用的命名规范、已有的常量变量、文件夹布局。贴近用户当前风格，不要盲目套用 `references/google-tag-manager.md` 的规范（偏离会造成运营人员困惑的不一致）。
3. **生成小型的增量 JSON**，只包含新增的标签/触发器/变量。能复用的触发器和变量尽量引用现有 ID。新增项放进已有的文件夹。
4. **让用户用“合并”（Merge）导入**（不是覆盖），冲突时选**“重命名”（Rename）**，保留现有项。覆盖会清空他们的容器。
5. **跳过基于示例的模式学习**（第 3 节）——现有容器就是结构的唯一真理。示例是给全新搭建准备的。

JSON 输出的命名遵循同样的规范，但 `{type}` 用 `gtm-add-meta-purchase-v1-20260426.json` 这类能一眼看出是补丁的描述性名称。
