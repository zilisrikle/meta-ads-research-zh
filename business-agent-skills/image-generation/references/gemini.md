# Nano Banana 提示词指南

如何写出能让 Google Gemini 图像模型（`gemini-3.1-flash-image-preview`（Nano Banana）和 `gemini-3-pro-image-preview`（Nano Banana Pro））产出好结果的提示词。基于 Google 官方提示词指南，示例模式取自 cookbook 与一线实践。

> **TL;DR：** 描述场景，不要罗列关键词。以强动词开头。在关键处极度具体（质感、光线、取景）。用正向表述而非反向提示词。用单点修改的对话式追问迭代。

---

## 最重要的一条原则

**描述场景，不要罗列关键词。**

Gemini 的图像模型针对深度语言理解做了调优。叙述性、描述性的句子胜过逗号分隔的关键词链。

| 弱 | 强 |
|---|---|
| `red panda, sticker, kawaii, cute, cel-shading, white background` | `A kawaii-style sticker of a happy red panda wearing a tiny bamboo hat. Cel-shaded with bold, clean outlines. The background must be white.` |
| `cat photo realistic 4k bokeh sunset` | `A close-up portrait of a black cat sitting on a windowsill at golden hour. Captured with an 85mm portrait lens, resulting in a soft, blurred background (bokeh).` |

本指南其余部分基本都是这一条规则的展开。

---

## 提示词公式

Google 官方指南中的五个框架，覆盖了你用 Gemini 会做的大部分事。

### 1. 文生图（text-to-image，无参考图）

```
[Subject] + [Action] + [Location/context] + [Composition] + [Style]
```

来自 Google 的原文示例：

> *"A striking fashion model wearing a tailored brown dress, sleek boots, and holding a structured handbag. Posing with a confident, statuesque stance, slightly turned. A seamless, deep cherry red studio backdrop. Medium-full shot, center-framed. Fashion magazine style editorial, shot on medium-format analog film, pronounced grain, high saturation, cinematic lighting effect."*

### 2. 多模态（multimodal，带参考图）

```
[Reference images] + [Relationship instruction] + [New scenario]
```

> *"Using the attached napkin sketch as the structure and the attached fabric sample as the texture, transform this into a high-fidelity 3D armchair render. Place it in a sun-drenched, minimalist living room."*

参考图上限（总数，主图 + 附加图）：最多 **14 张（Flash，10 张物体 + 4 张人物）/ 11 张（Pro，6 张物体 + 5 张人物）**。

### 3. 实时信息（Google Search grounding）

```
[Source/Search request] + [Analytical task] + [Visual translation]
```

> *"Search for the current weather and date in San Francisco. Use this data to modify the scene (e.g., if raining, make it look grey and rainy). Visualize this in a miniature city-in-a-cup concept embedded within a realistic modern smartphone UI."*

这是 Gemini 独有的——模型可以抓取新鲜事实并渲染出来。适合实时体育回顾、新闻配图、天气可视化。

### 4. 图内文字（含本地化）

```
[Scene] + [Quoted text strings, font, placement] + [Optional language directive]
```

> *"A high-end, glossy commercial beauty shot of a sleek, minimalist nude-colored face moisturizer jar on a warm studio background. The lighting is soft and radiant. Next to the product, render three lines of text with this exact styling: top line `GLOW` in flowing elegant Brush Script; middle line `10% OFF` in heavy blocky Impact; bottom line `Your First Order` in thin minimalist Century Gothic. Then translate the text into Korean and Arabic."*

### 5. 创意总监式 brief

广告战役、编辑工作，以及一切美术指导比技术正确性更重要的场景，把提示词写成像在给摄影师或设计师下 brief。见下文"创意总监词汇"。

---

## 最佳实践

### 以强动词开头

用你想让模型*做*什么来开头：**Generate**、**Create**、**Photograph**、**Render**、**Transform**、**Restyle**、**Compose**。强开头动词能标示操作类型，比以名词短语开头效果更好。

### 极度具体

具体的名词短语每次都胜过泛泛的：

| 泛泛 | 极度具体 |
|---|---|
| Fantasy armor | Ornate elven plate armor, etched with silver leaf patterns, with a high collar and pauldrons shaped like falcon wings |
| Suit jacket | Navy blue tweed jacket with subtle herringbone pattern and leather elbow patches |
| Coffee mug | Minimalist matte-glaze ceramic mug in cream, with a hand-thrown texture and a single thin rust-colored band near the rim |

