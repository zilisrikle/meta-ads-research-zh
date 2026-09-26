# Veo 3.1 提示词指南（Prompt Guide，详细参考）

如何写出能从 Google 的 Veo 3.1 获得出色结果的提示词。提示词用英文写——模型为此做了调优。

> **核心原则：细节 = 控制（Core principle: detail = control）。** 提示词越具体，你对结果的控制就越多。泛泛的输入只能得到泛泛的输出。本指南是一份长长的清单，列出你可以具体化的各个维度。

---

## 目录（Table of contents）

1. [五要素公式（The 5-element formula）](#五要素公式the-5-element-formula)
2. [主体描述（Subject specification）](#主体描述subject-specification)
3. [动作与运动（Action and motion）](#动作与运动action-and-motion)
4. [摄影指导（Cinematography）](#摄影指导cinematography)
5. [光影与氛围（Lighting and atmosphere）](#光影与氛围lighting-and-atmosphere)
6. [视觉风格（Visual style）](#视觉风格visual-style)
7. [氛围质感（Ambiance）](#氛围质感ambiance)
8. [时间元素（Temporal elements）](#时间元素temporal-elements)
9. [电影技巧（Cinematic techniques）](#电影技巧cinematic-techniques)
10. [音频指导（Audio direction：dialogue、SFX、ambient）](#音频指导audio-directiondialoguesfxambient)
11. [负向提示词（Negative prompts）](#负向提示词negative-prompts)
12. [时间戳提示（Timestamp prompting）](#时间戳提示timestamp-prompting)
13. [高级工作流（Advanced workflows）](#高级工作流advanced-workflows)
14. [迭代式提示词优化（Iterative prompt refinement）](#迭代式提示词优化iterative-prompt-refinement)
15. [各用例模板（Templates by use case）](#各用例模板templates-by-use-case)
16. [最佳实践速查表（Best-practices cheat sheet）](#最佳实践速查表best-practices-cheat-sheet)

---

## 五要素公式（The 5-element formula）

写出结构良好的视频提示词的基础结构。按大致顺序组合五个要素：

```
[Cinematography] + [Subject] + [Action] + [Context] + [Style & Ambiance]
```

| 要素（Element） | 涵盖内容（What it covers） | 示例（Examples） |
|---|---|---|
| **Cinematography** | 运镜、构图（Camera work, framing） | `Medium shot`、`Crane shot`、`Slow pan` |
| **Subject** | 主要主体（The main subject） | `a tired corporate worker`、`a golden retriever` |
| **Action** | 发生了什么 / 如何运动（What's happening / how it moves） | `rubbing his temples`、`running through a meadow` |
| **Context** | 场景、环境（Setting, environment） | `in a cluttered office late at night`、`on a sunlit beach` |
| **Style & Ambiance** | 情绪、光线、年代、观感（Mood, lighting, era, look） | `Retro aesthetic, shot on 1980s color film, slightly grainy` |

### 实践示例（Worked example，official）

```
Medium shot, a tired corporate worker, rubbing his temples in exhaustion,
in front of a bulky 1980s computer in a cluttered office late at night.
The scene is lit by the harsh fluorescent overhead lights and the green
glow of the monochrome monitor. Retro aesthetic, shot as if on 1980s
color film, slightly grainy.
```

接下来的章节将逐一深入公式中的每个要素。

---

## 主体描述（Subject specification）

写具体。细节主导 Veo 的行为——泛泛的名词只能产生泛泛的结果。

| 通用说法（Generic） | 具体说法（Specific） |
|---|---|
| `a man` | `a seasoned detective in his late 50s with grey stubble and a worn trench coat` |
| `a dog` | `a playful Golden Retriever puppy with damp fur` |
| `a building` | `a brutalist concrete library with vertical slit windows, weather-stained` |
| `a woman` | `a woman in her twenties with wavy brown hair and light freckles` |

### 多个主体（Multiple subjects）

分别描述每个主体及其相互关系。示例（Example）：

> *"A group of diverse friends laughing around a campfire while a curious fox watches from the shadows."*

### 超具体（当极端细节重要时）（Hyper-specific (when extreme detail matters)）

Veo 奖励极度详细的主体描述。官方逐字示例（Verbatim official example）：

> *"A hyper-realistic, cinematic portrait of a wise, androgynous shaman of indeterminate age. Their weathered skin is etched with intricate, bioluminescent circuit-like tattoos that pulse with a soft, cyan light. They are draped in ceremonial robes woven from dark moss and shimmering, metallic fiber-optic threads. Their expression is serene and ancient, eyes holding a deep, knowing look."*

当角色会出现在多个镜头中时，在时间戳提示的**第一个**片段或参考图像中锁定描述，之后的片段用 "the detective" / "the shaman" 回指。

---

## 动作与运动（Action and motion）

动作是视频的动词。需要指明：

| 类别（Category） | 示例（Examples） |
|---|---|
| **基本运动（Basic movement）** | walking, flying, dancing, running, climbing |
| **互动（Interactions）** | talking, hugging, cooking, fighting, shaking hands |
| **情绪表达（Emotional expressions）** | smiling, frowning, appearing thoughtful, eyes welling |
| **细微动作（Subtle actions）** | a breeze ruffling hair, a gentle nod, fingers tapping |
| **转变（Transformations）** | a flower blooming in fast-motion, ice melting, a wall crumbling |

### 编排动作示例（Choreographed-action example，verbatim）

> *"A gloved hand carefully slices open the spine of an ancient, leather-bound book with a scalpel. The hand then delicately extracts a tiny, metallic data chip hidden within the binding."*

对于快节奏动作序列，要一步一步写编排。模糊的动作（"they fight"）要写成 "they exchange three rapid blows, the second one connects, she stumbles backward, recovers, charges again." 这种细节级别是 Veo 让动作看起来有意图所需要的。
---

## 摄影指导（Cinematography）

### 运镜（Camera movement）

| 术语（Term） | 效果（Effect） |
|---|---|
| `Static camera` | Camera fixed; dialogue, stable observation（机位固定；对话、稳定观察） |
| `Dolly in / Dolly out` | Camera physically moves toward / away from subject; intimacy or release（机位实际向主体推进 / 拉开；亲近感或释放感） |
| `Tracking shot` | Camera moves laterally with the subject; motion（机位随主体横向移动；运动感） |
| `Truck left / Truck right` | Sideways camera movement at fixed distance（固定距离的横向移动） |
| `Pan` / `Tilt` | Horizontal / vertical camera rotation（水平 / 垂直旋转机位） |
| `Slow pan` | Calm, deliberate horizontal sweep（平静、从容的水平扫视） |
| `Whip pan` | Extremely fast pan; transition between scenes（极快摇摄；场景间转场） |
| `Crane shot` | Large vertical camera movement; scale, grandeur（大幅垂直运镜；规模感、壮阔感） |
| `Aerial view` / `Drone shot` | High-altitude perspective; landscape, location reveal（高空视角；风景、地点揭示） |
| `Arc shot` | Circular camera movement around subject（绕主体环绕运镜） |
| `POV shot` | First-person view; immersion（第一人称视角；沉浸感） |
| `Handheld` | Slight handheld shake; documentary feel, urgency（轻微手持晃动；纪实感、紧迫感） |

#### 摇臂镜头示例（Crane shot example，official verbatim）

> *"Crane shot starting low on a lone hiker and ascending high above, revealing they are standing on the edge of a colossal, mist-filled canyon at sunrise, epic fantasy style, awe-inspiring, soft morning light."*

### 构图与景别（Framing and shot size）

| 术语（Term） | 说明（Description） |
|---|---|
| `Wide shot` / `Establishing shot` | Full environment; sets the location（完整环境；交代地点） |
| `Medium shot` | Waist-up; the default for conversation（腰部以上；对话默认景别） |
| `Two-shot` | Two characters in the same frame（两个角色同框） |
| `Over-the-shoulder` | Frame from behind one character; conversation reverse（从一角色身后取景；对话反打） |
| `Close-up` | Face fills the frame; expression（脸部充满画面；表情） |
| `Extreme close-up` | Eye, hand, single detail（眼睛、手、单个细节） |
| `Low angle` | Looking up; power, intimidation（仰视；力量、压迫感） |
| `High angle` | Looking down; vulnerability, isolation（俯视；脆弱、孤立） |
| `Bird's-eye view` | Straight-down map perspective（垂直俯视的地图视角） |
| `Top-down shot` | Cooking, work surfaces, choreography（烹饪、工作台面、编排） |

### 镜头与对焦（Lens and focus）

| 术语（Term） | 效果（Effect） |
|---|---|
| `Shallow depth of field` | Background blur; isolates subject（背景虚化；突出主体） |
| `Deep focus` | Everything sharp; environmental detail（全画面清晰；环境细节） |
| `Wide-angle lens` | Wide field of view; spaciousness, distortion at edges（宽视野；开阔感、边缘畸变） |
| `Telephoto compression` | Compresses depth; intimate, flattened（压缩纵深；亲近、扁平化） |
| `Macro lens` | Tiny subjects rendered large（微小主体放大呈现） |
| `Soft focus` | Gentle, dreamlike feel（柔和、梦幻感） |
| `Lens flare` | Light catching the lens; drama（光线掠过镜头；戏剧性） |
| `Rack focus` | Focus shifts between two planes during the shot（镜头内焦点在两个平面间切换） |
| `Fisheye distortion` | Extreme barrel distortion; surreal, action-cam（极端桶形畸变；超现实、运动相机感） |
| `Dolly zoom` (vertigo effect) | Zoom and physical movement in opposite directions; disorienting background warp（变焦与机位移动反向；眩晕的背景扭曲） |

#### 浅景深示例（Shallow DOF example，official verbatim）

> *"Close-up with very shallow depth of field, a young woman's face, looking out a bus window at the passing city lights with her reflection faintly visible on the glass, inside a bus at night during a rainstorm, melancholic mood with cool blue tones, moody, cinematic."*

---

## 光影与氛围（Lighting and atmosphere）

| 术语（Term） | 情绪（Mood） |
|---|---|
| `Golden hour` | Warm sunrise / sunset; intimate, hopeful（温暖的日出 / 日落；亲密、希望） |
| `Blue hour` | Just-after-sunset cool light; quiet, transitional（日落后不久的冷色光；安静、过渡） |
| `Natural lighting` | Realistic, untreated（真实、不加修饰） |
| `Soft morning sunlight` | Calm, gentle（平静、柔和） |
| `Harsh midday sun` | Stark, exposed（刺眼、暴露感） |
| `Dramatic spotlight` | Theatrical, single hard source（戏剧化、单一硬光源） |
| `Volumetric rays` | Light beams visible through atmosphere（穿过空气的光束） |
| `Backlit` / `Silhouette` | Light behind the subject; drama（主体背光；戏剧性） |
| `Rembrandt lighting` | Classical portrait lighting; one cheek lit, triangle on the other（古典肖像布光；一侧脸颊受光、另一侧呈三角光斑） |
| `High-key lighting` | Bright, low-contrast; clean, optimistic（明亮、低对比；干净、乐观） |
| `Low-key lighting` | Dark, high-contrast; suspense, noir（黑暗、高对比；悬疑、黑色电影感） |
| `Neon glow` | Cyberpunk, urban night（赛博朋克、城市夜晚） |
| `Harsh fluorescent` | Office, hospital, convenience store（办公室、医院、便利店） |
| `Dappled sunlight` | Sunlight through leaves（穿过树叶的斑驳阳光） |
| `Warm tones` / `Cool blue tones` | Intimacy vs isolation（亲密感 vs 疏离感） |

### 情绪词汇（Tone and mood vocabulary）

`happy` / `joyful`, `sad` / `melancholic`, `suspenseful` / `tense`, `peaceful` / `serene`, `epic` / `grandiose`, `futuristic`, `vintage` / `retro`, `romantic`, `horror`, `dreamlike`, `austere`, `intimate`, `claustrophobic`, `expansive`.

---

## 视觉风格（Visual style）

视觉风格是独立于光影的维度。先确定外观，再把提示词的其余部分叠加上去。

| 模式（Mode） | 措辞（Phrasing） |
|---|---|
| **Photorealistic** | `photorealistic, cinematic film look, shot on 35mm` |
| **Animation — anime** | `Japanese anime style, cel shading, vivid colors` |
| **Animation — Pixar** | `Pixar-style 3D animation, soft surfaces, expressive characters` |
| **Animation — claymation** | `claymation, visible thumbprints, slight imperfections` |
| **Animation — flat / 2D** | `flat 2D vector animation, bold outlines, limited palette` |
| **Animation — cel-shaded** | `cel-shaded animation, hard shadow lines, bold colors` |
| **Art movements** | `Van Gogh style, swirling impasto`, `Surrealist style`, `Impressionistic, soft brushstrokes` |
| **Specific looks** | `gritty graphic novel`, `watercolor painting, washes and bleeds`, `blueprint schematic, white lines on dark blue`, `1980s VHS aesthetic, scan lines and tracking errors`, `film noir, deep shadows, venetian blind shadows` |

### 视觉风格示例（Visual-style example，verbatim）

> *"A dynamic scene in a vibrant Japanese anime style. A magical girl with silver hair and glowing blue eyes walks in a forest."*

任何提示词的**第一个决策**都是你处于哪种模式。写实词汇（镜头、颗粒、调色）用在黏土动画提示词里是浪费的；动漫词汇（赛璐珞着色、鲜艳色彩）用在写实提示词里也是浪费的。选好赛道，再装备正确的词汇。

---

## 氛围质感（Ambiance）

文本与氛围层。三个子维度：

### 颜色色板（Color palette）

`monochromatic`, `vibrant tropical`, `muted earthy`, `cool futuristic blues and cyans`, `warm autumn oranges and ochres`, `pastel cottagecore`, `cyberpunk neon magenta and electric blue`, `noir black-and-white with a single accent color`.

### 氛围效果（Atmospheric effects）

`fog`, `mist rising from water`, `desert heat haze`, `falling snow`, `swirling embers`, `glowing particles in the air`, `dust motes in sunlight`, `rain streaking the lens`, `steam rising from a manhole`.

### 质感（Textural quality）

`rough hewn stone`, `polished chrome reflecting the environment`, `soft fabric with visible weave`, `dewdrops on a leaf`, `cracked dry earth`, `wet asphalt`, `weathered wood with grain visible`, `peeling paint`, `iridescent oil-slick surfaces`.

这三个维度可以叠加到任何视觉风格上。一个场景可以是 `claymation + warm autumn palette + falling leaves + soft fabric`，也可以是 `photorealistic + cool blue futuristic palette + glowing particles + polished chrome`。

---

## 时间元素（Temporal elements）

Veo 可以控制片段内的时间流。

| 术语（Term） | 效果（Effect） |
|---|---|
| `slow-motion` | Action in fluid extreme slow motion（流畅的极慢动作） |
| `fast-paced` / `quick cuts` | High-energy, action-driven（高能量、动作驱动） |
| `time-lapse` | Sun moving across the sky, traffic accelerating（太阳划过天空、车流加速） |
| `Hyperlapse` | Time-lapse with a moving camera（移动机位的延时） |
| `Real-time` | Default; one-second-per-second（默认；一秒对一秒） |
| `Stop motion` | Frame-by-frame stutter aesthetic（逐帧顿挫美学） |
| `Pulsating rhythm` | Light or motion synchronized to a beat（光或运动与节拍同步） |
| `Evolution` | A flower bud unfurling, a candle burning down（花苞绽放、蜡烛燃尽） |

### 延时示例（Time-lapse example，verbatim）

> *"A time-lapse of a bustling city skyline as day transitions to night. The camera is static. Watch as the sun sets, casting long shadows across the buildings, and the city lights begin to twinkle on, with car headlights creating streaks of light on the streets below."*

---

## 电影技巧（Cinematic techniques）

明确点名时 Veo 能理解的剪辑与构图手法。

| 术语（Term） | 效果（Effect） |
|---|---|
| `Match cut` | Visual or thematic continuity across a cut (e.g. coffee cup → planet)（剪辑点两侧的视觉或主题连续，例如咖啡杯 → 星球） |
| `Jump cut` | Sharp temporal cut within the same composition; energy or ellipsis（同一构图内的锐利时间跳跃；能量感或省略） |
| `Establishing shot sequence` | Wide → medium → close-up progression（远 → 中 → 近的推进） |
| `Montage` | Series of brief shots compressing time or theme（压缩时间或主题的一系列短镜头） |
| `Split diopter effect` | Foreground and background simultaneously sharp（前景与背景同时清晰） |
| `Dutch angle` | Tilted horizon; instability, unease（倾斜地平线；不稳定、不安） |
| `Whip pan transition` | Fast pan used as a cut（用作剪辑的快速摇摄） |

### 跳切示例（Jump-cut example，verbatim）

> *"A person sitting in the same position but wearing different outfits, with sharp jump cuts between each outfit change."*
---

## 音频指导（Audio direction：dialogue、SFX、ambient）

Veo 3.1 根据提示词线索生成与视频同步的音频。三种音频元素组合在一起。

### 对白（Dialogue）

对话用引号括起。为增加细腻度，可加上说话者的语气、口音或表达方式。

```
A woman says, "We have to leave now."
The detective looks up and says in a weary voice, "Of all the offices in this town, you had to walk into mine."
A polished British accent speaks in a serious, urgent tone: "Your story has holes."
She replies with a slight smile, "You were highly recommended."
```

说明（Notes）：
- 对话始终用 `" "` 括起。
- 加上情绪、语气或口音（`in a weary voice`、`whispered`、`voice tight with fear`、`in a polished British accent`）。
- 同一提示词中出现多个说话者也可以——点名或按描述指代即可。

### 音效（Sound effects，SFX）

把具体音效写出来。以 `SFX:` 开头可让意图明确无误。

```
SFX: thunder cracks in the distance
SFX: glass shattering on the floor
Engine roaring loudly. Tires screeching on wet pavement.
The rustle of dense leaves, distant exotic bird calls.
SFX: a creaking door, a ticking clock.
```

### 环境音（Ambient）

设定环境音频底。以 `Ambient:` 开头更清晰。

```
Ambient: the quiet hum of a starship bridge
Ambient: the distant chatter of a busy café
Ambient: city traffic and distant sirens
Upbeat background music plays softly.
A swelling, gentle orchestral score begins to play.
```

### 完整音频示例（Full audio example）

```
Misty Pacific Northwest forest, two exhausted hikers discover fresh claw marks
on a tree. The man turns to the woman and says, "That's no ordinary bear."
The woman replies, voice tight with fear, "Then what is it?"
SFX: rough bark scraping, snapping twigs. Ambient: a lone bird chirps in the
distance, wind through pine needles.
```

### 把声音设计当作创作工作（Sound design as creative work）

音频不是事后诸葛亮——要像电影作曲家一样，有意地把它和画面配对。一张宁静的草甸航拍镜头，只要把 `Ambient: gentle wind, birdsong` 换成 `Ambient: low rumbling drone, distant crows` 就会变得不祥。用音频层独立于画面设定情绪。

---

## 负向提示词（Negative prompts）

### 用名词 / 形容词列表，而非命令（Use noun / adjective lists, not commands）

负向提示词是排除元素的列表——写成名词或形容词，而非命令。

| 不要这样写（Don't write） | 这样写（Write） |
|---|---|
| `Don't include walls` | `walls, urban structures` |
| `No blurry images` | `blurry, out of focus` |
| `Remove all text` | `text, watermark, subtitle` |

### 尽量用正向描述（Frame exclusions positively when you can）

对于某些内容，更干净的做法是描述你想要的*正向*版本，而非否定不需要的版本：

| 负向写法（Negative-style） | 正向写法（Positive-style） |
|---|---|
| `no buildings, no roads, no man-made structures` | `a desolate landscape, untouched wilderness, raw nature` |
| `no people, no cars` | `an empty pedestrian street at dawn` |

把 `--negative-prompt` 用在那些反复混入的东西上（文字叠加、水印、模糊、抖动的机位），用正向提示词定义场景本身。

### 通用画质提升负向词（Default quality-improving negatives）

一套适用于大多数片段的通用排除词：

```
blurry, low quality, distorted faces, text overlay, watermark, shaky camera, artifacts
```

按风格区分：
- **写实（Photorealistic）** — `cartoon, anime, 3D render, CGI look, oversaturated`
- **动画 / 插画（Animated / illustrated）** — `photorealistic, live action, natural skin texture`
- **产品（Product）** — `people, hands, text, logo, cluttered background`

---

## 时间戳提示（Timestamp prompting）

在一个 8 秒片段中，可以用时间戳标签安排多个镜头。这样一次生成就能装下多次剪辑——序列工作的强大技巧。

### 语法（Syntax）

```
[00:00-00:02] Description of shot 1
[00:02-00:04] Description of shot 2
[00:04-00:06] Description of shot 3
[00:06-00:08] Description of shot 4
```

### 示例：探险家序列（Example: explorer sequence，verbatim）

```
[00:00-00:02] Medium shot from behind a young female explorer with a leather
satchel and messy brown hair in a ponytail, as she pushes aside a large jungle
vine to reveal a hidden path.
[00:02-00:04] Reverse shot of the explorer's freckled face, her expression filled
with awe as she gazes upon ancient, moss-covered ruins in the background.
SFX: The rustle of dense leaves, distant exotic bird calls.
[00:04-00:06] Tracking shot following the explorer as she steps into the clearing
and runs her hand over the intricate carvings on a crumbling stone wall.
Emotion: Wonder and reverence.
[00:06-00:08] Wide, high-angle crane shot, revealing the lone explorer standing
small in the center of the vast, forgotten temple complex, half-swallowed by
the jungle. SFX: A swelling, gentle orchestral score begins to play.
```

### 小贴士（Tips）

- 在各片段间变换摄影手法，给片段节奏感。
- SFX 和音频线索可以按片段放置。
- 第一个片段详细描述角色外观；后面的片段可以回指（"the explorer"、"she"）。
- 8 秒 4 镜头的序列最典型。2 镜头和 3 镜头序列也适用于更慢的节奏。

---

## 高级工作流（Advanced workflows）

### 工作流 1：首尾帧插值（Workflow 1: First and Last Frame interpolation）

提供起始图和结束图；Veo 在两者之间插值生成运动。适用于大运镜、变换和昼夜转场。

1. 生成起始图（用单独的图像生成工具）。
2. 生成结束图。
3. 运行 Veo 首尾帧模式桥接两者。

官方逐字示例（Verbatim official example）：

```
The camera performs a smooth 180-degree arc shot, starting with the
front-facing view of the singer and circling around her to seamlessly
end on the POV shot from behind her on stage. The singer sings "when
you look me in the eyes, I can see a million stars."
```

### 工作流 2：配料式生成视频（角色 / 素材统一）（Workflow 2: Ingredients to Video (character / asset consistency)）

提供参考图像作为"配料"，使生成片段保留特定角色、产品或环境的外观。在提示词中按描述引用这些输入。

1. 生成角色、产品、环境的参考图像。
2. 写场景提示词并引用这些输入。

官方逐字示例（Verbatim official example）：

```
Using the provided images for the detective, the woman, and the office
setting, create a medium shot of the detective behind his desk. He looks
up at the woman and says in a weary voice, "Of all the offices in this
town, you had to walk into mine."
```

对于多镜头系列，用同一组参考素材做多次生成，每个镜头配略有不同的场景提示词。参考素材让外观在剪辑之间保持恒定。

### 工作流 3：图像生成视频最佳实践（Workflow 3: Image-to-Video best practices）

- 选择已最接近期望最终效果的静态图。
- 在提示词中**描述运动和变化**——不要复述图像里已有的内容。
- 当脸部应保持突出时，在提示词中加 `portrait`。
- 产品类：相比快速运镜，更推荐 `slow rotation`、`gentle parallax`、`subtle camera push-in`——它们能保持产品外观完整。

### 工作流 4：物体添加 / 删除（Veo 2 模式）（Workflow 4: Object add / remove (Veo 2 mode)）

Veo 2 支持在已有视频中添加或删除特定物体。**注意：Veo 2 生成不带音频。** 适用于清理背景，或向已完成的素材中插入特定元素。

本 skill 打包的脚本针对 Veo 3.1（含音频）；Veo 2 的物体添加/删除未在此接入。需要该能力时请使用 Vertex AI 控制台或直接调用 Veo 2 API。

---

## 迭代式提示词优化（Iterative prompt refinement）

不要指望一次写出完美提示词。逐层叠加细节。

### 第 1 步：骨架（subject + action）（Step 1: bare bones）

```
A woman walking on a beach.
```

### 第 2 步：加具体性（表情、气质）（Step 2: add specificity）

```
A woman walking along a beach, content and relaxed, looking toward the
horizon at sunset.
```

### 第 3 步：加摄影（Step 3: add cinematography）

```
Tracking shot, a woman walking along a beach, content and relaxed,
looking toward the horizon at sunset.
```

### 第 4 步：加氛围与风格（Step 4: add atmosphere and style）

```
Tracking shot, a woman walking along a beach, content and relaxed,
looking toward the horizon at sunset. Warm golden light, gentle waves
lapping at her feet, shallow depth of field. Cinematic, shot on 35mm
film with slight grain.
```

### 第 5 步：加音频 + 负向词（Step 5: add audio + negatives）

```
Tracking shot, a woman walking along a beach, content and relaxed,
looking toward the horizon at sunset. Warm golden light, gentle waves
lapping at her feet, shallow depth of field. Cinematic, shot on 35mm
film with slight grain. Ambient: gentle waves, distant seagulls, soft
wind.
```
+ `--negative-prompt "text, watermark, blurry, shaky camera"`

### 用聊天模型增强简易提示词（Use a chat model to enhance simple prompts）

来自 Google 的一个实用元技巧：**让 Gemini 聊天模型把一句话提示词改写成完整的电影感提示词**，再送给 Veo。把草稿粘贴进 Gemini，问 "rewrite this as a detailed Veo 3.1 prompt with cinematography, lighting, and audio direction." 输出通常是可进一步编辑的强起点。

---

## 各用例模板（Templates by use case）

### 产品展示（Product showcase）

```
[Cinematography]: Slow dolly shot circling around the product
[Subject]: [product name / category]
[Context]: Clean white studio with soft gradient lighting
[Style]: Hyper-realistic, product photography aesthetic, shallow depth of field
[Audio]: Ambient: subtle, elegant electronic music
```

示例（Example）：

```
Slow dolly shot circling around a sleek matte-black wireless headphone
on a reflective surface. Clean white studio with soft gradient lighting
from above. Hyper-realistic, product photography aesthetic, shallow
depth of field highlighting the premium materials. Ambient: subtle,
elegant electronic music.
```

### 竖屏短视频社交（9:16）（Vertical short-form social (9:16)）

```
[Cinematography]: Dynamic handheld or POV shot
[Subject]: [subject]
[Action]: Energetic, attention-grabbing movement
[Style]: Vibrant colors, fast-paced, trending aesthetic
```

### 电影感风景（Cinematic landscape）

```
[Cinematography]: Aerial drone shot or crane shot
[Subject]: [type of landscape]
[Context]: Time of day, weather, season
[Style]: Cinematic, epic scale, natural color grading
[Audio]: Ambient soundscape + orchestral score
```

示例（Example）：

```
Aerial drone shot following a winding river through a dense autumn
forest. The trees are ablaze with red, orange, and gold leaves.
Morning mist rises from the water surface. Cinematic, epic scale,
natural color grading. Ambient: flowing water, rustling leaves.
A gentle orchestral score swells.
```

### 对白场景（Dialogue scene）

```
[Cinematography]: Medium shot / two-shot / over-the-shoulder
[Subject]: Characters with specific appearance details
[Action]: Dialogue with emotional cues
[Context]: Setting that reinforces the mood
[Audio]: Quoted dialogue with tone + ambient sounds
```

### 审讯场景（Interrogation scene，verbatim official example）

```
A medium shot in a dimly lit interrogation room. The seasoned detective
says: "Your story has holes." The nervous informant, sweating under a
single bare bulb, replies: "I'm telling you everything I know." Ambient:
the low hum of an air conditioner, the occasional drip of water.
```

### 烹饪 / 食谱片段（Cooking / recipe clip）

```
[Cinematography]: Top-down shot / close-up / slow motion
[Subject]: Food preparation steps
[Context]: Kitchen setting with specific lighting
[Style]: Warm, appetizing color grading
[Audio]: SFX of cooking (sizzling, chopping, pouring)
```

### 多镜头叙事（时间戳提示）（Multi-shot narrative (timestamp prompting)）

```
[00:00-00:02] [Shot 1: setup — wide or establishing shot, set the scene]
[00:02-00:04] [Shot 2: subject closer — emotion, reaction]
[00:04-00:06] [Shot 3: action — what's happening]
[00:06-00:08] [Shot 4: payoff — wide reveal, emotional resolution, or punchline]
```

让每个片段的镜头类型不同，这样片段才有节奏。

---

## 最佳实践速查表（Best-practices cheat sheet）

起草时快速参考：

1. **细节 = 控制（Detail = control）。** 泛泛的输入 → 泛泛的输出。细节主导结果。
2. **用五要素结构（Use the 5-element structure）。** Cinematography + Subject + Action + Context + Style/Ambiance。
3. **先指明摄影（Specify cinematography first）。** 它是决定基调的最强单一维度。
4. **先选定视觉风格赛道（Pick a visual-style lane up front）**（photorealistic、anime、claymation 等）。词汇因赛道而异。
5. **明确定义音频（Define audio explicitly）**——对话加引号，`SFX:` 和 `Ambient:` 标清楚。
6. **用时间戳提示（Use timestamp prompting）**在一个 8 秒片段中做多镜头序列。
7. **能正向就正向（Frame negatives positively）**；用 `--negative-prompt` 对付顽固的不想要元素。
8. **单变量迭代（Iterate with single changes）**，不要整篇重写。一次只叠加一个新维度。
9. **锁定角色（Lock characters）**在第一个片段 / 用参考图像；之后用 "the woman"、"the detective" 回指。
10. **让聊天模型改写弱提示词（Let a chat model rewrite weak prompts）**成电影感版本，再送给 Veo。
