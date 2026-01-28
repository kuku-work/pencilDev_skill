# Pencil.dev 提示詞撰寫方法論完整指南

**Pencil.dev 是一個革命性的 MCP 驅動設計畫布工具**，它直接整合到 IDE 中，讓開發者能透過自然語言提示詞將視覺設計轉化為程式碼。本指南彙整官方範例、社群經驗與跨平台最佳實踐，提供一套系統化的提示詞撰寫方法論。核心發現：**精確具體的提示詞比簡短模糊的提示詞產生的程式碼快 19 秒、少 152 行、且成本更低**。掌握「選取設計 → 明確風格 → 漸進迭代」的三段式模式，是有效使用 Pencil.dev 的關鍵。

---

## 平台架構與 MCP 協議如何運作

Pencil.dev 由 Tom Krcha 創建並獲得 a16z 投資，其核心創新在於**設計檔案直接存放於 Git 儲存庫中**，使用開放格式讓 AI 代理能夠讀寫。與傳統設計工具不同，Pencil 的 MCP（Model Context Protocol）架構實現雙向溝通：AI 不僅能讀取當前設計狀態，還能根據自然語言指令直接修改設計。

技術流程為：`使用者提示詞 → MCP 客戶端（Cursor/VS Code）→ Pencil MCP 伺服器 → 畫布 API → 設計更新 → 程式碼生成`。這意味著在畫布上繪製的元素會即時生成對應的 HTML/CSS，而 AI 生成的設計變更也會同步反映在程式碼中。設置時需在專案中建立 `.cursor/mcp.json` 配置檔，啟用後在 Cursor 聊天中輸入「**Open pencil canvas**」即可開始。

Pencil 的三大核心功能徹底改變了設計工作流：**Notes（便利貼）** 可直接轉換為可執行提示詞；**Context（情境）** 提供 AI 背景資訊；**Prompt Cards（提示卡）** 讓設計師能在聊天視窗外執行 AI 指令。這創造出一個「**代理工作空間**」而非靜態設計檔案。

---

## 官方提示詞範例庫：12 個核心模式

官方提示詞範例庫（pencil.dev/prompts）展示了四大類別的提示詞模式，每個模式都經過驗證能產生高品質結果：

### 模式一：全新設計生成

| 範例 | 結構分析 |
|------|----------|
| 「Design a web app for managing rocket launches. Use a technical style.」 | 平台類型 + 用途 + 風格描述 |
| 「Design a mobile app for tracking music royalties. Use a Scandinavian minimalistic style.」 | 指定行動應用 + 具體領域 + 美學風格 |
| 「Design a website for a specialty cafe in Haight Ashbury, San Francisco.」 | 網站類型 + 商業類型 + 地理情境 |

**關鍵語法**：`Design a [平台類型] for [具體用途]. Use a [風格描述] style.`

### 模式二：基於選取的迭代設計

這是 Pencil.dev 最強大的功能，透過參照畫布上已選取的元素進行精確修改：

- 「**Look at the selected design.** Explore a totally different design direction.」（探索全新方向）
- 「**Look at the selected design.** Explore a different layout, but keep the current design direction. Create a new design for it.」（保持風格、改變佈局）
- 「**Look at the selected design.** Change it to the light mode. Create a new design for it.」（主題切換）
- 「**Look at the selected design.** Change this to a simpler and cleaner design direction. Create a new design for it.」（簡化設計）

**必記句型**：`Look at the selected design. [動作指令]. Create a new design for it.`

### 模式三：對話延續迭代

在同一個聊天對話中持續優化，每次專注於單一變更：

```
第一輪：「Let's go more bold and rock'n'roll, make the headline much larger, 
        drop boxes around prompts and just focus on typography, one column layout, 
        most emphasis on prompts, everything else secondary, Swiss layout.」

第二輪：「That's great. Now change fonts to something more classy. Create a new design for it.」

第三輪：「Great! Now use a sidenav. Create a new design for it.」
```

這種漸進式方法能保持設計一致性，同時允許精細調整。

### 模式四：情境感知整合

將設計與現有程式碼專案連結：

