---
name: google-maps-research
description: "使用 Google Maps Platform API 进行位置、本地市场、竞争对手、路线、地址、便利设施与环境背景的研究。当用户要求 Google Maps API 研究、场所/地图商户或设施查询、本地竞争对手扫描、门店密度、评分与评论数、营业时间、地理编码、反向地理编码、地址验证、行程时间或路线矩阵对比、海拔、时区、街景元数据、道路、空气质量、花粉、天气、太阳能，或任何由 Google Maps Platform 支持的只读位置情报调查时使用。"
---

# Google Maps 研究

使用 Google Maps Platform API 进行只读的本地与地理空间研究：地点发现、竞争对手列表、评分、评论数、营业时间、网站、电话号码、地理编码、路线与行程时间对比、本地密度统计、地址验证、地形、时区、道路、街景元数据，以及环境背景。

优先使用 API 提供的证据，而非浏览器抓取。在最终回答中展示确切的查询、端点、字段掩码（field mask）、geo/radius、路线模式、语言/区域以及样本限制。

## 目录结构

```text
google-maps-research/
├── SKILL.md
├── scripts/
│   ├── google_maps_api.py    # Generic Google Maps Platform CLI
│   └── local_market_scan.py  # Places-based local competitor/market summary
└── references/
    └── workflows.md          # Command patterns, endpoint map, and caveats
```

## 前置条件

- **Python 3.10+** 必须已安装。
- 假设 Google Maps Platform 凭证已可从操作系统环境或附近的 `.env` 文件获取。
- 不要打印、查看、编辑或提交密钥值。

支持的环境变量：

```text
GOOGLE_MAPS_API
```

本 skill 仅使用 `GOOGLE_MAPS_API`。除非用户明确要求，否则不要回退到其他变量名。

### 如何运行脚本

脚本为 PEP 723 内联脚本（inline scripts），其依赖（`python-dotenv`）已在每个文件顶部声明。以下任一调用方式均可：

```bash
# Option A: uv (recommended)
uv run <skill-dir>/scripts/google_maps_api.py check-auth

# Option B: pipx
pipx run <skill-dir>/scripts/google_maps_api.py check-auth

# Option C: pip + plain python
pip install python-dotenv
python3 <skill-dir>/scripts/google_maps_api.py check-auth
```

下面的命令示例使用 `uv run`，但在依赖已安装的情况下，普通的 `python3` 运行的是同一脚本。

## 快速开始

```bash
uv run skills/google-maps-research/scripts/google_maps_api.py check-auth

uv run skills/google-maps-research/scripts/local_market_scan.py \
  "渋谷駅 カフェ" \
  --center 35.658034,139.701636 \
  --radius 1000 \
  --included-type cafe

uv run skills/google-maps-research/scripts/google_maps_api.py places-text \
  "美容クリニック 新宿" \
  --language-code ja \
  --region-code JP \
  --page-size 20

uv run skills/google-maps-research/scripts/google_maps_api.py route-matrix \
  --origin "東京駅" \
  --destination "渋谷駅" \
  --destination "新宿駅" \
  --travel-mode TRANSIT
```

关于端点选择、字段掩码（field mask）指南与注意事项，请阅读 [references/workflows.md](references/workflows.md)。

## 工作流程

1. 将请求转化为一个研究单元（research unit）：
   - 本地竞争对手、设施列表、评分与营业时间：使用 `local_market_scan.py` 或 `google_maps_api.py places-text`。
   - 某坐标周边的便利设施：使用 `google_maps_api.py places-nearby`。
   - 按地点 ID（Place ID）查门店/设施详情：使用 `google_maps_api.py place-details`。
   - 按区域/类型/评分的地点密度/计数：使用 `google_maps_api.py places-aggregate`。
   - 地址标准化：使用 `geocode`、`reverse-geocode` 或 `address-validate`。
   - 行程时间/可达性对比：使用 `route`、`route-matrix` 或 `distance-matrix`。
   - 地形/本地背景：使用 `elevation`、`timezone`、`street-view-metadata`、`roads`、`air-quality`、`pollen`、`weather` 或 `solar`。
   - 新加入或少见的只读端点：使用 `google_maps_api.py request`。
