# GPT Image 2 提示词指南

如何写出能让 OpenAI 的 `gpt-image-2` 产出好结果的提示词。基于 OpenAI 官方图像生成模型提示词指南，配以从业者社区的具体示例。

> **TL;DR：** 像导演给摄影师下 brief 一样写提示词。先写场景，点名主体，层层加细节，最后加约束。在关键处具体，其余交给模型。避免关键词链——用平实的句子写。

---

## gpt-image-2 与老一代生成器的区别

如果你从 Stable Diffusion、Midjourney 或 DALL·E 2 转过来，有几个习惯很不好使：

| 老习惯 | gpt-image-2 下更好的做法 |
|---|---|
| 关键词链：`cat, photorealistic, 4k, masterpiece, hdr, vray` | 句子：`A close-up portrait of a black cat sitting on a windowsill at golden hour, shot like a 35mm film photograph.` |
| 反向提示词（`--no people, watermark`） | 显式保留：`Do not include any people. No watermark. No text.` |
| 调几十个权重和种子 | 对话中单点修改迭代 |
| `((emphasis))` 与权重语法 | 平实语言："the focal point is the bottle on the left" |
| 猜晦涩的模型专属魔法词 | 摄影词汇、美术指导语言、brief 式文案 |

模型响应的是**美术指导**，不是搜索 query。

---

## 核心结构

顺序不如清晰重要，但可靠的顺序能减少意外：

```
1. Style / medium       — "Photorealistic 35mm film photograph"
2. Subject              — "an elderly sailor on a small fishing boat"
3. Environment          — "at the harbor at dawn, mist over the water"
4. Lighting & mood      — "soft coastal daylight, honest and unposed"
5. Composition          — "medium close-up at eye level, shallow depth of field"
6. Technical / constraints — "subtle film grain, no retouching, no text"
```

把最重要的放在**前 ~50 词**——模型对靠前的 token 加权更高。提示词变长时，用换行或分段标签代替密集段落；结构有助于调试。

---

## 像记录现实一样写

要写实，最强的一招是把提示词写成仿佛正在拍摄一张真实照片：

- *"Photorealistic 35mm film photograph of…"*
- *"iPhone snapshot of…"*
- *"Professional studio product photography of…"*
- *"Documentary-style candid photo of…"*

然后叠加具体的质感要求：
- *"visible wrinkles, pores, and sun texture"*
- *"natural skin imperfections, no plastic skin"*
- *"subtle film grain, natural color balance"*
- *"worn materials and everyday detail; no glamorization, no heavy retouching"*

cookbook 的标准示例：

> *"Create a photorealistic candid photograph of an elderly sailor standing on a small fishing boat. He has weathered skin with visible wrinkles, pores, and sun texture… Shot like a 35mm film photograph, medium close-up at eye level… soft coastal daylight, shallow depth of field, subtle film grain, natural color balance. The image should feel honest and unposed."*

非写实工作，把纪实框换成合适的框——*"editorial illustration in the style of…"*、*"flat vector mockup of…"*、*"product render on a turntable studio set…"*——并相应调整质感词汇。

---

## 指定主体

明确说明人物或物体在画面中的位置：

- **身体取景**——*"full body visible, feet included"* 或 *"medium close-up from the chest up"*。
- **相对比例**——*"child-sized relative to the table"*、*"the bottle takes up roughly one third of the frame"*。
- **视线与互动**——*"looking down at the open book, not at the camera"*。
- **姿态与动作**——*"hands naturally gripping the handlebars, weight on the back foot"*。

模糊的描述会让模型默认对称、摆拍、直视镜头的构图。具体的描述能打破这个默认。

### 编辑时的人物身份保留

编辑时，明确列出不得改变的内容。cookbook 称之为**外科手术式约束语言（surgical constraint language）**：

> *"Do not change her face, facial features, skin tone, body shape, pose, or identity in any way. Preserve her exact likeness."*

每次迭代都重复这份保留清单。多轮中漂移累积很快；重述不变量是最便宜的防范。

---

## 构图、取景、光线

| 元素 | 实用词汇 |
|---|---|
| 视角 | close-up、medium shot、wide shot、top-down、over-the-shoulder |
| 角度 | eye-level、low-angle、high-angle、bird's-eye、dutch angle |
| 光线 | soft diffuse、golden hour、harsh midday、rim light、backlight、neon、fluorescent |
| 氛围 | quiet、intimate、energetic、somber、dreamlike、austere |
| 位置 | *"logo top-right"*、*"subject centered with negative space on left"*、*"text running vertically along the right edge"* |

