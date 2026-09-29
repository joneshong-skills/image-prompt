# Platform Guide

SKILL.md's 7-component prompt is what `/image-gen` sends to Gemini. Use the sections below for
platform-specific syntax and for any platform the user names. Each section says when it was last checked; an unchecked claim is labelled.

| Platform | Prompt form | Negative channel | Aspect ratio |
|---|---|---|---|
| Gemini (web, via `/image-gen`) | 7-component prompt (Google suggests paragraphs — see below) | None — describe the wanted state | In words at the end of the prompt |
| Grok (`grok-imagine-*` API, via `/image-gen`) | Descriptive paragraph | None | `aspect_ratio` in the API body |
| Midjourney | Paragraph or short phrase + parameters | `--no a, b` | `--ar W:H` |
| GPT-image (ChatGPT / OpenAI API) | Descriptive paragraph | None | Size / in the prompt |
| Flux | Descriptive paragraph | None (guidance-distilled models) | width / height |
| Stable Diffusion 1.5 / SDXL | Comma-separated tags with weights | Full negative prompt | width / height |

---

## Gemini

Checked 2026-09-29 against Google's "How to prompt Gemini 2.5 Flash Image Generation for the
best results" (Google Developers Blog).

- Google's guide favours a descriptive paragraph over a keyword list. In this skill's blind A/B
  (2026-09-29, 30 pairs) paragraph prompts did not beat the 7-component format: fewer text errors,
  lower aesthetic scores. `templates.md` has the paragraph templates when a user wants them.
- Worth taking from Google's guide (not tested one by one here): exact text in double quotes
  with a font style, semantic negatives ("an empty street", not "no cars"), camera language, stating the purpose.
- `/image-gen` prefixes the prompt with "Create an image of:" — do not add it yourself.
- Editing an existing image is a different prompt shape ("Using the provided image of X, change
  only the Y to Z") — that is `/image-edit`'s job.
- Iterate in conversation: a follow-up "make the light warmer, keep everything else" usually
  beats rewriting the whole prompt.

## Grok (xAI `grok-imagine-image` API)

Checked 2026-09-29: xAI publishes API parameters but no prompting guide.

- Use the same paragraph as for Gemini; nothing platform-specific is documented.
- Set aspect ratio with the `aspect_ratio` field (`"16:9"` → 1280x720 measured); the prompt
  text alone is not the control.

## Midjourney

Checked 2026-09-29 by reading 80 prompts from the public Explore "Top day" feed (job pages are
readable without login; the official docs site was not reachable for this check).

Top images come from two different recipes:

- **Short phrase + a style code (20% of the sample)** — "mirror ball --sref 2444940319 --raw",
  "watercolor, vintage christmas teddy bear --profile baqug5i". The look lives in the code, not
  the words. `--profile` is a personal aesthetic built from one user's ratings, `--sref` a
  style reference; neither exists on any other platform, so these prompts do not port.
- **A long scene paragraph (39% run 60+ words)** — the same moves as a well-filled 7-component prompt: exact
  placement ("SIDE VIEW… facing LEFT"), a named kind of photograph ("authentic 1950s documentary
  photo"), mark-making ("scratchy charcoal outlines, dry brush strokes, visible paper grain"),
  a named palette, and a closing exclusion sentence (21% have one).

Quality filler is nearly absent: 6 of 80 prompts contain any of masterpiece / 8k / highly
detailed / high resolution.

Writing for Midjourney: the SKILL.md prompt, then parameters.

| Parameter | Seen in sample | Use |
|---|---|---|
| `--ar W:H` | 76% | Always set it |
| `--profile <code>` | 46% | Only the user's own code — ask, never invent one |
| `--hd` | 30% | High-resolution mode (not checked against the docs) |
| `--chaos 5–65` | 26% | Variety across the grid; 10–30 typical |
| `--raw` / `--style raw` | 20% | Less of Midjourney's default polish; pair with low `--stylize` (≈50) for literal, photographic results |
| `--stylize` | 16% | Higher leans on Midjourney's own aesthetic, lower follows the words |
| `--sref <code or URL>` | 15% | A style the user already has |
| `--exp` | 8% | Seen at 15–30 |
| `--no a, b` | 5% | Most exclusions are written as a sentence in the prompt instead |

Sample, per-parameter tallies and the collector: `~/workshop/outputs/image-prompt-study/mj/`.

## GPT-image (ChatGPT / OpenAI API)

Not re-checked since the move off DALL-E 3; treat as unverified.

- Paragraph prompts; the model rewrites short prompts internally, so a detailed paragraph keeps
  control with you.
- No negative prompt — describe the wanted state.
- Strong at text in images; still keep text short and quoted.

## Flux (Black Forest Labs)

Not re-checked in this revision.

- Paragraph prompts; long prompts are handled well.
- No negative prompt on the guidance-distilled models (dev / schnell / pro) — describe the
  wanted state.
- Lower guidance (≈2–4) tends to look more natural; higher follows the prompt more literally.

## Stable Diffusion 1.5 / SDXL

The one family where tag lists, weights and negative prompts are the native interface — the
tag-trained checkpoints (especially anime models) learned tags such as `masterpiece` and
`best quality` from their training captions, so here they do carry meaning.

Syntax: comma-separated descriptors; `(important:1.3)`, `[less important]`.

| Parameter | Typical | Note |
|---|---|---|
| `steps` | 20–50 | |
| `cfg_scale` | 7 | prompt adherence |
| `sampler` | DPM++ 2M Karras | |
| `width` / `height` | 1024² SDXL, 512² SD1.5 | |
| `clip_skip` | 2 for anime models | |

Prompt budget: the CLIP text encoder reads 77 tokens; UIs such as A1111 split longer prompts
into chunks, so keep the terms that matter in the first ~75.

Standard negative prompt:

```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit,
fewer digits, cropped, worst quality, low quality, normal quality,
jpeg artifacts, signature, watermark, username, blurry
```

Portraits add: `deformed iris, deformed pupils, mutated hands and fingers, poorly drawn,
wrong anatomy`. Per-category additions: `meta-schema.md` → 常見失敗.
