---
name: google-keyword-research
description: "使用 Google Ads Keyword Planner 与 Google Ads API 研究搜索需求：生成关键词创意，查看历史搜索量、月度趋势、竞争度、首页出价区间、CPC 信号、geo/语言定位常量，以及 GAQL 驱动的 Google Ads 账户查询数据。当用户要求 Keyword Planner 研究、关键词研究、SEO/SEM 需求研究、搜索量、相关关键词发现、CPC 或竞争度估算、按区域/语言划分的市场需求，或基于 API 的 Google Ads 关键词规划调查时使用。"
---

# Google 关键词研究

使用 Google Ads API 的 Keyword Planner 服务，对搜索需求、关键词创意、历史指标、月度量模式、CPC 区间、竞争度以及 geo/语言定位常量进行只读研究。优先使用 API 提供的证据，而非浏览器抓取，并在最终回答中展示确切的种子关键词、URL、geo、语言、网络、客户 ID 与采样限制。

## 目录结构

```text
google-keyword-research/
├── SKILL.md
├── scripts/
│   ├── google_keyword_api.py  # Generic Keyword Planner + GAQL CLI
│   └── keyword_scan.py        # Topic scan combining ideas and historical metrics
└── references/
    └── workflows.md           # Command patterns, endpoint map, and caveats
```

## 前置条件

- **Python 3.10+** 必须已安装。
- 假设 Google Ads API 凭证已可从操作系统环境或附近的 `.env` 文件获取。
- 不要打印、查看、编辑或提交密钥值。

支持的环境变量：

```text
GOOGLE_ADS_DEVELOPER_TOKEN
GOOGLE_ADS_CLIENT_ID
GOOGLE_ADS_CLIENT_SECRET
GOOGLE_ADS_REFRESH_TOKEN
GOOGLE_ADS_CUSTOMER_ID
GOOGLE_ADS_LOGIN_CUSTOMER_ID
```

脚本仅使用这些 Google Ads 变量名。`GOOGLE_ADS_CUSTOMER_ID` 是被研究的目标客户；当访问通过经理账户（manager account）进行时使用 `GOOGLE_ADS_LOGIN_CUSTOMER_ID`。

### 如何运行脚本

脚本为 PEP 723 内联脚本（inline scripts），其依赖（`google-ads`、`python-dotenv`）已在每个文件顶部声明。以下任一调用方式均可：

```bash
# Option A: uv (recommended)
uv run <skill-dir>/scripts/google_keyword_api.py check-auth

# Option B: pipx
pipx run <skill-dir>/scripts/google_keyword_api.py check-auth

# Option C: pip + plain python
pip install google-ads python-dotenv
python3 <skill-dir>/scripts/google_keyword_api.py check-auth
```

下面的命令示例使用 `uv run`，但在依赖已安装的情况下，普通的 `python3` 运行的是同一脚本。

## 快速开始

```bash
uv run skills/google-keyword-research/scripts/google_keyword_api.py check-auth

uv run skills/google-keyword-research/scripts/keyword_scan.py \
  "AI agent" \
  --geo JP \
  --language ja \
  --max-ideas 50 \
  --max-historical-keywords 25

uv run skills/google-keyword-research/scripts/google_keyword_api.py ideas \
  "生成AI" "AIエージェント" \
  --geo JP \
  --language ja \
  --max-results 50

uv run skills/google-keyword-research/scripts/google_keyword_api.py historical \
  "生成AI" "AIエージェント" \
  --geo JP \
  --language ja \
  --include-average-cpc
```

关于细节、端点选择、查询常量与注意事项，请阅读 [references/workflows.md](references/workflows.md)。

## 工作流程

1. 将请求转化为一个研究单元（research unit）：
   - 宽泛主题、市场或产品类别：使用 `keyword_scan.py`。
   - 相关关键词发现：使用 `google_keyword_api.py ideas`。
   - 确切词条、搜索量、CPC、竞争度与月度趋势：使用 `google_keyword_api.py historical`。
   - Geo 或语言常量：使用 `google_keyword_api.py geo-targets` 或 `languages`。
   - 其他只读 Google Ads 查询数据：使用 `google_keyword_api.py gaql`。
2. 先发出范围最窄的有效请求。先用小规模的主题扫描或确切关键词列表，再扩展到大量创意。
3. 保留研究参数：
   - 种子关键词与可选的 URL 种子
   - 端点名称
   - 客户 ID、geo 定位 ID、语言 ID 与搜索网络
   - 结果数与关键词样本量
   - 是否请求了 CPC
4. 分析返回的数据：
   - 平均月搜索量
   - 月度搜索量序列
   - 竞争度等级与竞争指数
   - 首页出价高/低区间，以及可用时的平均 CPC
   - 历史指标返回的近似变体（close variants）
   - 生成创意中的相关关键词簇
5. 清晰报告注意事项。Keyword Planner 指标是近似值，常与近似变体合并统计，受账户/API 访问权限影响，并非所有搜索行为的完整普查。

## 脚本选择

| 任务 | 脚本 | 说明 |
|---|---|---|
| 检查凭证 | `google_keyword_api.py check-auth` | 测试 OAuth 刷新令牌（refresh-token）认证与客户访问权限，不打印密钥。 |
| 主题需求扫描 | `keyword_scan.py TOPIC` | 生成创意，为种子词+排名靠前的创意获取历史指标，并打印汇总 JSON。 |
| 相关关键词创意 | `google_keyword_api.py ideas` | 封装 `GenerateKeywordIdeas`；接受关键词种子、URL 种子或两者。 |
| 确切历史指标 | `google_keyword_api.py historical` | 封装 `GenerateKeywordHistoricalMetrics`；用于确切词条对比。 |
| Geo 定位查询 | `google_keyword_api.py geo-targets QUERY` | 查找可定位的国家、地区、城市、DMA 或其他 geo 常量。 |
| 语言查询 | `google_keyword_api.py languages QUERY` | 查找可定位的语言常量。 |
| 只读 GAQL 查询 | `google_keyword_api.py gaql QUERY` | 用于账户/客户元数据与常量的逃生通道。 |

## 输出标准

回答用户时，请包含：

- 确切的种子关键词或 URL 种子。
- 使用的端点：关键词创意、历史指标、geo 定位、语言常量或 GAQL。
- geo 定位 ID、语言 ID、网络、客户 ID 与样本量。
- 将发现与注意事项分开列出。
- 搜索量、CPC 与竞争度是确切关键词指标还是生成创意的指标。

不要声称 Keyword Planner 输出代表确切的 Google 搜索总需求。应将其视为近似的规划数据，受所请求的区域、语言、网络、近似变体合并与账户/API 访问权限影响。
