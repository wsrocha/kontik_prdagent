---
name: slidev-presentation
description: >
  Help users create Slidev presentations from existing content (proposals, documents, notes).
  Generates a complete Slidev project with slides.md, style.css, global-bottom.vue, and
  package.json — ready to run with `npx slidev`. Use this skill when the user wants to create
  a slide deck, presentation, or talk from a document, proposal, or set of notes. Also trigger
  when they say things like "turn this into slides", "make a presentation from this", "I need
  a deck for this proposal", or "create slides for my talk."
---

# Slidev Presentation Builder

You help users turn documents, proposals, or notes into polished Slidev presentations. The goal
is to produce a self-contained Slidev project that the user can run immediately with `npx slidev`.

## Why this matters

Presentations are how technical decisions get communicated to stakeholders. But going from a
detailed document to a well-paced slide deck is tedious — people end up dumping paragraphs onto
slides or spending hours on layout. Your job is to distill content into a visual, scannable format
that respects slide constraints (one idea per slide, no overflow, proper visual hierarchy).

## What you produce

A directory containing four files:

| File | Purpose |
|------|---------|
| `slides.md` | All slide content with YAML frontmatter |
| `style.css` | Design system (colors, typography, layout components) |
| `global-bottom.vue` | Footer with page numbers and copyright |
| `package.json` | Dependencies (slidev CLI + theme + icons) |

## How the conversation works

### Step 1: Understand the source material

Read the source document(s) the user points you to. Then clarify:

- **Audience**: Who is this for? (execs, technical peers, customers)
- **Length**: How many slides roughly? (compact = 8-12, standard = 15-20, detailed = 20+)
- **Language**: What language should the slides be in?
- **Appendix**: Should detailed content go into appendix slides?

If the source is already clear on audience and scope, skip questions you can infer.

### Step 2: Create the project

Generate all four files in a single directory. The user picks the location, or default to
a `preso/` subdirectory next to the source document.

### Step 3: Run and iterate

Start the dev server so the user can see the result. Then iterate based on their feedback —
fix overflow, adjust diagrams, change wording.

**If the user is running manually**, suggest: `npx slidev --open`

**If you (Claude) are starting the server**, Slidev's CLI reads stdin for keyboard shortcuts
and exits immediately without a TTY. Use this pattern:

```bash
# Check if default port 3030 is already in use; if so, pick the next available
PORT=3030
while lsof -i :$PORT -sTCP:LISTEN >/dev/null 2>&1; do PORT=$((PORT+1)); done
node node_modules/.bin/slidev --port $PORT --open false < /dev/zero > /tmp/slidev.log 2>&1 &
echo "Slidev running on http://localhost:$PORT/ (PID: $!)"
```

Key details:
- `< /dev/zero` keeps stdin open so Slidev doesn't exit
- `--open false` prevents trying to launch a browser
- Auto-increment the port to avoid conflicts when multiple presentations are running

## Slide structure conventions

### Frontmatter

```yaml
---
theme: default
title: Presentation Title
info: One-line description
transition: fade
mdc: true
colorSchema: light
layout: default
---
```

### Slide separators

Slides are separated by `---`. Each slide can have optional YAML frontmatter:

```markdown
---
layout: center
---

## Slide Title
Content here
```

### Speaker notes

Put speaker notes in HTML comments below slide content:

```markdown
## Slide Title

Content here.

<!--
Speaker note: explain this point in more detail during the talk.
-->
```

### Layouts used

- `layout: default` — standard left-aligned content (most slides)
- `layout: center` — centered content (for quotes, key takeaways)
- `class: section-divider` — large section headers
- `class: vcenter` — vertically centered content
- `class: appendix-divider` — marks the start of appendix slides

## Design system

### CSS variables

```css
:root {
  --amazon-dark: #161D26;
  --amazon-warm-white: #F5F3EF;
  --amazon-orange: #FF6200;      /* Single accent color, use sparingly */
  --font-base: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --bg-primary: #F5F3EF;
  --text-primary: #161D26;
  --text-secondary: #232F3E;
  --card-bg: rgba(22, 29, 38, 0.04);
  --card-border: rgba(22, 29, 38, 0.08);
}
```

### Typography hierarchy

- **h1**: 4.5rem, weight 700, line-height 0.9 — title slides only
- **h2**: 2.75rem, weight 700, line-height 1.1 — slide titles
- **h3**: 1.5rem, weight 700 — section headers within a slide
- **p**: 1.35rem, weight 400, line-height 1.5 — body text
- **Byline**: 1.1rem, opacity 0.6, negative top margin — sits tight under h2

### Byline pattern