氛围场景（弱光、雨、霓虹、雾）要在光线上多花笔墨——否则模型会用表面细节换掉氛围。例如：*"soft coastal daylight, shallow depth of field, subtle film grain, natural color balance"* 能让观感保持真实。

关于相机的一点说明：高层观感胜过深层相机参数。*"35mm film photograph, medium close-up at eye level"* 比 `f/1.4 85mm Sony A7R IV ISO 400 1/250s` 更可靠。模型对具体相机参数的解读比较宽松。

---

## 图像内文字

`gpt-image-2` 渲染文字异常准确——包括非拉丁文字（中文、日文、韩文、阿拉伯文等），保真度约 95%+。要榨干它：

1. **引用字面文字**或用全大写，让模型分清什么是指令、什么是要渲染的字符串：
   > *Place "GRAND OPENING" centered at the top in large bold sans-serif white text.*

2. **指定排印**：字重、字族（sans/serif/script/mono）、字距、颜色、位置。

3. 保真度重要时**要求逐字准确**：
   > *Render this text exactly as written, character by character, with no extra characters or marks.*

4. **难拼的品牌名逐字母拼出来**：
   > *The product name reads "AURORA" — A, U, R, O, R, A.*

5. 文字密集或小字号时**调高质量**：信息图、多字体海报、小字号说明用 `--quality high`。主视觉排印用 `medium` 就够。

6. 模型重复文字时**约束只出现一次**：
   > *Ensure the text appears once and is perfectly legible.*

---

## 多图输入

当你用 `image_to_image.py --images ...` 传入多张参考图时，按**序号加描述**指代它们，再描述交互：

> *"Image 1 is the product photo. Image 2 is the style reference. Apply the style from Image 2 to the product in Image 1, keeping the product's geometry and label legibility unchanged."*

构图示例：

> *"Place the dog from Image 2 into the setting of Image 1, right next to the woman. Use the same lighting, color grading, and composition as Image 1."*

明确说明哪张图控制哪个维度（风格、主体、布局、光线）。不说清楚，模型会取平均。

---

## 约束，而非反向

`gpt-image-2` 没有独立的"反向提示词"通道。用平实指令代替：

- *"No watermark."*
- *"No additional text or logos."*
- *"No people in the background."*
- *"Background must be a plain solid color, no patterns."*

编辑时最强的模式是成对出现：

> *Change only the background. Keep the subject, lighting, color grading, and framing exactly the same. No accessories, text, logos, or watermarks added.*

编辑链的每次迭代都要重述这些约束。

---

## 基于蒙版的局部编辑

`gpt-image-2` 支持 `--mask` 参数：带 alpha 通道的 PNG，与主图同尺寸。透明（alpha=0）区域可编辑；不透明（alpha=255）区域锁定。

用蒙版时，提示词只描述**蒙版区域内的变化**。不要重复其余场景——蒙版已经保护了它。

> *"Add a flamingo standing in the pool."*（`lounge_mask.png` 只覆盖泳池区域）

如果效果溢出蒙版，收紧蒙版几何形状，而不是加更多提示词语言。

---

## 质量设置

| `--quality` | 何时用 |
|---|---|
| `low` | 大批量探索、草稿、内部预览。快速便宜。 |
| `medium` | 多数生产工作的默认。质量与延迟兼顾。 |
| `high` | 密集文字、细节信息图、特写人像、身份敏感编辑、2560×1440 以上的任何内容。 |
| `auto`（默认） | 让模型自己选。多数情况都够用。 |

如果结果"差一点但偏软"，先试 `--quality high`，再重写提示词。

---

## 迭代策略

多轮编辑是 `gpt-image-2` 真正的强项之一。模式：

1. 从干净的**基础提示词**开始，产出一张合理的图。
2. 用**单点修改**精调：
   - *"Make the lighting warmer."*
   - *"Remove the second tree on the right."*
   - *"Restore the original background."*
3. 用 *"same style as before"* 或 *"the subject"* 指代前文——但**漂移了的关键细节要重新明确**。
4. 每次迭代重述保留清单。成本很低；防漂移的效果很好。

不要试图一次巨型重写解决所有问题。小的外科手术式修改能让你看清到底是什么起了作用。

