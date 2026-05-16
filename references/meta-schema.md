# Composable JSON Meta-Schema — Advanced Prompt Architecture

蠶食自 ConardLi/garden-skills `gpt-image-2/references/prompt-writing.md:119-250`（2026-05-16）。

**何時用本檔**：當 7-component framework（SKILL.md Step 2）對特定場景（character variants、product 系列、poster 系列、storyboard）顯得不夠結構化、需要參數化 / 可重用 / 跨多張一致時。一般單張快圖直接用 7-component 即可。

**何時 NOT 用**：單張快圖、嘗試性探索、user 只想要一個 prompt string 不要 JSON。

## Base Schema（任何場景皆有）

```json
{
  "type": "<category>",          // photo / illustration / product / character / poster / ui-mockup / storyboard / academic-infographic / etc
  "goal": "<one-line intent>",    // "招聘 LinkedIn cover 用" / "教學 slide background"
  "subject": {
    "primary": "<core noun phrase>",
    "secondary": ["<list of supporting elements>"],
    "count": <int|"unspecified">
  },
  "scene": {
    "setting": "<where>",         // "mountain ridge at dawn"
    "time": "<when>",             // "golden hour" / "midnight"
    "atmosphere": "<mood>"        // "tense, anticipatory"
  },
  "layout": {
    "composition": "<rule>",      // "rule of thirds, subject left"
    "perspective": "<angle>",     // "low angle, slight Dutch tilt"
    "depth": "<depth cues>"       // "shallow DoF, bokeh background"
  },
  "style": {
    "medium": "<art medium>",     // "oil painting" / "3D render" / "35mm film"
    "references": ["<artist / studio / movement>"],
    "render_engine": "<optional>" // "Octane" / "Unreal 5"
  },
  "details": {
    "texture": "<surface qualities>",
    "lighting": "<light setup>",
    "color_palette": "<dominant + accent>"
  },
  "constraints": {
    "must_include": ["<non-negotiable elements>"],
    "must_exclude": ["<things to keep out>"],
    "aspect_ratio": "<W:H>",
    "quality": "<universal boosters>"
  }
}
```

## 9 Scope-Specific Extensions（按 type 補充）

只在 base 對應 `type` 時加入。**不要把所有 extension 塞進每個 prompt**。

### `type: "character"` — 角色設計 / 變體
```json
"character": {
  "base_design": "<canonical look>",   // 角色固定設定（保證跨變體一致）
  "variant_axis": "<what changes>",    // "wardrobe" / "expression" / "age"
  "variant_value": "<this variant>",   // "casual streetwear"
  "consistency_anchors": ["<facial features>", "<hair>", "<signature accessory>"]
}
```

### `type: "product"` — 商品攝影 / 渲染
```json
"product": {
  "brand_voice": "<premium / playful / utilitarian>",
  "surface_treatment": "<matte / glossy / brushed>",
  "context": "<lifestyle / studio / in-use>",
  "scale_reference": "<optional human / object>"
}
```

### `type: "poster"` — 海報 / 活動視覺
```json
"poster": {
  "hierarchy": ["<headline>", "<sub-headline>", "<date/location>"],
  "negative_space": "<top / left / center>",  // 留白給 typography overlay
  "format": "<portrait / landscape / square>",
  "print_safe_margin": <bool>
}
```

### `type: "ui-mockup"` — UI 模板 / dashboard
```json
"ui_mockup": {
  "platform": "<web / iOS / Android / desktop>",
  "framework_hint": "<material / HIG / fluent / custom>",
  "components_visible": ["<list>"],
  "data_density": "<sparse / medium / dense>"
}
```

### `type: "storyboard"` — 分鏡 / 多格敘事
```json
"storyboard": {
  "frame_index": <int>,
  "total_frames": <int>,
  "shot_size": "<wide / medium / close-up / extreme close>",
  "action": "<beat description>",
  "continuity_from": "<previous frame ref>"
}
```

### `type: "academic-infographic"` — 學術圖示 / 流程圖
```json
"academic": {
  "discipline": "<biology / engineering / cs / etc>",
  "diagram_type": "<flowchart / cross-section / annotated>",
  "labels_needed": <bool>,
  "publication_context": "<journal / textbook / slide>"
}
```

