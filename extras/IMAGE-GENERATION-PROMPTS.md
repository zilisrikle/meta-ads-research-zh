# Genie by Veyu AI — 图像生成提示词（Image Generation Prompts）

每个提示词都是完全独立的。复制粘贴到你的图像模型中，并同时上传标注的参考图。

所有图片：**1080×1080px** 正方形。

---

# 广告系列 A："证据"（THE PROOF）— 轮播（5 张）

**故事弧线：**
1. "你在为永远不会成为客户的点击付费。"→ 个人化、直击内心、愧疚感
2. "你的代理商说不清花在哪。Meta 也不会告诉你。"→ 制造空洞，点名辜负你的人
3. "Genie 能。10 分钟。"→ 填补空洞，展示产品
4. 前后对比数据 → 打消质疑
5. 免费审计 CTA → 轻松行动

---

## 第 A1 张 — "指控"（The Accusation）

**需上传的参考图：**
- `creatives_examples/crousals/crousal-1.jpg` — tell the model: "THIS IS THE MOST IMPORTANT REFERENCE. Match the text scale EXACTLY. Notice how each word is MASSIVE — each line of text spans 70-80% of the frame width. The text IS the image. The 3D robot head is a small supporting element tucked into the bottom-right corner, taking up only about 20-25% of the frame. The text fills the rest. I need my text to be THIS large — each word huge, filling the frame. Match this text-to-visual ratio exactly."

**提示词：**

```
Create a square social media ad, 1080x1080 pixels.

THIS IS A TEXT-FIRST DESIGN. The text is the HERO — it must be MASSIVE, filling 70-75% of the frame. The 3D element is a small supporting visual tucked into one corner.

BACKGROUND: Very dark navy-black (#0A0B14). Subtle grain texture.

TEXT — THIS IS THE MAIN VISUAL. Positioned in the upper-left and center of the frame, left-aligned. Use a bold, modern sans-serif font (like Inter Black, Satoshi Black, or Clash Display Bold). Each line of text should span 70-80% of the frame width. The text fills the majority of the canvas like a poster.

Layout — each phrase gets its own line, stacked vertically with tight line spacing:

"You're paying" — 80pt, white (#FFFFFF), regular weight. This is the setup line, slightly lighter weight.

"for clicks" — 90pt, white (#FFFFFF), bold weight. Slightly larger, emphasis building.

"that never become" — 80pt, muted blue-grey (#6B7DB8), regular weight. The color shift creates rhythm — like how the crousal-1 reference uses different shades of grey/purple for different words.

"customers." — 100pt, white (#FFFFFF), extra-bold/black weight. THE LARGEST TEXT ON THE SLIDE. This is the punch word. It should be the heaviest, most dominant word on the entire canvas.

These four lines should fill the top 60-65% of the frame. They should feel overwhelming, confrontational, impossible to scroll past without reading.

Below the main text block, with a gap of about 30px:

"₹47,000 gone last month." — 36pt, blue (#2542EC), bold weight. Smaller than the headline but still prominent. The blue color ties it to the brand and makes the number pop without using red.

3D SUPPORTING ELEMENT — bottom-right corner, taking up about 25% of the frame:
A small cluster of 3-4 Indian rupee coins, photorealistic 3D render. The top 1-2 coins are subtly cracking with faint orange-red embers floating off them — just enough to suggest destruction without dominating the visual. The coins have a cool metallic blue-steel finish with blue (#2542EC) reflections. This element adds visual interest to the bottom-right but does NOT compete with the text. It's an accent, like the robot head in the crousal-1 reference — secondary, tucked away, supporting.

Lighting on the coins: Subtle cinematic lighting, the cracking coins have a faint warm glow, but the overall image stays dark and cool-toned.

BRANDING: Bottom-right, near the coins — "Genie ✦" in 16pt white with blue (#2542EC) sparkle.

CRITICAL COMPOSITION RULE: The text must feel like a poster headline — MASSIVE, confrontational, filling the frame. If you were to blur the image, the text block should be the dominant shape, not the 3D coins. The crousal-1 reference shows exactly this ratio: text = 75% of visual weight, 3D = 25%. Match it.
```

---

## 第 A2 张 — "空洞"（The Void）

