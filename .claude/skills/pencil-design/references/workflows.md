# Pencil.dev Workflow Implementations

## NEW_DESIGN Workflow

When creating a design from scratch:

```
1. Check document state
   mcp__pencil__get_editor_state(include_schema: false)

2. Open new document if needed
   mcp__pencil__open_document("new")

3. Gather requirements (if insufficient):
   - Platform: mobile app / web app / website
   - Purpose: what does it do
   - Style: visual direction
   - Components: key elements needed
   - Users: target audience

4. Get style inspiration
   mcp__pencil__get_style_guide_tags()
   mcp__pencil__get_style_guide(tags: [relevant_tags])

5. Create design
   mcp__pencil__batch_design(operations: "...")

6. Verify result
   mcp__pencil__get_screenshot(nodeId: "...")
```

### Prompt Template
```
Design a [platform] for [purpose].
Use a [style] style.
Include: [components].
Target users: [description].
```

---

## ITERATE Workflow

When modifying an existing selected design:

```
1. Verify selection exists
   mcp__pencil__get_editor_state(include_schema: false)
   → If no selection, ask user to select first

2. Understand current structure
   mcp__pencil__batch_get(nodeIds: [selectedId], readDepth: 3)

3. Identify changes
   - What to keep unchanged
   - What to modify
   - What to avoid touching

4. Apply modifications
   mcp__pencil__batch_design(operations: "U(...)")

5. Verify result
   mcp__pencil__get_screenshot(nodeId: "...")
```

### Prompt Template
```
Look at the selected design. [change instruction]. Create a new design for it.
Keep: [elements to preserve]
Change: [specific modifications]
Don't touch: [elements to leave alone]
```

---

## EXPLORE Workflow

When exploring different style directions:

```
1. Verify selection exists
   mcp__pencil__get_editor_state(include_schema: false)

2. Get style options
   mcp__pencil__get_style_guide_tags()

3. Get inspiration for target direction
   mcp__pencil__get_style_guide(tags: [direction_tags])

4. Create variation
   mcp__pencil__find_empty_space_on_canvas(direction: "right")
   mcp__pencil__batch_design(operations: "...")

5. Show result
   mcp__pencil__get_screenshot(nodeId: "...")
```

### Prompt Template
```
Look at the selected design.
Explore a [totally different / slightly modified] design direction
with [style keywords]: [specific description].
Create a new design for it.
```

---

## EXTEND Workflow

When creating new pages that match existing design:

```
1. Verify base design is selected
   mcp__pencil__get_editor_state(include_schema: false)

2. Analyze base design for tokens
   mcp__pencil__batch_get(nodeIds: [baseId], readDepth: 4)
   Extract: colors, typography, spacing, patterns

3. Find space for new page
   mcp__pencil__find_empty_space_on_canvas(direction: "right")

4. Create new page with consistent styling
   mcp__pencil__batch_design(operations: "...")

5. Verify consistency
   mcp__pencil__get_screenshot(nodeId: "...")
```

### Prompt Template
```
Use the selected design as the base design, that's my current app.
Design a new page, the [page name] page now.
Include: [components for this page].
Match the existing visual style.
Create a new design for it.
```

---

## CODE Workflow

When generating code from design:

```
1. Get code guidelines
   mcp__pencil__get_guidelines(topic: "code")
   mcp__pencil__get_guidelines(topic: "tailwind")  // if using Tailwind

2. Extract design tokens
   mcp__pencil__get_variables(filePath: "...")

3. Understand full structure
   mcp__pencil__batch_get(nodeIds: [...], readDepth: 5)

4. Generate code matching user's tech stack
   - Framework: React, Vue, Next.js, etc.
   - Styling: Tailwind, CSS Modules, etc.
   - Components: shadcn/ui, Material UI, etc.

5. Write files to appropriate locations
```

### Tech Stack Prompt
```
Generate code using [framework], [styling], and [component library].
Look at the selected design and implement it.
Follow existing patterns in [code location].
```

---

## FIX Workflow

When diagnosing and fixing design issues:

```
1. Get current state
   mcp__pencil__get_editor_state(include_schema: false)

2. Detect layout problems
   mcp__pencil__snapshot_layout(problemsOnly: true)

3. Visual inspection
   mcp__pencil__get_screenshot(nodeId: "...")

4. Diagnose root cause
   - Overlapping: layout/positioning issue
   - Clipping: overflow/size issue
   - Misalignment: flex/grid settings

5. Apply targeted fix
   mcp__pencil__batch_design(operations: "U(...)")

6. Verify fix
   mcp__pencil__get_screenshot(nodeId: "...")
   mcp__pencil__snapshot_layout(problemsOnly: true)
```

### Common Fixes
```javascript
// Fix overlapping
U("container", {layout: "vertical", gap: 16})

// Fix clipping
U("parent", {overflow: "visible", height: "hug_contents"})

// Fix alignment
U("container", {alignItems: "center", justifyContent: "space-between"})
```

---

## Multi-Page Design Workflow

For complex tasks requiring multiple pages:

```
Phase 1: Establish Foundation
1. Create first page with full design system
2. Define: colors, typography, spacing, components
3. Get user approval before continuing

Phase 2: Extend to Additional Pages
For each page:
1. Find empty space on canvas
2. Create with consistent tokens
3. Show progress and get feedback

Phase 3: Refinement
1. Check all pages for consistency
2. Fix any issues
3. Present complete set
```

### Progress Tracking
```
設計電商 App
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[✓] 1. 首頁設計
[✓] 2. 商品列表頁
[▶] 3. 商品詳情頁 ← 進行中
[ ] 4. 購物車頁
[ ] 5. 一致性檢查
```
