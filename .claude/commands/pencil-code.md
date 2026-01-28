# Pencil.dev Code Generation

Generate production-ready code from Pencil.dev designs.

## Instructions

You are a Pencil.dev code generation specialist. Follow these steps:

### Step 1: Gather Technical Requirements

Ask user about:
- **Framework**: React, Next.js, Vue, Svelte, etc.
- **Styling**: Tailwind CSS, CSS Modules, styled-components, etc.
- **Component library**: shadcn/ui, Material UI, Chakra UI, etc.
- **Output location**: where to place generated files

### Step 2: Analyze the Design

1. Call `mcp__pencil__get_editor_state` to get selected design
2. Call `mcp__pencil__batch_get` to understand full structure
3. Call `mcp__pencil__get_variables` to extract design tokens

### Step 3: Get Code Guidelines

1. Call `mcp__pencil__get_guidelines` with topic="code" for code generation rules
2. If using Tailwind, also call with topic="tailwind"

### Step 4: Generate Code

Construct prompt with framework context:
```
Generate code using [framework], [styling], and [component library].
Look at the selected design and implement it following existing patterns
in [code location].
```

### Step 5: Integrate with Codebase

1. Check existing code patterns in the project
2. Match naming conventions
3. Reuse existing components where possible
4. Follow project's file structure

## Framework-Specific Templates

### React + Tailwind + shadcn/ui
```
Generate code using React, Tailwind CSS, and shadcn/ui components.
Look at the selected design and implement it.
Use TypeScript with proper type definitions.
Follow the component structure in src/components/.
```

### Next.js 14 + App Router
```
Generate code using Next.js 14 App Router with server components.
Use Tailwind CSS for styling.
Look at the selected design and implement it as a page component.
Place in app/[route]/page.tsx.
```

### Vue 3 + Composition API
```
Generate code using Vue 3 with Composition API.
Use <script setup> syntax.
Look at the selected design and implement it.
Use scoped CSS for component styles.
```

## Code Quality Checklist

- [ ] Components are properly typed (TypeScript)
- [ ] Accessibility attributes included (aria-*, roles)
- [ ] Responsive breakpoints implemented
- [ ] Design tokens extracted to variables
- [ ] No hardcoded colors/sizes
- [ ] Matches existing code patterns

## Design Token Extraction

Extract from `mcp__pencil__get_variables`:
- Colors -> CSS custom properties / Tailwind config
- Typography -> font families, sizes, weights
- Spacing -> spacing scale
- Border radius -> consistent corner values

## Anti-Patterns to Avoid

- Do NOT generate code without understanding the full design structure
- Do NOT ignore existing codebase patterns
- Do NOT hardcode values that should be design tokens
- Do NOT skip accessibility considerations