**需上传的参考图：**
- `creatives_examples/crousals/crousal-1.jpg` — tell the model: "Again match the text-to-visual ratio from this reference. The text is MASSIVE and fills most of the frame. Any 3D/visual element is a small supporting accent, not the hero."
- `creatives_examples/crousals/crousal-3.jpg` — tell the model: "Match the dark moody atmosphere and the dramatic backlight glow on the 3D element. But keep the 3D element SMALL — as a supporting visual, not the hero."

**提示词：**

```
Create a square social media ad, 1080x1080 pixels.

TEXT-FIRST DESIGN. The text fills 70% of the frame. 3D element is a small supporting accent.

BACKGROUND: Very dark navy-black (#0A0B14). Subtle grain texture.

TEXT — the HERO. Left-aligned, filling the upper and center portion of the frame. Same bold sans-serif font as slide A1. Each phrase on its own line, spanning 70-80% of the frame width:

"Your agency" — 80pt, muted blue-grey (#6B7DB8), regular weight. Setup — lighter, less emphasis.

"can't explain" — 90pt, white (#FFFFFF), extra-bold. Emphasis starts building.

"where." — 110pt, white (#FFFFFF), extra-bold/black weight. THE LARGEST WORD. It hangs heavy. One word, massive, full weight. The period makes it final.

A gap of about 20px.

"Neither" — 70pt, muted blue-grey (#6B7DB8), regular weight.
"can Meta." — 80pt, white (#FFFFFF), bold. "Meta" slightly larger or bolder than "can" — it's the second named failure.

A gap of about 30px.

"But someone can. →" — 32pt, blue (#2542EC), italic. The arrow (→) invites a swipe. This line is deliberately MUCH smaller than the headlines above — it's a whisper after the shout. It creates the open loop: who? Swipe to find out.

3D SUPPORTING ELEMENT — bottom-right corner, about 20-25% of frame:
A 3D magnifying glass, at a slight angle. Dark gunmetal frame. The lens is smoky/murky — you can't see through it clearly. A dramatic blue backlight glow (#2542EC) rims the edges of the glass from behind, like the backlight in crousal-3. The magnifying glass represents the search for answers — but it's foggy, useless. Nobody can see through it. Keep it small, atmospheric, secondary to the text.

BRANDING: Bottom-right — "Genie ✦" in 16pt white with blue sparkle.

COMPOSITION: The word "where." should be the visual center of gravity — the largest, heaviest element on the canvas. Everything else supports it. Massive breathing room around the text blocks. The 3D magnifying glass is just a mood element in the corner.
```

---

## 第 A3 张 — "答案"（The Answer）（重磅）

**需上传的参考图：**
- `creatives_examples/cr13.jpeg` — tell the model: "Match this composition: bold headline at top, product chat UI card in center-bottom, dark background with subtle grid. The chat UI looks like a real working product. Match this polish."
- `our_creatives/crousals/meta_ads_crousal3.png` — tell the model: "This is our existing product UI design. Keep the same chat card concept — dark rounded card, Genie name with online indicator, findings with icons. Upgrade the quality to match the Intercom reference."

**提示词：**

```
Create a square social media ad, 1080x1080 pixels.

BACKGROUND: Very dark navy-black (#0A0B14) with subtle grid pattern (thin lines at 4% opacity).

HEADLINE — top area, left-aligned, about 12% from left. This should be LARGE and confident — not small caption text. Use bold sans-serif:

"Genie can." — 56pt, white (#FFFFFF), extra-bold
"In 10 minutes." — 56pt, blue (#2542EC), extra-bold
"While you do nothing." — 28pt, muted grey (#8B8FA3), italic

The first two lines are a bold answer to slide 2's void. "While you do nothing" drives home the effortlessness.

PRODUCT UI CARD — below the headline, centered, about 70% frame width:
Sleek dark chat interface card with slight 3D depth:
- Card background: dark (#111827), rounded corners (12px)
- Subtle blue glow shadow (#2542EC at 12% opacity)
- Thin border (#1E293B)

Card header:
- Left: blue circle with sparkle icon, "Genie" in white bold 15pt
- Right: green dot ● "online" in small green text

Card content — 4 findings + 1 result:

❌ "₹2.7L/month → broken landing page" — ❌ red, "₹2.7L" bold white, rest light grey (#C9CDD6)
❌ "2 campaigns bidding against each other" — light grey
❌ "12 old ads still spending — ₹18K wasted" — "₹18K" bold white
❌ "Best ad getting only 8% of budget" — "8%" bold white

✅ "Fix these = save ₹3.4L/month" — green (#22C55E), "₹3.4L" bold green

Below findings: "Audit done in 8 minutes." — 12pt, muted grey (#6B7280), italic

BRANDING: Bottom-right — "Genie ✦" 16pt white with blue sparkle.
```

