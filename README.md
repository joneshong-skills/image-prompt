# image-prompt

A Claude skill that converts vague descriptions into professional, structured image generation prompts compatible with Midjourney, DALL-E, Flux, Stable Diffusion, and other text-to-image models.

## What It Does

- Parses user intent from natural-language descriptions (supports any language, including Chinese and Japanese)
- Builds structured prompts using a **7-Component Framework**: Subject, Style, Composition, Lighting, Color Palette, Details & Texture, and Atmosphere
- Appends platform-aware quality boosters and generates negative prompts to avoid common artifacts
- Outputs both a ready-to-use prompt string and a structured JSON format for API/MCP integration
- Suggests appropriate aspect ratios and model recommendations based on the requested style

## Installation

Copy the skill directory into your Claude skills folder:

```
~/.claude/skills/image-prompt/
```

The directory should contain:

```
SKILL.md
references/
  platform-guide.md
  style-dictionary.md
```

## Usage

Trigger the skill by asking Claude to generate an image prompt. Example phrases:

- "Generate an image prompt for a cyberpunk cityscape at night"
- "Write a prompt for AI art of a cat sitting in a sunlit garden"
- "幫我寫生圖提示詞：一個女孩在櫻花樹下看書"
- "Optimize this image prompt: a dragon flying over mountains"

The skill will return a complete prompt with negative prompts, style presets, aspect ratio suggestions, and recommended models.

## Reference Files

- **`references/platform-guide.md`** — Platform-specific syntax for Midjourney, DALL-E, Flux, and Stable Diffusion
- **`references/style-dictionary.md`** — 200+ curated style, mood, lighting, and composition keywords

## License

MIT