### 给出上下文与意图

说明*图像的用途*。模型会据此在色调、取景和完成度上做出无数细微决策。

> 弱：*"Create a logo."*
> 强：*"Create a logo for a high-end, minimalist skincare brand. The mark needs to look at home on a small product label and on a billboard."*

### 用正向表述，不用反向提示词（negative prompt）

Gemini 没有独立的反向提示词通道。描述你*想要*什么，而不是要排除什么。

| 弱 | 强 |
|---|---|
| *"a street, no cars, no people"* | *"an empty pedestrian street at dawn"* |
| *"portrait, no smile, not posed"* | *"a candid, contemplative expression"* |

当你确实需要禁止某物时，明确说出来：*"No watermark. No additional text. No people in the background."*

### 把复杂场景拆成步骤

对于要求很多的提示词，写成逐步指令：

> *"Generate a children's book illustration of a fox stargazer. Step 1: Establish a meadow at twilight, with tall grass swaying gently. Step 2: Place a small red fox in the foreground, lying on its back. Step 3: Render the night sky overhead with a swirling Milky Way. Style: soft watercolor, warm color palette, gentle painterly brushwork."*

### 迭代，不要重写

Gemini 支持对话式追问。用单点修改来精调：
- *"Make the lighting warmer."*
- *"Move the subject slightly to the left."*
- *"Restore the original background."*

这比从头重写 200 词的提示词更快、效果更好。

### 文本优先技巧（text-first hack）

对于文字量大的图像，先在对话中把文字生成出来，再要求生成包含这些文字的图像。两次 pass 比试图一次搞定文案和图像更可靠。

---

## 创意总监词汇

cookbook 的"像创意总监一样写提示词"一节是 Gemini 提示词中杠杆最高的部分。四个杠杆：

### 1. 设计光线

| 风格 | 表述 |
|---|---|
| Studio | *"Three-point softbox setup to evenly light the product, soft shadow bottom-right"* |
| Cinematic | *"Chiaroscuro lighting with harsh, high contrast"* |
| Golden hour | *"Golden hour backlighting creating long shadows and warm rim light"* |
| Editorial | *"Soft north-facing window light, slightly cool, with subtle fall-off"* |
| Neon / night | *"Mixed cool fluorescent interior light spilling onto the street, with magenta neon glow from above"* |

### 2. 选择相机、镜头与景深

具体说明图像"拍摄"于*哪种相机*，会极大改变观感：

| 器材 | 美学 |
|---|---|
| GoPro / fisheye | 沉浸感、畸变、运动相机质感 |
| Fujifilm / X100V | 真实胶片色彩科学，温和暖调 |
| 35mm film SLR | 经典新闻摄影质感 |
| Disposable camera | 粗粝、怀旧、生硬闪光 |
| iPhone | 随意、当代、轻微 HDR 过度修正 |
| Phase One / medium format | 高细节编辑风、时装拍摄质感 |

配合镜头词汇：
- `low-angle shot with shallow depth of field (f/1.8)`——英雄感、亲密
- `wide-angle lens`——开阔尺度、环境语境
- `macro lens`——精细质感、近主体细节
- `telephoto compression`——压缩扁平、亲密距离
- `bird's-eye view`——图案感、俯瞰

### 3. 定义调色与胶片

| 氛围 | 表述 |
|---|---|
| Nostalgic / gritty | *"Render as if on 1980s color film, slightly grainy, faded magenta"* |
| Modern / moody | *"Cinematic color grading with muted teal and orange tones"* |
| Editorial / clean | *"Natural color balance, slight cool cast, no aggressive grading"* |
| Warm / homey | *"Warm tungsten color temperature, slight orange shift in shadows"* |

### 4. 强调材质与质感

对任何产品或人像提示词最可靠的一次升级：明确点出材质。

- *"navy blue tweed"* 而不是 *"jacket"*
- *"ornate elven plate armor, etched with silver leaf"* 而不是 *"armor"*
- *"matte-glaze ceramic mug with hand-thrown texture"* 而不是 *"coffee mug"*
- *"weathered linen with subtle creasing"* 而不是 *"shirt"*