---

## 第 A4 张 — "证明"（The Proof）

**需上传的参考图：**
- `creatives_examples/cr16.png` — tell the model: "Match the dark background and horizontal bar chart format. The winning bar visually dominates. My 'after' green bars should clearly dominate against dim red 'before' bars."

**提示词：**

```
Create a square social media ad, 1080x1080 pixels.

BACKGROUND: Very dark navy-black (#0A0B14).

HEADLINE — top area, centered:
"One founder's account." — 28pt, muted grey (#8B8FA3), regular, sans-serif
"Before Genie vs after." — 36pt, white (#FFFFFF), bold

DATA CARD — centered, about 70% width:
Dark card (#111827), rounded corners, subtle border (#1E293B), faint blue glow shadow.

Two sections, divided by thin line:

TOP — "BEFORE":
"BEFORE" — 13pt, red (#EF4444), bold, uppercase, wide tracking

Three bar rows:
"Monthly Waste" — bar ~60%, red (#EF4444) — value "₹60,000" 18pt white bold
"ROAS" — bar ~25%, red-orange (#F97316) — value "1.8x" 18pt white bold
"CPA" — bar ~70%, red (#EF4444) — value "₹480" 18pt white bold

[Thin divider]

BOTTOM — "AFTER GENIE":
"AFTER" — 13pt, green (#22C55E), bold, uppercase

"Monthly Waste" — bar ~3%, green — value "₹0" 18pt green bold
"ROAS" — bar ~75%, green — value "4.2x" 18pt green bold
"CPA" — bar ~25%, green — value "₹195" 18pt green bold

Below card: "30 days. Same budget. Same ads." — 16pt, grey (#52525B), italic

BRANDING: Bottom-right — "Genie ✦" 16pt.
```

---

## 第 A5 张 — "行动号召"（The CTA）

**需上传的参考图：**
- `our_creatives/crousals/meta_ads_crousal4.png` — tell the model: "Match this centered sparkle CTA layout. Upgrade sparkle — larger, more luminous, blue (#2542EC) glow."

**提示词：**

```
Create a square social media ad, 1080x1080 pixels.

BACKGROUND: Very dark navy-black (#0A0B14), clean.

HERO — upper-center, top 35%:
Large luminous 3D four-pointed sparkle (Genie icon), ~200x200px, centered.
Translucent frosted crystal, internally lit with blue-white light. Soft blue glow (#2542EC → #4F6AFF) radiating outward. Halo effect. Apple-style frosted glass. Tiny blue light particles scattered around.

TEXT — below sparkle, centered. These lines should be BIG and clear — this is the payoff moment:

"See where your money actually goes." — 38pt, white (#FFFFFF), bold, sans-serif
"Free. 10 minutes. No calls." — 28pt, muted grey (#8B8FA3), regular

Gap 36px.

Pill-shaped button, centered:
- Background: blue (#2542EC)
- Padding: 16px vertical, 44px horizontal
- Border-radius: 28px (pill shape)
- Inside: white WhatsApp icon (left), "Get my free audit →" 22pt white bold

Gap 16px.

"genie.veyu.ai" — 20pt, blue (#2542EC)

BRANDING:
Bottom-right: "Genie ✦" 16pt
Bottom-left: "your meta ads, granted." 14pt dark grey (#3B3F4A)
```

---

# 广告系列 B："48 倍"（THE 48x）— 单图

---

## 图 B1 — "48 倍"（The 48x）

