---
name: x-research
description: "使用 X API 研究 X：调查帖子、搜索查询、主题声量、趋势、新闻、用户、账户、竞争对手、对话、列表（Lists）、Spaces、粉丝、提及、互动，以及 API 支持的公开舆论信号。当用户请求 X/Twitter 研究、趋势发现、社媒聆听（social listening）、账户分析、对话分析、受众或社群研究、帖子查找、话题标签或关键词研究，或任何只读 X API 调查时使用。"
---

# X 研究

使用 X API 对主题、帖子、用户、对话、趋势、新闻、列表（Lists）、Spaces 和公开活动模式进行只读研究。优先使用 API 支持的证据，而非浏览器抓取，并在最终答案中保持精确的查询、端点、时间窗口和采样限制可见。

## 目录结构

```text
x-research/
├── SKILL.md
├── scripts/
│   ├── x_api.py              # 通用 X API CLI + 可复用客户端
│   ├── trend_scan.py         # 主题/查询声量与帖子样本分析
│   ├── account_scan.py       # 账户画像、时间线与提及分析
│   └── conversation_scan.py  # 帖子/对话与回复/引用分析
└── references/
    └── workflows.md          # 端点映射、命令模式与研究防护规则
```

## 前置条件

- 必须安装 **Python 3.10+**。
- 假设 X API 凭证已可从操作系统环境或附近的 `.env` 文件获取。
- 不要打印、查看、编辑或提交密钥值。

支持的环境变量：

```text
X_API_BEARER_TOKEN
X_API_CONSUMER_KEY
X_API_CONSUMER_KEY_SECRET
X_API_ACCESS_TOKEN
X_API_ACCESS_TOKEN_SECRET
```

对大多数公开读取端点使用 Bearer 认证。仅在端点要求用户上下文或自有数据读取时使用 OAuth 1.0a。除非用户明确请求并确认副作用，否则绝不要用本 skill 执行发布、点赞、关注、删除、静音、屏蔽或更改账户状态等写入操作。

### 如何运行脚本

这些脚本编写为 [PEP 723](https://peps.python.org/pep-0723/) 内联脚本。其依赖（`python-dotenv`）在每个文件顶部声明。以下任意一种调用方式均可：

```bash
# 方案 A：uv（推荐 —— 最快，自动解析依赖）
uv run <skill-dir>/scripts/x_api.py check-auth

# 方案 B：pipx（当 uv 不可用时）
pipx run <skill-dir>/scripts/x_api.py check-auth

# 方案 C：pip + 原生 python（最通用 —— 一次性安装依赖）
pip install python-dotenv
python3 <skill-dir>/scripts/x_api.py check-auth
```

下面的命令示例使用最短形式 `uv run`，但方案 B 和方案 C 运行的是完全相同的脚本。

## 快速开始

```bash
uv run skills/x-research/scripts/x_api.py check-auth
uv run skills/x-research/scripts/trend_scan.py "AI agents lang:ja -is:retweet" --max-posts 50
uv run skills/x-research/scripts/account_scan.py xdevelopers --timeline-pages 1 --mentions-pages 1
uv run skills/x-research/scripts/conversation_scan.py 1460323737035677698 --max-posts 50
```

关于细节、示例、端点选择和注意事项，请阅读 [references/workflows.md](references/workflows.md)。

## 工作流

1. 将用户请求翻译为研究单元：
   - 主题或趋势：使用 `trend_scan.py`。
   - 账户、竞争对手、创作者或品牌：使用 `account_scan.py`。
   - 具体帖子、帖子串、回复或引用活动：使用 `conversation_scan.py`。
   - X API GET 端点支持的其他任何内容：使用 `x_api.py request`。
2. 先发出最窄的有效 API 请求。在获取大量结果集之前，先从计数或小样本开始。
3. 保留研究参数：
   - 查询字符串与搜索运算符
   - 端点名称
   - 时间窗口
   - 结果数量与页数
   - 认证模式
4. 分析返回的数据：
   - 声量随时间变化
   - 代表性帖子
   - 反复出现的话题标签、提及、链接、语言和上下文注解
   - 按样本频率和互动指标排序的头部作者
   - 账户状态、近期主题、提及和受众信号
5. 清楚报告注意事项。X API 访问级别、速率限制、搜索窗口限制、已删除/受保护帖子、个性化，以及查询设计都可能显著影响结论。

## 脚本选择

| 任务 | 脚本 | 说明 |
|---|---|---|
| 检查凭证 | `x_api.py check-auth` | 测试 Bearer 和 OAuth 1.0a，不打印密钥。 |
| 搜索近期或全档案帖子 | `x_api.py search` | 仅在 API 套餐允许全档案访问时使用 `--all`。 |
| 统计主题声量 | `x_api.py counts` | 收集帖子之前先用它。 |
| 主题趋势汇总 | `trend_scan.py` | 结合计数、样本、话题标签、域名、提及和作者。 |
| 账户/竞争对手分析 | `account_scan.py` | 查找画像、近期帖子和提及样本。 |
| 对话分析 | `conversation_scan.py` | 查找某帖子，并通过 `conversation_id` 搜索回复。 |
| 查找帖子/用户 | `x_api.py post`、`x_api.py user` | 帖子支持 URL 和 ID。 |
| 列表（Lists）、粉丝、关注、Spaces、新闻、趋势 | `x_api.py` 子命令 | 优先使用专用子命令。 |
| 新增或不常见的 GET 端点 | `x_api.py request` | 只读 X API 端点的通用逃生通道。 |

## 输出标准

在回答用户时，包括：

- 使用的确切 X 查询或端点。
- 日期/时间窗口，以及数据是近期搜索、全档案、时间线、提及、趋势、新闻还是其他来源。
- 抽样的帖子/用户/条目数量。
- 结论与注意事项分开陈述。
- 用户名可用时，附上代表性帖子的链接。

除非端点和访问层级支持该论断，否则不要声称抽样的 API 结果代表整个 X。
