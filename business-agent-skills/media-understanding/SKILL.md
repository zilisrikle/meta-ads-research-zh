---
name: media-understanding
description: 使用 Gemini 的多模态能力分析本地视频和音频文件。支持视频摘要、带时间戳的场景描述、音频转录、可能的说话人标注、情绪/语气估计、音乐与环境音解读、问答以及结构化数据提取。当用户提到视频分析、"这个视频里有什么"、"总结这个视频"、从视频中提取信息、视频转录、音频分析、音频转录、录音内容、播客内容、音频摘要、录音分析或"分析这个片段"时使用此 skill。当提供了视频或音频文件路径时积极触发。
---

# 媒体理解（Media Understanding）

使用 Gemini 的多模态能力分析本地视频和音频文件。视频同时覆盖画面和音频轨道；音频覆盖语音、音乐和环境音。

## 目录结构

```
media-understanding/
├── SKILL.md                        # This file
└── scripts/
    ├── analyze_video.py            # Local video file → analysis
    └── analyze_audio.py            # Local audio file → analysis
```

## 此 skill 可以做什么

### 视频
- **摘要（Summarization）** — 对内容进行简洁的总结。
- **场景描述（Scene description）** — 结合画面与音频的详细走查（带时间戳）。
- **转录（Transcription）** — 转录视频中的口语音频。
- **问答（Q&A）** — 回答有关视频内容的问题。
- **结构化提取（Structured extraction）** — 以 JSON 格式提取信息。
- **定点分析（Spot analysis）** — 检查特定时间点。
- **裁剪（Clipping）** — 仅分析指定的时间范围。

### 音频
- **转录（Transcription）** — 转录独立音频（多语言）。
- **摘要（Summarization）** — 总结录音、播客、会议记录。
- **说话人 / 情绪估计（Speaker / emotion estimation）** — 在可区分时标注可能的说话人，并估计语气或情绪变化。
- **音乐与环境音解读（Music and ambient interpretation）** — 分析非语音音频（音乐、音效、环境）。
- **问答（Q&A）** — 回答有关音频内容的问题。
- **结构化提取（Structured extraction）** — 以 JSON 格式提取信息。
- **区域分析（Region analysis）** — 在提示词中通过 `MM:SS` 引用检查特定的时间范围。

---

## 前置条件

- 必须安装 **Python 3.10+**。
- `GEMINI_API_KEY` 假定已可从操作系统环境或附近的 `.env` 文件获取。
- 不要打印、检查、编辑或提交机密值。

### 如何运行脚本

