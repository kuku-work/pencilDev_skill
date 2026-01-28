# Figma 常見模式與 Pencil 對應

## Auto-Layout 對應表

### 基本方向

| Figma 設定 | Pencil 屬性 |
|------------|-------------|
| Direction: Horizontal | `layout: "horizontal"` |
| Direction: Vertical | `layout: "vertical"` |
| Wrap | `flexWrap: "wrap"` |

### 對齊方式

| Figma 設定 | Pencil 屬性 |
|------------|-------------|
| Align left | `alignItems: "start"` |
| Align center | `alignItems: "center"` |
| Align right | `alignItems: "end"` |
| Space between | `justifyContent: "space-between"` |

### 尺寸模式

| Figma 設定 | Pencil 屬性 |
|------------|-------------|
| Hug contents | `width: "hug_contents"` |
| Fill container | `width: "fill_container"` |
| Fixed | `width: 200` (數值) |

### 間距

| Figma 設定 | Pencil 屬性 |
|------------|-------------|
| Gap (between items) | `gap: 16` |
| Padding (all) | `padding: 16` |
| Padding (individual) | `padding: [16, 24, 16, 24]` |

---

## 常見 Figma 結構模式

### 模式 1：卡片元件

**Figma 結構**
```
Card (Auto-layout: vertical, padding: 16, gap: 12)
├── Image (Fill, height: 200)
├── Content (Auto-layout: vertical, gap: 8)
│   ├── Title (Text)
│   └── Description (Text)
└── Footer (Auto-layout: horizontal, space-between)
    ├── Price (Text)
    └── Button (Component)
```

**Pencil 對應**
```javascript
card = I(parent, {
  type: "frame",
  layout: "vertical",
  padding: 16,
  gap: 12,
  fill: "#FFFFFF",
  cornerRadius: 8
})

image = I(card, {
  type: "frame",
  width: "fill_container",
  height: 200
})
G(image, "stock", "product photo")

content = I(card, {
  type: "frame",
  layout: "vertical",
  gap: 8
})

title = I(content, {
  type: "text",
  content: "Product Name",
  fontSize: 18,
  fontWeight: "600"
})

desc = I(content, {
  type: "text",
  content: "Product description...",
  fontSize: 14,
  fill: "#6B7280"
})

footer = I(card, {
  type: "frame",
  layout: "horizontal",
  justifyContent: "space-between",
  alignItems: "center"
})
```

### 模式 2：導航列

**Figma 結構**
```
Navigation (Auto-layout: horizontal, space-between, padding: 16)
├── Logo (Frame)
├── NavItems (Auto-layout: horizontal, gap: 24)
│   ├── NavItem (Text)
│   ├── NavItem (Text)
│   └── NavItem (Text)
└── Actions (Auto-layout: horizontal, gap: 12)
    ├── Search (Icon)
    └── Profile (Avatar)
```

**Pencil 對應**
```javascript
nav = I(parent, {
  type: "frame",
  layout: "horizontal",
  justifyContent: "space-between",
  alignItems: "center",
  padding: [16, 24],
  width: "fill_container"
})

logo = I(nav, {
  type: "frame",
  width: 120,
  height: 40
})

navItems = I(nav, {
  type: "frame",
  layout: "horizontal",
  gap: 24
})

// NavItem 可建為元件
["Home", "Products", "About"].forEach(item => {
  I(navItems, {
    type: "text",
    content: item,
    fontSize: 16
  })
})

actions = I(nav, {
  type: "frame",
  layout: "horizontal",
  gap: 12,
  alignItems: "center"
})
```

### 模式 3：表單輸入

**Figma 結構**
```
FormField (Auto-layout: vertical, gap: 4)
├── Label (Text)
├── InputWrapper (Auto-layout: horizontal, padding: 12)
│   ├── Icon (optional)
│   └── Input (Text)
└── HelperText (Text, optional)
```

