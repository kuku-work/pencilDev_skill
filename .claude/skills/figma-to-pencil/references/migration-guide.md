# Figma → Pencil 遷移指南

## 工作流概覽

```
Figma（探索/設計階段）
    │
    │ 複製貼上
    ↓
Pencil（生產/開發階段）
    │
    │ 解析優化
    ↓
Production Code
```

## 設計端建議（給設計師）

### 在 Figma 階段就做好的事

#### 1. 使用 Auto-Layout
```
✅ 好的做法：
- 所有容器都使用 Auto-layout
- 明確設定 gap 和 padding
- 使用 Hug/Fill 而非固定尺寸

❌ 避免的做法：
- 絕對定位堆疊元素
- 手動設定固定寬高
- 使用負 margin 達成效果
```

#### 2. 建立 Design Tokens
```
✅ 好的做法：
- 在 Figma 使用 Variables 或 Styles
- 顏色、字型、間距都有命名
- 遵循命名規範（如 color/primary/500）

❌ 避免的做法：
- 每次直接選色
- 複製貼上樣式而非使用 Style
- 使用截圖顏色
```

#### 3. 語意化命名
```
✅ 好的做法：
- Frame: "Header", "ProductCard", "CTAButton"
- 有意義的層級：Header > Navigation > NavItem

❌ 避免的做法：
- Frame 1, Frame 2, Frame 3
- Group 1, Rectangle 1
- 隨意巢狀
```

#### 4. 元件化思維
```
✅ 好的做法：
- 重複元素建立為 Component
- 使用 Variants 處理狀態
- Instance 保持連結

❌ 避免的做法：
- 複製貼上而非建立 Instance
- Detach 所有 Instance
- 一個 Component 處理太多狀態
```

### 貼上前的檢查清單

- [ ] 移除隱藏圖層
- [ ] 檢查所有 Auto-layout 設定
- [ ] 確認沒有遺漏的 overflow
- [ ] 文字沒有被截斷
- [ ] 圖片有適當解析度

---

## 工程端建議（給開發者）

### 解析後的結構評估

#### 理想結構
```
Screen (frame)
├── Header (frame, layout: horizontal)
│   ├── Logo (frame)
│   └── Navigation (frame, layout: horizontal)
├── Main (frame, layout: vertical)
│   ├── Hero (frame)
│   └── Content (frame)
└── Footer (frame)
```

#### 問題結構
```
Frame 1
├── Group 1
│   ├── Frame 2
│   │   ├── Frame 3        ← 過深
│   │   │   ├── Frame 4    ← 過深
│   │   │   │   └── Text   ← 過深
```

### 常見問題與解法

#### 問題 1：過深巢狀

**症狀**：結構超過 4 層深

**原因**：
- Figma 的 Group 功能被過度使用
- 設計師習慣用 Frame 包裝單一元素

**解法**：
```javascript
// 識別可移除的中間層
// 條件：只有一個子元素的 frame/group
if (node.children.length === 1 && !hasLayoutSettings(node)) {
  // 將子元素提升到父層
  M(node.children[0].id, node.parent.id)
  D(node.id)
}
```

#### 問題 2：混合定位

**症狀**：同一容器內有 Auto-layout 和絕對定位混用

**原因**：
- Figma 的「絕對定位」功能
- 浮動元素（badge、icon）

**解法**：
```javascript
// 方案 A：將絕對定位元素移到正確位置
// 方案 B：使用 Pencil 的 position 屬性
U("badge", {
  position: "absolute",
  top: 8,
  right: 8
})
```

#### 問題 3：硬編碼值

**症狀**：顏色、字型、間距都是直接數值

**原因**：
- Figma 沒有使用 Styles/Variables
- 複製貼上樣式

**解法**：
```javascript
// 1. 收集所有唯一值
const colors = collectUniqueColors(design)
const fonts = collectUniqueFonts(design)

// 2. 建立變數
mcp__pencil__set_variables({
  variables: {
    ...colorsToVariables(colors),
    ...fontsToVariables(fonts)
  }
})

// 3. 替換引用
mcp__pencil__replace_all_matching_properties({
  parents: [rootId],
  properties: {
    fillColor: colors.map(c => ({
      from: c.hex,
      to: `var(--${c.name})`
    }))
  }
})
```

