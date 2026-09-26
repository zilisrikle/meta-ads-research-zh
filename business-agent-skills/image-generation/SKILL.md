---
name: image-generation
description: 使用 Google 的 Nano Banana / Nano Banana Pro（Gemini Image）或 OpenAI 的 gpt-image-2 生成或编辑图片。当用户提到图像生成、AI 图像、缩略图、横幅、海报、主视觉图、信息图、图表、产品照片、样机图、插画、照片生成、图像编辑、Nano Banana、gpt-image，或用 Gemini / OpenAI 生成图像时，使用此 skill。擅长在图像内准确渲染文字（包括非拉丁文字），同样适用于纯视觉内容。
---

# 图像生成与编辑

此 skill 支持两个提供商——按任务需要任选其一。两者都支持文生图（text-to-image）与图像编辑（image editing，包括多参考图合成）。

| 提供商 | 模型 | 擅长 |
|---|---|---|
| **Google Gemini** (Nano Banana) | `gemini-3.1-flash-image-preview`、`gemini-3-pro-image-preview` | 基于宽高比（aspect ratio）的输出、图内文字（尤其非拉丁文字）、最多 11–14 张参考图的合成。 |
| **OpenAI** (gpt-image-2) | `gpt-image-2` | 像素级精确尺寸、基于蒙版（mask）的局部编辑、一次调用生成多个变体（`--n`）、PNG 以外的输出格式（jpeg/webp）。 |

如果用户没有指定，默认使用 **Gemini Nano Banana（`gemini-3.1-flash-image-preview`）**——日常任务中最快、最便宜。

---

## 前置条件

- 必须安装 **Python 3.10+**。
- 假设相关 API 密钥（API key）已可从操作系统环境或附近的 `.env` 文件获取：
  - `GEMINI_API_KEY`（Google Gemini 脚本用）
  - `OPENAI_API_KEY`（OpenAI 脚本用）

请勿打印、查看、编辑或提交密钥值。

### 如何运行脚本

