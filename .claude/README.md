# Pencil.dev 技能系統技術文件

這是 Pencil.dev AI 設計助手的技術文件，說明 Skill 與 Command 的架構設計。

---

## 架構概覽

本系統採用雙軌制設計：

| 類型 | 觸發方式 | 適用場景 |
|------|----------|----------|
| **Skill** | AI 自動判斷對話內容 | 自然對話，無需記指令 |
| **Command** | 使用者輸入 `/指令` | 明確控制特定功能 |

```
.claude/
├── skills/                      # 自動觸發
│   ├── pencil-design/           # 設計助手
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── workflows.md
│   │       ├── prompt-templates.md
│   │       └── style-keywords.md
│   │
│   └── figma-to-pencil/         # Figma 匯入解析
│       ├── SKILL.md
│       └── references/
│           ├── migration-guide.md
│           └── common-patterns.md
│
└── commands/                    # 手動觸發
    ├── pencil.md                # 智能路由
    ├── design.md                # 複雜工作流
    ├── figma-parse.md           # Figma 解析
    └── pencil-*.md              # 專用指令
```

---

## Skill 架構詳解

### 什麼是 Skill？

Skill 是 Claude Code 的「model-invoked」功能。AI 會根據對話內容自動判斷是否啟動，使用者不需要輸入任何指令。

### SKILL.md 檔案結構

```yaml
---
name: pencil-design              # 必填：1-64 字元，小寫+數字+連字號
description: |                   # 必填：1-1024 字元，決定何時觸發
  Automatically activates when user asks to design...
---

# 技能內容

這裡寫 AI 應該如何執行這個技能的詳細指令。
```

### name 命名規則

- 長度：1-64 字元
- 允許字元：小寫字母、數字、連字號
- 禁止：開頭/結尾連字號、連續連字號
- **必須與資料夾名稱完全相同**

### description 的重要性

**description 是整個 Skill 最關鍵的部分**，它決定 AI 何時啟動這個技能。

好的 description 應該：
- 明確列出觸發條件
- 包含關鍵詞和同義詞
- 涵蓋中英文表達

範例：
```yaml
description: |
  Automatically activates when user:
  - Asks to design, create, or build UI/UX
  - Wants to modify or iterate existing designs
  - Mentions .pen files or Pencil.dev
  - Uses keywords: wireframe, mockup, prototype
```

### references 資料夾

存放 SKILL.md 引用的參考文件：

```
references/
├── workflows.md        # 工作流程實作細節
├── prompt-templates.md # 提示詞模板庫
└── style-keywords.md   # 風格關鍵詞參考
```

AI 透過 SKILL.md 中的明確引用來發現這些檔案。

### scripts 資料夾（可選）

存放可執行腳本（Python、Bash、JavaScript）：

```
scripts/
├── validate-design.py  # 設計驗證腳本
└── export-tokens.js    # 導出設計 token
```

---

## Command 架構詳解

### 什麼是 Command？

Command 是傳統的 Slash Command，需要使用者輸入 `/指令名` 來觸發。

### 檔案結構

```markdown
# 指令標題

指令說明和 AI 應該執行的步驟。

## Instructions

詳細的執行指令...
```

### 可用的 Commands

| 指令 | 檔案 | 用途 |
|------|------|------|
| `/pencil` | pencil.md | 智能路由，自動判斷意圖 |
| `/design` | design.md | 複雜多頁面工作流 |
| `/figma-parse` | figma-parse.md | 解析 Figma 匯入的設計 |
| `/pencil-new` | pencil-new.md | 建立全新設計 |
| `/pencil-iterate` | pencil-iterate.md | 修改現有設計 |
| `/pencil-explore` | pencil-explore.md | 探索風格方向 |
| `/pencil-extend` | pencil-extend.md | 延伸到新頁面 |
| `/pencil-code` | pencil-code.md | 產生程式碼 |
| `/pencil-fix` | pencil-fix.md | 診斷修復問題 |

---

## Skill vs Command 設計哲學

### 何時用 Skill

