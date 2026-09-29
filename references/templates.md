# Category Templates

Paragraph templates for Gemini — use them when the user asks for paragraph prompts or the layout
is fixed. Not A/B-tested here; in the 2026-09-29 study paragraph prompts cut text errors but
scored lower on aesthetics than the 7-component format.

Fill the brackets with concrete, visible detail, then read the result as a
paragraph — if a filled slot still reads generic ("a nice background"), it is not filled yet.

Templates marked **[Google]** are verbatim from Google's "How to prompt Gemini 2.5 Flash Image
Generation for the best results" (Google Developers Blog, checked 2026-09-29). Templates marked
**[ours]** follow the same shape for categories the guide does not cover.

## Photorealistic scene [Google]

```
A photorealistic [shot type] of [subject], [action or expression], set in [environment]. The
scene is illuminated by [lighting description], creating a [mood] atmosphere. Captured with a
[camera/lens details], emphasizing [key textures and details]. The image should be in a
[aspect ratio] format.
```

## Stylized illustration / sticker [Google]

```
A [style] sticker of a [subject], featuring [key characteristics] and a [color palette]. The
design should have [line style] and [shading style]. The background must be white.
```

Drop the last sentence for a full illustration and describe the background instead.

## Text in the image — logo, poster, cover [Google]

```
Create a [image type] for [brand/concept] with the text "[text to render]" in a [font style].
The design should be [style description], with a [color scheme].
```

Add placement and hierarchy for posters: where the title sits, what else is on the page, and
how much empty space surrounds the type.

## Product shot [Google]

```
A high-resolution, studio-lit product photograph of a [product description] on a [background
surface/description]. The lighting is a [lighting setup, e.g., three-point softbox setup] to
[lighting purpose]. The camera angle is a [angle type] to showcase [specific feature].
Ultra-realistic, with sharp focus on [key detail]. [Aspect ratio].
```

Fill `[lighting setup]` with the effect ("soft light from the upper left, a thin bright rim along
the cup"), not the gear: filled with "a large softbox", Gemini drew the softbox into the frame.

## Minimalist / negative space [Google]

```
A minimalist composition featuring a single [subject] positioned in the [bottom-right/top-left/
etc.] of the frame. The background is a vast, empty [color] canvas, creating significant
negative space. Soft, subtle lighting. [Aspect ratio].
```

## Comic panel / storyboard frame [Google]

```
A single comic book panel in a [art style] style. In the foreground, [character description and
action]. In the background, [setting details]. The panel has a [dialogue/caption box] with the
text "[Text]". The lighting creates a [mood] mood. [Aspect ratio].
```

## Illustration with a named look [ours]

```
A [era/region/publication] [medium] illustration of [subject and action], [setting]. [How the
marks are made: line quality, brush or pencil, paper grain, how colour is laid down — flat
blocks, washes, halftone]. Palette: [4–5 named colours]. [Mood]. [Aspect ratio].
```

The mark-making sentence carries most of the style; "watercolor illustration" alone lets the
model fall back to its default look.

## Infographic / explainer diagram [ours]

```
A clean [flat vector / hand-drawn / isometric] infographic explaining [topic] for [audience].
[Central visual and what it shows]. [Each labelled part: the exact label text in quotes and where
it sits, and the arrows or flow between parts]. [Title text in quotes and its position].
[Palette]. White background, generous spacing between labels. [Aspect ratio].
```

Keep labels to a few words each; every extra word is another chance for a misspelling.

## Collectible figure / toy [ours]

```
A photograph of a [scale, e.g. 1/7] collectible figure of [subject], [pose], made of [glossy PVC
/ matte vinyl / resin] with [paint details], standing on [base] on [real surface]. Beside it,
[packaging: box shape, window, printed artwork]. [Lighting]. [Lens and depth of field].
[Aspect ratio].
```

Describing it as a photograph of a physical object keeps the result from looking like a render.
