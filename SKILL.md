---
name: image-prompt
description: "prompt, image, generate, write, ai, art, convert, 幫我寫生圖提示詞, 產生圖片 prompt。要真的產生圖片用 /image-gen，修改既有圖片用 /image-edit，不確定用 /prompt-router"
version: 0.1.0
argument-hint: "描述想要的畫面（中文或英文皆可）"
---

# Image Prompt Engineer

Convert vague descriptions into professional, structured image generation prompts.
Output model-agnostic prompts compatible with Midjourney, DALL-E, Flux, Stable Diffusion, etc.

## Core Workflow

### Step 1 — Parse User Intent

Extract from the user's description:
- **Subject**: What is the main focus?
- **Scene/Setting**: Where does it take place?
- **Mood/Emotion**: What feeling should it convey?
- **Any explicit style preferences**: Mentioned artists, media, or aesthetics?

If the description is too vague (fewer than 3 extractable elements), ask one focused
clarification question before proceeding.

### Step 2 — Build Structured Prompt

**Two paths — pick based on scope:**

| Scope | Use | Why |
|---|---|---|
| 單張快圖 / 探索性 / user 要 prompt string | **7-component framework**（下方） | 快、通用、零 schema 負擔 |
| 多張變體一致 / character 系列 / product 系列 / poster / UI mockup / storyboard / 要 JSON 重用 | **Composable Meta-Schema**（讀 [`references/meta-schema.md`](references/meta-schema.md)） | 結構化、9 個 scope extensions、Parameter Tiers、category-specific negative prompts |

預設走 7-component；當 user 提到「角色變體」「系列」「多張一致」「要 JSON」「要重用」任一關鍵字，切換到 meta-schema。

Assemble the prompt using the **7-Component Framework**. The brief's defining words lead the
prompt: 「霧中的阿里山」 opens with "Alishan in thick mist", 「日式極簡客廳」 with "a Japanese
minimalist living room" — a quality buried later in the prompt comes out weak.

| # | Component | Description | Example |
|---|-----------|-------------|---------|
| 1 | **Subject** | Main focal point, clearly described, carrying the brief's defining words | "a lone samurai standing on a cliff edge" |
| 2 | **Style** | Artistic style or medium | "digital painting, Studio Ghibli inspired" |
| 3 | **Composition** | Framing, angle, perspective | "wide-angle shot, rule of thirds, low camera angle" |
| 4 | **Lighting** | What the light does — direction, softness, time of day. Name the effect, not the gear: "softbox" gets drawn into the frame | "golden hour backlighting, volumetric god rays" |
| 5 | **Color Palette** | Dominant colors, tone | "warm amber and deep indigo, muted earth tones" |
| 6 | **Details & Texture** | Surface quality, fine details | "intricate armor engravings, weathered fabric" |
| 7 | **Atmosphere** | Environmental mood, effects | "misty mountains, cherry blossom petals drifting" |

**When the image carries text** (poster, logo, cover, label, infographic):
- Give each piece of text once, in double quotes, with its font style and place:
  `the title "SUMMER WAVE 2026" in bold rounded sans-serif across the top`.
- Right after the text description, add `no other text` (Step 3's quality tokens still go
  last). In the A/B, prompts without it got invented line-ups, publishers and captions.
- Never call the text "quoted" or "in quotes" — the model then draws the quote marks around
  every label.
- A few words per label; long text comes out misspelled.

### Step 3 — Apply Quality Boosters

Append the quality tokens below based on the target platform (their effect on Gemini was not
separable in the 2026-09 A/B — see the end of this file):