### `type: "map"` — 地圖 / 地理視覺
```json
"map": {
  "projection": "<satellite / topographic / isometric / fantasy>",
  "annotations": ["<list of POIs>"],
  "scale_visible": <bool>,
  "split_columns": <bool>  // dual-column comparison map
}
```

### `type: "branding"` — 品牌資產 / logo applied
```json
"branding": {
  "logo_treatment": "<lockup / icon-only / wordmark>",
  "color_system": "<primary / secondary / tertiary>",
  "applied_surface": "<business card / signage / packaging>"
}
```

### `type: "avatar"` — 大頭照 / profile
```json
"avatar": {
  "use_case": "<social / corporate / gaming>",
  "framing": "<headshot / bust / half-body>",
  "background_treatment": "<solid / gradient / contextual>"
}
```

## Parameter Classification Tiers — 防 over-asking

每個 schema field 屬於三類之一。**只追問 Must-Ask；其餘可預設或隨機**。

| Tier | 含義 | 範例 fields |
|---|---|---|
| **Must-Ask** | 缺則 prompt 失意義，必須問 user | `goal`, `subject.primary`, `type`（若不明顯） |
| **Can-Default** | 有合理預設，user 沒提就用 best practice | `quality`（universal boosters）, `aspect_ratio`（按 type 預設）, `negative_prompt`（按 type 預設） |
| **Can-Randomize** | user 沒提就讓 model 自由發揮，反而更有創意 | `style.references`, `details.color_palette`, `layout.perspective`（無構圖偏好時）|

**Anti-pattern**：把 Can-Default 當 Must-Ask 問 → user 體驗變表單填寫。

## Negative Prompt — 按 type 分類

舊版（SKILL.md Step 4）用 universal negative prompt。本檔升級為 category-specific：

### `type: "character" | "avatar"` — 人像
基礎 + `extra fingers, missing fingers, deformed hands, asymmetric eyes, fused features`

### `type: "product"` — 商品
基礎 + `text overlay, watermark, logo distortion, blurred edges, inaccurate proportions`

### `type: "ui-mockup"` — UI
基礎 + `unreadable text, broken layout, misaligned elements, lorem ipsum visible, oversaturated buttons`

### `type: "academic-infographic"` — 學術
基礎 + `fictional labels, wrong arrows, misleading scale, decorative noise`

### `type: "poster"` — 海報
基礎 + `cluttered hierarchy, illegible headline, no negative space, generic stock photo`

### `type: "storyboard"` — 分鏡
基礎 + `continuity break, inconsistent character, scale drift between frames`

### `type: "photo" | "illustration"`（無特殊 type）
原 SKILL.md universal negative prompt 即可。

## Atomic Template File 模式

對於常用組合（如「LinkedIn cover」「Notion banner」「製品 hero shot」），建議**每個 use case 一個 .md 範本檔**，內含一份填好的 meta-schema JSON + 一句使用說明。

範本檔放在 `references/templates/<use-case-slug>.md`，**不要把多個 use case 塞進同一個檔案**（gpt-image-2 教訓：18 分類各 atomic 才不會 cross-contamination）。

範本檔格式：
```markdown
# Use Case: LinkedIn Cover (Tech Recruiter)

**何時用**: 招聘性質的 LinkedIn 個人頁封面
**輸出 ratio**: 4:1 (1584×396)

\```json
{
  "type": "branding",
  "goal": "...",
  ...
}
\```
```

## Phase Gating（何時讀本檔）

- **Step 1 Parse User Intent** 之後判斷：subject 是 character / product 系列 / 跨多張一致？→ 讀本檔切換 schema 模式
- **單張快圖場景** → 不讀，直接用 SKILL.md 7-component
- **user 明確要 JSON output** → 讀本檔取 schema

## 與 SKILL.md 7-component 的關係

| 場景 | 用哪個 |
|---|---|
| 單張快圖、探索性、user 想要 prompt string | 7-component（SKILL.md Step 2）|
| 多張變體一致、需要 JSON、要參數化重用 | Meta-schema（本檔）|
| Hybrid: 用 schema 生成後輸出 7-component string | 兩個結合，schema → render → string |

7-component 沒被 deprecate，是「快通用」；本檔是「結構化進階」。
