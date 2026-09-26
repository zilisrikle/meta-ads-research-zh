---
name: video-generation
description: 使用 Google 的 Veo 3.1 生成或编辑 AI 视频。支持文本生成视频（text-to-video）、图像生成视频（image-to-video）、首尾帧插值，以及配料式生成视频（ingredients-to-video，用参考图像统一角色或产品）。对话、音效、环境音等同步音频随视频一同生成。当用户提到视频生成、AI 视频、Veo、动态图形、动画片段、社交短视频、产品视频，或用 Gemini 生成电影片段时，使用此 skill。
---

# 视频生成（Video Generation，Veo 3.1）

使用 Google 的 Veo 3.1 API 生成高质量短视频。本 skill 打包了四个脚本，覆盖 Veo 支持的四种输入形态：纯文本、单图、首尾帧插值、配料式参考图像。

## 目录结构

```
video-generation/
├── SKILL.md                        # This file (workflow, script usage, options)
├── references/
│   └── prompt-guide.md             # Detailed prompt reference (5-element formula, cinematography vocabulary, audio direction, templates)
└── scripts/
    ├── text_to_video.py            # Text → video (default)
    ├── image_to_video.py           # Image → video (animate from a starting frame)
    ├── keyframes_to_video.py       # First + last image → video (frame interpolation)
    └── refs_to_video.py            # Reference images → video (preserve characters / products)
```

## Veo 3.1 的擅长之处

- **同步音频（Synchronized audio）** — 在提示词中加入对话、音效或环境音指导，模型会生成与视频匹配的音频。
- **高质量输出** — 最高 4K 分辨率，每个片段最长 8 秒。
- **图像生成视频（Image-to-video）** — 用一张静态图像作为开场帧，从此开始动画。
- **首尾帧（First & Last Frame）** — 提供起始图和结束图；Veo 在两者之间插值生成运动。
- **配料式生成视频（Ingredients to Video）** — 传入最多三张参考图像（人物、物体、风格），Veo 会在生成片段中保留它们的外观。
- **多镜头序列** — 通过时间戳提示（`[00:00-00:02]` … `[00:06-00:08]`）在单个 8 秒片段中安排多个镜头。
- **负向提示词（Negative prompts）** — 明确排除不需要的元素。

> Veo 2 还支持在已有视频中添加或删除物体，但不带音频。本 skill 针对 Veo 3.1；需要物体添加/删除能力时请直接使用 Veo 2。

---

## 前置条件

- 必须安装 **Python 3.10+**。
- 假定 `GEMINI_API_KEY` 已可从操作系统环境变量或同目录的 `.env` 文件获取。
- 不要打印、查看、编辑或提交密钥值。

### 如何运行脚本

