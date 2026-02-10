[English](README.md) | [繁體中文](README.zh.md)

# image-prompt

一個 Claude 技能，將模糊描述轉換為專業、結構化的圖片生成提示詞，相容於 Midjourney、DALL-E、Flux、Stable Diffusion 和其他文字生圖模型。

## 功能特色

- 從自然語言描述解析使用者意圖（支援任何語言，包括中文和日文）
- 使用 **7 要素框架** 建構結構化提示詞：主體、風格、構圖、光線、色彩調性、細節與質感、氛圍
- 附加平台感知的品質增強詞，並產生負面提示詞以避免常見瑕疵
- 同時輸出可直接使用的提示詞字串和結構化 JSON 格式（供 API/MCP 整合）
- 根據所要求的風格建議適當的長寬比和推薦模型

## 安裝

將技能目錄複製到 Claude 技能資料夾：

```
~/.claude/skills/image-prompt/
```

目錄應包含：

```
SKILL.md
references/
  platform-guide.md
  style-dictionary.md
```

## 使用方式

透過要求 Claude 產生圖片提示詞來觸發此技能。範例用語：

- 「Generate an image prompt for a cyberpunk cityscape at night」
- 「Write a prompt for AI art of a cat sitting in a sunlit garden」
- 「幫我寫生圖提示詞：一個女孩在櫻花樹下看書」
- 「Optimize this image prompt: a dragon flying over mountains」

技能會回傳完整的提示詞，包含負面提示詞、風格預設、長寬比建議和推薦模型。

## 參考檔案

- **`references/platform-guide.md`** — 各平台專屬語法（Midjourney、DALL-E、Flux、Stable Diffusion）
- **`references/style-dictionary.md`** — 200+ 精選風格、情緒、光線和構圖關鍵字

## 授權

MIT
