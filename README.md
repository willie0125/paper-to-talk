# paper-to-talk：論文 → 10 分鐘研討會口頭報告

這是一個 Claude Skill。它會把研討會論文（可搭配完整研究，例如期刊版或學位論文）整理成一套報告材料，包括：

- 論文解析
- 簡報總覽
- 可以直接交給生成式 AI 製作 PowerPoint 的逐頁規格，含 Morph／轉化動畫和講稿
- 主講內容與備用頁的建議

> A Claude skill that turns a research paper into a 10-minute conference talk: an evidence-graded paper analysis, a deck plan, a slide-by-slide PowerPoint build spec (with Morph transitions and speaker notes), and core-vs-backup slide recommendations. Documentation is in Traditional Chinese.

## 會產出什麼

產出的檔案會放在論文所在資料夾裡的 `talk-prep/` 資料夾。

| 階段 | 內容 | 產出檔 |
|---|---|---|
| 0 盤點 | 分辨研討會版與完整研究，讀完全部圖表，建立圖表索引與版本差異表 | `00-inventory.md` |
| 1 論文解析 | 依 10 個項目整理：背景、文獻缺口、研究問題、故事線、實驗設計、圖表、統計、機制、貢獻與限制、可能被追問的問題 | `01-paper-analysis.md` |
| 2 簡報總覽 | 依論證鏈規劃逐頁總覽表、時間核算、轉化序列，以及關鍵訊息在各頁的鋪陳與收回 | `02-deck-plan.md` |
| 3 逐頁規格 | 每張投影片的完整文字、版面（cm）、配圖要求、物件名稱、動畫、跨頁轉化、講稿與銜接句 | `03-slide-specs.md` |
| 4 取捨建議 | 主講內容與備用頁清單，以及超時時的刪減順序 | `04-core-vs-backup.md` |

## 安裝

### Claude Code

執行下面這行，就會把 skill 下載到你的個人 skills 資料夾：

```bash
git clone https://github.com/willie0125/paper-to-talk.git ~/.claude/skills/paper-to-talk
```

日後要更新：

```bash
cd ~/.claude/skills/paper-to-talk && git pull
```

如果只想在某個專案中使用，把它 clone 到該專案的 `.claude/skills/paper-to-talk`。

### Claude.ai（網頁版或桌面版）

在 GitHub 頁面點「Code → Download ZIP」下載本 repo，再到 claude.ai 的 Skills 設定中上傳這個 ZIP 檔。你的帳號需要已經開啟 Skills 功能。

### 其他 AI 工具（ChatGPT、Gemini、Copilot 等）

不需要安裝。打開 [`docs/研討會口頭報告_AI提示詞套組.md`](docs/研討會口頭報告_AI提示詞套組.md) 或同名的 `.docx` 檔，在同一個對話中依序貼上四段提示詞即可。

## 使用方式（Claude Code）

1. 把論文 PDF 放進一個資料夾，在該資料夾中啟動 Claude Code。
2. 用一句話說明需求，例如：

   > 幫我用這篇論文準備 10 分鐘的研討會口頭報告。研討會版是 `conference.pdf`，完整研究是 `thesis.pdf`。

   也可以直接輸入 `/paper-to-talk`。
3. Claude 完成「簡報總覽」後會先停下來，請你確認頁面架構，再展開逐頁規格。如果想一次做完，開頭就說「全部階段一次完成」。
4. 把 `03-slide-specs.md` 交給生成式 AI 製作 PowerPoint，或自己照著規格製作。
   - 若要讓 Claude 直接產生 `.pptx`，需要另外安裝 Anthropic 的 pptx skill（[anthropics/skills](https://github.com/anthropics/skills)），然後說「依規格直接產生 .pptx」。

報告長度不是 10 分鐘時，直接告訴 Claude 長度，時間分配會按比例調整。

## 設計原則

- **研討會版優先**：實驗設計、數據和結論以研討會論文為準；完整研究只用來補充背景、方法細節和機制說明。兩版數字不一致時，會列出差異。
- **證據分級**：每條陳述都會標上【實測】【文獻】【作者推論】【推算】【未交代】【建議】其中之一，講稿措辭也會配合，例如推論不會被說成實測。
- **出處可追溯**：所有數字、圖表和機制都附上頁碼、圖號或表號。
- **不補資料**：論文沒有交代的內容會標為【未交代】，不會自行補上實驗、數據或因果關係。
- **轉化動畫可直接製作**：跨頁物件一律用 `!!` 前綴命名，PowerPoint 的轉化（Morph）就能穩定比對同一個物件；微觀或化學機制會分成「初始狀態 → 作用過程 → 結果」逐頁呈現。

## 檔案結構

```
paper-to-talk/
├── SKILL.md                          # 流程總覽、核心原則、交付前自檢
├── references/
│   ├── stage1-paper-analysis.md      # 論文解析規格與輸出模板
│   ├── stage2-deck-plan.md           # 簡報總覽規格、時間預算、轉化序列
│   ├── stage3-slide-specs.md         # 逐頁規格：全域設計、命名、動畫、範例
│   ├── stage4-core-vs-backup.md      # 主講與備用頁的取捨規格
│   └── prompts.md                    # 精簡版提示詞（給 skill 使用者參考）
└── docs/
    ├── 研討會口頭報告_AI提示詞套組.md     # 給其他 AI 工具用的完整提示詞
    └── 研討會口頭報告_AI提示詞套組.docx   # 同上，Word 版，方便分享
```

## 限制

- 產出的品質取決於 PDF 能否被正確讀取。掃描檔請先做文字辨識（OCR）。
- 講述時間和講稿字數都是估算值，請實際彩排確認。
- 生成式 AI 照規格製作 PowerPoint 時，物件命名和轉化設定可能仍需要手動檢查。
- 論文內容的正確性，最終仍需報告者自行確認。

## 授權

[MIT](LICENSE)
