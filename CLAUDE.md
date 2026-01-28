# Pencil.dev Project Configuration

This project uses Pencil.dev MCP tools for AI-driven design-to-code workflows.

---

## Intelligent Command System

### Primary Entry Points (Recommended)

| Command | Purpose |
|---------|---------|
| `/pencil` | **Smart router** - Analyzes user intent and routes to appropriate workflow |
| `/design` | **Workflow orchestrator** - Handles complex multi-step design tasks |

These commands understand natural language and automatically determine the right action.

### How Intent Recognition Works

When user speaks naturally, classify into:

| Intent | Trigger Examples | Auto-Action |
|--------|------------------|-------------|
| NEW_DESIGN | "做一個 App", "Design a..." | Create from scratch |
| ITERATE | "改成...", "換顏色", "調整間距" | Modify selected design |
| EXPLORE | "試試不同風格", "更大膽一點" | Generate style variations |
| EXTEND | "再做一頁", "加設定頁" | Create consistent new page |
| CODE | "產生程式碼", "轉成 React" | Generate implementation |
| FIX | "壞了", "重疊", "跑版" | Diagnose and repair |

### Decision Matrix

| User Says | Has Selection | Resolved Intent |
|-----------|---------------|-----------------|
| "做一個記帳 App" | No | NEW_DESIGN |
| "做一個記帳 App" | Yes | Ask: new or extend? |
| "改成深色" | Yes | ITERATE |
| "改成深色" | No | Ask: which element? |
| "試試科技風" | Yes | EXPLORE |
| "加一個設定頁" | Yes | EXTEND |

---

## Core Workflow Principles

### The Three-Phase Pattern

Every Pencil.dev interaction follows: **Select Design -> Specify Style -> Iterate Progressively**

1. **Select**: Always reference the selected design with "Look at the selected design"
2. **Specify**: Use concrete style keywords (Swiss layout, Scandinavian minimalistic)
3. **Iterate**: Each prompt focuses on a single change; continue in conversation

### Prompt Quality Hierarchy

Precise prompts produce faster, cheaper, better results:

| Quality | Example |
|---------|---------|
| Bad | "Design a dashboard" |
| Good | "Design a sales dashboard with top 5 performers, revenue chart, and deal pipeline" |
| Best | "Design a desktop sales dashboard for managers checking morning standups, showing top 5 performers with names and revenue, team quota progress bar, deal pipeline by stage. Card-based layout, green for on-track, yellow for at-risk." |

---

## MCP Tool Usage Protocol

### Starting a Design Task

```
1. mcp__pencil__get_editor_state(include_schema: false)
2. mcp__pencil__open_document("new") if no document open
3. mcp__pencil__get_style_guide_tags() for inspiration
4. mcp__pencil__get_style_guide(tags: [...]) for style guidance
```

### Modifying Existing Designs

```
1. mcp__pencil__get_editor_state(include_schema: false)
2. mcp__pencil__batch_get(nodeIds: [selectedId], readDepth: 3)
3. mcp__pencil__batch_design(operations: "...")
4. mcp__pencil__get_screenshot(nodeId: "...") to verify
```

### Code Generation

```
1. mcp__pencil__get_guidelines(topic: "code")
2. mcp__pencil__get_guidelines(topic: "tailwind") if using Tailwind
3. mcp__pencil__get_variables(filePath: "...") for design tokens
4. mcp__pencil__batch_get() to understand structure
```

---

## Available Slash Commands

### Smart Commands (Recommended)

| Command | Purpose |
|---------|---------|
| `/pencil` | Intelligent router - understands natural language, auto-routes to correct workflow |
| `/design` | Workflow orchestrator - handles multi-page designs, style comparisons, full pipelines |

### Specific Commands (When you know exactly what you need)

| Command | Purpose |
|---------|---------|
| `/pencil-new` | Create new design from scratch |
| `/pencil-iterate` | Modify selected design with specific changes |
| `/pencil-explore` | Explore different style directions |
| `/pencil-extend` | Create new pages matching existing design |
| `/pencil-code` | Generate production code from design |
| `/pencil-fix` | Debug and fix design issues |

---

## Prompt Templates

### New Design
```
Design a [platform] for [purpose].
Use a [style] style.
Include: [components].
Target users: [description].
```

### Design Iteration
```
Look at the selected design. [change]. Create a new design for it.
Keep: [preserve]
Change: [modify]
Don't touch: [exclude]
```

### Style Exploration
```
Look at the selected design.
Explore a [totally different / slightly modified] design direction
with [style keywords]: [description].
Create a new design for it.
```

### Page Extension
```
Use the selected design as the base design, that's my current app.
Design a new page, the [page name] page now.
Include: [components].
Match the existing visual style.
Create a new design for it.
```

---

## Style Keywords Reference

### Layout
- Swiss layout
- One column layout
- Two-column grid
- Card-based layout
- Use a sidenav

### Aesthetics
- Scandinavian minimalistic
- Technical style
- Bold and rock'n'roll
- Clean and professional
- High contrast

### Principles
- Focus on typography
- Most emphasis on [element]
- Everything else secondary
- Generous white space

---

## Anti-Patterns to Avoid

1. **Vague prompts**: "Design a nice page" lacks specificity
2. **Overloaded prompts**: One prompt should do one thing
3. **No selection reference**: Always use "Look at the selected design"
4. **Missing constraints**: Specify what NOT to do
5. **Skipping wireframes**: Start with structure, then style

---

## Quality Verification

After every design operation:

1. `mcp__pencil__get_screenshot()` - visual verification
2. `mcp__pencil__snapshot_layout(problemsOnly: true)` - detect layout issues
3. Check against original requirements

---

## File Handling

- `.pen` files are encrypted; use only MCP tools to read/modify
- Do NOT use `Read`, `Grep`, or `cat` on `.pen` files
- Design files are version controlled with Git
