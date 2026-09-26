---
name: youtube-research
description: "使用 YouTube Data API 研究 YouTube：调查视频、搜索查询、频道、创作者、竞争对手、评论、播放列表、热门视频、公开表现指标、发布模式、主题样本，以及 API 支持的视频平台信号。当用户请求 YouTube 研究、创作者或频道分析、视频查找、评论分析、主题发现、竞争对手研究、公开视频表现分析，或任何只读 YouTube Data API 调查时使用。"
---

# YouTube 研究

使用 YouTube Data API 对视频、频道、创作者、评论、播放列表、主题搜索结果和公开表现模式进行只读研究。优先使用 API 支持的证据，而非浏览器抓取，并在最终答案中保持精确的查询、端点、过滤器、时间窗口和采样限制可见。

## 目录结构

```text
youtube-research/
├── SKILL.md
├── scripts/
│   ├── youtube_api.py      # 通用 YouTube Data API CLI + 可复用客户端
│   ├── trend_scan.py       # 主题/查询视频样本分析
│   ├── channel_scan.py     # 频道画像与近期上传分析
│   └── video_scan.py       # 视频详情与评论串分析
└── references/
    └── workflows.md        # 端点映射、命令模式与防护规则
```

## 前置条件

- 必须安装 **Python 3.10+**。
- 假设 YouTube Data API 凭证已可从操作系统环境或附近的 `.env` 文件获取。
- 不要打印、查看、编辑或提交密钥值。

支持的环境变量：

```text
YOUTUBE_API_KEY
```

对公开读取端点使用 API 密钥认证。本 skill 明确为只读；除非用户明确请求并确认副作用或 OAuth 要求，否则不要将其用于上传、修改、评分、审核或账户特定读取。

### 如何运行脚本

这些脚本编写为 PEP 723 内联脚本。其依赖（`python-dotenv`）在每个文件顶部声明。以下任意一种调用方式均可：

```bash
# 方案 A：uv（推荐）
uv run <skill-dir>/scripts/youtube_api.py check-auth

# 方案 B：pipx
pipx run <skill-dir>/scripts/youtube_api.py check-auth

# 方案 C：pip + 原生 python
pip install python-dotenv
python3 <skill-dir>/scripts/youtube_api.py check-auth
```

下面的命令示例使用 `uv run`，但在依赖已可用时，原生 `python3` 运行的是同样的脚本。

## 快速开始

```bash
uv run skills/youtube-research/scripts/youtube_api.py check-auth --region JP
uv run skills/youtube-research/scripts/trend_scan.py "AI agents" --region-code JP --relevance-language ja --max-videos 25
uv run skills/youtube-research/scripts/channel_scan.py "@YouTubeCreators" --max-videos 20
uv run skills/youtube-research/scripts/video_scan.py "https://www.youtube.com/watch?v=dQw4w9WgXcQ" --max-comments 50
```

关于细节、示例、端点选择和注意事项，请阅读 [references/workflows.md](references/workflows.md)。

## 工作流

1. 将用户请求翻译为研究单元：
   - 主题、关键词、类别或趋势样本：使用 `trend_scan.py`。
   - 频道、创作者、竞争对手或品牌：使用 `channel_scan.py`。
   - 具体视频、评论或公开互动：使用 `video_scan.py`。
   - YouTube Data API GET 端点支持的其他任何内容：使用 `youtube_api.py request`。
2. 先发出最窄的有效 API 请求。在获取评论或多页数据之前，先做一个小规模搜索、频道查找或视频查找。
3. 保留研究参数：
   - 搜索查询与过滤器
   - 端点名称
   - 发布时间窗口
   - 地区、语言、类别、排序、结果数量和页数
   - API 密钥认证模式
4. 分析返回的数据：
   - 搜索结果估计值与抽样视频
   - 返回时的观看、点赞和评论指标
   - 发布日期、时长分桶、频道、标签、话题标签和类别
   - 频道状态、近期上传，以及公开的订阅者数/视频数/观看数
   - 评论主题、按点赞数排序的热门评论，以及评论者频率
5. 清楚报告注意事项。YouTube API 配额、搜索排名、缺失的私密/已删除数据、已关闭的评论、隐藏的订阅者数、不可用的点赞数，以及查询设计都可能显著影响结论。

## 脚本选择

| 任务 | 脚本 | 说明 |
|---|---|---|
| 检查凭证 | `youtube_api.py check-auth` | 使用最小化的公开 `videos.list` 请求，不打印密钥。 |
| 搜索视频 | `youtube_api.py search` | 直接的 `search.list` 封装；适用于原始分页或 JSONL 输出。 |
| 查找视频 | `youtube_api.py videos` | 接受视频 ID 和常见 YouTube URL。 |
| 查找频道 | `youtube_api.py channels` | 支持频道 ID、自定义名称和旧用户名。 |
| 热门视频 | `youtube_api.py popular` | 按地区/类别的 `videos.list chart=mostPopular`。 |
| 主题趋势汇总 | `trend_scan.py` | 结合搜索结果、视频详情、频道详情、类别和样本分析。 |
| 频道/竞争对手分析 | `channel_scan.py` | 解析频道后分析其近期上传。 |
| 视频/评论分析 | `video_scan.py` | 查找视频并抽样评论串。 |
| 仅评论 | `youtube_api.py comment-threads` | 获取某视频的评论串。 |
| 播放列表/上传列表 | `youtube_api.py playlist-items` | 获取播放列表条目，包括上传播放列表。 |
| 不常见的 GET 端点 | `youtube_api.py request` | 只读 Data API 端点的通用逃生通道。 |

## 输出标准

在回答用户时，包括：

- 使用的确切 YouTube 查询或端点。
- 日期/时间窗口，以及数据来自搜索结果、视频查找、频道查找、热门榜单、播放列表条目还是评论。
- 地区、语言、排序、类别、页数，以及抽样的视频/评论/频道数量。
- 结论与注意事项分开陈述。
- 在有 ID 可用时，附上代表性视频和频道的链接。

不要声称抽样的 API 结果代表整个 YouTube。`search.list` 排名和 `pageInfo.totalResults` 是 API 估计值，取决于查询、过滤器、配额、访问权限和排名行为。