```
「Look at the selected design. Adjust the prompts page code to reflect it. 
You can find the images in the /public folder. Don't worry about header 
and footer, keep it as it is on the current page.」

「Use the selected design as the base design, that's my current app. 
Design a new page, the Missions page now. Create a new design for it.」
```

---

## 提示詞結構最佳實踐：三層框架

綜合 Pencil.dev 官方範例與 Vercel v0.dev 的驗證研究，**高效提示詞遵循三層框架**：

### 第一層：產品表面（Product Surface）

不要寫「一個儀表板」，而要列出具體的資料、元件與操作：

| ❌ 模糊寫法 | ✅ 精確寫法 |
|------------|-------------|
| 「Build a user profile page」 | 「Build a user profile page showing: profile photo, display name, username, email, bio, member since date, activity stats (posts, comments, followers), edit profile and settings buttons」 |
| 「Design a dashboard」 | 「Dashboard displaying: top 5 performers with names and revenue, team revenue vs quota progress bar, deal pipeline with stages」 |

### 第二層：使用情境（Context of Use）

明確說明**誰**在**何時**使用，以及**為什麼**：

```
✅ 「Dashboard for sales managers (non-technical) checking during morning 
    standups on desktop monitors to quickly spot underperformers」
```

這層資訊幫助 AI 做出適當的設計決策，例如為非技術使用者提供更直觀的介面。

### 第三層：約束與風格（Constraints & Taste）

定義視覺風格、平台限制與排除項目：

```
✅ 「Card-based layout with clear hierarchy. Color code: green for on-track, 
    yellow for at-risk. Desktop-first. Professional but approachable.」
    
✅ 「Mobile-first, light theme, high contrast. Don't include navigation footer.」
```

**Vercel 測試數據顯示**：具體的產品表面描述使生成時間減少 19 秒（從 1分38秒 到 1分19秒），程式碼行數減少 152 行（從 595 行到 443 行），成本也降低 7.5%。

---

## 社群發現的實戰技巧

### 便利貼作為可執行提示詞

社群使用者發現最有效的工作流之一是**將便利貼轉換為提示詞**。在畫布上放置便利貼標註設計變更需求，然後「執行」它們。這讓畫布成為一個空間化的提示詞組織工具，特別適合複雜的多元件設計。

### 混合編輯策略

**手動編輯小調整、提示詞處理大變更**。Pencil 提供類似 Figma 的圖層面板和 CSS 屬性編輯器，對於更改文字、微調間距等小修改，直接手動操作比撰寫提示詞更快且不消耗 Claude Code tokens。

### 風格錨點關鍵詞

社群驗證的高效風格描述包括：
- **佈局風格**：Swiss layout、one column layout、use a sidenav
- **美學方向**：Scandinavian minimalistic、technical style、bold and rock'n'roll
- **設計原則**：focus on typography、most emphasis on [element]、everything else secondary

### Figma 到 Pencil 工作流

最佳實踐是：`Figma（探索階段）→ 複製貼上 → Pencil（生產階段）→ Git 提交`。從 Figma 複製的向量圖、文字和樣式會完整保留，不會出現 SVG 損壞或字型遺失問題。

---

## 必須避免的反模式

| 反模式 | 問題 | 解決方案 |
|--------|------|----------|
| **模糊的單句提示詞** | 「Design a nice landing page」會讓 AI 猜測，產生通用的紫色 UI 和錯誤的間距層級 | 明確指定元件、資料和互動 |
| **單一提示詞包含過多任務** | 嘗試在一個提示詞中完成整個應用會導致混亂且難以調整的結果 | 每個提示詞專注於一個畫面、一個變更或一個方向 |
| **未參照選取的設計** | 提示詞與畫布上現有設計脫節，產生不一致的輸出 | 始終使用「Look at the selected design」建立情境 |
| **忽略約束條件** | AI 會自行發明不需要的功能 | 明確說明「Don't include...」或「Keep it as it is...」 |
| **跳過線框圖階段** | 直接從概念跳到提示詞會缺乏結構 | 先在 Figma、Whimsical 或紙上繪製草圖 |

---

## 實用提示詞模板庫

### 模板 A：全新應用設計

```
Design a [mobile app/web app/website] for [具體用途/領域]. 
Use a [風格描述] style.
Include: [元件列表，用逗號分隔].
Target users: [使用者描述].
```

