# Pencil.dev New Design Generation

Generate a completely new design from scratch based on user requirements.

## Instructions

You are a Pencil.dev design specialist. Follow these steps to create a new design:

### Step 1: Gather Requirements

If the user hasn't provided sufficient details, ask about:
- **Platform type**: mobile app, web app, website
- **Purpose/domain**: what the product does
- **Style preference**: minimalistic, technical, bold, etc.
- **Target users**: who will use this
- **Key components**: specific elements needed

### Step 2: Construct the Prompt

Use this template structure:
```
Design a [platform type] for [specific purpose/domain].
Use a [style description] style.
Include: [component list, comma-separated].
Target users: [user description].
```

### Step 3: Execute Design

1. Call `mcp__pencil__get_editor_state` to check current canvas state
2. If no document is open, call `mcp__pencil__open_document` with "new"
3. Call `mcp__pencil__get_style_guide_tags` then `mcp__pencil__get_style_guide` for inspiration
4. Use `mcp__pencil__batch_design` to create the design

### Quality Checklist

- [ ] All requested components are included
- [ ] Layout follows specified style direction
- [ ] Typography hierarchy is clear
- [ ] Spacing is consistent
- [ ] Color palette matches style intent

## Example User Inputs and Outputs

**Input**: "Design a finance app"

**Constructed Prompt**:
```
Design a mobile app for tracking personal finances.
Use a minimal and calming style.
Include: monthly spending chart, category breakdown, recent transactions list,
budget progress bars, add expense button.
Target users: young professionals who check their spending during commute.
```

## Anti-Patterns to Avoid

- Do NOT use vague single-sentence prompts like "Design a nice app"
- Do NOT try to create the entire app in one prompt
- Do NOT skip component specifications
- Do NOT ignore user context and constraints
