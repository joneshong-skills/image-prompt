---
name: image-prompt
description: "prompt, image, generate, write, ai, art, convert, 幫我寫生圖提示詞, 產生圖片 prompt"
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

Assemble the prompt using the **7-Component Framework**:

| # | Component | Description | Example |
|---|-----------|-------------|---------|
| 1 | **Subject** | Main focal point, clearly described | "a lone samurai standing on a cliff edge" |
| 2 | **Style** | Artistic style or medium | "digital painting, Studio Ghibli inspired" |
| 3 | **Composition** | Framing, angle, perspective | "wide-angle shot, rule of thirds, low camera angle" |
| 4 | **Lighting** | Light source, quality, mood | "golden hour backlighting, volumetric god rays" |
| 5 | **Color Palette** | Dominant colors, tone | "warm amber and deep indigo, muted earth tones" |
| 6 | **Details & Texture** | Surface quality, fine details | "intricate armor engravings, weathered fabric" |
| 7 | **Atmosphere** | Environmental mood, effects | "misty mountains, cherry blossom petals drifting" |

### Step 3 — Apply Quality Boosters

Append proven quality-enhancing tokens based on the target platform:

**Universal boosters** (work across most models):
- `masterpiece, best quality, highly detailed, sharp focus`
- `professional photography` / `award-winning illustration`
- `8K UHD, high resolution`

**Platform-specific boosters** — see `references/platform-guide.md`

### Step 4 — Generate Negative Prompt

Always include a negative prompt to avoid common artifacts:

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

## Continuous Improvement

This skill evolves with each use. After every invocation:

1. **Reflect** — Identify what worked, what caused friction, and any unexpected issues
2. **Record** — Append a concise lesson to `lessons.md` in this skill's directory
3. **Refine** — When a pattern recurs (2+ times), update SKILL.md directly

### lessons.md Entry Format

```
### YYYY-MM-DD — Brief title
- **Friction**: What went wrong or was suboptimal
- **Fix**: How it was resolved
- **Rule**: Generalizable takeaway for future invocations
```

Accumulated lessons signal when to run `/skill-optimizer` for a deeper structural review.

## Additional Resources

### Reference Files

| File | Load when | Purpose |
|---|---|---|
| `references/platform-guide.md` | Step 3 quality boosters / Step 4 negative prompt | Platform-specific syntax (Midjourney / DALL-E / Flux / SD) |
| `references/style-dictionary.md` | Step 2 style assembly (按需查) | 200+ curated style/mood/lighting/composition keywords |
| `references/meta-schema.md` | Step 2 路徑切換時（多張一致 / JSON / 系列）| Composable schema + 9 scope extensions + Parameter Tiers + category-specific negative prompts |

> 蠶食自 [ConardLi/garden-skills/gpt-image-2](https://github.com/ConardLi/garden-skills/tree/main/skills/gpt-image-2) — composable meta-schema（meta-schema.md）+ 18 分類結構化模板索引（references/gpt-image-2-templates/INDEX.md）