#### 問題 4：遺失的響應式

**症狀**：設計在不同尺寸下破版

**原因**：
- Figma 使用固定尺寸
- Constraints 設定不正確

**解法**：
```javascript
// 將固定尺寸改為彈性
U("container", {
  width: "fill_container",  // 替代固定 px
  minWidth: 320,            // 設定最小值
  maxWidth: 1200            // 設定最大值
})
```

### Token 提取策略

#### 顏色命名規範
```javascript
// 建議的命名結構
{
  // 基礎色
  "color-primary": "#3B82F6",
  "color-secondary": "#10B981",
  "color-accent": "#F59E0B",

  // 語意色
  "color-success": "#22C55E",
  "color-warning": "#EAB308",
  "color-error": "#EF4444",

  // 灰階
  "color-gray-50": "#F9FAFB",
  "color-gray-100": "#F3F4F6",
  "color-gray-900": "#111827",

  // 語意別名
  "color-text-primary": "var(--color-gray-900)",
  "color-text-secondary": "var(--color-gray-600)",
  "color-bg-primary": "var(--color-white)",
  "color-bg-secondary": "var(--color-gray-50)"
}
```

#### 間距命名規範
```javascript
// 基於 8px grid
{
  "spacing-0": 0,
  "spacing-1": 4,   // 0.5x
  "spacing-2": 8,   // 1x
  "spacing-3": 12,  // 1.5x
  "spacing-4": 16,  // 2x
  "spacing-5": 20,  // 2.5x
  "spacing-6": 24,  // 3x
  "spacing-8": 32,  // 4x
  "spacing-10": 40, // 5x
  "spacing-12": 48, // 6x
  "spacing-16": 64  // 8x
}
```

#### 字型命名規範
```javascript
{
  // 字型家族
  "font-family-sans": "Inter, system-ui, sans-serif",
  "font-family-mono": "JetBrains Mono, monospace",

  // 字型大小
  "font-size-xs": 12,
  "font-size-sm": 14,
  "font-size-base": 16,
  "font-size-lg": 18,
  "font-size-xl": 20,
  "font-size-2xl": 24,
  "font-size-3xl": 30,
  "font-size-4xl": 36,

  // 字重
  "font-weight-normal": 400,
  "font-weight-medium": 500,
  "font-weight-semibold": 600,
  "font-weight-bold": 700,

  // 行高
  "line-height-tight": 1.25,
  "line-height-normal": 1.5,
  "line-height-relaxed": 1.75
}
```

---

## 程式碼生成考量

### 結構對應

| Pencil 結構 | React 建議 | HTML 建議 |
|-------------|------------|-----------|
| Screen frame | Page component | `<main>` |
| Section frame | Section component | `<section>` |
| Card frame | Card component | `<article>` |
| List frame | List component | `<ul>` / `<ol>` |
| Navigation | Nav component | `<nav>` |
| Header | Header component | `<header>` |
| Footer | Footer component | `<footer>` |

### 元件拆分建議

```
當滿足以下條件時，應該拆成獨立元件：

1. 重複使用 ≥ 2 次
2. 有獨立的狀態或邏輯
3. 可以獨立測試
4. 語意上是獨立單位
```

### 樣式策略

| 場景 | 建議方案 |
|------|----------|
| 一次性樣式 | Tailwind utility |
| 重複樣式 | CSS 變數 + @apply |
| 元件樣式 | CSS Modules / styled-components |
| 主題變數 | CSS custom properties |

---

## 品質檢查清單

### 結構品質
- [ ] 巢狀深度 ≤ 4 層
- [ ] 所有容器使用 layout（非絕對定位）
- [ ] 語意化圖層命名
- [ ] 無空的 frame/group

### Token 品質
- [ ] 顏色都提取為變數
- [ ] 間距遵循 8px grid
- [ ] 字型使用有限的 scale
- [ ] 命名遵循規範

### 響應式品質
- [ ] 容器使用彈性尺寸
- [ ] 設定合理的 min/max
- [ ] 文字不會 overflow
- [ ] 圖片有 aspect ratio

### 元件品質
- [ ] 重複元素已元件化
- [ ] 元件有清楚的 props 介面
- [ ] 狀態變化有對應 variant