- 使用者可能不知道有這個功能
- 希望 AI 主動識別需求
- 對話導向的使用體驗
- 功能範圍較廣（涵蓋多種意圖）

### 何時用 Command

- 使用者明確知道要做什麼
- 需要精確控制執行流程
- 功能範圍較窄（單一明確任務）
- 需要傳入參數

### 本系統的設計

```
pencil-design (Skill)
    │
    │ 自動觸發，判斷意圖
    │
    ├─→ NEW_DESIGN    → 類似 /pencil-new
    ├─→ ITERATE       → 類似 /pencil-iterate
    ├─→ EXPLORE       → 類似 /pencil-explore
    ├─→ EXTEND        → 類似 /pencil-extend
    ├─→ CODE          → 類似 /pencil-code
    └─→ FIX           → 類似 /pencil-fix
```

Skill 作為智能入口，內部路由到與 Commands 相同的邏輯。

---

## 意圖識別系統

### 觸發訊號對照表

| 意圖 | 中文觸發詞 | 英文觸發詞 |
|------|------------|------------|
| NEW_DESIGN | 設計、做一個、建立、創建 | Design, Create, Build, Make |
| ITERATE | 改成、換成、調整、修改 | Change, Modify, Adjust, Update |
| EXPLORE | 試試看、不同風格、另一個方向 | Try, Explore, What if |
| EXTEND | 再做一頁、新增頁面、延伸 | Add page, New page, Extend |
| CODE | 產生程式碼、轉成 React | Generate code, Convert to |
| FIX | 壞了、錯誤、重疊、跑版 | Broken, Error, Overlapping |

### 決策矩陣

| 使用者說 | 有選取？ | 判定意圖 |
|----------|----------|----------|
| 做一個 App | No | NEW_DESIGN |
| 做一個 App | Yes | 詢問：新建或延伸？ |
| 改成藍色 | Yes | ITERATE |
| 改成藍色 | No | 詢問：哪個元素？ |
| 試試科技風 | Yes | EXPLORE |
| 加一個設定頁 | Yes | EXTEND |

---

## MCP 工具參考

### 常用工具

| 工具 | 用途 |
|------|------|
| `get_editor_state` | 取得畫布狀態和選取項 |
| `open_document` | 開啟或建立 .pen 檔案 |
| `batch_get` | 讀取節點結構 |
| `batch_design` | 建立/修改設計 |
| `get_screenshot` | 視覺驗證 |
| `snapshot_layout` | 偵測排版問題 |
| `get_style_guide_tags` | 取得風格選項 |
| `get_style_guide` | 取得風格靈感 |
| `get_guidelines` | 取得程式碼生成規則 |
| `get_variables` | 提取設計 token |

### 標準執行流程

```
1. get_editor_state     → 了解當前狀態
2. batch_get            → 讀取結構
3. get_style_guide      → 取得靈感（可選）
4. batch_design         → 執行操作
5. get_screenshot       → 驗證結果
```

---

## 最佳實踐

### 三層提示詞框架

1. **產品表面**：具體元件、資料、操作
2. **使用情境**：誰在用、何時用、為什麼
3. **約束條件**：風格、平台、排除項

### 對話延續性

- 記住當前設計任務
- 理解指代詞：「這個」「它」「剛剛那個」
- 累積多輪對話的需求
- 保持風格一致性

### 品質驗證

每次重大變更後：
1. `get_screenshot()` - 視覺檢查
2. `snapshot_layout(problemsOnly: true)` - 排版檢查
3. 對照原始需求

---

## 疑難排解

| 問題 | 解決方案 |
|------|----------|
| Skill 沒有觸發 | 檢查 description 是否涵蓋使用者的表達方式 |
| 意圖判斷錯誤 | 使用更明確的 Command |
| 沒有文件開啟 | 執行 `open_document("new")` |
| 沒有選取項 | 請使用者先在畫布上選取 |
| 排版問題 | 使用 `snapshot_layout(problemsOnly: true)` |

---

## 版本資訊

- **版本**：2.0.0
- **更新日期**：2025-01-28
- **架構**：Skill + Command 雙軌制
- **相容性**：Claude Code + Pencil.dev MCP
