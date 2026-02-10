# Platform-Specific Prompt Guide

## Midjourney

### Syntax
```
/imagine prompt: [description] --ar [ratio] --v [version] --style [style] --s [stylize] --c [chaos] --q [quality]
```

### Key Parameters

| Parameter | Values | Description |
|-----------|--------|-------------|
| `--ar` | `1:1`, `16:9`, `9:16`, `4:3`, `3:2`, `21:9` | Aspect ratio |
| `--v` | `6.1`, `6`, `5.2` | Model version (latest: 6.1) |
| `--style` | `raw` | Less opinionated, more photographic |
| `--s` | `0`–`1000` (default `100`) | Stylization strength |
| `--c` | `0`–`100` (default `0`) | Chaos / variation |
| `--q` | `0.25`, `0.5`, `1` | Quality / detail level |
| `--no` | `text, watermark` | Negative prompt (things to avoid) |
| `--tile` | (no value) | Seamless tiling pattern |
| `--seed` | `0`–`4294967295` | Reproducible results |

### Midjourney v6+ Tips
- Natural language works better than keyword stuffing
- Longer, descriptive prompts produce better results
- Use `--style raw` for photorealism
- Quotation marks `""` can emphasize specific text rendering
- Supports multi-prompt with `::` weight syntax: `cat::2 dog::1`

### Quality Boosters (Midjourney)
```
masterpiece, award-winning, highly detailed, sharp focus, 8K UHD
professional photography, cinematic lighting, dramatic composition
```

---

## DALL-E 3 (via ChatGPT / API)

### Syntax
Plain English descriptions. No special parameters — DALL-E 3 auto-interprets.

### API Parameters

| Parameter | Options | Description |
|-----------|---------|-------------|
| `size` | `1024x1024`, `1792x1024`, `1024x1792` | Image dimensions |
| `quality` | `standard`, `hd` | Detail level |
| `style` | `vivid`, `natural` | Vivid = hyper-real; Natural = subtle |
| `n` | `1` | Number of images (always 1 for DALL-E 3) |

### DALL-E 3 Tips
- Accepts and benefits from long, detailed descriptions
- Automatically rewrites/expands short prompts internally
- Best for: photorealistic, illustration, digital art
- Does NOT support negative prompts natively
- Workaround for negatives: describe what you want, not what to avoid
- Supports text rendering in images (better than most models)

### Quality Boosters (DALL-E 3)
```
highly detailed, professional quality, sharp focus, vivid colors
studio lighting, award-winning photography, editorial quality
```

---

## Flux (by Black Forest Labs)

### Models
| Model | Speed | Quality | Best For |
|-------|-------|---------|----------|
| `flux-pro` | Slow | Highest | Final output, commercial work |
| `flux-dev` | Medium | High | Development, iteration |
| `flux-schnell` | Fast | Good | Prototyping, quick tests |

### Syntax
Plain English descriptions via API. No special syntax markers.

### API Parameters (via Replicate / fal.ai / BFL API)

| Parameter | Options | Description |
|-----------|---------|-------------|
| `width` | `256`–`1440` (multiples of 32) | Image width |
| `height` | `256`–`1440` (multiples of 32) | Image height |
| `num_inference_steps` | `1`–`50` (schnell: 1-4, dev: 20-50) | Denoising steps |
| `guidance_scale` | `1.0`–`20.0` (default `3.5`) | Prompt adherence |
| `seed` | integer | Reproducible results |

### Flux Tips
- Excels at photorealism, text rendering, and human anatomy
- Lower guidance (2.0–4.0) often produces more natural results
- Supports very long prompts effectively
- No native negative prompt — describe positively
- Best text-in-image rendering of any open model
- Works well with comma-separated descriptors

### Quality Boosters (Flux)
```
masterpiece, best quality, highly detailed, sharp focus
professional photography, cinematic, 8K resolution
photorealistic, high dynamic range, studio quality
```

---

## Stable Diffusion (SDXL / SD 1.5)

### Syntax
Comma-separated descriptors. Supports weighted tokens.

```
(important element:1.3), normal element, (less important:0.7)
```

### Key Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `steps` | `20`–`50` | Sampling steps (more = detailed, slower) |
| `cfg_scale` | `7.0` | Classifier-free guidance (prompt adherence) |
| `sampler` | `DPM++ 2M Karras` | Sampling algorithm |
| `seed` | random | Reproducible results |
| `width` / `height` | `1024x1024` (SDXL) | Image dimensions |
| `clip_skip` | `1`–`2` | Skip CLIP layers (2 for anime) |

### SDXL vs SD 1.5

| Feature | SDXL | SD 1.5 |
|---------|------|--------|
| Resolution | 1024x1024 native | 512x512 native |
| Quality | Higher baseline | Needs more prompting |
| LoRA ecosystem | Growing | Massive |
| Speed | Slower | Faster |
| Negative prompt | Supported | Supported |

### Negative Prompt (Stable Diffusion)

**Standard:**
```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit,
fewer digits, cropped, worst quality, low quality, normal quality,
jpeg artifacts, signature, watermark, username, blurry
```

**For portraits, add:**
```
deformed iris, deformed pupils, semi-realistic, cgi, 3d, render, sketch,
cartoon, drawing, anime, mutated hands and fingers, deformed, distorted,
disfigured, poorly drawn, bad anatomy, wrong anatomy
```

**For anime, add:**
```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit,
fewer digits, cropped, worst quality, low quality, normal quality,
jpeg artifacts, signature, watermark, username, blurry, artist name,
bad-artist, bad-hands-5
```

### Prompt Weight Syntax
```
(keyword)       → weight 1.1
((keyword))     → weight 1.21
(keyword:1.5)   → explicit weight 1.5
[keyword]       → weight 0.9 (de-emphasis)
```

### Quality Boosters (Stable Diffusion)
```
masterpiece, best quality, highly detailed, sharp focus, 8K UHD,
high resolution, professional, intricate details, beautiful lighting
```

### Recommended Samplers

| Sampler | Best For |
|---------|----------|
| `DPM++ 2M Karras` | General purpose, fast |
| `DPM++ SDE Karras` | Fine details, slower |
| `Euler a` | Creative, varied results |
| `DDIM` | Consistent, reproducible |

---

## Cross-Platform Comparison

| Feature | Midjourney | DALL-E 3 | Flux | Stable Diffusion |
|---------|-----------|----------|------|-----------------|
| Negative prompt | `--no` | Not supported | Not supported | Full support |
| Aspect ratio | `--ar` | API size param | width/height | width/height |
| Prompt weighting | `::` syntax | Not supported | Not supported | `()` or `:weight` |
| Text rendering | Good (v6+) | Best | Excellent | Poor |
| Photorealism | Excellent | Excellent | Best | Good (SDXL) |
| Anime/Illustration | Good | Good | Good | Best (with LoRA) |
| Speed | Medium | Fast | Varies by model | Varies |
| Local/Self-hosted | No | No | Yes | Yes |
| Max prompt length | ~6000 chars | ~4000 chars | ~2000 tokens | 77 tokens (SD1.5) / 150 (SDXL) |

## Platform Selection Guide

| Goal | Recommended Platform |
|------|---------------------|
| Best photorealism | Flux Pro or Midjourney v6 `--style raw` |
| Best anime/illustration | Stable Diffusion + anime LoRA |
| Best text in image | Flux Pro or DALL-E 3 |
| Fastest iteration | Flux Schnell or DALL-E 3 |
| Most control | Stable Diffusion (full parameter access) |
| Easiest to use | DALL-E 3 (natural language, auto-enhance) |
| Commercial use | Check each platform's license terms |
