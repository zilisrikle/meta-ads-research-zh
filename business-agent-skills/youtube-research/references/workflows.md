# YouTube 研究工作流

在选择端点、组织查询或报告 YouTube Data API 支持的研究时使用本参考。

## 研究立场

- 优先使用只读端点。
- 先做小规模搜索或查找，确认有用后再获取详情、评论或更多分页。
- 在答案中保留原始查询、端点、过滤器和采样限制。
- 将 API 结果视为有界样本。YouTube 搜索排名不是人口普查。
- 不要暴露 `.env` 值、API 密钥、包含密钥的请求 URL 或请求头。
- 未经用户明确确认，不要从本 skill 执行写入操作。

## 命令模式

### 凭证检查

```bash
uv run skills/youtube-research/scripts/youtube_api.py check-auth --region JP
```

### 主题扫描

```bash
uv run skills/youtube-research/scripts/trend_scan.py \
  "AI agents" \
  --region-code JP \
  --relevance-language ja \
  --published-after 2026-06-01T00:00:00Z \
  --max-videos 50
```

### 原始视频搜索

```bash
uv run skills/youtube-research/scripts/youtube_api.py search \
  "ChatGPT" \
  --type video \
  --order viewCount \
  --region-code JP \
  --relevance-language ja \
  --max-results 10 \
  --max-pages 1
```

### 频道扫描

```bash
uv run skills/youtube-research/scripts/channel_scan.py "@YouTubeCreators" \
  --max-videos 20
```

### 视频与评论扫描

```bash
uv run skills/youtube-research/scripts/video_scan.py \
  "https://www.youtube.com/watch?v=dQw4w9WgXcQ" \
  --max-comments 50 \
  --comment-order relevance
```

### 热门视频

```bash
uv run skills/youtube-research/scripts/youtube_api.py popular \
  --region-code JP \
  --max-results 10
```

### 通用只读端点

```bash
uv run skills/youtube-research/scripts/youtube_api.py request GET /channels \
  --param part=snippet,statistics \
  --param forHandle=@YouTubeCreators
```

## 端点映射

| 研究需求 | 默认命令或端点 |
|---|---|
| 搜索视频/频道/播放列表 | `youtube_api.py search QUERY` -> `GET /youtube/v3/search` |
| 视频详情与指标 | `youtube_api.py videos VIDEO_ID_OR_URL` -> `GET /youtube/v3/videos` |
| 频道画像与指标 | `youtube_api.py channels VALUE` -> `GET /youtube/v3/channels` |
| 频道近期上传 | `channel_scan.py VALUE` -> `channels.list` 上传播放列表 + `playlistItems.list` + `videos.list` |
| 按地区/类别的热门视频 | `youtube_api.py popular` -> `GET /youtube/v3/videos?chart=mostPopular` |
| 某视频的评论串 | `youtube_api.py comment-threads VIDEO_ID_OR_URL` -> `GET /youtube/v3/commentThreads` |
| 某评论的回复 | `youtube_api.py comments PARENT_COMMENT_ID` -> `GET /youtube/v3/comments` |
| 播放列表条目 | `youtube_api.py playlist-items PLAYLIST_ID` -> `GET /youtube/v3/playlistItems` |
| 视频类别 | `youtube_api.py video-categories` -> `GET /youtube/v3/videoCategories` |
| 新增或不常见的 GET 端点 | `youtube_api.py request GET PATH` | 通用只读逃生通道。 |

## 查询指南

保持对比受控：

- 使用相同的 `publishedAfter` / `publishedBefore` 窗口。
- 使用相同的 `regionCode`、`relevanceLanguage`、`safeSearch`、`order`、`videoDuration` 和样本量。
- 新鲜度优先用 `order=date`，高触达样本优先用 `order=viewCount`。
- 除非用户要求查频道或播放列表，主题扫描使用 `type=video`。
- 比较围绕某主题的上传时，用 `channelId` 将搜索限制到单个频道。
- 类别特定研究使用 `videoCategoryId`，并记录用于类别名称的地区。

研究中常用的搜索与结果过滤器：

- `q`：搜索文本
- `type`：`video`、`channel` 或 `playlist`
- `order`：`date`、`rating`、`relevance`、`title`、`videoCount`、`viewCount`
- `publishedAfter`、`publishedBefore`：RFC 3339 时间戳
- `regionCode`：ISO 3166-1 alpha-2 国家代码
- `relevanceLanguage`：ISO 语言代码
- `videoDuration`：`short`、`medium` 或 `long`
- `videoDefinition`：`high` 或 `standard`
- `videoCategoryId`：类别过滤器
- `eventType`：`live`、`upcoming` 或 `completed`

## 报告注意事项

始终说明主要限制：

- `search.list` 是排名抽样；它不是 YouTube 的完整普查。
- `pageInfo.totalResults` 是估计值，调用之间可能变化。
- 搜索结果可能遗漏私密、已删除、年龄限制、地区屏蔽或其他不可用的视频。
- 公开指标是获取时的快照，可能遗漏隐藏或不可用字段。
- 评论抽样取决于评论是否启用且可访问。
- 各端点的 API 配额成本不同；搜索请求相对昂贵。
- 频道订阅者数可能被隐藏。

## 官方文档

- YouTube Data API 概览：https://developers.google.com/youtube/v3
- Search: list：https://developers.google.com/youtube/v3/docs/search/list
- Videos: list：https://developers.google.com/youtube/v3/docs/videos/list
- Channels: list：https://developers.google.com/youtube/v3/docs/channels/list
- CommentThreads: list：https://developers.google.com/youtube/v3/docs/commentThreads/list
- Comments: list：https://developers.google.com/youtube/v3/docs/comments/list
- PlaylistItems: list：https://developers.google.com/youtube/v3/docs/playlistItems/list
- VideoCategories: list：https://developers.google.com/youtube/v3/docs/videoCategories/list
- 配额成本：https://developers.google.com/youtube/v3/determine_quota_cost