这些脚本是 [PEP 723](https://peps.python.org/pep-0723/) 内联脚本——其依赖（`google-genai`、`python-dotenv`）已在每个文件顶部声明。以下任何一种调用方式都可以：

```bash
# Option A: uv (recommended — fastest, auto-resolves dependencies)
uv run <skill-dir>/scripts/analyze_video.py "prompt" video.mp4

# Option B: pipx (when uv isn't available)
pipx run <skill-dir>/scripts/analyze_video.py "prompt" video.mp4

# Option C: pip + plain python (most universal — install dependencies once)
pip install google-genai python-dotenv
python3 <skill-dir>/scripts/analyze_video.py "prompt" video.mp4
```

下面的命令示例使用最短的形式 `uv run`，但选项 B 和 C 以完全相同的方式运行相同的脚本。

---

## 工作流

### 选择正确的脚本

| 输入文件 | 脚本 |
|---|---|
| 视频文件（`.mp4`、`.mov`、`.webm` 等） | `analyze_video.py` |
| 音频文件（`.mp3`、`.wav`、`.flac` 等） | `analyze_audio.py` |

### 分析前

与用户确认以下事项。如果有缺失，请询问：

1. **他们想要什么？** — 摘要、转录文本、场景描述、结构化提取等。
2. **输出格式** — 纯文本，还是结构化 JSON？
3. **范围** — 整个文件，还是特定的时间范围？

> 在下面的命令示例中，将 `<skill-dir>` 替换为此 skill 实际安装的路径。脚本位于 `<skill-dir>/scripts/` 下。

---

## 视频分析（`analyze_video.py`）

```bash
uv run <skill-dir>/scripts/analyze_video.py "prompt" video_file_path [options...]
```

示例：

```bash
# Quick summary
uv run <skill-dir>/scripts/analyze_video.py \
  "Summarize this video in 3 sentences." \
  presentation.mp4

# Detailed timestamped breakdown
uv run <skill-dir>/scripts/analyze_video.py \
  "Describe the key events in this video with timestamps. Include both audio and visual details." \
  meeting_recording.mp4

# Transcribe the spoken audio in a video
uv run <skill-dir>/scripts/analyze_video.py \
  "Transcribe the audio. If there are multiple speakers, label likely speakers when distinguishable." \
  interview.mp4

# Structured JSON extraction
uv run <skill-dir>/scripts/analyze_video.py \
  "List every product that appears in this video. Include name, color, and notable features for each." \
  product_review.mp4 --json

# Analyze only a specific range (30s–90s)
uv run <skill-dir>/scripts/analyze_video.py \
  "Describe what's happening in this segment in detail." \
  long_video.mp4 --start 30 --end 90

# Lower resolution for fast / cheap analysis of large videos
uv run <skill-dir>/scripts/analyze_video.py \
  "Give me an overview of this video." \
  large_video.mp4 --media-resolution low

# Higher fps for fast-moving content
uv run <skill-dir>/scripts/analyze_video.py \
  "Identify all the text and numbers visible on screen." \
  sports_highlight.mp4 --fps 5
```

### 视频选项

| 选项 | 取值 | 默认值 | 说明 |
|---|---|---|---|
| `--model` | model ID | `gemini-3-flash-preview` | Gemini model to use |
| `--json` | flag | off | Structured JSON output (uses API `response_mime_type`) |
| `--schema` | inline JSON or `.json` path | none | Output JSON schema (auto-enables `--json`) |
| `--fps` | number | `1` | Frame sampling rate (frames per second) |
| `--start` | seconds | none | Clip start offset |
| `--end` | seconds | none | Clip end offset |
| `--media-resolution` | `low`, `medium`, `high` | none | Media resolution (affects token usage) |

---

## 音频分析（`analyze_audio.py`）

```bash
uv run <skill-dir>/scripts/analyze_audio.py "prompt" audio_file_path [options...]
```

示例：

```bash
# Verbatim transcription
uv run <skill-dir>/scripts/analyze_audio.py \
  "Transcribe this audio verbatim. If there are multiple speakers, label likely speakers when distinguishable." \
  interview.mp3

# Meeting recording summary
uv run <skill-dir>/scripts/analyze_audio.py \
  "Summarize this meeting recording in 3–5 sentences. Then list the decisions made and the action items as bullet points." \
  meeting.mp3

# Region analysis (timestamps go in the prompt as MM:SS)
uv run <skill-dir>/scripts/analyze_audio.py \
  "Transcribe what is said between 02:30 and 03:29." \
  podcast.mp3

# Speaker and emotion estimation
uv run <skill-dir>/scripts/analyze_audio.py \
  "For this call recording, estimate each distinguishable speaker's speaking style, tone, and emotional shifts over time." \
  call_recording.wav

# Music and ambient sound interpretation
uv run <skill-dir>/scripts/analyze_audio.py \
  "Describe every sound in this audio (music, sound effects, ambient noise) chronologically." \
  ambient.flac

# Structured JSON output (extract meeting minutes)
uv run <skill-dir>/scripts/analyze_audio.py \
  "Extract the agenda items, decisions, and action items from this meeting audio." \
  meeting.mp3 --json
```

### 音频选项

| 选项 | 取值 | 默认值 | 说明 |
|---|---|---|---|
| `--model` | model ID | `gemini-3-flash-preview` | Gemini model to use |
| `--json` | flag | off | Structured JSON output (uses API `response_mime_type`) |
| `--schema` | inline JSON or `.json` path | none | Output JSON schema (auto-enables `--json`) |

> 音频没有视频专用的标志（`--fps`、`--start`、`--end`、`--media-resolution`）。在提示词本身中用 `MM:SS` 指定时间范围（例如 `from 00:30 to 02:00`）。

---

## 可用模型

任何支持多模态视频 / 音频输入的 Gemini 模型都可以通过 `--model` 传入。

| 模型 ID | 名称 | 上下文 | 备注 |
|---|---|---|---|
| `gemini-3-flash-preview` | Gemini 3 Flash | 1M | **Default.** Top multimodal understanding. |
| `gemini-3.1-flash-lite-preview` | Gemini 3.1 Flash-Lite | 1M | High cost-performance for high-volume / low-latency work. |
| `gemini-3.1-pro-preview` | Gemini 3.1 Pro | 1M | Highest reasoning and agentic capability. |

不要使用 `gemini-3-pro-preview`；Gemini 模型页面将其标记为已弃用并下线。使用 `gemini-3.1-pro-preview` 作为 Pro 档。

---

## 提示词技巧

### 时间戳引用

使用 `MM:SS` 引用特定时刻：

```
Describe the diagrams shown at 00:05 and 01:30.
Transcribe what is said between 02:30 and 03:29.
```

### 同时使用画面和音频（视频）

Gemini 会同时处理视觉帧和音频轨道。在重要时明确说明：

```
Describe both what is shown on screen and what is being said.
```

### 非语音音频解读

Gemini 能理解音乐、音效和环境噪声——不只是语音。有意地提出要求：

```
Identify the genre, tempo, and instrumentation of the music in this audio.
Estimate the location and time of day from the ambient sound in the background.
```

### 结构化提取（Structured Output）

传入 `--schema` 可通过 API 的 `response_mime_type` + `response_json_schema` 严格返回带类型的 JSON。与基于提示词的 JSON 输出不同，类型和必填字段在 API 层面得到保证。

```bash
# Video — scene-analysis schema
uv run <skill-dir>/scripts/analyze_video.py \
  "Analyze the scenes in this video." \
  video.mp4 \
  --schema '{"type":"object","properties":{"scenes":{"type":"array","items":{"type":"object","properties":{"timestamp":{"type":"string"},"visual":{"type":"string"},"audio":{"type":"string"},"objects":{"type":"array","items":{"type":"string"}}},"required":["timestamp","visual","audio","objects"]}}},"required":["scenes"]}'

# Audio — meeting-minutes schema
uv run <skill-dir>/scripts/analyze_audio.py \
  "Extract minutes from this meeting recording." \
  meeting.mp3 \
  --schema '{"type":"object","properties":{"agenda_items":{"type":"array","items":{"type":"string"}},"decisions":{"type":"array","items":{"type":"string"}},"action_items":{"type":"array","items":{"type":"string"},"items":{"type":"object","properties":{"task":{"type":"string"},"owner":{"type":"string"},"due":{"type":"string"}}}}},"required":["agenda_items","decisions","action_items"]}'

# Schema in a .json file
uv run <skill-dir>/scripts/analyze_video.py \
  "Extract action items from this meeting video." \
  meeting.mp4 \
  --schema action_items_schema.json

# Loose JSON output (no schema constraint, format only)
uv run <skill-dir>/scripts/analyze_audio.py \
  "List every likely speaker's utterances in chronological order." \
  audio.mp3 --json
```

---

## Token 用量粗略数字

### 视频

| 设置 | 每秒 Token | 每分钟 | 每 10 分钟 |
|---|---|---|---|
| 默认分辨率（1 fps） | ~300 | ~18,000 | ~180,000 |
| 低分辨率（1 fps） | ~100 | ~6,000 | ~60,000 |

### 音频

| 设置 | 每秒 Token | 每分钟 | 每 10 分钟 | 每小时 |
|---|---|---|---|---|
| 标准 | 32 | ~1,920 | ~19,200 | ~115,200 |

音频比视频便宜得多。即使是数小时的录音也在合理范围内。

对于需要反复分析的长视频，用 `--start` / `--end` 缩小范围。对于长音频，在提示词中缩小范围（`from 02:30 to 03:29`）。

---

## 支持的格式

### 视频
`.mp4`、`.mpeg`、`.mov`、`.avi`、`.flv`、`.mpg`、`.webm`、`.wmv`、`.3gp`

### 音频
`.mp3`、`.wav`、`.aac`、`.m4a`、`.ogg`、`.flac`、`.aiff`

---

## 内联 vs Files API 行为

### 视频

| 大小 | 行为 |
|---|---|
| ≤20 MB 总请求大小 | 内联发送（更快） |
| >20 MB 总请求大小、长视频或重复分析 | 先通过 Files API 上传 |

### 音频

| 大小 | 行为 |
|---|---|
| ≤20 MB | 内联发送（更快） |
| >20 MB | 先通过 Files API 上传 |

> 音频的 20 MB 上限来自 Gemini API 的单请求总大小限制。经由 Files API，单个请求的音频时长可达 9.5 小时。

Files API 的限额在付费档为 20 GB，免费档为 2 GB。

---

## 局限

- **视频** — 实际上，单请求的视频时长受模型上下文窗口限制。
- **音频** — 每个提示词最多 9.5 小时（如传入多个文件则为合并时长）。
- 音频会被下采样到 16 kbps，多声道会被合并为单声道。
- 适用安全过滤器。

---

## 故障排查

| 问题 | 解决方法 |
|---|---|
| `GEMINI_API_KEY` 未设置 | 在操作系统环境或附近的 `.env` 文件中定义。 |
| `google-genai` 未找到 | 若使用 `uv run` 或 `pipx run` 运行，依赖会自动解析。若使用纯 `python3`，先运行 `pip install google-genai python-dotenv`。 |
| 空响应 | 可能是被安全过滤器拦截。调整提示词。 |
| 上传很慢 | 总请求大小超过 20 MB 的视频文件、长视频、重复的视频分析，以及超过 20 MB 的音频文件都会经过 Files API。用 `--start` / `--end`（视频）或在提示词中（音频）缩小范围。 |
| 视频中的快速运动被漏掉 | 提高 `--fps`（例如 `--fps 5`）。Token 用量会相应增加。 |
| 达到 Token 上限 | 对于视频，降低到 `--media-resolution low` 和/或使用 `--start` / `--end`。对于音频，在提示词中缩小时间范围。 |
| 音频转录准确度低 | 在提示词中说明语言和说话人数量（例如 "two-person Japanese conversation"）。对于长录音，分块分析。 |

---

## 常见模式

### 视频

| 用例 | 示例提示词 |
|---|---|
| 会议纪要 | "Transcribe this meeting video and organize it by agenda item. Also extract the decisions and action items." |
| 产品评测分析 | "List the positive points and negative points mentioned about the product in this review video." |
| 讲座摘要 | "Summarize the key points of this lecture and write 5 review questions for study." |
| 竞品广告分析 | "Analyze the target audience, value proposition, CTA, and creative techniques used in this ad video." |
| QC 质检 | "In this manufacturing-line video, identify any anomalies or issues with timestamps." |
| 社交视频转录 | "Transcribe everything that's said in this video." |

### 音频

| 用例 | 示例提示词 |
|---|---|
| 会议录音 → 纪要 | "Transcribe this meeting recording and organize it by agenda. Extract decisions and action items." |
| 播客摘要 | "Summarize the main topics and the speakers' arguments in this podcast episode." |
| 客户通话质量审查 | "For this call recording, analyze the customer's pain points, the agent's response quality, and concrete improvement opportunities." |
| 访谈转录 | "Transcribe this interview audio with speakers labeled. Pull out 5 quotable lines." |
| 讲座 / 研讨会摘要 | "Organize the main points of this seminar audio into chapters with a summary per chapter." |
| 多语言翻译 | "Transcribe this audio and translate it to English. Show original and translation side by side." |
| 音乐 / 环境音分析 | "From the music (genre, tempo, instrumentation) and ambient sound, infer the scene and location." |
