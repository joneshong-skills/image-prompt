# gpt-image-2 模板索引

蠶食自 [ConardLi/garden-skills/gpt-image-2](https://github.com/ConardLi/garden-skills/tree/main/skills/gpt-image-2)。
17 個分類、93 個結構化模板，外加 `prompt-writing.md`（通用提示詞撰寫指南）。

每個 `.md` 模板針對一種具體輸出形式（海報、UI 樣機、信息圖、學術配圖、技術圖、漫畫分鏡等），
包含使用場景、必要元素清單、配色 / 構圖 / 排版建議，可直接套用或當作 prompt 骨架。

## 分類總覽

| # | 分類目錄 | 檔數 | 一句話用途 |
|---|---------|------|-----------|
| 1 | `academic-figures/` | 9 | 論文 Graphical Abstract、方法 pipeline、機制圖等學術發表用配圖 |
| 2 | `assets-and-props/` | 2 | 遊戲內截圖 mockup、擬物 / 復古圖標等素材道具 |
| 3 | `avatars-and-profile/` | 5 | 多版本角色網格、文化系列肖像、貼圖包等頭像 / Profile 視覺 |
| 4 | `branding-and-packaging/` | 6 | 飲料 / 食品標籤、品牌識別板、IP 周邊看板等品牌包裝設計 |
| 5 | `editing-workflows/` | 5 | 換背景、物件移除 / 替換等圖像編輯工作流（搭配 `scripts/edit.js`） |
| 6 | `grids-and-collages/` | 5 | 多 banner 拼貼、Anime pitch board、2×2 banner grid 等格狀 / 拼貼視覺 |
| 7 | `infographics/` | 6 | Bento grid、對比表、手繪信息圖等高密度資訊圖 |
| 8 | `maps/` | 5 | 城市美食地圖、插畫風城市地圖、行程地圖等手繪地圖視覺 |
| 9 | `portraits-and-characters/` | 5 | 角色設定三視圖、創辦人肖像、姿勢參考表等角色設計 |
| 10 | `poster-and-campaigns/` | 8 | Web hero banner、品牌海報、概念海報等海報 / 投放素材 |
| 11 | `product-visuals/` | 6 | 電商銷售看板、爆炸視圖、生活情境產品圖等商品視覺 |
| 12 | `scenes-and-illustrations/` | 4 | 電影感大場景、療癒場景、極簡氛圍場景等概念插畫 |
| 13 | `slides-and-visual-docs/` | 4 | 霞關風格高密度 slides、教育圖解、政策風 slides 等簡報式視覺 |
| 14 | `storyboards-and-sequences/` | 8 | 動漫 KV、角色關係圖、電影分鏡 grid 等敘事 / 分鏡視覺 |
| 15 | `technical-diagrams/` | 7 | ER 圖、決策流程圖、技術心智圖等工程感技術圖（位圖輸出） |
| 16 | `typography-and-text-layout/` | 2 | 雙語並置版式、標題安全區海報等字型 / 版式設計 |
| 17 | `ui-mockups/` | 6 | 聊天介面、Landing page case study、直播電商 UI 等介面樣機 |

**通用指南**: [`prompt-writing.md`](./prompt-writing.md) — 跨分類通用的提示詞撰寫法、結構與品質要點。

## 使用方式

1. 從上表找到最接近需求的分類。
2. `ls` 目錄看具體模板檔名，挑一個對應場景的 `.md`。
3. 直接讀該模板：通常前段是「適用場景」、中段是「必備元素 / 構圖 / 配色」、後段是 prompt 範例。
4. 把模板內容當骨架，用 image-prompt skill 的 7-Component Framework 補完細節。