**需上传的参考图：**
- `creatives_examples/crousals/crousal-1.jpg` — tell the model: "Match the text scale from this reference. Text is the HERO — massive, frame-filling. Each line spans 70-80% of the frame width. Any visual element is small and secondary."
- `creatives_examples/cr6.jpg` — tell the model: "Use this for the 3D element style — how a 3D object looks on a dark background with cinematic lighting. But keep the 3D element SMALL — it's supporting the text, not competing with it."

**提示词：**

```
Create a square social media ad, 1080x1080 pixels.

TEXT-FIRST DESIGN. Text fills 70%+ of the frame. 3D elements are small supporting visuals.

BACKGROUND: Very dark navy-black (#0A0B14). Subtle grain.

TEXT — the HERO. Left-aligned, stacked vertically, filling most of the canvas. Bold sans-serif font. Each line is MASSIVE:

"They check" — 70pt, muted blue-grey (#6B7DB8), regular weight. Setup.

"once a day." — 90pt, white (#FFFFFF), extra-bold. Emphasis on the inadequacy.

A gap of 10px.

"We check" — 70pt, muted blue-grey (#6B7DB8), regular weight. Mirror structure.

"48 times." — 100pt, white (#FFFFFF), extra-bold/black weight. THE LARGEST TEXT. The number "48" should be especially heavy and dominant. This is the punch.

A gap of 25px.

"₹40,000/mo" — 40pt, muted blue-grey (#6B7DB8), regular weight, with a visible strikethrough line through the text.
"₹3,999/mo" — 50pt, white (#FFFFFF), extra-bold. Positioned right below the struck-through price. The price gap is immediately visible.

A gap of 15px.

"Free audit → genie.veyu.ai" — 24pt, blue (#2542EC), bold.

3D SUPPORTING ELEMENTS — bottom-right corner, about 20% of frame:
Two small 3D clocks side by side. Left clock: dusty, frozen, grey, cracked glass — the dead agency clock. Right clock: pristine, glowing blue (#2542EC), motion-blurred spinning hand, particles of light — the alive Genie clock. These are SMALL — accent elements that visually reinforce the text comparison. They do NOT dominate the frame. Think of them like the robot head in crousal-1 — a small visual reward in the corner.

The clocks have subtle cinematic lighting — the Genie clock casts faint blue light, the agency clock is in shadow.

BRANDING: Bottom-right near the clocks — "Genie ✦" 16pt white with blue sparkle.

COMPOSITION: "48 times." should be the visual center of gravity — the largest, boldest element. The comparison structure (they/we, old price/new price) creates a rhythm that's easy to scan in 2 seconds. The text should feel like a poster — confrontational, bold, impossible to ignore at phone size.
```

---

# 广告系列 C："坦白"（THE CONFESSION）— WhatsApp 英文风格

---

## 图 C1 — "坦白"（The Confession）

**需上传的参考图：**
- `our_creatives/meta_ads_message_style.png` — tell the model: "Match the WhatsApp conversation layout — bubbles, header, dark mode. But more NATURAL than this — less polished, more like a real screenshot. Use a common Indian name instead of 'Aurora.' Messages should feel casual and overheard."
- `creatives_examples/cr13.jpeg` — tell the model: "Use this for the premium dark background wrapper outside the phone. Subtle grid texture, dark treatment, soft glow behind the phone."

**提示词：**

```
Create a square social media ad, 1080x1080 pixels.

BACKGROUND: Very dark navy-black (#0A0B14) with subtle grid (3-4% opacity). Soft blue glow (#2542EC at 8%) from behind phone.

TOP — above phone, centered:
"Overheard between two founders." — 18pt, muted grey (#8B8FA3), italic, sans-serif.

PHONE — centered, ~72% of frame height:
Modern smartphone, dark frame, thin bezels. Subtle blue edge-glow. Soft shadow.

Screen — WhatsApp DARK MODE (#0B141A):

Header: Back arrow, initials avatar "RK", "Rahul K." white bold, "online" small green, grey icons.

Chip: "Today"

CONVERSATION — 8 bubbles. Outgoing: green (#005C4B), right. Incoming: dark grey (#202C33), left. White text ~14pt. Timestamps. Blue ✓✓ on outgoing.

[Outgoing] "still with that agency?" — 3:42 PM ✓✓

[Incoming] "nah switched 2 months ago" — 3:42 PM

[Outgoing] "to what" — 3:43 PM ✓✓

[Incoming] "Genie. best agency experience I've ever had" — 3:43 PM

[Outgoing] "how much" — 3:43 PM ✓✓

[Incoming] "₹3,999/month. Found ₹2.3L waste my old agency missed for 8 months" — 3:44 PM

[Outgoing] "bro WHAT. link?" — 3:44 PM ✓✓

[Incoming] "genie.veyu.ai — get the free audit 😄" — 3:45 PM

Input bar at bottom of screen.

CRITICAL: ₹3,999, ₹2.3L, and genie.veyu.ai must be perfectly legible. Natural uneven bubble spacing. "bro WHAT" is the emotional peak.

BOTTOM — below phone:
Dark banner (#111827, rounded, ~50px):
Left: green WhatsApp icon. Center: "Free Meta ads audit" 24pt white bold. Right: "genie.veyu.ai" 16pt blue (#2542EC).

BRANDING: Bottom-right — "Genie ✦" 14pt.
```