这些脚本是 [PEP 723](https://peps.python.org/pep-0723/) 内联脚本——依赖已在每个文件顶部声明。以下任一调用方式均可：

```bash
# Option A: uv (recommended — fastest, auto-resolves dependencies)
uv run <skill-dir>/scripts/<provider>/text_to_image.py "prompt"

# Option B: pipx (when uv isn't available)
pipx run <skill-dir>/scripts/<provider>/text_to_image.py "prompt"

# Option C: pip + plain python (most universal — install dependencies once)
pip install google-genai openai python-dotenv
python3 <skill-dir>/scripts/<provider>/text_to_image.py "prompt"
```

以下命令示例使用最短的 `uv run` 形式，但方式 B 和 C 运行的是完全相同的脚本。

---

## 模型

### Gemini

| 模型 | 模型 ID | 用途 |
|---|---|---|
| **Nano Banana** | `gemini-3.1-flash-image-preview` | 快速且便宜（默认）。用于日常任务。 |
| **Nano Banana Pro** | `gemini-3-pro-image-preview` | 最高质量。留给对文字保真度或照片质量要求严苛的生产任务。 |

只有当质量起决定性作用时，才从 Flash 切换到 Pro——例如：要求图像内文字准确渲染、需要高质量摄影质感（广告、生产环境落地页），或信息密集的信息图需要精细细节。

| 规格 | Nano Banana Pro | Nano Banana |
|---|---|---|
| **宽高比** | `1:1`、`2:3`、`3:2`、`3:4`、`4:3`、`4:5`、`5:4`、`9:16`、`16:9`、`21:9` | 以上全部，外加 `1:4`、`1:8`、`4:1`、`8:1` |
| **图像尺寸** | `1K`（默认）、`2K`、`4K` | `512px`、`1K`、`2K`、`4K` |
| **参考图上限** | 最多 11 张（6 张物体 + 5 张人物） | 最多 14 张（10 张物体 + 4 张人物） |

### GPT Image

| 模型 | 模型 ID | 用途 |
|---|---|---|
| **GPT Image 2** | `gpt-image-2` | 高质量生成与编辑：像素级精确尺寸、基于蒙版的局部编辑、多变体输出。 |

| 规格 | gpt-image-2 |
|---|---|
| **尺寸** | 绝对像素尺寸——两条边均为 16 的倍数，最大 3840，最大宽高比 3:1。常用预设：`1024x1024`、`1024x1536`、`1536x1024`、`2048x2048`。传 `auto` 让模型自行选择。 |
| **质量** | `low`、`medium`、`high`、`auto`（默认） |
| **输出格式** | `png`（默认）、`jpeg`、`webp`（jpeg/webp 可用 `--output-compression 0–100`） |
| **多个变体** | `--n`，一次调用生成多张图像 |
| **基于蒙版的编辑** | `--mask`，配合与主图同尺寸的带 alpha 通道 PNG |

---

## 价格（约数，Standard 档）

价格会变动——依赖这些数字前请务必查看实时价格页面。

### Gemini

按分辨率计的单图输出成本。输入按 $0.50/100 万 token（Flash）或 $2/100 万 token（Pro）另行计费。

| 模型 | 512px | 1K | 2K | 4K |
|---|---|---|---|---|
| Nano Banana Flash (`gemini-3.1-flash-image-preview`) | ~$0.045 | ~$0.067 | ~$0.101 | ~$0.15 |
| Nano Banana Pro (`gemini-3-pro-image-preview`) | — | ~$0.134 | ~$0.134 | ~$0.24 |

来源：[Vertex AI 生成式 AI 定价](https://cloud.google.com/vertex-ai/generative-ai/pricing)。

### OpenAI gpt-image-2

按质量与尺寸计的单图成本。输入按 $5/100 万文本输入 token、$8/100 万图像输入 token、$30/100 万图像输出 token 另行计费。

| 质量 | 1024×1024 | 非正方形（如 1024×1536、1536×1024） |
|---|---|---|
| Low | ~$0.006 | ~$0.005 |
| Medium | ~$0.053 | ~$0.041 |
| High | ~$0.211 | ~$0.165 |

精确估算请使用 OpenAI 的[图像生成计算器](https://developers.openai.com/api/docs/guides/image-generation#calculating-costs)。来源：[OpenAI 定价](https://developers.openai.com/api/docs/pricing)。

---

## 工作流

### 生成前

向用户收集以下信息。如有缺失，请提问：

1. **使用场景（use case）**——缩略图、横幅、产品照片、社媒帖子等？
2. **主体**——画面中要呈现什么？
3. **风格**——摄影、插画、3D？
4. **文字**——图像内需要渲染什么文字？
5. **尺寸 / 宽高比**——横版（16:9）、正方形（1:1）、竖版（9:16）？
6. **参考图**——要编辑的图，或用作风格 / 构图参考的图？
7. **提供商偏好**——Gemini（默认）或 OpenAI？

### 选择合适的脚本

| 情形 | 脚本 |
|---|---|
| 无参考图——从零生成 | `<provider>/text_to_image.py` |
| 有参考图——编辑它（换背景、加文字、修饰局部区域） | `<provider>/image_to_image.py` |
| 有参考图——用作新图的风格或构图参考 | `<provider>/image_to_image.py` |
| 把多张图合成一张 | `<provider>/image_to_image.py`（额外图片用 `--images` 传入） |
| 限定在特定区域的局部编辑 | `gpt-image/image_to_image.py --mask <mask.png>` |
| 一次调用生成多个变体 | `gpt-image/text_to_image.py --n N` |

拿不准时：有参考图吗？有 → `image_to_image.py`；没有 → `text_to_image.py`。默认提供商 → `gemini/`。需要 gpt-image 的特定能力时再切换到 `gpt-image/`。

> 以下命令示例中，请把 `<skill-dir>` 替换为此 skill 的实际安装路径。脚本位于 `<skill-dir>/scripts/gemini/` 和 `<skill-dir>/scripts/gpt-image/`。

---

## Gemini 命令

### 文生图

```bash
uv run <skill-dir>/scripts/gemini/text_to_image.py "prompt" [output filename] [options...]
```

| 选项 | 默认值 | 说明 |
|---|---|---|
| `--model` | `gemini-3.1-flash-image-preview` | 模型 ID |
| `--aspect-ratio` | 无 | 宽高比（如 `16:9`、`1:1`、`9:16`） |
| `--image-size` | 无 | 图像尺寸（`512px`、`1K`、`2K`、`4K`） |

示例：

```bash
# Basic generation (default model: Nano Banana Flash)
uv run <skill-dir>/scripts/gemini/text_to_image.py "A cute cat sitting on a windowsill at sunset"

# With output filename, aspect ratio, and image size
uv run <skill-dir>/scripts/gemini/text_to_image.py "Product package mockup on white background, studio lighting, soft shadows" mockup.png --aspect-ratio 1:1 --image-size 2K

# Switch to the high-quality Pro model
uv run <skill-dir>/scripts/gemini/text_to_image.py "Premium product photo with precise typographic overlay" hero.png --model gemini-3-pro-image-preview
```

### 图生图

```bash
uv run <skill-dir>/scripts/gemini/image_to_image.py "edit instruction" reference_image_path [output filename] [options...]
```

| 选项 | 默认值 | 说明 |
|---|---|---|
| `--model` | `gemini-3.1-flash-image-preview` | 模型 ID |
| `--aspect-ratio` | 无 | 宽高比 |
| `--image-size` | 无 | 图像尺寸 |
| `--images` | 无 | 额外的参考图（可传多张） |

示例：

```bash
# Replace the background
uv run <skill-dir>/scripts/gemini/image_to_image.py "Change the background to a cherry blossom avenue in full bloom, blend edges naturally" input.png edited.png

# Add a text overlay on top of an image
uv run <skill-dir>/scripts/gemini/image_to_image.py 'Add "SPRING SALE NOW ON" in large white sans-serif bold text at the top center, with a subtle drop shadow' product.png sale_banner.png

# Combine multiple reference images into a unified composition
uv run <skill-dir>/scripts/gemini/image_to_image.py "Combine all products into a unified catalog-style image on white background, consistent lighting" product1.png catalog.png --images product2.png product3.png
```

---

## GPT Image 命令

### 文生图

```bash
uv run <skill-dir>/scripts/gpt-image/text_to_image.py "prompt" [output filename] [options...]
```

| 选项 | 默认值 | 说明 |
|---|---|---|
| `--model` | `gpt-image-2` | 模型 ID |
| `--size` | `auto` | 图像尺寸，如 `1024x1024`、`1536x1024`、`2048x2048`，或 `auto` |
| `--quality` | `auto` | `low`、`medium`、`high`、`auto` |
| `--output-format` | `png` | `png`、`jpeg`、`webp` |
| `--output-compression` | 无 | 0–100（仅 jpeg/webp） |
| `--n` | `1` | 一次调用生成的图像数量 |

示例：

```bash
# Basic generation
uv run <skill-dir>/scripts/gpt-image/text_to_image.py "Studio product shot of a ceramic teapot on a marble counter, soft daylight"

# Specific size and high quality, JPEG output
uv run <skill-dir>/scripts/gpt-image/text_to_image.py "Hero banner: minimalist mountain landscape at dawn" hero.jpg --size 1536x1024 --quality high --output-format jpeg --output-compression 85

# Generate four variants in one call
uv run <skill-dir>/scripts/gpt-image/text_to_image.py "App icon concept, geometric, vibrant gradient" icon.png --n 4
```

### 图生图

```bash
uv run <skill-dir>/scripts/gpt-image/image_to_image.py "edit instruction" reference_image_path [output filename] [options...]
```

| 选项 | 默认值 | 说明 |
|---|---|---|
| `--model` | `gpt-image-2` | 模型 ID |
| `--size` | `auto` | 输出尺寸 |
| `--quality` | `auto` | `low`、`medium`、`high`、`auto` |
| `--output-format` | `png` | `png`、`jpeg`、`webp` |
| `--output-compression` | 无 | 0–100（仅 jpeg/webp） |
| `--mask` | 无 | 带 alpha 通道的 PNG，将编辑限定在特定区域 |
| `--images` | 无 | 额外的参考图（可传多张） |
| `--n` | `1` | 生成的变体数量 |

示例：

```bash
# Background replacement
uv run <skill-dir>/scripts/gpt-image/image_to_image.py "Replace the background with a soft beige studio backdrop" product.png edited.png

# Mask-based localized edit (only the masked region changes)
uv run <skill-dir>/scripts/gpt-image/image_to_image.py "Add a flamingo standing in the pool" lounge.png composition.png --mask lounge_mask.png

# Combine four reference images into one composition
uv run <skill-dir>/scripts/gpt-image/image_to_image.py "Arrange these as a flat-lay product set on a wooden table" body-lotion.png set.png --images bath-bomb.png incense.png soap.png
```

---

## 提示词写作基础

**用英文写指令。** 当外围指令为英文时，模型效果更强。图像*内部*要渲染的文字字符串可以用任意语言。

各提供商的提示词指南位于 `references/`：
- Gemini：[references/gemini.md](references/gemini.md)——摄影词汇、宽高比输出、非拉丁文字渲染。
- OpenAI：[references/gpt-image.md](references/gpt-image.md)——导演式美术指导、基于蒙版的编辑、多图合成。

请阅读与你所用提供商对应的那一份。

### 速查

1. **用英文写指令。** 图像内要渲染的文字可以用任意语言。
2. **具体。** 写明主体、构图、风格、色彩和氛围。
3. **用摄影词汇。** `low angle`、`telephoto lens`、`bokeh`、`natural light` 效果很好。
4. **引用要渲染的文字。** `Place "GRAND OPENING" in large bold text at the top`。
5. **用反向表述。** `no text, no watermark`。

### 提示词结构模板

```
[Detailed description of the subject].
[Camera setup: lens, angle, depth of field].
[Lighting: type and direction of light source].
[Color tone and mood].
[Additional instructions: text placement, exclusions, etc.]
```

---

## 常见业务模式

| 使用场景 | 示例提示词 |
|---|---|
| 缩略图 | `YouTube thumbnail, landscape 16:9. Surprised person on the left, "WHAT HAPPENED NEXT" in large bold white text on the right. Gradient background, high contrast` |
| 产品样机 | `Minimal product photography on white background. Glass perfume bottle with "AURORA" label. Soft studio lighting, natural shadow` |
| 社媒横幅 | `Square image for Instagram post. Cozy cafe interior photo with "GRAND OPENING" text overlay in elegant serif font. Brand accent color` |
| 信息图 | `Business flow infographic on white background. 3 steps arranged left to right, each with an icon and a short caption underneath` |
| 房产广告 | `Luxury apartment exterior photo. Semi-transparent banner at bottom with "South-facing units, 3 min from station" in white text. Professional real estate photography` |

各使用场景的提示词模板，见你所用提供商的提示词指南：
- [references/gemini.md](references/gemini.md)
- [references/gpt-image.md](references/gpt-image.md)

---

## 局限

- **水印**：Gemini 生成的图像带有不可见的 SynthID 水印。OpenAI 生成的图像包含 C2PA 来源元数据。
- **安全过滤器（safety filter）**：涉及真人、暴力或其他受限内容时，两个提供商都可能拦截或修改生成请求。
- **速率限制（rate limit）**：两个 API 都有按分钟和按天的配额——详情见各提供商文档。
- **背景（gpt-image-2）**：目前不支持透明背景；API 返回不透明背景。

---

## 故障排查（troubleshooting）

| 问题 | 解决方法 |
|---|---|
| `GEMINI_API_KEY` / `OPENAI_API_KEY` 未设置 | 在操作系统环境或附近的 `.env` 文件中定义相关变量。 |
| 找不到 `google-genai` 或 `openai` | 用 `uv run` 或 `pipx run` 运行时依赖会自动解析；用纯 `python3` 时先运行 `pip install google-genai openai python-dotenv`。 |
| 只返回文字、没有图像（Gemini） | 调整提示词或切换模型。 |
| 安全过滤器拦截了请求 | 调整提示词——避免以人物为主或暴力的画面。 |
| 空响应 | 在提供商控制台验证 API 密钥的有效性与配额。 |
| 图像内文字渲染错误 | 减少文字量，和/或切换到更高质量的模型（`gemini-3-pro-image-preview` 或 `gpt-image-2 --quality high`）。 |
| 图像与意图不符 | 把提示词结构写得更清晰，多用反向指令。用 `--mask`（OpenAI）做区域限定的编辑。 |
| OpenAI：宽高比被拒绝 | 两条边都必须是 16 的倍数，最大 3840，最大宽高比 3:1。 |

---

## 针对具体任务要问的问题

1. 图像的用途是什么？（缩略图、横幅、产品照片、社媒帖子等）
2. 图像内需要文字吗？如果需要，写什么？
3. 风格偏好——摄影、插画、3D？
4. 宽高比或具体像素尺寸？
5. 有参考图吗，或有现成的图要编辑？
6. 提供商偏好——Gemini（默认）或 OpenAI？选择的理由是什么（比如需要基于蒙版的编辑、多个变体、特定输出格式）？
7. 要遵循的品牌色或设计规范？