对于人像，点名**皮肤质感**：*"visible skin texture and micro pores"*、*"natural skin imperfections"*、*"no plastic skin"*——这些表述能避免过度磨皮的塑料娃娃观感。

---

## 文字渲染

Gemini 在图像内渲染文字异常出色，包括非拉丁文字。官方指南：

1. **引用字面文字**：`"Happy Birthday"`、`"URBAN EXPLORER"`。引号标示什么是 *指令*、什么是 *字符串*。
2. **指定字体**：字族、字重、字号、颜色。*"Bold, white, sans-serif font"* 或 *"Century Gothic 12px"*。
3. **本地化**：用英文写提示词；指定渲染文字的目标语言。*"Render this in Japanese"*、*"Translate the text into Korean and Arabic"*。
4. **复杂文字设计用文本优先技巧**：先在对话中让 Gemini 起草文案，再要求生成带这些文字的图像。

要获得最佳文字保真度（长文案、多字体排版、小字号说明文字），用 **Pro**（`gemini-3-pro-image-preview`）。

### 以排印为主题

Gemini 擅长以文字为*视觉焦点*的构图：

> *"A typographic poster with a solid black background. Bold letters spell `NEW YORK`, filling the center of the frame. The text acts as a cut-out window — a photograph of the New York skyline is visible only inside the letterforms."*

其他模式：由自然元素（树叶、烟雾、水）构成的文字、环绕物体的文字、带重叠色块的分层排印。

---

## 图像编辑

### 内补绘制（inpainting，语义蒙版）

你不需要给 Gemini 显式蒙版文件——用语言描述区域，并告诉它其余部分不要动：

> *"Using the provided image of a living room, change only the blue sofa to a vintage brown leather Chesterfield. Keep the wall color, art on the walls, rug, and lighting exactly as they are."*

对话式蒙版对中大型区域效果很好。像素级精确编辑则改用 OpenAI 脚本的 `--mask` 参数。

### 添加或移除元素

> *"Using the provided image of my cat, add a small knitted wizard hat on its head. Match the original lighting, perspective, and image grain."*

> *"Remove the man from the background of this photo. Reconstruct the wall and shelving naturally behind where he was."*

### 风格迁移（style transfer）

精确指定目标风格（艺术家、流派、时代），以及保持不变的部分（构图、主体身份）。

> *"Transform the provided photograph into the style of Vincent van Gogh's `Starry Night`. Render all elements with swirling, impasto brushstrokes and a deep blue and yellow palette. Preserve the composition and subject identity exactly."*

### 多图合成

> *"Create a professional e-commerce fashion photo. Take the blue floral dress from Image 1 and have the woman from Image 2 wear it. Match the studio lighting from Image 1."*

明确说明哪张图控制哪个维度（主体、服装、光线、背景）。

### 跨视角的人物一致性

要做 360° 转体或多姿态 sheet，迭代生成——把上一张输出作为下一角度的参考图。每步说明想要的姿态：

> *"Using the previous image as a reference for the character's appearance, generate the same character from a side profile, three-quarter view from the right."*

人物工作建议用 **Pro**（人物参考图上限 5 张，Flash 是 4 张）。

### 高保真细节保留

编辑真人或品牌产品时，逐项列出不得改变的内容：

> *"Ensure the woman's face, facial features, and skin tone remain completely unchanged. The brand logo on the bottle must remain pixel-perfect."*

每次迭代都重复这份保留清单；否则漂移（drift）会迅速累积。

---

## 常见陷阱

| 陷阱 | 症状 | 解决方法 |
|---|---|---|
| 关键词链 | 通用、"图库照片"观感 | 改写成描述场景的句子 |
| `--no` 式反向 | 不想要的元素仍出现，或反而引来注意 | 用正向表述——描述你想要什么 |
| 模糊的主体 | 对称、摆拍、直视镜头的默认构图 | 明确指定视线、姿态、比例、取景 |
| 塑料 / 过度磨皮的脸 | 主体看起来像 AI 生成的 | 加上 *"visible skin texture, natural imperfections, no plastic skin"* |
| 一次要求太多 | 部分要求被忽略 | 用逐步指令，或拆成多次迭代 |
| 渲染的文字不对 | 文案乱码或重复 | 引用字面文字、指定字体、文本优先生成、切换到 Pro |
| 编辑时主体漂移 | 每次迭代身份都在变 | 每轮重述保留清单 |
| 试图用详细相机参数 | 参数被忽略或弱应用 | 改用构图 / 观感语言（见"创意总监词汇"） |

