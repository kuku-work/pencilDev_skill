# Pencil.dev Page Extension

Create new pages that extend from an existing design, maintaining visual consistency.

## Instructions

You are a Pencil.dev page extension specialist. Follow these steps:

### Step 1: Verify Base Design

1. Call `mcp__pencil__get_editor_state` to check current selection
2. The selected design will serve as the base/reference
3. Call `mcp__pencil__batch_get` to understand the design system components

### Step 2: Gather New Page Requirements

Ask user about:
- **Page name**: what is this page called
- **Page purpose**: what does this page do
- **Key components**: what elements are needed
- **Relationship to base**: how does it connect to existing pages

### Step 3: Construct Extension Prompt

Use this template:
```
Use the selected design as the base design, that's my current app.
Design a new page, the [page name] page now.
Include: [components and features for this page].
Match the existing visual style.
Create a new design for it.
```

### Step 4: Execute Extension

1. Analyze base design with `mcp__pencil__batch_get` to extract:
   - Color palette
   - Typography scale
   - Spacing system
   - Component patterns
2. Use `mcp__pencil__batch_design` to create new page
3. Verify consistency with `mcp__pencil__get_screenshot`

## Common Page Types

### Dashboard Page
```
Use the selected design as the base design, that's my current app.
Design a new page, the Dashboard page now.
Include: overview cards, activity chart, recent items list, quick actions.
Match the existing visual style.
Create a new design for it.
```

### Settings Page
```
Use the selected design as the base design, that's my current app.
Design a new page, the Settings page now.
Include: profile section, notification preferences, account settings,
security options, danger zone.
Match the existing visual style.
Create a new design for it.
```

### Detail Page
```
Use the selected design as the base design, that's my current app.
Design a new page, the [Item] Detail page now.
Include: header with title, metadata section, main content area,
related items sidebar, action buttons.
Match the existing visual style.
Create a new design for it.
```

## Consistency Checklist

- [ ] Same color palette applied
- [ ] Typography matches base design
- [ ] Spacing system is consistent
- [ ] Component styles are reused
- [ ] Navigation pattern is maintained

## Anti-Patterns to Avoid

- Do NOT create pages without referencing the base design
- Do NOT introduce new visual elements without justification
- Do NOT forget shared components (header, footer, nav)
