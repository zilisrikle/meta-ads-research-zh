# X 研究工作流

在选择端点、组织查询或报告 X API 支持的研究时使用本参考。

## 研究立场

- 优先使用只读端点。
- 先做计数，再获取样本。
- 在答案中保留原始查询和端点。
- 除非端点明确返回完整资源，否则将 API 结果视为有界样本。
- 不要暴露 `.env` 值或请求头。
- 未经用户明确确认，不要从本 skill 执行写入操作。

## 命令模式

### 凭证检查

```bash
uv run skills/x-research/scripts/x_api.py check-auth
```

### 主题扫描

```bash
uv run skills/x-research/scripts/trend_scan.py \
  "AI agents lang:ja -is:retweet" \
  --max-posts 100 \
  --granularity hour
```

### 仅主题声量

```bash
uv run skills/x-research/scripts/x_api.py counts \
  "AI agents lang:ja -is:retweet" \
  --granularity hour
```

### 近期搜索

```bash
uv run skills/x-research/scripts/x_api.py search \
  "from:xdevelopers -is:retweet" \
  --max-results 10 \
  --max-pages 1
```

### 全档案搜索

```bash
uv run skills/x-research/scripts/x_api.py search \
  "openai lang:en -is:retweet" \
  --all \
  --start-time 2025-01-01T00:00:00Z \
  --end-time 2025-01-31T23:59:59Z
```

仅在账户套餐有全档案访问权限时使用 `--all`。

### 账户扫描

```bash
uv run skills/x-research/scripts/account_scan.py xdevelopers \
  --timeline-pages 1 \
  --mentions-pages 1
```

### 对话扫描

```bash
uv run skills/x-research/scripts/conversation_scan.py \
  https://x.com/xdevelopers/status/1460323737035677698 \
  --max-posts 50
```

### 通用只读端点

```bash
uv run skills/x-research/scripts/x_api.py request GET /2/users/by/username/xdevelopers \
  --param user.fields=created_at,description,public_metrics,verified
```

## 端点映射

| 研究需求 | 默认命令或端点 |
|---|---|
| 按查询搜索近期帖子 | `x_api.py search QUERY` -> `GET /2/tweets/search/recent` |
| 按查询搜索全档案帖子 | `x_api.py search QUERY --all` -> `GET /2/tweets/search/all` |
| 近期帖子声量 | `x_api.py counts QUERY` -> `GET /2/tweets/counts/recent` |
| 全档案声量 | `x_api.py counts QUERY --all` -> `GET /2/tweets/counts/all` |
| 按用户名或 ID 查找用户 | `x_api.py user VALUE` -> 用户查找端点 |
| 已认证用户 | `x_api.py user --me --auth oauth1`，如支持也可用 `--auth bearer` |
| 账户时间线 | `x_api.py timeline USERNAME` -> `GET /2/users/:id/tweets` |
| 账户的提及 | `x_api.py timeline USERNAME --mentions` -> `GET /2/users/:id/mentions` |
| 用户点赞的帖子 | `x_api.py timeline USERNAME --liked` -> `GET /2/users/:id/liked_tweets` |
| 粉丝/关注 | `x_api.py follow USERNAME --type followers|following` |
| 帖子查找 | `x_api.py post POST_ID_OR_URL` |
| 点赞或转发的用户 | `x_api.py post-engagement POST_ID --type liking-users|reposted-by` |
| 引用帖子 | `x_api.py post-engagement POST_ID --type quotes` |
| 列表帖子 | `x_api.py list-posts LIST_ID` |
| Spaces 搜索 | `x_api.py spaces QUERY` |
| 按 WOEID 查趋势 | `x_api.py trends --woeid 1` |
| 个性化趋势 | `x_api.py trends --personalized --auth oauth1` |
| 新闻搜索 | `x_api.py news QUERY` |
| 用量 | `x_api.py usage` |

## 查询指南

X 搜索语法功能强大但容易引入偏差。优先使用显式运算符：

- 语言：`lang:ja`、`lang:en`
- 排除转发：`-is:retweet`
- 排除回复：`-is:reply`
- 账户：`from:username`、`to:username`、`@username`
- 话题标签：`#topic`
- URL/域名：`url:"example.com"`
- 对话：`conversation_id:POST_ID`
- 支持时的互动过滤器：`min_faves:10`、`min_retweets:5`、`min_replies:3`

比较主题时，保持查询窗口、语言过滤器、排除项和采样大小一致。

## 报告注意事项

始终说明主要限制：

- 近期搜索窗口和全档案访问取决于 X API 套餐。
- 由于已删除、受保护、屏蔽或不可访问的帖子，计数和搜索结果可能不一致。
- 互动指标是获取时的快照。
- 趋势可能因地区、个性化或访问层级而异。
- 小样本可以识别主题，但不应被视为人口普查。

## 官方文档

- X 开发者平台概览：https://docs.x.com/overview
- 发起你的第一次请求：https://docs.x.com/make-your-first-request
- X API 索引：https://docs.x.com/x-api/llms.txt
- 帖子搜索文档：https://docs.x.com/x-api/posts/search/introduction.md
- 搜索运算符：https://docs.x.com/x-api/posts/search/integrate/operators.md
- 帖子计数：https://docs.x.com/x-api/posts/counts/introduction.md
- 用户查找：https://docs.x.com/x-api/users/lookup/introduction.md
- 趋势：https://docs.x.com/x-api/trends/trends-by-woeid/introduction.md