---

## 常见陷阱

| 陷阱 | 症状 | 解决方法 |
|---|---|---|
| 首个提示词过载 | 模型抓住几个要求、丢掉其余 | 从简单开始，迭代加细节 |
| 写实工作流里用电影感词汇 | 输出像电影海报，不像真照片 | 删掉 "cinematic"、"epic"、"stylized"——用纪实语言 |
| 详细相机参数 | 参数被宽松解读或忽略 | 改用构图语言（"medium close-up, shallow DOF"） |
| 把反向当独立通道 | 不想要的元素仍出现 | 转成显式的 "Do not include / no X" 句子 |
| 跨迭代漂移 | 主体变形、背景不请自来 | 每轮重述保留清单 |
| 模糊的文字指令 | 字符错误、文字重复 | 引用字面文字、指定"只出现一次"、用 `--quality high` |

---

## 按使用场景的模板

### 写实人像

```
Photorealistic 35mm film photograph of [subject description: age, build, clothing, demeanor].
[Setting] at [time of day], [environmental details].
Shot like a candid documentary frame. Medium close-up at eye level.
Soft natural light from [direction], shallow depth of field, subtle film grain.
Visible skin texture, natural imperfections, no plastic skin, no heavy retouching.
The image should feel honest and unposed. No watermark, no text.
```

### 白底产品样机

```
Professional studio product photography. [Product description] centered on a plain white background.
Soft studio lighting from above-left, natural soft shadow on the right.
Product label reads "[BRAND NAME]" in [font style].
Sharp focus throughout, catalog-quality finish.
No additional props, no text overlays, no watermark.
```

### 营销 / 广告战役

```
Polished campaign image for [brand], a [brand description].
The shot shows [subject and action] in [setting].
Tagline overlay reads "[TAGLINE]" in [font/placement].
Mood: [energetic / contemplative / aspirational / playful].
Composition: [centered hero / rule-of-thirds / cinematic widescreen].
The image should feel [stylistic descriptor] — modern, tasteful, on-brand.
```

### UI 样机

```
Realistic mobile app UI mockup for [app type and purpose].
Show [specific screen/state] with [list of UI elements].
Layout: clean header, [main content area], [bottom nav or footer].
Typography: [font family], readable at thumbnail size.
The interface should look like a real, well-designed, beautiful app — not a wireframe.
Aspect ratio: [9:19.5 for iPhone / 16:9 for desktop / etc.].
```

### 信息图（文字密集）

```
Detailed infographic explaining [topic].
Layout: [vertical poster / horizontal flow / grid].
Include: [list of labeled steps, components, or data points].
Title at the top reads "[TITLE]" in [font weight] sans-serif.
Each section labeled with short captions in clean kerning.
Color palette: [primary] and [secondary] on [background].
Style: flat design, technical clarity, no decorative noise.
Use --quality high for legible small text.
```

### 风格迁移（`image_to_image.py --images`）

```
Image 1: [description — the subject/scene to keep].
Image 2: [description — the style reference].

Apply the visual style of Image 2 to Image 1.
Keep Image 1's [subject identity / composition / framing] unchanged.
Match Image 2's [palette / brushwork / lighting treatment / texture].
No watermark, no text.
```

### 外科手术式产品抠图

```
Extract the product from the input image and place it on a plain white opaque background.
Preserve product geometry, label legibility, and color exactly.
Soft natural shadow underneath.
Do not restyle, recolor, or retouch the product itself.
Only remove background and lightly polish.
No reflections, no props, no text.
```

### 局部蒙版编辑

```
[Description of the change to make inside the masked region only.]
[Optional: lighting / shadow guidance for matching the rest of the scene.]
```
*The mask handles the rest. Don't restate the whole scene.*

---

## 何时调到 Pro / `--quality high`

默认用 `--quality auto`。以下任一成立时调到 `high`：

- 图像任一边**超过 2560×1440**。
- 包含**密集或小字号文字**、多字体排印，或字符保真度重要的非拉丁文字。
- **特写人像**，微观细节（毛孔、单根发丝、眼睛反光）是观感的一部分。
- 涉及真人或产品的**身份保留编辑**。
- 会印刷，或用于压缩伪影可见的高保真场景。

其余一切——草稿、社媒小图、探索——`auto` 或 `medium` 更快、更便宜、足够好。
