# simple-webpage

A vanilla HTML/CSS/JS reference repository and OpenCode agent skill. No frameworks, no build steps, no bloat.

## Quick Start

1. Open any file in `cookbook/` in your browser
2. Copy patterns into your project
3. Customize with tokens from `tokens/`

## Structure

- **`SKILL.md`** — Agent skill definition (reusable in any project)
- **`cookbook/`** — Standalone HTML pattern demos (layouts, navigation, forms, cards, effects, interactivity, typography)
- **`tokens/`** — Design token CSS files (base → semantic → components → typography)
- **`docs/`** — Principles, component specs, guides, tutorials
- **`templates/`** — Starting point files (blank, starter, with-framework)

## For AI Agents

Load `SKILL.md` before generating any webpage. It defines:
- Philosophy (ponytail YAGNI ladder)
- Decision framework (when to use what)
- HTML/CSS/JS patterns with code examples
- CSS framework allowlist (classless, shadcn-aesthetic, minimal-class)
- Anti-patterns, design reference, accessibility minimums
- Cookbook index with links to all patterns

## Serving Locally

```bash
python3 -m http.server 8000
# or: npx serve .
# or: php -S localhost:8000
```

## Techniques Covered

From Coding2GO, 37signals, and modern CSS best practices:

- OKLCH colors, relative colors, `currentColor`
- CSS nesting, `@layer`, `@property`
- `clamp()` fluid typography, `text-wrap: balance`
- `:has()`, `:not()`, `::scroll-button()`
- Glassmorphism, neumorphism, gradient text
- Scroll-snap carousels, anchor positioning
- `<dialog>`, `<details>`, Popover API
- Dark mode with `light-dark()` and localStorage
- Responsive grid with `auto-fit`/`minmax()`
- `@starting-style` entry animations
- Scroll-driven animations