**實例**：
```
Design a mobile app for tracking personal finances. 
Use a minimal and calming style.
Include: monthly spending chart, category breakdown, recent transactions list, 
budget progress bars, add expense button.
Target users: young professionals who check their spending during commute.
```

### 模板 B：設計迭代

```
Look at the selected design. [變更指令]. Create a new design for it.

[選用] Keep: [保留的元素]
[選用] Change: [要改變的具體項目]
[選用] Don't touch: [不要修改的部分]
```

**實例**：
```
Look at the selected design. Change the layout to a two-column grid 
with sidebar navigation on the left. Create a new design for it.
Keep: current color palette and typography.
Change: card spacing to 24px.
Don't touch: header and footer.
```

### 模板 C：風格探索

```
Look at the selected design. 
Explore a [totally different / slightly modified] design direction 
with [風格關鍵詞]: [具體風格描述].
Create a new design for it.
```

**實例**：
```
Look at the selected design. 
Explore a totally different design direction 
with emphasis on typography: large bold headlines, minimal decorative elements, 
generous white space, monochromatic color scheme with one accent color.
Create a new design for it.
```

### 模板 D：從現有設計延伸新頁面

```
Use the selected design as the base design, that's my current app.
Design a new page, the [頁面名稱] page now.
Include: [該頁面的元件和功能].
Match the existing visual style.
Create a new design for it.
```

---

## 與類似工具的最佳實踐對照

從 Vercel v0.dev、Builder.io 和 Locofy 等工具的最佳實踐中，可以萃取出適用於 Pencil.dev 的通用原則：

**來自 v0.dev 的三輸入框架**已被驗證能顯著提升結果品質：
1. **產品表面**：具體列出元件、資料、操作
2. **使用情境**：定義使用者、時機、目的
3. **約束條件**：指定平台、視覺風格、排除項目

**來自 Builder.io 的程式碼匹配原則**：
- 提供現有程式碼範例讓 AI 模仿風格
- 使用 context file 建立大型程式碼庫的情境
- 在通用指令中納入 WCAG 無障礙要求

**Pencil.dev 獨特優勢**：與單向的 Figma 匯出不同，MCP 架構實現持續的設計-程式碼同步，這意味著**迭代式優化比完整重新生成更有效率**。

---

## 進階技巧：情境感知與除錯

### 錯誤處理提示詞

當生成結果有問題時，直接將錯誤訊息餵給 AI：
```
Here's the error I'm seeing: [貼上完整錯誤訊息]. 
Fix the selected design to resolve this issue.
```

### 框架指定

在提示詞早期明確技術棧：
```
Generate code using React, Next.js 14, Tailwind CSS, and shadcn/ui components.
Look at the selected design and implement it following existing patterns 
in src/components/.
```

### 漸進式複雜度建構

對於複雜頁面，採用分層方法：
```
第一輪：「Design the basic layout structure with header, main content area, 
        and footer placeholders.」
第二輪：「Look at the selected design. Now add the navigation menu 
        with 5 items: Home, Products, About, Blog, Contact.」
第三輪：「Look at the selected design. Add hero section with headline, 
        subheadline, and CTA button.」
```

---

## 結論：提示詞撰寫的核心原則

成功使用 Pencil.dev 的關鍵在於理解它是一個**代理工作空間而非靜態設計工具**。AI 能夠讀取畫布狀態、理解程式碼情境，並執行精確的設計操作。

**五大核心原則**：

1. **精確勝於簡潔**：詳細的提示詞產生更快、更精準的結果，且成本更低
2. **選取建立情境**：始終參照「the selected design」讓 AI 理解你在修改什麼
3. **漸進式迭代**：每次提示詞專注於單一變更，保持對話延續以維護一致性
4. **風格錨點明確**：使用具體的風格描述詞（Swiss layout、Scandinavian minimalistic）而非模糊的「好看」
5. **約束防止發散**：明確告訴 AI 不要做什麼，與告訴它要做什麼同樣重要

Pencil.dev 的 MCP 原生架構代表設計工具的典範轉移：設計檔案不再是需要「交接」的靜態產物，而是與程式碼同步版本控制的活文件。掌握其提示詞系統，就是掌握這個新工作流的核心技能。