**Universal boosters** (this skill's long-standing default; not tested on their own):
- `masterpiece, best quality, highly detailed, sharp focus`
- `professional photography` / `award-winning illustration`
- `8K UHD, high resolution`

**Platform-specific syntax** (Midjourney parameters, SD weights) — see `references/platform-guide.md`

### Step 4 — Generate Negative Prompt

Write a negative prompt for the JSON output. Only Stable Diffusion takes it as-is and
Midjourney as `--no`; `/image-gen` does not send it to Gemini or Grok (see the platform guide
table). When the image should carry text, drop `text`, `signature` and `username` from it.

**Standard negative prompt:**
```
lowres, bad anatomy, bad hands, text, error, missing fingers,
extra digit, fewer digits, cropped, worst quality, low quality,
normal quality, jpeg artifacts, signature, watermark, username, blurry,
deformed, disfigured, mutation, mutated, extra limbs
```

Adjust based on subject (portraits add face-specific terms, landscapes add different terms).

### Step 5 — Output Format

Return the prompt in **both formats**:

#### A. Ready-to-Use Prompt (Single String)
```
A lone samurai standing on a cliff edge, digital painting, Studio Ghibli inspired,
wide-angle shot, golden hour backlighting with volumetric god rays, warm amber and
deep indigo palette, intricate armor engravings, misty mountains with cherry blossom
petals drifting, masterpiece, best quality, highly detailed, 8K UHD
```

#### B. Structured JSON (For MCP / API Integration)
```json
{
  "prompt": "<the full prompt string>",
  "negative_prompt": "<negative prompt string>",
  "style_preset": "anime_illustration",
  "aspect_ratio": "16:9",
  "suggested_models": ["flux-pro", "midjourney-v6", "sdxl"],
  "components": {
    "subject": "a lone samurai standing on a cliff edge",
    "style": "digital painting, Studio Ghibli inspired",
    "composition": "wide-angle shot, rule of thirds, low camera angle",
    "lighting": "golden hour backlighting, volumetric god rays",
    "color_palette": "warm amber and deep indigo, muted earth tones",
    "details": "intricate armor engravings, weathered fabric",
    "atmosphere": "misty mountains, cherry blossom petals drifting"
  }
}
```

## Style Presets Quick Reference

| Preset | Key Tokens | Best For |
|--------|-----------|----------|
| `photorealistic` | cinematic, 35mm film, depth of field, natural lighting | Product shots, portraits |
| `anime_illustration` | anime style, cel shading, vibrant colors, clean lines | Characters, scenes |
| `oil_painting` | oil on canvas, visible brushstrokes, impasto technique | Artistic portraits, landscapes |
| `concept_art` | concept art, matte painting, epic scale | Game/film environments |
| `watercolor` | watercolor painting, soft edges, paper texture, translucent | Delicate scenes, florals |
| `pixel_art` | pixel art, 16-bit, retro gaming aesthetic | Game assets, icons |
| `3d_render` | 3D render, octane render, subsurface scattering, PBR | Products, characters |
| `ink_drawing` | ink drawing, pen and ink, cross-hatching, high contrast | Illustrations, comics |
| `flat_design` | flat design, vector art, minimal, bold colors | UI mockups, icons |
| `cyberpunk` | cyberpunk, neon lights, rain-soaked streets, holographic | Sci-fi scenes |

## Aspect Ratio Guidelines

| Ratio | Pixels | Use Case |
|-------|--------|----------|
| `1:1` | 1024×1024 | Social media, avatars |
| `16:9` | 1920×1080 | Desktop wallpapers, cinematic |
| `9:16` | 1080×1920 | Mobile wallpapers, stories |
| `4:3` | 1536×1152 | Blog images, presentations |
| `3:2` | 1536×1024 | Photography style |
| `21:9` | 2520×1080 | Ultra-wide, panoramic |

## Language Handling

- Accept input in **any language** (Chinese, English, Japanese, etc.)
- Always output prompts in **English** (best model compatibility)
- Provide a Chinese summary of what the prompt describes

## Important Rules

- Never output NSFW, violent, or harmful content
- If the request is ambiguous, bias toward aesthetic and artistic interpretation
- Always suggest 2-3 model recommendations based on the style
- Keep prompt length under 200 tokens for best results (75-150 is ideal)
- Order components by importance — most models weight early tokens more heavily
- Aspect ratio goes in the JSON `aspect_ratio`, or in words at the end of the prompt
  ("horizontal 4:3 format") — never as a bare `4:3` token in the string

## Additional Resources

### Reference Files

| File | Load when | Purpose |
|---|---|---|
| `references/platform-guide.md` | Step 3 platform syntax / Step 4 negative prompt / user names a platform | Per-platform syntax: Gemini, Grok, Midjourney (from 80 Explore prompts), GPT-image, Flux, SD |
| `references/templates.md` | Gemini, when the user asks for paragraph prompts or the layout is fixed (logo, product, comic panel) | Google's six paragraph templates + three of ours; not A/B-tested here |
| `references/style-dictionary.md` | Step 2 style assembly (按需查) | 200+ curated style/mood/lighting/composition keywords |
| `references/meta-schema.md` | Step 2 路徑切換時（多張一致 / JSON / 系列）| Composable schema + 9 scope extensions + Parameter Tiers + category-specific negative prompts |

> 蠶食自 [ConardLi/garden-skills/gpt-image-2](https://github.com/ConardLi/garden-skills/tree/main/skills/gpt-image-2) — composable meta-schema（meta-schema.md）+ 18 分類結構化模板索引（references/gpt-image-2-templates/INDEX.md）

> 2026-09-29 blind A/B on Gemini Flash, 30 pairs over two rounds: rewriting this skill as
> Google-style paragraphs without quality tokens lost 13–17. Paragraph prompts had fewer text
> errors and scored lower on aesthetics. This file keeps its format and took only targeted
> fixes: `no other text` (invented text in both rounds), never "quoted" (quote marks drawn,
> round 2), light as effect not gear and the brief's defining words first (each lost in round 1,
> won back in round 2 — except p09, where old won both rounds), aspect ratio in words (a bare
> `4:3` token, round 1). Each fix changed alongside others, so none is proven on its own. Data:
> `~/workshop/outputs/image-prompt-study/`.