Every content slide should have a byline under the title:

```markdown
## Slide Title
<p class="byline">Short punchy subtitle, max one sentence.</p>
```

### Color coding for diagrams

When showing architecture or component diagrams, use consistent color coding:

- **Green** (`rgba(46,125,50,0.08)`, stroke `#2E7D32`) — existing/reused components
- **Blue** (`rgba(21,101,192,0.08)`, stroke `#1565C0`) — new components
- **Yellow** (`rgba(255,152,0,0.1)`, stroke `#FF9800`) — managed services / external
- **Red** (`rgba(211,47,47,0.06)`) — error paths, warnings

Include a legend when using color coding.

## Component patterns

Reference the `references/components.md` file in this skill's directory for the full catalog
of reusable CSS component classes and their HTML structure.

## Critical rules for slide content

### Overflow prevention

Slides have fixed dimensions. Content that overflows the bottom is silently cut off — the user
won't see it. This is the most common problem.

**Rules:**
- **Max 3-4 items** in a ladder/numbered list per slide
- **Reduce padding** on ladder steps when you have 4 items: `padding: 0.45rem 0` and
  `font-size: 2rem` on numbers, `1.05rem` on titles, `0.85rem` on descriptions
- **No more than 7 rows** in a table per slide
- **Two-column layouts** help fit more content without overflow
- **Test mentally**: if a slide has a title + byline + content, the content area is roughly
  60-65% of the slide height

### SVG diagrams in Slidev

Slidev's global CSS aggressively overrides font properties with `!important`. This breaks
inline SVG text sizing. Always follow these rules:

1. Set `style="font-size: 1px;"` on the `<svg>` element to reset inherited font-size
2. Use `style="font-size: Xpx; font-weight: 700;"` inline on every `<text>` element
3. Never use `font-size` or `font-weight` as SVG attributes — always use `style`

```html
<svg viewBox="0 0 900 160" xmlns="http://www.w3.org/2000/svg"
     style="width: 100%; margin: 1rem 0; font-size: 1px;">
  <text x="85" y="70" text-anchor="middle"
        style="font-size: 15px; font-weight: 700;"
        fill="#161D26">Label</text>
</svg>
```

### Mermaid diagrams

Mermaid diagrams may need scaling to fit. Use the scale parameter:

```markdown
```mermaid {scale: 0.45}
flowchart LR
    A --> B --> C
```​
```

Prefer `LR` (left-right) over `TB` (top-bottom) for better slide fit. Keep node labels short.

### Lists

Custom bullet styling uses orange squares. The CSS handles this automatically for `<ul>` elements.
For lists inside HTML components (cards, grids), use `<div>` elements instead.

### Icons

The project includes `@phosphor-icons/vue`. Import in a `<script setup>` block:

```vue
<script setup>
import { PhSparkle, PhBookOpen } from '@phosphor-icons/vue'
</script>

<PhSparkle :size="48" weight="thin" class="card-icon" />
```

Always use `weight="thin"`. Sizes: 24px (inline), 36px (small cards), 48px (medium), 64px (large).

## Global bottom component

Always include this footer component:

```vue
<template>
  <div class="slide-footer">
    <span class="copyright">© 2026 Amazon Web Services</span>
    <span class="divider">·</span>
    <span class="page-number">{{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}</span>
  </div>
</template>

<style scoped>
.slide-footer {
  position: fixed;
  bottom: 1.25rem;
  right: 3rem;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.65rem;
  font-weight: 400;
  color: #232F3E;
  opacity: 0.35;
  z-index: 10;
  pointer-events: none;
}
.divider { opacity: 0.5; }
</style>
```

Adjust the copyright year and org name for the user's context.

## Important guidelines

- **One idea per slide.** If you're explaining two things, make two slides. Slides are free.
- **Distill, don't dump.** A slide is not a paragraph reformatted. Extract the key point and
  present it visually — cards, numbered steps, two-column comparisons, SVG diagrams.
- **Always run the dev server** after generating so the user can see results immediately.
  See "Step 3" above for the correct invocation depending on whether you or the user starts it.
- **Fix overflow first.** When iterating, overflow is the top priority — invisible content is
  worse than imperfect content.
- **Keep technical terms.** Don't translate service names (Lambda, Step Functions, DynamoDB,
  AgentCore Runtime, etc.) even when the rest of the presentation is in another language.
- **Speaker notes are optional.** Add them if the user is preparing for a talk. Skip them
  if the deck is meant to be read standalone.
- **Appendix slides** go after the main content, separated by an appendix divider slide.
  They contain detailed reference material the presenter can jump to if asked deeper questions.
