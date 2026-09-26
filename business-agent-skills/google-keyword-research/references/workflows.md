# Google 关键词研究工作流

在选择命令、组装 Keyword Planner 请求，或报告基于 Google Ads API 的搜索需求研究时使用本参考。

## 研究立场

- 优先使用只读调用。
- 从小规模种子列表开始，仅在有用时扩展。
- 在回答中保留种子词、URL 种子、端点、geo、语言、网络、客户 ID 与限制。
- 将搜索量、CPC 与竞争度视为近似的规划信号，而非确切的市场总量。
- 不要泄露 `.env` 值、OAuth 令牌、开发者令牌、客户端密钥或请求头。
- 不要通过本 skill 创建广告系列、广告组、关键词、素材、预算或转化。

## 命令模式

### 凭证检查

```bash
uv run skills/google-keyword-research/scripts/google_keyword_api.py check-auth
```

### 主题扫描

```bash
uv run skills/google-keyword-research/scripts/keyword_scan.py \
  "AI agent" \
  --geo JP \
  --language ja \
  --network GOOGLE_SEARCH \
  --max-ideas 50 \
  --max-historical-keywords 25 \
  --include-average-cpc
```

### 生成关键词创意

```bash
uv run skills/google-keyword-research/scripts/google_keyword_api.py ideas \
  "CRM" "営業管理" \
  --url https://example.com/ \
  --geo JP \
  --language ja \
  --network GOOGLE_SEARCH \
  --max-results 50
```

### 确切词条的历史指标

```bash
uv run skills/google-keyword-research/scripts/google_keyword_api.py historical \
  "CRM" "営業管理" "SFA" \
  --geo JP \
  --language ja \
  --include-average-cpc
```

### Geo 定位查询

```bash
uv run skills/google-keyword-research/scripts/google_keyword_api.py geo-targets Tokyo --max-results 20
uv run skills/google-keyword-research/scripts/google_keyword_api.py geo-targets JP --max-results 20
```

### 语言查询

```bash
uv run skills/google-keyword-research/scripts/google_keyword_api.py languages Japanese
uv run skills/google-keyword-research/scripts/google_keyword_api.py languages ja
```

### 只读 GAQL 查询

```bash
uv run skills/google-keyword-research/scripts/google_keyword_api.py gaql \
  "SELECT customer.id, customer.descriptive_name, customer.currency_code, customer.time_zone FROM customer LIMIT 1"
```

## 端点映射

| 研究需求 | 默认命令或 API 方法 |
|---|---|
| 某主题的搜索需求汇总 | `keyword_scan.py TOPIC` |
| 相关关键词发现 | `google_keyword_api.py ideas` -> `KeywordPlanIdeaService.GenerateKeywordIdeas` |
| 确切搜索量、CPC、竞争度、月度趋势 | `google_keyword_api.py historical` -> `KeywordPlanIdeaService.GenerateKeywordHistoricalMetrics` |
| 国家、地区或城市定位常量 | `google_keyword_api.py geo-targets` -> GAQL on `geo_target_constant` |
| 语言常量 | `google_keyword_api.py languages` -> GAQL on `language_constant` |
| 客户/账户元数据 | `google_keyword_api.py gaql` -> `GoogleAdsService.Search` |

## 参数指南

默认假设：

- `--geo JP`
- `--language ja`
- `--network GOOGLE_SEARCH`

进行非日本或多语言研究时请覆盖这些默认值。对比词条时使用相同的 geo、语言、网络与关键词列表。

脚本内置的常用 geo 快捷方式：

| 代码 | Geo 定位 ID |
|---|---:|
| JP | 2392 |
| US | 2840 |
| GB | 2826 |
| CA | 2124 |
| AU | 2036 |
| DE | 2276 |
| FR | 2250 |
| IN | 2356 |
| KR | 2410 |
| TW | 2158 |
| SG | 2702 |

脚本内置的常用语言快捷方式：

| 代码 | 语言 ID |
|---|---:|
| ja | 1011 |
| en | 1000 |
| de | 1001 |
| fr | 1002 |
| es | 1003 |
| ko | 1012 |
| zh | 1017 |
| id | 1025 |
| vi | 1040 |
| th | 1044 |

当某个 geo 或语言不在快捷方式表中时，直接传递数字 ID 或资源名称，或使用 `geo-targets` / `languages` 查找。

## 解读指南

使用 `historical` 对确切词条进行同口径对比。使用 `ideas` 进行发现，然后用 `historical` 复核有潜力的创意。

对每个关键词，检查：

- `avg_monthly_searches`：近似平均搜索量。
- `monthly_search_volumes`：近期月度模式；季节性主题请对比相同月份。
- `competition`：广告主竞争度分档。
- `competition_index`：可用时的 0-100 广告主竞争度信号。
- `low_top_of_page_bid` / `high_top_of_page_bid`：以账户货币计价的出价区间。
- `average_cpc`：仅在请求了 `--include-average-cpc` 且可用时的平均 CPC。
- `close_variants`：由历史指标返回；搜索量可能包含变体。

## 报告注意事项

始终说明主要限制：

- Keyword Planner 搜索量是近似的规划指标，不是确切的搜索次数。
- 历史指标可能包含近似变体。
- 结果取决于 geo 定位、语言、搜索网络、客户账户与 API 访问权限。
- 低搜索量词条可能被四舍五入、分档、省略或显示为零。
- CPC 与出价区间是广告市场信号，不是 SEO 难度。
- 基于 URL 种子的创意取决于 Google 对该页面的抓取与解读。
- 是否包含搜索合作伙伴会改变需求覆盖面；请注明 `GOOGLE_SEARCH` 与 `GOOGLE_SEARCH_AND_PARTNERS`。

## 官方文档

- 关键词创意：https://developers.google.com/google-ads/api/docs/keyword-planning/generate-keyword-ideas
- 历史指标：https://developers.google.com/google-ads/api/docs/keyword-planning/generate-historical-metrics
- Python 客户端认证：https://developers.google.com/google-ads/api/docs/client-libs/python/authentication
- Python 客户端配置：https://developers.google.com/google-ads/api/docs/client-libs/python/configuration
- Geo 定位常量：https://developers.google.com/google-ads/api/reference/data/geotargets
- 语言常量：https://developers.google.com/google-ads/api/reference/data/codes-formats#languages
