# Google Maps 研究工作流

在选择 Google Maps Platform 端点、组装请求，或报告基于 API 的本地/地理空间研究时使用本参考。

## 研究立场

- 优先使用只读端点。
- 获取的数据与错误仅打印到 stdout。
- 只请求研究问题所需的字段。
- 在回答中保留端点、字段掩码、查询、坐标、半径、过滤器、路线模式与样本量。
- 不要泄露 `.env` 值、API 密钥、包含密钥的请求 URL 或携带密钥的请求头。
- 遵守 Google Maps Platform 条款与产品政策；不要把 Places 内容当作免费的批量数据库。

## 字段集

Places 命令支持 `--field-set`：

| 字段集 | 用途 |
|---|---|
| `ids` | 最便宜的身份查询：仅返回 ID/资源名称。 |
| `basic` | 名称、地址、位置、类型、Google Maps URL。 |
| `research` | 默认。增加营业状态、评分、评论数、价格、网站、电话、营业时间、时区。 |
| `atmosphere` | 在可用时增加评论/摘要与便利设施类属性。更贵。 |
| `all` | 使用 `*`；仅用于调试。常规研究请避免使用。 |

当需要更严格地控制成本或 API 行为时，使用 `--fields` 传递显式字段掩码。

## 命令模式

### 凭证检查

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py check-auth
```

### 本地竞争对手或市场扫描

```bash
uv run skills/google-maps-research/scripts/local_market_scan.py \
  "渋谷駅 カフェ" \
  --center 35.658034,139.701636 \
  --radius 1000 \
  --included-type cafe \
  --language-code ja \
  --region-code JP
```

### 文本地点搜索

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py places-text \
  "美容クリニック 新宿" \
  --language-code ja \
  --region-code JP \
  --field-set research \
  --page-size 20 \
  --max-pages 1
```

### 周边便利设施搜索

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py places-nearby \
  35.681236,139.767125 \
  --radius 1000 \
  --included-type cafe \
  --rank-preference POPULARITY
```

### 地点详情

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py place-details \
  ChIJ51cu8IcbXWARiRtXIothAS4 \
  --field-set research \
  --language-code ja \
  --region-code JP
```

### Places Aggregate 计数

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py places-aggregate \
  --center 35.658034,139.701636 \
  --radius 1000 \
  --included-type cafe \
  --min-rating 4.0
```

对于复杂的聚合过滤器，传递官方 JSON 请求体：

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py places-aggregate \
  --body '{"insights":["INSIGHT_COUNT"],"filter":{"locationFilter":{"circle":{"latLng":{"latitude":35.658034,"longitude":139.701636},"radius":1000}},"typeFilter":{"includedTypes":["cafe"]}}}'
```

### 地理编码

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py geocode \
  "東京都千代田区丸の内1丁目"

uv run skills/google-maps-research/scripts/google_maps_api.py reverse-geocode \
  --latlng 35.681236,139.767125 \
  --language ja \
  --region jp
```

### 路线与路线矩阵

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py route \
  "東京駅" "渋谷駅" \
  --travel-mode TRANSIT \
  --language-code ja

uv run skills/google-maps-research/scripts/google_maps_api.py route-matrix \
  --origin "東京駅" \
  --destination "渋谷駅" \
  --destination "新宿駅" \
  --travel-mode TRANSIT \
  --language-code ja
```

### 地址验证

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py address-validate \
  "1600 Amphitheatre Pkwy" \
  --region-code US \
  --locality "Mountain View"
```

### 本地环境与背景

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py elevation \
  --location 35.681236,139.767125

uv run skills/google-maps-research/scripts/google_maps_api.py timezone \
  35.681236,139.767125

uv run skills/google-maps-research/scripts/google_maps_api.py air-quality current \
  35.681236,139.767125 \
  --extra-computation LOCAL_AQI \
  --extra-computation POLLUTANT_CONCENTRATION \
  --language-code ja

uv run skills/google-maps-research/scripts/google_maps_api.py pollen \
  35.681236,139.767125 \
  --days 3 \
  --language-code ja

uv run skills/google-maps-research/scripts/google_maps_api.py weather-current \
  35.681236,139.767125 \
  --language-code ja

uv run skills/google-maps-research/scripts/google_maps_api.py weather hourly \
  35.681236,139.767125 \
  --hours 12 \
  --page-size 12 \
  --language-code ja

uv run skills/google-maps-research/scripts/google_maps_api.py weather daily \
  35.681236,139.767125 \
  --days 3 \
  --language-code ja

uv run skills/google-maps-research/scripts/google_maps_api.py street-view-metadata \
  --location 35.681236,139.767125 \
  --radius 50
```