---

# META ADS MANAGER 广告文案（AD COPY FOR META ADS MANAGER）

填入广告搭建时的文本字段——不要放在图片上。

---

## 广告系列 A："证据"轮播

**正文（英文版）：**
```
Last month, the average Indian Meta ad account wasted ₹1.8 lakh.
Not on bad ads — on audiences that don't convert, campaigns fighting each other, and ads that should've been paused weeks ago.

Your agency didn't catch it. Meta won't tell you.
Genie will. In 10 minutes. For free.

Swipe to see what a real audit finds →
```

**正文（Hinglish 版）：**
```
Pichle mahine average Indian Meta ad account ne ₹1.8 lakh waste kiya.
Kharab ads ki wajah se nahi — galat audiences, campaigns ek dusre se compete kar rahe the, aur ads jo weeks pehle band hone chahiye the.

Agency ne nahi pakda. Meta bhi nahi batayega.
Genie batayega. 10 minute mein. Free.

Swipe karo →
```

**标题：**Your agency missed this. Genie found it in 10 minutes.
**描述：**Free audit — no forms, no calls.
**CTA 按钮：**Send WhatsApp Message

---

## 广告系列 B："48 倍"单图

**正文（英文版）：**
```
Your agency checks your ads once a day.
In the other 23 hours, your campaigns could be bleeding money.

CPA spikes at 2 AM? Genie catches it at 2:30 AM.
Your agency finds out Tuesday.

Same skills. 48x more attentive. ₹3,999/month.
```

**标题：**48x more attentive. 1/10th the price.
**描述：**Free audit in 10 minutes.
**CTA 按钮：**Send WhatsApp Message

---

## 广告系列 C："坦白"WhatsApp

**正文：**
```
One founder switched from a ₹40K/month agency to Genie.
Found ₹2.3L/month going to audiences that never convert.
The agency missed it for 8 months.

Genie costs ₹3,999/month and checks your account 48 times a day.

Free audit → genie.veyu.ai
```

**标题：**"₹2.3L waste. 8 months. Agency didn't notice."
**描述：**Free audit — see what you're wasting.
**CTA 按钮：**Send WhatsApp Message

---

# 参考图速查表（REFERENCE IMAGE CHEAT SHEET）

| 提示词 | 上传这些文件 | 对标要点 |
|--------|-------------------|---------------|
| A1 | `crousals/crousal-1.jpg` | 文字比例——文字占画面 75%，3D 是角落小点缀 |
| A2 | `crousals/crousal-1.jpg` + `crousals/crousal-3.jpg` | 同样的文字比例 + 暗黑氛围与戏剧性背光 |
| A3 | `cr13.jpeg` + `our_creatives/crousals/meta_ads_crousal3.png` | 深色网格背景上的聊天 UI，产品截图质感 |
| A4 | `cr16.png` | 数据条形图，胜者/败者对比分明 |
| A5 | `our_creatives/crousals/meta_ads_crousal4.png` | 闪光星星 CTA 构图 |
| B1 | `crousals/crousal-1.jpg` + `cr6.jpg` | 文字比例（文字是主角，3D 很小）+ 电影感 3D 光影 |
| C1 | `our_creatives/meta_ads_message_style.png` + `cr13.jpeg` | WhatsApp 聊天布局 + 深色高级外框 |
