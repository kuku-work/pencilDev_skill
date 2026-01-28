# Pencil.dev Prompt Templates

## Template A: New Application Design

```
Design a [mobile app/web app/website] for [specific purpose/domain].
Use a [style description] style.
Include: [component list, comma-separated].
Target users: [user description].
```

### Examples

**Finance App**
```
Design a mobile app for tracking personal finances.
Use a minimal and calming style.
Include: monthly spending chart, category breakdown, recent transactions list,
budget progress bars, add expense button.
Target users: young professionals who check their spending during commute.
```

**E-commerce Website**
```
Design a website for a sustainable fashion brand.
Use a clean, eco-friendly aesthetic with earthy tones.
Include: hero section with featured collection, product grid, sustainability story,
newsletter signup, shopping cart icon.
Target users: environmentally conscious millennials.
```

**SaaS Dashboard**
```
Design a web app for managing team projects.
Use a technical, productivity-focused style.
Include: project list with progress bars, team member avatars, due date calendar,
task board with columns, notification bell.
Target users: project managers in tech companies.
```

---

## Template B: Design Iteration

```
Look at the selected design. [change instruction]. Create a new design for it.

Keep: [elements to preserve]
Change: [specific modifications]
Don't touch: [elements to leave alone]
```

### Examples

**Theme Switch**
```
Look at the selected design. Change it to dark mode. Create a new design for it.
Keep: layout and component structure.
Change: background to dark gray, text to white, adjust contrast.
Don't touch: icons and images.
```

**Layout Change**
```
Look at the selected design. Change the layout to a two-column grid
with sidebar navigation on the left. Create a new design for it.
Keep: current color palette and typography.
Change: card spacing to 24px, add sidebar with 240px width.
Don't touch: header and footer.
```

**Typography Focus**
```
Look at the selected design. Make the typography more prominent.
Create a new design for it.
Keep: overall structure and colors.
Change: headline size to 48px, increase line height, add font weight contrast.
Don't touch: images and icons.
```

---

## Template C: Style Exploration

```
Look at the selected design.
Explore a [totally different / slightly modified] design direction
with [style keywords]: [specific style description].
Create a new design for it.
```

### Examples

**Bold Direction**
```
Look at the selected design.
Explore a totally different design direction
with emphasis on typography: large bold headlines, minimal decorative elements,
generous white space, monochromatic color scheme with one accent color.
Create a new design for it.
```

**Softer Direction**
```
Look at the selected design.
Explore a slightly modified design direction
with Scandinavian minimalistic style: muted colors, rounded corners,
soft shadows, clean lines, warm neutrals.
Create a new design for it.
```

**Technical Direction**
```
Look at the selected design.
Explore a totally different design direction
with technical style: grid-based layout, data-dense displays,
monospace fonts for data, dark theme, neon accents.
Create a new design for it.
```

---

## Template D: Page Extension

```
Use the selected design as the base design, that's my current app.
Design a new page, the [page name] page now.
Include: [components and features for this page].
Match the existing visual style.
Create a new design for it.
```

### Examples

**Settings Page**
```
Use the selected design as the base design, that's my current app.
Design a new page, the Settings page now.
Include: profile section with avatar upload, notification toggles,
account settings form, security options, danger zone with delete account.
Match the existing visual style.
Create a new design for it.
```

**Detail Page**
```
Use the selected design as the base design, that's my current app.
Design a new page, the Product Detail page now.
Include: image gallery, product title and price, size/color selectors,
add to cart button, product description tabs, related products.
Match the existing visual style.
Create a new design for it.
```

**List Page**
```
Use the selected design as the base design, that's my current app.
Design a new page, the Order History page now.
Include: filter dropdown, search bar, order cards with status badges,
pagination, empty state illustration.
Match the existing visual style.
Create a new design for it.
```

---

## Template E: Code Generation

```
Generate code using [framework], [styling], and [component library].
Look at the selected design and implement it.
Use TypeScript with proper type definitions.
Follow the component structure in [code location].
```

### Examples

**React + Tailwind**
```
Generate code using React, Tailwind CSS, and shadcn/ui components.
Look at the selected design and implement it.
Use TypeScript with proper type definitions.
Follow the component structure in src/components/.
Extract colors to Tailwind config.
```

**Next.js App Router**
```
Generate code using Next.js 14 App Router with server components.
Use Tailwind CSS for styling.
Look at the selected design and implement it as a page component.
Place in app/dashboard/page.tsx.
Include loading.tsx and error.tsx.
```

**Vue 3**
```
Generate code using Vue 3 with Composition API.
Use <script setup> syntax and scoped CSS.
Look at the selected design and implement it.
Place in src/views/ directory.
Use Pinia for any state management.
```

---

## Template F: Bug Fix

```
Here's the issue I'm seeing: [describe problem].
[Optional: paste error message]
Fix the selected design to resolve this issue.
```

### Examples

**Layout Issue**
```
Here's the issue I'm seeing: cards are overlapping each other
instead of stacking vertically.
Fix the selected design to resolve this issue.
```

**Responsive Issue**
```
Here's the issue I'm seeing: content gets cut off on mobile screens
and there's no horizontal scroll.
Fix the selected design to resolve this issue.
Make it responsive with proper breakpoints.
```

**Visual Glitch**
```
Here's the issue I'm seeing: some text is invisible because
it has the same color as the background.
Fix the selected design to resolve this issue.
Ensure proper contrast throughout.
```