## 端点映射

| 研究需求 | 默认命令或 API |
|---|---|
| 按文本发现商户/设施 | `places-text` -> Places API Text Search (New) |
| 某坐标周边的便利设施 | `places-nearby` -> Places API Nearby Search (New) |
| 单个地点的详细信息 | `place-details` -> Places API Place Details (New) |
| 地点消歧 | `places-autocomplete` -> Places API Autocomplete (New) |
| 按区域/类型/评分统计/密度 | `places-aggregate` -> Places Aggregate API |
| 地址转坐标 | `geocode` -> Geocoding API |
| 坐标转地址 | `reverse-geocode` -> Geocoding API |
| 地址可送达性/标准化 | `address-validate` -> Address Validation API |
| 单条路线 | `route` -> Routes API computeRoutes |
| 多起点/终点行程时间矩阵 | `route-matrix` -> Routes API computeRouteMatrix |
| 地址字符串矩阵（便捷方式） | `distance-matrix` -> Distance Matrix API (Legacy) |
| 海拔/地形 | `elevation` -> Elevation API |
| 本地时区 | `timezone` -> Time Zone API |
| 空气质量（当前/预报/历史） | `air-quality` -> Air Quality API |
| 花粉预报 | `pollen` -> Pollen API |
| 当前/逐小时/逐日天气 | `weather` -> Weather API currentConditions / forecast.hours / forecast.days |
| 太阳能/建筑数据 | `solar` -> Solar API |
| 街景可用性 | `street-view-metadata` -> Street View Static API metadata |
| 道路吸附/最近道路 | `roads` -> Roads API |
| 少见的 JSON 端点 | `request` | Generic read-only escape hatch |

## 报告注意事项

始终说明主要限制：

- Places 搜索是排序/抽样的，不一定穷尽。
- Places Text Search 返回数量不超过文档规定的页面/结果上限；分页与排序影响覆盖度。
- 评分、评论数、营业状态、营业时间与网站都是某一时间点的快照。
- 字段掩码既影响返回内容，也影响计费。
- 某些 API 需在 Google Cloud 中单独启用；即使密钥有效，未启用的 API 仍可能返回 `REQUEST_DENIED` 或权限错误。
- Places Aggregate 可能返回计数或 Place ID，具体取决于洞察类型与结果规模。
- Distance Matrix 是旧版端点；尽可能使用 Routes API 的路线矩阵。
- 环境类 API 有地理覆盖限制。
- Google Maps Platform 数据在缓存、展示与归因方面受条款与产品政策限制。

## 官方文档

- Google Maps Platform 文档：https://developers.google.com/maps/documentation
- Places Text Search (New)：https://developers.google.com/maps/documentation/places/web-service/text-search
- Places Nearby Search (New)：https://developers.google.com/maps/documentation/places/web-service/nearby-search
- Places Details (New)：https://developers.google.com/maps/documentation/places/web-service/place-details
- Places Aggregate API：https://developers.google.com/maps/documentation/places-aggregate/overview
- Geocoding API：https://developers.google.com/maps/documentation/geocoding
- Address Validation API：https://developers.google.com/maps/documentation/address-validation
- Routes API：https://developers.google.com/maps/documentation/routes
- Elevation API：https://developers.google.com/maps/documentation/elevation
- Time Zone API：https://developers.google.com/maps/documentation/timezone
- Air Quality API：https://developers.google.com/maps/documentation/air-quality
- Pollen API：https://developers.google.com/maps/documentation/pollen
- Weather API：https://developers.google.com/maps/documentation/weather
- Solar API：https://developers.google.com/maps/documentation/solar
- Street View Static API 元数据：https://developers.google.com/maps/documentation/streetview/metadata
- Roads API：https://developers.google.com/maps/documentation/roads
