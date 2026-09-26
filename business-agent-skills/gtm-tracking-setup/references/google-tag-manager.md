# GTM（Google Tag Manager）配置手册

> **用途：** 指导 AI 与用户协作设计 GTM 容器并输出可导入的 JSON 文件的规范。
> JSON 规范基于 GTM 的容器导出/导入格式（`exportFormatVersion: 2`），符合 Tag Manager API v2 定义的 ContainerVersion 模型。
> 唯一可信来源：[Tag Platform / Tag Manager 开发者中心](https://developers.google.com/tag-platform/tag-manager)——API v2 参考、容器导出格式和模板的入口。

---

## 目录

1. **前置条件**——要收集的信息、基本原则（PII、单代码片段规则）
2. **容器结构设计**——文件夹、触发顺序、典型标签、触发器共享、dataLayer、常量变量
3. **命名规范**——标签 / 触发器 / 变量 / 文件夹 / 版本
4. **容器 JSON 规范**——导出/导入格式、顶层结构、Tag/Trigger/Variable/BuiltInVariable/Parameter/Folder 的 schema、内置触发器 ID、导入注意事项、示例容器 JSON 索引
5. **各平台参考**——指向各平台手册的指针
6. **更多用例**——跨域追踪、内部流量排除、文件下载/站外链接/电话拨打追踪
7. **运维规则**——版本管理、工作区、性能
8. **检查清单**——新容器搭建、发布前、命名与结构
9. **调试**——预览模式、辅助工具

第 4 节是承载 JSON 规范的核心——生成或编辑容器 JSON 时都要读。第 6–9 节是运维内容，只在相关场景查阅。

---

## 1. 前置条件

### 1.1 向用户收集的信息

| # | 项目 | 示例 | 必填 |
|---|------|-----|------|
| 1 | GTM 账号 ID | `123456` | ✅ |
| 2 | GTM 容器 ID | `789012` | ✅ |
| 3 | 容器公开 ID | `GTM-XXXXXXX` | ✅ |
| 4 | 站点域名 | `example.com` | ✅ |
| 5 | GA4 衡量 ID | `G-XXXXXXXXXX` | 使用 GA4 时 |
| 6 | Google Ads 转化 ID | `AW-XXXXXXXXX` | 使用 Google Ads 时 |
| 7 | Meta Pixel ID | `123456789012345` | 使用 Meta 时 |
| 8 | TikTok Pixel ID | `XXXXXXXXXX` | 使用 TikTok 时 |
| 9 | X Pixel ID | `XXXXX` | 使用 X 时 |
| 10 | Clarity 项目 ID | `xxxxxxxxxx` | 使用 Clarity 时 |
| 11 | 使用的平台列表 | GA4、Meta、Clarity | ✅ |
| 12 | 要追踪的转化 | 购买、线索收集 | ✅ |
| 13 | 电商 / 非电商 | 电商 | 用于确定站点类型 |
| 14 | SPA / MPA | MPA | 用于确定 page_view 控制方式 |

### 1.2 基本原则

- 各追踪供应商的标签在 GTM 后台统一管理。HTML 中只安装**一个 GTM 代码片段**。
- 追踪所需的 `dataLayer.push` 调用在应用源代码中实现。
- **不传输 PII（个人身份信息）：** 不向 GA4 或其他追踪工具发送邮箱、电话号码、姓名等。对于 `tel:` / `mailto:` 链接追踪，只记录点击事件及其上下文（位置、类型）；原始数据要脱敏或分类。
- 不要对同一用途同时用 gtag.js 和 GTM 部署**重复的标签**。
- 有官方推荐的搭建方法时，遵循官方方法。

---

## 2. 容器结构设计

### 2.1 容器原则

| 规则 | 说明 |
|--------|------|
| 一家公司一个账号 | 每个公司创建一个 GTM 账号 |
| 一个站点一个容器 | 每个域名一个容器。子域名共用同一容器 |
| 保持精简 | 定期清理不用的标签、触发器和变量。先暂停，过一段时间再删除。删除记录在版本名称/描述中 |

### 2.2 文件夹结构

```
_Global                    ← 共享触发器和变量（_ 开头使其排在最上方）
GA4                        ← GA4 相关
GA4 - Ecommerce            ← 使用电商追踪时
Google Ads                 ← Google Ads 相关
Meta                       ← Meta Pixel 相关
TikTok                     ← TikTok Pixel 相关
X                          ← X Pixel 相关
Clarity                    ← Microsoft Clarity 相关
Utilities                  ← 辅助变量等
```

- 新建的标签、触发器和变量**必须始终放入文件夹**。
- 文件夹名前加 `_` 可使其显示在列表最上方。

### 2.3 标签触发顺序

| 优先级 | 触发器 | 用途 |
|--------|---------|------|
| 1 | Consent Initialization - All Pages | CMP（同意管理）标签 |
| 2 | Initialization - All Pages | Google 代码（GA4 配置）、Conversion Linker |
| 3 | All Pages | 各平台基础标签（Clarity、Meta Base 等） |
| 4 | 事件触发器 | 转化、点击事件等 |

### 2.4 典型标签配置

**通用基础：**

| 标签名 | 标签类型 | 触发器 | 备注 |
|--------|----------|---------|------|
| GA4 - Config | Google 代码（`googtag`） | Initialization - All Pages | `send_page_view: true`（默认） |
| Google Ads - Conversion Linker | Conversion Linker（`gclidw`） | Initialization - All Pages | 使用 Google Ads 时一般会安装 |

> **关于 `page_view`：** Google 代码默认会自动发送 `page_view`（`send_page_view` 默认为 `true`）。单独再建一个 `page_view` 标签会造成重复计数。只有 SPA 需要手动控制时，才把 Google 代码的 `send_page_view` 设为 `false`，并通过 History Change 触发器手动触发。

**各平台的标签配置：** 见各手册 → [第 5 节](#5-各平台参考)

### 2.5 共享触发器

多个平台的标签在相同时机触发时，**共用同一个触发器**。不要为每个平台单独建条件相同的触发器。共享触发器存放在 `_Global` 文件夹。

示例：只建一个表单提交触发器 `CE - form_submit`，挂载到 GA4、Meta、TikTok 的线索事件标签上。

### 2.6 dataLayer 设计

应用侧 `dataLayer.push` → GTM 侧通过自定义事件触发器和数据层变量接收。直接引用 DOM 元素很脆弱，能用数据层就优先用数据层。

```javascript
// 表单提交完成
dataLayer.push({
  event: 'form_submit',
  form_name: 'contact',
  form_id: 'contact-form-01'
});

// 购买完成（符合 GA4 ecommerce 规范）
dataLayer.push({
  event: 'purchase',
  ecommerce: {
    transaction_id: 'T12345',
    value: 49.99,
    currency: 'USD',
    items: [{ item_id: 'SKU001', item_name: 'Product Name', price: 49.99, quantity: 1 }]
  }
});
```

**dataLayer 事件名 vs GA4 事件名：**

| 类型 | 作用 | 示例 |
|------|------|-----|
| dataLayer 事件名 | GTM 触发器的标识符。应用侧可自由命名 | `form_submit`、`purchase` |
| GA4 事件名 | 在 GA4 报告中展示。应遵循[推荐事件](https://support.google.com/analytics/answer/9267735?hl=en) | `generate_lead`、`purchase` |

### 2.7 用常量变量集中管理 ID

把衡量 ID 和像素 ID 存放在常量变量中，供多个标签引用。ID 变更时只需改一处。

| 变量名 | 示例值 |
|--------|--------|
| `Const - GA4 Measurement ID` | `G-XXXXXXXXXX` |
| `Const - Google Ads Conversion ID` | `AW-XXXXXXXXX` |
| `Const - Meta Pixel ID` | `123456789012345` |
| `Const - TikTok Pixel ID` | `XXXXXXXXXX` |
| `Const - X Pixel ID` | `XXXXX` |
| `Const - Clarity Project ID` | `xxxxxxxxxx` |

---

## 3. 命名规范

### 3.1 基本方针

| 项目 | 规则 |
|------|--------|
| 分隔符 | ` - `（连字符 + 空格） |
| 语言 | 英文 |
| 大小写 | 平台名首字母大写；事件名用 snake_case |

### 3.2 标签

```
[平台] - [标签类型] - [详情]
```

| 示例 | 说明 |
|-----|------|
| `GA4 - Config` | GA4 配置标签 |
| `GA4 - Event - generate_lead` | GA4 事件 |
| `GA4 - Event - click_cta - hero_section` | 带详情的事件 |
| `Meta - Base` | 基础标签 |
| `Meta - Event - Purchase` | 转化 |
| `Clarity - Base` | 基础标签 |
| `TikTok - Event - CompletePayment` | 转化 |
| `Google Ads - Conversion - purchase` | 转化 |
| `Google Ads - Remarketing` | 再营销 |
| `X - Event - Purchase` | 转化 |

**平台前缀：**

| 前缀 | 平台 |
|---------------|-------|
| GA4 | Google Analytics 4 |
| Google Ads | Google Ads |
| Meta | Meta（Facebook/Instagram） |
| TikTok | TikTok |
| X | X（Twitter） |
| Clarity | Microsoft Clarity |
| LinkedIn | LinkedIn |

### 3.3 触发器

```
[触发器类型] - [详情]
```

| 示例 | 说明 |
|-----|------|
| `PV - All Pages` | 所有页面 |
| `PV - Thank You Page` | 条件页面浏览 |
| `Click - CTA Button` | 点击 |
| `Link Click - External Links` | 链接点击 |
| `Link Click - File Download` | 文件下载 |
| `Link Click - Phone Call` | 电话拨打 |
| `Form - Contact Submit` | 表单提交 |
| `Scroll - 50 Percent` | 滚动 |
| `CE - purchase` | 自定义事件 |
| `CE - form_submit` | 自定义事件 |
| `Timer - 30s` | 计时器 |
| `Visibility - Pricing Section` | 元素可见性 |

**触发器类型前缀：**

| 前缀 | 触发器类型 |
|---------------|---------------|
| PV | Page view（页面浏览） |
| Click | All elements click（所有元素点击） |
| Link Click | Just links click（仅链接点击） |
| Form | Form submission（表单提交） |
| Scroll | Scroll depth（滚动深度） |
| CE | Custom event（自定义事件，dataLayer.push） |
| Timer | Timer（计时器） |
| Visibility | Element visibility（元素可见性） |

例外（阻塞）触发器用 `Blocking - ` 前缀：`Blocking - Internal Traffic`、`Blocking - Staging Domain`。

### 3.4 变量

```
[变量类型] - [详情]
```

| 示例 | 说明 |
|-----|------|
| `Const - GA4 Measurement ID` | 常量 |
| `DLV - ecommerce` | 数据层变量 |
| `DLV - form_name` | 数据层变量 |
| `URL - Page Path` | URL 变量 |
| `URL - Query Parameter - utm_source` | URL 查询参数 |
| `Cookie - user_id` | Cookie |
| `CJS - Trim Page Title` | 自定义 JS |
| `LUT - Page Type` | 查询表 |
| `Regex - Clean URL` | 正则表 |
| `DOM - H1 Text` | DOM 元素 |
| `AEV - Click Classes` | 自动事件变量 |

**变量类型前缀：**

| 前缀 | 变量类型 |
|---------------|-----------|
| Const | Constant（常量） |
| DLV | Data layer variable（数据层变量） |
| URL | URL variable（URL 变量） |
| Cookie | First-party cookie（第一方 Cookie） |
| CJS | Custom JavaScript（自定义 JavaScript） |
| LUT | Lookup table（查询表） |
| Regex | Regex table（正则表） |
| DOM | DOM element（DOM 元素） |
| AEV | Auto-event variable（自动事件变量） |
| JSV | JavaScript variable（JavaScript 变量） |

### 3.5 文件夹

文件夹名直接用平台名。`_` 开头会显示在最上方。

### 3.6 版本

```
[平台] - [操作] - [对象]
```

示例：`GA4 - Add - scroll event`、`Meta - Add - purchase conversion`、`Clarity - Initial Setup`。

在描述中记录**谁、做了什么、为什么**。变更分小批量发布（不要把多个无关变更塞进一个版本）。

---

## 4. 容器 JSON 规范

### 4.1 导出 / 导入

**导出：** GTM 后台 → Admin → Export Container → 选择版本或工作区。

**导入：** GTM 后台 → Admin → Import Container → 选择 JSON 文件。

| 导入方式 | 行为 |
|---------------|------|
| Overwrite（覆盖） | 删除现有全部内容，用导入的内容替换 |
| Merge（合并） | 保留现有项并新增。冲突时在 "Overwrite" 或 "Rename" 之间选择 |

### 4.2 顶层结构

| 字段 | 类型 | 说明 |
|-----------|-----|------|
| `exportFormatVersion` | number | 导出格式版本。当前为 `2` |
| `exportTime` | string | 导出时间戳（UTC） |
| `containerVersion` | object | 全部容器版本数据 |

### 4.3 containerVersion

| 字段 | 类型 | 说明 |
|-----------|-----|------|
| `path` | string | API 资源路径 |
| `accountId` | string | GTM 账号 ID |
| `containerId` | string | GTM 容器 ID |
| `containerVersionId` | string | 版本 ID |
| `name` | string | 版本显示名 |
| `description` | string | 版本描述 |
| `container` | object | 容器元数据（name、publicId、usageContext 等） |
| `tag` | Tag[] | 标签数组 |
| `trigger` | Trigger[] | 触发器数组 |
| `variable` | Variable[] | 用户自定义变量数组 |
| `builtInVariable` | BuiltInVariable[] | 启用的内置变量数组 |
| `folder` | Folder[] | 文件夹数组 |
| `customTemplate` | CustomTemplate[] | 自定义模板数组 |
| `zone` | Zone[] | Zone 数组 |
| `gtagConfig` | GtagConfig[] | Google 代码配置数组 |
| `fingerprint` | string | 哈希值（用于变更检测） |

### 4.4 Tag

| 字段 | 类型 | 说明 |
|-----------|-----|------|
| `accountId` | string | GTM 账号 ID |
| `containerId` | string | GTM 容器 ID |
| `tagId` | string | 唯一标签 ID |
| `name` | string | 标签显示名 |
| `type` | string | 标签类型标识符 |
| `parameter` | Parameter[] | 参数数组 |
| `firingTriggerId` | string[] | 触发触发器 ID 数组（任一满足即触发） |
| `blockingTriggerId` | string[] | 阻塞触发器 ID 数组（任一满足即阻止触发） |
| `tagFiringOption` | enum | 触发选项 |
| `liveOnly` | boolean | `true` 时仅在正式环境触发 |
| `priority` | Parameter | 触发优先级（数值越大越先触发） |
| `setupTag` | SetupTag[] | 前置标签（最多 1 个） |
| `teardownTag` | TeardownTag[] | 后置标签（最多 1 个） |
| `parentFolderId` | string | 父文件夹 ID |
| `paused` | boolean | `true` 时为暂停状态 |
| `scheduleStartMs` | string | 计划开始时间（毫秒） |
| `scheduleEndMs` | string | 计划结束时间（毫秒） |
| `notes` | string | 备注 |
| `monitoringMetadata` | Parameter | 标签监控用元数据 |
| `consentSettings` | object | 同意设置 |
| `fingerprint` | string | 哈希值 |

**tagFiringOption：**

| 取值 | 说明 |
|----|------|
| `UNLIMITED` | 每个事件可触发多次 |
| `ONCE_PER_EVENT` | 每个事件一次（每次页面加载可触发多次） |
| `ONCE_PER_LOAD` | 每次页面加载一次 |

**主要标签类型标识符（type）：**

| type | 标签 |
|------|------|
| `googtag` | Google 代码（GA4 配置、Google Ads 集成） |
| `gaawc` | GA4 配置标签（旧版） |
| `gaawe` | GA4 事件标签 |
| `awct` | Google Ads 转化追踪 |
| `sp` | Google Ads 再营销 |
| `gclidw` | Conversion Linker |
| `html` | 自定义 HTML |
| `img` | 自定义图片 |
| `flc` | Floodlight 计数器 |
| `fls` | Floodlight 销售 |
| `cvt_*` | 社区模板（`cvt_XXXXXXXXXX` 格式） |

### 4.5 Trigger

| 字段 | 类型 | 说明 |
|-----------|-----|------|
| `accountId` | string | GTM 账号 ID |
| `containerId` | string | GTM 容器 ID |
| `triggerId` | string | 唯一触发器 ID |
| `name` | string | 触发器显示名 |
| `type` | enum | 触发器类型 |
| `customEventFilter` | Condition[] | 自定义事件触发条件（全部满足即触发） |
| `filter` | Condition[] | 附加过滤条件（全部满足即触发） |
| `autoEventFilter` | Condition[] | 自动事件过滤器 |
| `parameter` | Parameter[] | 附加参数 |
| `parentFolderId` | string | 父文件夹 ID |
| `notes` | string | 备注 |
| `fingerprint` | string | 哈希值 |

**触发器类型（Web）：**

| type | 触发器 | GTM 界面标签 |
|------|---------|-----------|
| `PAGEVIEW` | 页面浏览 | Page View |
| `DOM_READY` | DOM Ready | DOM Ready |
| `WINDOW_LOADED` | Window loaded | Window Loaded |
| `CUSTOM_EVENT` | 自定义事件 | Custom Event |
| `CLICK` | 所有元素点击 | All Elements |
| `LINK_CLICK` | 仅链接点击 | Just Links |
| `FORM_SUBMISSION` | 表单提交 | Form Submission |
| `SCROLL_DEPTH` | 滚动深度 | Scroll Depth |
| `ELEMENT_VISIBILITY` | 元素可见性 | Element Visibility |
| `YOU_TUBE_VIDEO` | YouTube 视频 | YouTube Video |
| `HISTORY_CHANGE` | 历史记录变更 | History Change |
| `JS_ERROR` | JavaScript 错误 | JavaScript Error |
| `TIMER` | 计时器 | Timer |
| `TRIGGER_GROUP` | 触发器组 | Trigger Group |
| `INIT` | 初始化 | Initialization |
| `CONSENT_INIT` | 同意初始化 | Consent Initialization |

服务端：`SERVER_PAGEVIEW`（服务端页面浏览）。

**触发器特有字段：**

| 触发器类型 | 特有字段 |
|--------------|--------------|
| Click | `selector`（CSS 选择器）、`checkValidation`、`waitForTags`、`waitForTagsTimeout` |
| Scroll | `verticalScrollPercentageList`、`horizontalScrollPercentageList` |
| Timer | `eventName`、`interval`（毫秒）、`limit`（最大触发次数） |

**Condition（过滤条件）：**

| 字段 | 说明 |
|-----------|------|
| `type` | 条件类型：`EQUALS`、`CONTAINS`、`STARTS_WITH`、`ENDS_WITH`、`MATCH_REGEX` 等 |
| `parameter[0]` | `arg0` = 左侧（变量引用，如 `{{_event}}`） |
| `parameter[1]` | `arg1` = 右侧（比较值，如 `purchase`） |

修饰参数：`negate`（`true` 时取反）、`ignore_case`（`true` 时忽略大小写）。

### 4.6 Variable

| 字段 | 类型 | 说明 |
|-----------|-----|------|
| `accountId` | string | GTM 账号 ID |
| `containerId` | string | GTM 容器 ID |
| `variableId` | string | 唯一变量 ID |
| `name` | string | 变量显示名 |
| `type` | string | 变量类型标识符 |
| `parameter` | Parameter[] | 参数数组 |
| `parentFolderId` | string | 父文件夹 ID |
| `notes` | string | 备注 |
| `formatValue` | FormatValue | 取值转换选项 |
| `fingerprint` | string | 哈希值 |

**主要变量类型标识符（type）：**

| type | 变量 | GTM 界面标签 |
|------|------|-----------|
| `c` | Constant（常量） | Constant |
| `v` | Data layer variable（数据层变量） | Data Layer Variable |
| `j` | JavaScript variable（JavaScript 变量） | JavaScript Variable |
| `jsm` | Custom JavaScript（自定义 JavaScript） | Custom JavaScript |
| `k` | First-party cookie（第一方 Cookie） | 1st Party Cookie |
| `u` | URL | URL |
| `d` | DOM element（DOM 元素） | DOM Element |
| `aev` | Auto-event variable（自动事件变量） | Auto-Event Variable |
| `gas` | Google Analytics settings（Google Analytics 设置） | Google Analytics Settings |
| `smm` | Lookup table（查询表） | Lookup Table |
| `remm` | Regex table（正则表） | RegEx Table |
| `e` | Custom event（自定义事件） | Custom Event |
| `dbg` | Debug mode（调试模式） | Debug Mode |
| `ctv` | Container version number（容器版本号） | Container Version Number |
| `vis` | Element visibility（元素可见性） | Element Visibility |
| `gtcs` | Google tag: settings（Google 代码：设置） | Google Tag: Configuration Settings |
| `gtes` | Google tag: event settings（Google 代码：事件设置） | Google Tag: Event Settings |

### 4.7 BuiltInVariable

**页面相关：**

| type | 说明 |
|------|------|
| `PAGE_URL` | 页面 URL |
| `PAGE_HOSTNAME` | 主机名 |
| `PAGE_PATH` | 路径 |
| `REFERRER` | 引荐来源 |
| `EVENT` | 事件 |

**点击相关：**

| type | 说明 |
|------|------|
| `CLICK_ELEMENT` | 点击元素 |
| `CLICK_CLASSES` | 类 |
| `CLICK_ID` | ID |
| `CLICK_TARGET` | 目标 |
| `CLICK_URL` | URL |
| `CLICK_TEXT` | 文本 |

**表单相关：**

| type | 说明 |
|------|------|
| `FORM_ELEMENT` | 表单元素 |
| `FORM_CLASSES` | 类 |
| `FORM_ID` | ID |
| `FORM_TARGET` | 目标 |
| `FORM_URL` | URL |
| `FORM_TEXT` | 文本 |

**滚动相关：**

| type | 说明 |
|------|------|
| `SCROLL_DEPTH_THRESHOLD` | 阈值 |
| `SCROLL_DEPTH_UNITS` | 单位 |
| `SCROLL_DEPTH_DIRECTION` | 方向 |

**历史记录相关：**

| type | 说明 |
|------|------|
| `NEW_HISTORY_URL` | 新历史记录 URL |
| `OLD_HISTORY_URL` | 旧历史记录 URL |
| `NEW_HISTORY_FRAGMENT` | 新片段 |
| `OLD_HISTORY_FRAGMENT` | 旧片段 |
| `HISTORY_SOURCE` | 来源 |

**视频相关：**

| type | 说明 |
|------|------|
| `VIDEO_PROVIDER` | 提供方 |
| `VIDEO_URL` | URL |
| `VIDEO_TITLE` | 标题 |
| `VIDEO_DURATION` | 时长 |
| `VIDEO_PERCENT` | 播放百分比 |
| `VIDEO_STATUS` | 状态 |
| `VIDEO_CURRENT_TIME` | 当前时间 |
| `VIDEO_VISIBLE` | 可见性 |

**元素可见性相关：**

| type | 说明 |
|------|------|
| `ELEMENT_VISIBILITY_RATIO` | 可见比例 |
| `ELEMENT_VISIBILITY_TIME` | 可见时长 |
| `ELEMENT_VISIBILITY_FIRST_TIME` | 首次可见时间 |
| `ELEMENT_VISIBILITY_RECENT_TIME` | 最近一次可见时间 |

**错误相关：**

| type | 说明 |
|------|------|
| `ERROR_MESSAGE` | 错误信息 |
| `ERROR_URL` | 错误 URL |
| `ERROR_LINE` | 行号 |

**其他：**

| type | 说明 |
|------|------|
| `DEBUG_MODE` | 调试模式 |
| `RANDOM_NUMBER` | 随机数 |
| `CONTAINER_ID` | 容器 ID |
| `CONTAINER_VERSION` | 容器版本 |
| `HTML_ID` | HTML ID |
| `ENVIRONMENT_NAME` | 环境名称 |

### 4.8 Parameter

标签、触发器、变量的所有参数共用这个通用结构。

| 字段 | 类型 | 说明 |
|-----------|-----|------|
| `type` | enum | 参数类型 |
| `key` | string | 参数名（顶层和 map 中必填；list 中忽略） |
| `value` | string | 取值（可包含 `{{变量名}}` 这类变量引用） |
| `list` | Parameter[] | list 类型的子参数数组 |
| `map` | Parameter[] | map 类型的键值子参数数组 |
| `isWeakReference` | boolean | 弱引用（仅 Transformation） |

**`type` 取值：**

| type | 说明 | 示例 |
|------|------|-----|
| `TEMPLATE` | 模板字符串（允许变量引用） | `"G-XXXXXXXXXX"`、`"{{DLV - ecommerce}}"` |
| `INTEGER` | 64 位整数 | `"30000"` |
| `BOOLEAN` | 布尔值 | `"true"`、`"false"` |
| `LIST` | 参数列表（存于 `list` 字段） | — |
| `MAP` | 键值参数映射（存于 `map` 字段） | — |
| `TRIGGER_REFERENCE` | 触发器 ID 引用 | `"1"` |
| `TAG_REFERENCE` | 标签名引用 | `"GA4 - Config"` |

### 4.9 Folder

| 字段 | 类型 | 说明 |
|-----------|-----|------|
| `accountId` | string | GTM 账号 ID |
| `containerId` | string | GTM 容器 ID |
| `folderId` | string | 唯一文件夹 ID |
| `name` | string | 文件夹名 |
| `fingerprint` | string | 哈希值 |

### 4.10 内置触发器 ID

| 固定 ID | 触发器 |
|--------|---------|
| `2147479553` | All Pages |
| `2147479573` | Initialization - All Pages |
| `2147479583` | Consent Initialization - All Pages |

**只有这些内置触发器 ID 可以硬编码。** 用户自建触发器的 ID 因容器而异。导入时用户自建触发器的 ID 会自动重新编号并更新，因此 JSON 中用户自建触发器的 ID 只需在 JSON 内部作为交叉引用保持一致即可。

### 4.11 导入注意事项

- `accountId` 和 `containerId` 会自动替换为目标容器的值。
- `tagId`、`triggerId`、`variableId`、`folderId` 在导入时重新编号。
- `firingTriggerId` 和 `blockingTriggerId` 中的 ID 引用会自动更新。
- 社区模板标签（`cvt_*`）通过 `customTemplate` 节中的 `galleryReference` 引用模板库模板。Clarity 使用 `cvt_MQDKZ`（Gallery Template ID 固定）。
- `fingerprint` 在导入时重新计算。

### 示例容器 JSON

`examples/` 目录按站点类型提供了示例容器 JSON。**把它们当作结构和命名模式的参考，不要原样导入**——根据用户实际的平台组合和转化搭建定制容器。`customTemplate` 节中带 `galleryReference` 的社区模板（如 Clarity）会在导入时自动安装；没有 `galleryReference` 的社区模板（`cvt_*`）必须替换为目标容器中已安装的模板 ID。

| 示例 | 平台 | 用例 |
|---|---|---|
| [corporate-site.json](../examples/corporate-site.json) | GA4 + Clarity | 企业站 / 首页。滚动、CTA 点击和电话拨打追踪 |
| [landing-page-with-clarity.json](../examples/landing-page-with-clarity.json) | GA4 + Clarity + Google Ads | Google Ads 落地页。线索转化（generate_lead）+ 滚动 |
| [landing-page-with-hotjar.json](../examples/landing-page-with-hotjar.json) | GA4 + Google Ads + Hotjar | 用 Hotjar 替代 Clarity 的落地页。用 Custom HTML 部署 Tracking Code + 针对 generate_lead 的 Hotjar Event，外加 GA4 线索/滚动和 Google Ads 转化 |
| [paid-search.json](../examples/paid-search.json) | GA4 + Clarity + Google Ads + Microsoft Ads | Google 与 Bing 的付费搜索。用 Custom HTML 部署 UET 基础代码 + 每个事件的 UET 标签（purchase、generate_lead） |
| [ecommerce.json](../examples/ecommerce.json) | GA4 + Clarity + Google Ads | 电商站基础。电商漏斗（view_item → add_to_cart → begin_checkout → purchase） |
| [lead-generation.json](../examples/lead-generation.json) | GA4 + Clarity + Google Ads + Meta | 线索收集站点。多广告平台的线索转化 + 电话拨打 |
| [b2b-lead-generation.json](../examples/b2b-lead-generation.json) | GA4 + Clarity + Google Ads + LinkedIn | B2B 线索收集。用 LinkedIn Insight Tag 做线索转化（Partner ID + 每个事件的 Conversion ID）+ 电话拨打 + 滚动 |
| [saas-lead-generation.json](../examples/saas-lead-generation.json) | GA4 + Clarity + Google Ads + LinkedIn + Reddit | SaaS / 开发者工具线索收集。用 Reddit 官方 Pixel 模板（cvt_PBGZL）部署 PageVisit + 每个事件的标签，在 Google Ads、LinkedIn（LEAD、SIGN_UP 规则）和 Reddit（Lead -> tracking_type LEAD、SignUp -> tracking_type SIGN_UP）之间镜像 generate_lead + sign_up 转化 |
| [social-ecommerce.json](../examples/social-ecommerce.json) | GA4 + Clarity + Google Ads + Meta + Pinterest | 视觉 / 社交电商。Meta（ViewContent、AddToCart、InitiateCheckout、Purchase）与 Pinterest（PageVisit、AddToCart、InitiateCheckout、Checkout）之间的完整电商漏斗（view_item、add_to_cart、begin_checkout、purchase）镜像 |
| [gen-z-ecommerce.json](../examples/gen-z-ecommerce.json) | GA4 + Clarity + Google Ads + Snap + TikTok | 面向年轻人的移动优先电商。Snap（VIEW_CONTENT、ADD_CART、START_CHECKOUT、PURCHASE）与 TikTok（ViewContent、AddToCart、InitiateCheckout、Purchase）之间的完整电商漏斗（view_item、add_to_cart、begin_checkout、purchase）镜像。Snap Pixel 使用占位符 cvt_SNAP_PIXEL_TEMPLATE_ID（无 galleryReference） |
| [ecommerce-multi-platform.json](../examples/ecommerce-multi-platform.json) | GA4 + Clarity + Google Ads + Meta + TikTok + X | 全平台电商。全平台的电商转化 + items 转化 CJS |

---

## 5. 各平台参考

各平台的事件定义、参数和 GTM 配置细节见各手册。

| 平台 | 手册 | 主要内容 |
|---|---|---|
| Google Analytics 4 | [google-analytics.md](./google-analytics.md) | 推荐事件、电商、自定义维度、增强型衡量 |
| Google Ads | [google-ads.md](./google-ads.md) | 转化设计、增强型转化、动态再营销 |
| Meta Pixel | [meta-pixel.md](./meta-pixel.md) | 标准事件、自定义事件、转化 API（Conversions API） |
| TikTok Pixel | [tiktok-pixel.md](./tiktok-pixel.md) | 标准事件、Events API |
| X Pixel | [x-pixel.md](./x-pixel.md) | 标准事件、转化 API（Conversion API） |
| Microsoft Clarity | [microsoft-clarity.md](./microsoft-clarity.md) | 热图、会话录制、自定义标签 |

---

## 6. 更多用例

### 6.1 跨域追踪

追踪跨多个域名（如 `example.com` 和 `shop.example.jp`）的用户行为，保持在同一会话内。**这完全在 GA4 管理后台配置，不需要 GTM 侧配置。**

**设置：**
1. GA4 后台 → 数据流 → Google 代码 → 配置代码设置 → **配置您的网域** → 添加所有目标域名。
2. 按需把支付处理器等添加到“不需要的引荐”列表。
3. 验证：确认跨域跳转时 URL 带上 `_gl=` 参数，且 GA4 DebugView 显示为同一会话。

**注意事项：**
- 仅支持 `<a>` 标签的链接点击。`<form>` 提交和 JS 跳转不会自动带 `_gl=`，需要自定义实现。
- 所有目标域名都要安装带同一 GA4 衡量 ID 的 GTM 代码片段。
- 子域名默认视为同一域名（无需设置）。

### 6.2 内部流量排除

#### 方法 A：IP 地址排除（办公室有固定 IP 时）

GA4 后台 → Google 代码 → 定义内部流量 → 添加办公室 IP。在 GA4 数据过滤器中先设为“测试”→ 验证后改为“启用”（启用后被排除的数据无法恢复）。

#### 方法 B：Cookie 法（支持远程办公和动态 IP）

1. **写 Cookie 的标签**（自定义 HTML）：访问特殊 URL（`?set_internal=true`）时写入 `is_internal=true` Cookie（90 天有效期）。
2. **读 Cookie 的变量**：`Cookie - is_internal`。
3. **查询表变量**：`is_internal=true` → `traffic_type=internal`。
4. 把 `traffic_type` 添加到 **Google 代码的配置参数**中。
5. 激活 GA4 数据过滤器（流程同方法 A）。

让内部成员把 `https://example.com/?set_internal=true` 加入书签（每 90 天重新访问一次）。

#### 方法 C：排除预发布环境

在 GA4 标签上加阻塞触发器 `Blocking - Staging Domain`（Page Hostname = `staging.example.com`）。

### 6.3 文件下载追踪

**GA4 增强型衡量（推荐）：** 在 GA4 数据流设置中打开“文件下载”，会自动采集 `file_download` 事件。目标扩展名：`pdf, xls, xlsx, doc, docx, txt, rtf, csv, exe, key, pps, ppt, pptx, 7z, pkg, rar, gz, zip, avi, mov, mp4, mpe, mpeg, wmv, mid, midi, mp3, wav, wma`。参数：`file_extension`、`file_name`、`link_text`、`link_url`。

**GTM 自定义追踪（要加扩展名或限定特定页面时）：**
- 触发器 `Link Click - File Download`：Just Links，Click URL 匹配正则 `\.(pdf|docx?|xlsx?|pptx?|csv|zip|rar)$`。
- 标签 `GA4 - Event - file_download`：参数 `file_name={{Click URL}}`、`link_text={{Click Text}}`。
- **使用 GTM 自定义追踪时，关闭增强型衡量的文件下载，避免重复。**

### 6.4 站外链接点击追踪

**GA4 增强型衡量（推荐）：** 打开“出站点击”，会自动采集 `CLICK` 事件（`outbound: true`）。参数：`link_classes`、`link_domain`、`link_id`、`link_url`、`outbound`。

**GTM 自定义追踪（只追踪特定站外链接时）：**
- 触发器 `Link Click - Outbound`：Just Links，Click URL 不包含本站域名。
- 标签 `GA4 - Event - outbound_click`：参数 `link_url={{Click URL}}`、`link_text={{Click Text}}`。
- **使用 GTM 自定义追踪时，关闭增强型衡量的“出站点击”，避免重复。**

### 6.5 电话拨打与邮箱链接追踪

**电话拨打：**
- 触发器 `Link Click - Phone Call`：Just Links，Click URL 以 `tel:` 开头。
- 标签 `GA4 - Event - phone_call_click`。
- **PII 注意：** 直接发送 `{{Click URL}}` 会把原始电话号码记入 GA4（禁止传输 PII）。改为发送位置（`header`、`footer`、`contact_page` 等）。在 HTML 侧加 `data-tracking-section` 属性，用自定义 JS 变量读取。

```javascript
// CJS - Phone Call Location
function() {
  var el = {{Click Element}};
  if (!el) return 'unknown';
  var section = el.closest('[data-tracking-section]');
  return section ? section.getAttribute('data-tracking-section') : 'other';
}
```

```html
<header data-tracking-section="header">
  <a href="tel:+12025550123">Call us</a>
</header>
```

**邮箱链接：** 同样的模式适用于 `mailto:`。把触发器条件改为以 `mailto:` 开头，事件名设为 `email_link_click`，位置作为参数一并发送。直接发送 `{{Click URL}}` 属于传输 PII，禁止。

---

## 7. 运维规则

### 版本管理

GTM 版本（发布历史）是支持回滚的变更历史。

```
版本名称：变更的一句话摘要
描述：
- 谁：负责人姓名
- 什么：新增、变更或删除了什么
- 为什么：变更的原因和背景
```

### 工作区

团队协作时，用不同工作区分开工作。命名：`[项目名] - [负责人/团队名]`。

### 性能

- 新增标签时用 **PageSpeed Insights** 衡量影响。
- 自定义 HTML 标签尽量少。
- 避免同一平台的重复标签。

---

## 8. 检查清单

### 新容器搭建时

- [ ] 创建 GTM 账号和容器（一家公司一个账号，一个站点一个容器）。
- [ ] 在 HTML `<head>` 和 `<body>` 中安装 GTM 代码片段。
- [ ] 创建文件夹：`_Global`、`GA4`，以及每个使用平台的文件夹。
- [ ] 创建常量变量：`Const - GA4 Measurement ID` 等。
- [ ] 在 Initialization - All Pages 上配置 Google 代码（GA4 - Config）。
- [ ] 使用 Google Ads 时，在 Initialization - All Pages 上配置 Conversion Linker。
- [ ] 在 All Pages 上配置各平台的基础标签。
- [ ] 所有标签、触发器、变量按命名规范命名。
- [ ] 所有标签、触发器、变量放入文件夹。

### 发布前

- [ ] 在 GTM 预览模式下验证所有标签的触发时机。
- [ ] 在 GA4 DebugView 中确认事件正确记录。
- [ ] 用各辅助工具验证（Meta Pixel Helper、Tag Assistant 等）。
- [ ] 确认 `page_view` 没有重复计数。
- [ ] 确认事件参数中不含 PII。
- [ ] 确认标签不会在非预期页面触发。
- [ ] 确认阻塞触发器正常工作。
- [ ] 确认转化事件参数（value、currency、transaction_id 等）正确。
- [ ] 记录版本名称和描述。

### 命名与结构

- [ ] 标签：`[平台] - [类型] - [详情]`。
- [ ] 触发器：`[触发器类型] - [详情]`。
- [ ] 变量：`[变量类型] - [详情]`。
- [ ] 没有未分类的标签、触发器、变量。
- [ ] 没有条件重复的触发器（使用共享触发器）。
- [ ] 共享触发器存放在 `_Global` 文件夹。

---

## 9. 调试

| 工具 | 用途 |
|--------|------|
| GTM 预览模式 | 验证标签触发和触发器条件 |
| GA4 DebugView | 实时检查 GA4 事件 |
| Meta Pixel Helper（Chrome 扩展） | 验证 Meta Pixel 触发 |
| TikTok Pixel Helper（Chrome 扩展） | 验证 TikTok Pixel 触发 |
| X Pixel Helper（Chrome 扩展） | 验证 X Pixel 触发 |
| Tag Assistant | 通用的 Google 代码验证 |

**发布前务必在预览模式下测试。** 尤其要验证触发器条件，以及没有在非预期页面触发。