**Pencil 對應**
```javascript
field = I(form, {
  type: "frame",
  layout: "vertical",
  gap: 4,
  width: "fill_container"
})

label = I(field, {
  type: "text",
  content: "Email",
  fontSize: 14,
  fontWeight: "500"
})

inputWrapper = I(field, {
  type: "frame",
  layout: "horizontal",
  padding: 12,
  gap: 8,
  alignItems: "center",
  stroke: "#D1D5DB",
  strokeWidth: 1,
  cornerRadius: 6,
  width: "fill_container"
})

input = I(inputWrapper, {
  type: "text",
  content: "placeholder@example.com",
  fontSize: 16,
  fill: "#9CA3AF"
})

helper = I(field, {
  type: "text",
  content: "We'll never share your email",
  fontSize: 12,
  fill: "#6B7280"
})
```

### 模式 4：清單項目

**Figma 結構**
```
ListItem (Auto-layout: horizontal, space-between, padding: 16)
├── Left (Auto-layout: horizontal, gap: 12)
│   ├── Avatar (Frame, 40x40)
│   └── Info (Auto-layout: vertical, gap: 4)
│       ├── Name (Text)
│       └── Subtitle (Text)
└── Right (Auto-layout: vertical, align-end)
    ├── Time (Text)
    └── Badge (optional)
```

**Pencil 對應**
```javascript
listItem = I(list, {
  type: "frame",
  layout: "horizontal",
  justifyContent: "space-between",
  alignItems: "center",
  padding: 16,
  width: "fill_container"
})

left = I(listItem, {
  type: "frame",
  layout: "horizontal",
  gap: 12,
  alignItems: "center"
})

avatar = I(left, {
  type: "ellipse",
  width: 40,
  height: 40,
  fill: "#E5E7EB"
})

info = I(left, {
  type: "frame",
  layout: "vertical",
  gap: 4
})

name = I(info, {
  type: "text",
  content: "John Doe",
  fontSize: 16,
  fontWeight: "500"
})

subtitle = I(info, {
  type: "text",
  content: "john@example.com",
  fontSize: 14,
  fill: "#6B7280"
})

right = I(listItem, {
  type: "frame",
  layout: "vertical",
  alignItems: "end",
  gap: 4
})

time = I(right, {
  type: "text",
  content: "2m ago",
  fontSize: 12,
  fill: "#9CA3AF"
})
```

---

## Figma 特殊功能處理

### 1. Constraints（約束）

Figma constraints 不直接對應到 Pencil，需要用 layout 屬性模擬：

| Figma Constraint | Pencil 替代方案 |
|------------------|-----------------|
| Left | 父層 `alignItems: "start"` |
| Right | 父層 `alignItems: "end"` |
| Left and Right | `width: "fill_container"` |
| Center | `alignSelf: "center"` |
| Scale | 使用百分比或 fill_container |

### 2. Effects（效果）

| Figma Effect | Pencil 屬性 |
|--------------|-------------|
| Drop shadow | `shadow: { x, y, blur, spread, color }` |
| Inner shadow | `innerShadow: { ... }` |
| Blur | `blur: number` |
| Background blur | `backdropBlur: number` |

### 3. Boolean Operations

Figma 的布林運算（Union, Subtract, Intersect, Exclude）在貼上後會：
- 保持為路徑（path）
- 或展平為單一形狀

**建議**：如果需要編輯，在 Figma 保持為可編輯狀態後再貼上。

### 4. Masks

Figma masks 可能需要在 Pencil 中重新設定：

```javascript
// 設定 mask
U("mask-frame", {
  clipContent: true  // 啟用內容裁切
})
```

### 5. Component Variants

Figma variants 不會自動轉換為 Pencil 元件變體。

**處理方式**：
1. 在 Figma 選擇要使用的 variant
2. 貼上該 variant
3. 在 Pencil 中建立為新元件（如需要）

---

## 問題診斷速查

| 症狀 | 可能原因 | 解決方案 |
|------|----------|----------|
| 元素重疊 | 絕對定位 | 改用 layout |
| 內容被裁切 | overflow 設定 | 設定 `overflow: "visible"` |
| 間距不一致 | 硬編碼值 | 提取為 spacing token |
| 文字溢出 | 固定寬度 | 使用 `hug_contents` 或設定 `textOverflow` |
| 顏色不對 | 色彩空間差異 | 確認使用相同的 hex 值 |
| 字型不對 | 字型未載入 | 確認 Pencil 有該字型 |
| 圖片模糊 | 解析度不足 | 使用 2x 或更高解析度 |
| 陰影消失 | effect 未轉換 | 手動添加 shadow 屬性 |