这些脚本是按 [PEP 723](https://peps.python.org/pep-0723/) 规范编写的内联脚本——依赖（`google-genai`、`python-dotenv`）在每个文件顶部声明。以下任一调用方式均可：

```bash
# Option A: uv (recommended — fastest, auto-resolves dependencies)
uv run <skill-dir>/scripts/text_to_video.py "prompt"

# Option B: pipx (when uv isn't available)
pipx run <skill-dir>/scripts/text_to_video.py "prompt"

# Option C: pip + plain python (most universal — install dependencies once)
pip install google-genai python-dotenv
python3 <skill-dir>/scripts/text_to_video.py "prompt"
```

下面的命令示例使用最简形式 `uv run`，但选项 B 和 C 运行的是相同的脚本，效果完全一致。

---

## 工作流

### 生成前

向用户收集以下信息。如有缺项，请提问：

1. **用例（Use case）** — 社交帖子、产品展示、广告、演示文稿视觉素材等？
2. **场景（Scene）** — 展示什么内容？什么在动？
3. **音频（Audio）** — 需要对话或音效吗？如果需要，对话内容或声音效果是什么？
4. **宽高比（Aspect ratio）** — 横向（16:9）还是竖向（9:16）？
5. **时长（Length）** — 4 秒、6 秒还是 8 秒？
6. **参考图像（Reference images）** — 是否有可用作起始帧、结束帧或角色/风格参考的图像？

### 推荐工作流：静态图 → 确认 → 视频

视频生成成本高、迭代慢。推荐的模式是：**先生成一张目标风格的静态图，与用户确认方向，然后再生成视频**。静态图生成又快又便宜，可以先锁定构图和主体，再支付视频生成的成本。

三种模式覆盖大多数需求：

#### 模式 A：起始帧 → 图像生成视频（默认）

最通用的模式。起始帧提前固定构图和主体。

1. 生成起始帧静态图（例如用图像生成工具）。
2. 向用户展示并**确认："这是您想要的视频方向吗？"**
3. 获得批准后运行 `image_to_video.py`。

#### 模式 B：起始帧 + 结束帧 → 首尾帧（运镜、转场）

适用于大幅运镜、日转夜转场、形变，或任何由"起点在哪里、终点在哪里"定义的效果。

1. 生成起始和结束两张静态图。
2. 与用户确认："希望 Veo 在这两个起点和终点之间做插值吗？"
3. 获得批准后运行 `keyframes_to_video.py`。

#### 模式 C：参考素材 → 配料式生成视频（统一角色 / 产品）

适用于片段中必须原样出现的特定角色、产品或风格化素材——广告片和系列内容中常见。

1. 生成 1–3 张参考图像（角色、产品、环境）。
2. 与用户确认："视频是否应以这些为外观基准？"
3. 获得批准后运行 `refs_to_video.py`。

#### 模式选择

| 用例 | 模式 |
|---|---|
| 通用片段（横向、产品展示、社交） | A：起始帧 → 图像生成视频 |
| 运镜、视角切换、延时 | B：起始帧 + 结束帧 → 首尾帧 |
| 需要统一角色或产品的多镜头剪辑 | C：参考素材 → 配料式生成视频 |
| 快速测试或一次性片段 | 直接文本生成视频（跳过静态图步骤） |

如果用户已有想要的参考图像，跳过静态图生成步骤，直接使用对应脚本。

> 以下命令示例中，请将 `<skill-dir>` 替换为该 skill 的实际安装路径。脚本位于 `<skill-dir>/scripts/` 下。

### 文本生成视频

```bash
uv run <skill-dir>/scripts/text_to_video.py "prompt" [output filename] [options...]
```

示例：

```bash
# Basic generation
uv run <skill-dir>/scripts/text_to_video.py \
  "A golden retriever running through a sunlit meadow in slow motion. Cinematic look."

# Vertical short-form clip
uv run <skill-dir>/scripts/text_to_video.py \
  "A barista making latte art in a cozy cafe. Close-up shot." \
  cafe_short.mp4 \
  --aspect-ratio 9:16 --duration 6

# With dialogue + ambient audio
uv run <skill-dir>/scripts/text_to_video.py \
  "A woman standing in front of a whiteboard, speaking to the camera: 'Today we'll learn about solar energy.' The room has soft ambient sounds." \
  presentation.mp4

# With a negative prompt
uv run <skill-dir>/scripts/text_to_video.py \
  "Aerial view of a coastal city at sunset. Cinematic drone footage." \
  city_aerial.mp4 \
  --negative-prompt "text, watermark, blurry, low quality"
```

### 图像生成视频

使用一张静态图作为开场帧并从此开始动画：

```bash
uv run <skill-dir>/scripts/image_to_video.py "prompt" input_image_path [output filename] [options...]
```

示例：

```bash
# Animate a product still
uv run <skill-dir>/scripts/image_to_video.py \
  "The product slowly rotates on a turntable. Soft studio lighting." \
  product.png product_video.mp4

# Animate a landscape photo
uv run <skill-dir>/scripts/image_to_video.py \
  "Clouds slowly move across the sky. A gentle breeze rustles the leaves." \
  landscape.jpg landscape_animated.mp4
```

### 首尾帧（插值）

提供起始图和结束图；Veo 在两者之间插值。**此模式时长固定为 8 秒。**

```bash
uv run <skill-dir>/scripts/keyframes_to_video.py "prompt" first_image_path last_image_path [output filename] [options...]
```

示例：

```bash
# 180° arc shot from front to back of the singer
uv run <skill-dir>/scripts/keyframes_to_video.py \
  "The camera performs a smooth 180-degree arc shot, starting with the front-facing view of the singer and circling around her." \
  singer_front.png singer_back.png singer_arc.mp4

# Day → night time lapse
uv run <skill-dir>/scripts/keyframes_to_video.py \
  "Time-lapse of a city skyline transitioning from day to night. Lights gradually turn on." \
  city_day.jpg city_night.jpg timelapse.mp4
```

### 配料式生成视频（参考素材）

最多 3 张参考图像（人物、物体、环境）用于保持外观统一。适用于角色或产品需要始终"在状态"的系列内容。

```bash
uv run <skill-dir>/scripts/refs_to_video.py "prompt" --images image1 [image2] [image3] [options...]
```

示例：

```bash
# Dialogue scene with character + setting references
uv run <skill-dir>/scripts/refs_to_video.py \
  "The detective behind his desk looks up and says in a weary voice, 'Of all the offices in this town, you had to walk into mine.'" \
  --images detective.png office_setting.png \
  --output detective_scene.mp4

# Product ad referencing two product angles
uv run <skill-dir>/scripts/refs_to_video.py \
  "A sleek product showcase with soft studio lighting. The headphone rotates slowly on a reflective surface." \
  --images headphone_front.png headphone_side.png \
  --output product_ad.mp4 --aspect-ratio 9:16
```

---

## 选项

| 选项 | 取值 | 默认值 | 说明 |
|---|---|---|---|
| `--aspect-ratio` | `16:9`、`9:16` | `16:9` | 横向或竖向 |
| `--duration` | `4`、`6`、`8` | `8` | 片段时长（秒） |
| `--resolution` | `720p`、`1080p`、`4k` | `720p` | 输出分辨率（1080p / 4k 需要 8 秒片段） |
| `--negative-prompt` | 文本 | 无 | 要排除的元素（配料式生成视频不支持） |
| `--person-generation` | `allow_all` | 无 | 人物生成的许可（不支持 `allow_adult`） |
| `--poll-interval` | 秒数 | `10` | 生成等待期间轮询状态的间隔秒数 |

---

## 提示词写作基础

用英文写提示词。围绕**五要素公式（5-element formula）**组织：`[Cinematography] + [Subject] + [Action] + [Context] + [Style & Ambiance]`。对话用引号括起，音效加前缀 `SFX:`，环境音加前缀 `Ambient:`。

摄影词汇表、音频指导模式、时间戳提示（在一个 8 秒片段中安排多个镜头）以及各用例模板，见 [references/prompt-guide.md](references/prompt-guide.md)。

---

## 限制

- 视频生成是异步的；每次请求预计需要几分钟。
- 单次请求的最长片段为 8 秒。
- 1080p 和 4K 分辨率需要 8 秒片段。
- 人物生成受安全过滤器约束，可能因地区受限。
- API 有速率限制。
- 生成的视频包含不可见的 SynthID 水印。

---

## 故障排除

| 问题 | 修复方法 |
|---|---|
| `GEMINI_API_KEY` 未设置 | 在操作系统环境变量或同目录的 `.env` 文件中定义。 |
| 找不到 `google-genai` | 若使用 `uv run` 或 `pipx run` 运行，依赖会自动解析。若使用纯 `python3`，先运行 `pip install google-genai python-dotenv`。 |
| 生成耗时过长 | 几分钟属正常。如有需要可调整 `--poll-interval`。 |
| 请求被安全过滤器拦截 | 调整提示词。如果需要生成人物，尝试 `--person-generation allow_all`。 |
| 分辨率 `1080p` / `4k` 报错 | 这些分辨率需要 `--duration 8`；请显式指定。 |
| 片段时长不对 | 显式指定 `--duration 4`、`6` 或 `8`。 |
| 输出与预期不符 | 把提示词写得更具体——详细描述运镜、光线和动作。 |

---

## 与其他工具联用

- **上游用静态图生成** — 先把起始帧、结束帧或参考素材生成为静态图，可以在投入视频生成之前低成本锁定方向。上面的模式 A / B / C 工作流均基于此假设。
- **下游做后期** — 当需要文本叠加、字幕、转场、音乐混音、多片段拼接，或超出生成能力的动画图表时，把 Veo 片段渲染出来，在专用视频剪辑工具中完成。