---

## 按使用场景的模板

### 写实人像

```
Generate a photorealistic [shot type] of [hyper-specific subject description].
[Setting and time of day], [environmental detail].
Captured with an [85mm portrait lens / 35mm prime], [depth of field], [lighting setup].
Visible skin texture, natural imperfections, no plastic skin, no heavy retouching.
Color grading: [palette/mood].
The image should feel [tone — honest / candid / editorial / dramatic].
```

### 产品样机 / 商业

```
Photograph a [hyper-specific product description] on [background].
[Three-point softbox / natural window / dramatic side] lighting, [shadow direction].
[Lens and angle].
Highlight the [material / finish / surface detail].
Catalog-quality finish, sharp focus throughout.
[Optional: text or label requirements with quoted strings.]
```

### Logo / 极简标识

```
Create a [logo type — wordmark / emblem / monogram] for [brand description and audience].
[Style direction — minimalist / vintage / playful / luxury].
Typography: [font family and weight].
Color palette: [primary] and [optional secondary].
The logo should work at both small (favicon) and large (signage) sizes.
Background: [solid color]. No additional text or decoration.
```

### 贴纸 / 插画

```
Create a [style — kawaii / flat-vector / cel-shaded] sticker of [hyper-specific subject].
Bold, clean outlines. [Color palette].
The background must be white. (Transparent backgrounds are not supported — request a solid white background and remove it in post if needed.)
```

### 编辑 / 时装

```
Photograph [subject] [pose and demeanor].
[Outfit, hair, props — be hyper-specific about materials].
[Backdrop and setting], [lighting].
Shot on [medium-format analog / 35mm / Fujifilm], [grain/grading].
[Magazine-style descriptor — "fashion magazine style editorial", "campaign image", etc.].
[Composition — medium-full / centered / off-center / rule-of-thirds].
```

### 连环画 / 分镜

```
Make a [N]-panel comic in a [art style — gritty noir / soft watercolor / Studio Ghibli pastel] style.
Panel 1: [scene and action].
Panel 2: [scene and action].
Panel 3: [scene and action].
Maintain [character / palette / line-weight] consistency across all panels.
```

### 极简 / 留白

```
A minimalist composition featuring [single subject] positioned in the [bottom-right / top-left / etc.].
Vast, empty [off-white / cream / charcoal] canvas dominates the frame.
[Subject details — material, color, scale relative to canvas].
[Soft directional lighting].
Designed to leave room for [text overlay / branding] in the empty area.
```

### 多图合成

```
Image 1: [description].
Image 2: [description].
[Image N: ...]

[Strong verb: Combine / Compose / Place / Apply]: [specific instruction about how to combine].
Match [lighting / palette / style] from Image [N].
Keep [identity / geometry / brand element] from Image [N] unchanged.
```

---

## 宽高比与分辨率

支持的宽高比：`1:1`、`2:3`、`3:2`、`3:4`、`4:3`、`4:5`、`5:4`、`9:16`、`16:9`、`21:9`。**Flash 还支持** `1:4`、`4:1`、`1:8`、`8:1`，用于超宽 / 超高构图。

分辨率：`512px`、`1K`、`2K`、`4K`。用大写 `K`（小写 `k` 会被拒绝）。

---

## 何时用 Pro

默认用 Flash（`gemini-3.1-flash-image-preview`）。以下情况切换到 Pro（`gemini-3-pro-image-preview`）：

- **密集或小字号文字**——多字体海报、信息图说明、字符保真度重要的非拉丁文字。
- **特写人像**，微观细节（毛孔、单根发丝、眼睛反光）是观感关键。
- 涉及真人或品牌产品的**身份敏感编辑**。
- **人物一致性工作**（Pro 支持 5 张人物参考图，Flash 是 4 张）。
- **4K** 或用于印刷的任何内容。
- **要求多、约束长的复杂提示词**——Pro 的推理更可靠。

日常工作——草稿、社媒小图、探索、单主体场景——Flash 更快、更便宜、足够好。
