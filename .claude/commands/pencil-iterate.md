# Pencil.dev Design Iteration

Iterate on an existing selected design with specific modifications.

## Instructions

You are a Pencil.dev iteration specialist. Follow these steps:

### Step 1: Verify Selection

1. Call `mcp__pencil__get_editor_state` to check current selection
2. If nothing is selected, ask user to select a design element first
3. Call `mcp__pencil__batch_get` to understand the selected node structure

### Step 2: Clarify Changes

If the user's request is vague, ask about:
- **What to keep**: elements that should remain unchanged
- **What to change**: specific modifications needed
- **What to avoid**: elements that should not be touched

### Step 3: Construct the Iteration Prompt

Use this template:
```
Look at the selected design. [change instruction]. Create a new design for it.

Keep: [elements to preserve]
Change: [specific modifications]
Don't touch: [elements to leave alone]
```

### Step 4: Execute Iteration

1. Use `mcp__pencil__batch_get` with the selected node to understand current structure
2. Use `mcp__pencil__batch_design` to apply modifications
3. Call `mcp__pencil__get_screenshot` to verify the result

## Common Iteration Patterns

### Theme Switch
```
Look at the selected design. Change it to the light mode. Create a new design for it.
```

### Layout Change
```
Look at the selected design. Change the layout to a two-column grid
with sidebar navigation on the left. Create a new design for it.
Keep: current color palette and typography.
```

### Simplification
```
Look at the selected design. Change this to a simpler and cleaner design direction.
Create a new design for it.
```

### Typography Focus
```
Look at the selected design. Focus more on typography, reduce decorative elements.
Create a new design for it.
```

## Incremental Iteration Strategy

For complex changes, use conversation continuity:

```
Round 1: "Look at the selected design. Adjust the spacing to 24px between cards."
Round 2: "Great. Now change the font to Inter."
Round 3: "Perfect. Now add subtle shadows to the cards."
```

## Anti-Patterns to Avoid

- Do NOT make changes without referencing the selected design
- Do NOT combine too many changes in one iteration
- Do NOT forget to specify what should be preserved
