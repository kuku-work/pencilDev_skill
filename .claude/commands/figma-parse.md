# Figma to Pencil Parser

Parse and optimize designs imported from Figma into Pencil.dev.

## Instructions

You are a Figma-to-Pencil migration specialist. When user runs this command, perform comprehensive analysis of pasted Figma content.

### Step 1: Detect Imported Content

```
mcp__pencil__get_editor_state(include_schema: false)
```

If no selection, ask user to select the imported frame.

### Step 2: Deep Structure Analysis

```
mcp__pencil__batch_get(
  nodeIds: [selectedId],
  readDepth: 6,
  resolveInstances: true
)
```

Analyze for:

**Structure Metrics**
- Total node count
- Maximum nesting depth
- Frame vs Group ratio
- Component instance count

**Design Token Candidates**
- Unique colors (fill, stroke, text)
- Font families and sizes
- Spacing values (gap, padding, margin)
- Border radius values
- Shadow definitions

**Potential Issues**
- Nesting > 4 levels
- Absolute positioning in responsive containers
- Hardcoded values that should be tokens
- Empty or single-child containers
- Unnamed layers

### Step 3: Generate Analysis Report

Output format:

```markdown
# Figma 匯入分析報告

## 📊 結構統計

| 指標 | 數值 | 評估 |
|------|------|------|
| 總節點數 | X | - |
| 最深巢狀 | X 層 | ✅ 良好 / ⚠️ 過深 |
| Frame 數 | X | - |
| 元件實例 | X | - |
| 文字圖層 | X | - |

## 🎨 設計 Token 分析

### 顏色 (共 X 個)
| 色彩 | 使用次數 | 建議命名 |
|------|----------|----------|
| #3B82F6 | 15 | primary |
| #1F2937 | 23 | text-primary |
| #F3F4F6 | 8 | bg-secondary |

### 字型
| 家族 | 大小 | 權重 | 使用次數 |
|------|------|------|----------|
| Inter | 16px | 400 | 12 |
| Inter | 24px | 600 | 5 |

### 間距模式
- 常用間距：8, 16, 24, 32, 48
- Grid 基準：8px ✅

## ⚠️ 發現的問題

### 1. 過深巢狀
**位置**: `frame-abc > wrapper > container > inner > content`
**建議**: 移除 `wrapper` 和 `inner`，減少 2 層

### 2. 硬編碼顏色
**數量**: 15 處
**建議**: 提取為設計變數

### 3. 絕對定位
**位置**: `header > logo`, `card > badge`
**建議**: 考慮是否可改為相對定位

## 🔧 優化建議

### 優先級 1（強烈建議）
- [ ] 提取 5 個主要顏色為變數
- [ ] 扁平化 3 處過深巢狀

### 優先級 2（建議）
- [ ] 統一間距為 8px 倍數
- [ ] 為 12 個未命名圖層添加語意名稱

### 優先級 3（可選）
- [ ] 將重複元素轉為元件
- [ ] 清理 4 個空容器
```

### Step 4: Offer Optimization Actions

```
我可以幫你執行以下優化：

**A. 自動修復（推薦）**
   一鍵執行所有優先級 1 的修復

**B. 互動式修復**
   逐項確認每個修改

**C. 僅提取 Token**
   只建立設計變數，不改動結構

**D. 產生報告**
   匯出詳細報告，稍後手動處理

請選擇 A/B/C/D，或告訴我你想要什麼。
```

### Step 5: Execute Chosen Optimization

**Option A: Auto-fix**
```javascript
// 1. Extract tokens
mcp__pencil__set_variables({
  variables: { /* extracted tokens */ }
})

// 2. Flatten nesting
M("deep-child", "appropriate-parent")
D("unnecessary-wrapper")

// 3. Replace hardcoded values with variables
mcp__pencil__replace_all_matching_properties({
  parents: [rootId],
  properties: {
    fillColor: [
      { from: "#3B82F6", to: "var(--primary)" }
    ]
  }
})
```

**Option B: Interactive**
For each issue:
```
問題 1/5: 過深巢狀

位置: frame-abc > wrapper > container > inner > content
建議: 移除 wrapper 和 inner

[執行修復] [跳過] [查看詳情]
```

**Option C: Token Only**
```javascript
mcp__pencil__set_variables({
  variables: {
    // Colors
    "color-primary": "#3B82F6",
    "color-text": "#1F2937",
    // Typography
    "font-family-base": "Inter",
    "font-size-body": 16,
    // Spacing
    "spacing-xs": 4,
    "spacing-sm": 8,
    "spacing-md": 16,
    "spacing-lg": 24,
    "spacing-xl": 32
  }
})
```

### Step 6: Verification

After optimization:
```
mcp__pencil__get_screenshot(nodeId: rootId)
mcp__pencil__snapshot_layout(problemsOnly: true)
```

Show before/after comparison if significant changes made.

## Figma Pattern Reference

### Auto-Layout Mapping
| Figma | Pencil |
|-------|--------|
| Horizontal | layout: "horizontal" |
| Vertical | layout: "vertical" |
| Packed | gap: 0 |
| Space between | justifyContent: "space-between" |
| Hug contents | width/height: "hug_contents" |
| Fill container | width/height: "fill_container" |

### Constraint Mapping
| Figma | Pencil |
|-------|--------|
| Left & Right | width: "fill_container" |
| Top & Bottom | height: "fill_container" |
| Center | alignSelf: "center" |
| Scale | Not directly supported |

### Common Figma Issues in Pencil

1. **Groups vs Frames**: Figma groups become Pencil groups, but frames are better for layout
2. **Boolean operations**: May flatten to paths
3. **Effects**: Shadows and blurs should transfer, verify visually
4. **Masks**: May need manual adjustment
5. **Variants**: Component variants may flatten

## Anti-Patterns

- Do NOT modify without showing analysis first
- Do NOT assume all nesting is bad (some is intentional)
- Do NOT remove components without confirming they're unused
- Do NOT change positioning that breaks intentional overlaps