2. 从窄的范围开始。只请求任务所需的字段。Places API 要求字段掩码；字段选择会影响响应大小和计费。
3. 保留研究参数：
   - 查询或 Place ID
   - 端点与字段掩码
   - 坐标、半径、偏置/限制（bias/restriction）、区域/语言
   - 路线起点/终点、模式、出发/到达时间
   - 每页大小、页数与样本量
4. 分析返回的数据：
   - 地点名称、类别、地址、坐标、营业状态
   - 评分、评论数、价格等级、网站、电话号码、营业时间
   - 来自 Places Aggregate 的本地密度/计数
   - 行程时间、距离、路线备选、路线矩阵对比
   - 地址有效性、地理编码精度、海拔、时区、天气、空气质量、花粉、太阳能，以及街景可用性
5. 清晰报告注意事项。Google Maps 结果是抽样/排序的，字段掩码与已启用的 API 很关键，Places 数据有缓存/展示限制，公开指标是某一时间点的快照。

## 脚本选择

| 任务 | 脚本 | 说明 |
|---|---|---|
| 检查 API 密钥 | `google_maps_api.py check-auth` | 使用最小化的 Geocoding 请求，不打印密钥。 |
| 本地竞争对手扫描 | `local_market_scan.py QUERY` | 汇总 Places 结果：数量、评分、评论数、类型、网站、排名靠前的地点。 |
| 文本地点搜索 | `google_maps_api.py places-text QUERY` | Places Text Search (New)。支持字段集、位置偏置/限制、类型、分页、JSONL。 |
| 周边地点搜索 | `google_maps_api.py places-nearby LAT,LNG` | Places Nearby Search (New)。适合查询某点周边的便利设施。 |
| 地点详情 | `google_maps_api.py place-details PLACE_ID` | 按已知的 Place ID 获取详情。 |
| 自动补全 | `google_maps_api.py places-autocomplete INPUT` | 适用于地点/实体消歧。 |
| 聚合计数 | `google_maps_api.py places-aggregate` | 按区域/类型/评分/价格统计或识别地点。 |
| 地理编码 | `google_maps_api.py geocode ADDRESS` | 将地址转为坐标与结构化地址组件。 |
| 反向地理编码 | `google_maps_api.py reverse-geocode --latlng LAT,LNG` | 将坐标转为候选地址。 |
| 地址验证 | `google_maps_api.py address-validate` | 验证/标准化邮政地址。 |
| 路线 | `google_maps_api.py route ORIGIN DESTINATION` | 单条路线与行程时间分析。 |
| 路线矩阵 | `google_maps_api.py route-matrix` | 多起点/终点对比。 |
| 距离矩阵 | `google_maps_api.py distance-matrix` | 旧版（Legacy）端点，但便于地址字符串矩阵。 |
| 海拔/时区 | `google_maps_api.py elevation`、`timezone` | 地形与当地时间背景。 |
| 环境 | `google_maps_api.py air-quality`、`pollen`、`weather`、`solar` | 空气质量、花粉、当前/逐小时/逐日天气、建筑太阳能数据。 |
| 道路/街景 | `google_maps_api.py roads`、`street-view-metadata` | 道路匹配与街景可用性。 |
| 原始端点 | `google_maps_api.py request` | 面向受支持的 JSON 端点的通用只读逃生通道。 |

## 输出标准

回答用户时，请包含：

- 使用的确切端点/脚本。
- 查询、地点 ID、地址、坐标、半径、路线起点/终点与过滤器。
- 使用的字段掩码或字段集。
- 返回的地点/路线/条目数与页数。
- 将发现与注意事项分开列出。
- 当 `googleMapsUri` 或 Place ID 可用时，附上 Google Maps 链接。

除非端点与分页语义支持该说法，否则不要声称结果是完整的普查。不要泄露 API 密钥、包含密钥的请求 URL 或携带密钥的请求头。
