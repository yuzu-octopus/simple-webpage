---
name: vanilla-webpage
description: >
  Build webpages with vanilla HTML/CSS/JS — no frameworks, no build steps, no bloat.
  Use when the user mentions "vanilla", "simple webpage", "no framework", "no build step",
  "plain HTML", "CSS only", "no React", "no Tailwind", "no Bootstrap", "static site",
  "landing page", "portfolio", "personal website", or "webpage".
  Covers HTML semantics, modern CSS (nesting, @layer, :has(), clamp(), container queries),
  vanilla JS patterns, design tokens, accessibility, and responsive design.
  For CSS art or animations specifically, see icon-design. For React/Vue/Svelte, see webapp.
  Inspired by 37signals (Campfire/Writebook), Coding2GO, and the ponytail YAGNI philosophy.
allowed-tools: Read Write Edit Bash WebSearch WebFetch
---

# Vanilla Webpage Skill

Build webpages with vanilla HTML, CSS, and JavaScript. No frameworks, no build steps, no bloat. Modern CSS is powerful enough. Patterns are reference material, not a menu — use the Ponytail Ladder to decide what you actually need.

## Quick Routing

| I need to... | Go to |
|---|---|
| Decide if I need a framework | §1 Philosophy + §2 Decision Framework |
| Write semantic HTML | §4 HTML Patterns |
| Style with CSS (custom properties, nesting, @layer) | §5 CSS Patterns |
| Use modern CSS (clamp, :has, container queries) | §5 CSS Patterns |
| Add interactivity with JS | §6 JavaScript Patterns |
| Choose a classless CSS framework | §7 Framework Allowlist |
| Avoid common mistakes | §8 Anti-Patterns |
| Check design tokens and spacing | §9 Design Quick Reference |
| Verify accessibility | §10 Accessibility Minimums |
| Browse working code examples | §11 Cookbook Reference |

---

## §1 Philosophy — The Ponytail Ladder

Stop at the first rung that holds:

1. **Does this need to exist at all?** Speculative need = skip it. (YAGNI)
2. **Already in this codebase?** Reuse existing patterns. Look before you write.
3. **Stdlib does it?** Use it.
4. **Native platform feature covers it?** `<dialog>` for modals, `<details>` for accordions, CSS over JS.
5. **Already-installed dependency solves it?** Use it.
6. **Can it be one line?** One line.
7. **Only then:** the minimum code that works.

---

## §2 Decision Framework

```
Can semantic HTML do it? → Yes → Done
No → Can 20 lines of vanilla CSS do it? → Yes → Write the CSS
No → Does a classless CSS framework solve it in one <link>? → Yes → Use it
No → Is the problem interactivity? → Use vanilla JS (or HTMX for complex cases)
No → OK, consider a lightweight component approach
```

---

## §3 File Structure Convention

- **Simple page:** single `index.html` (embedded `<style>` in `<head>`, `<script>` before `</body>`)
- **Multi-section:** `index.html` + `style.css` + `script.js` at root
- **Split files** when they exceed ~400 lines
- **No `src/` / `dist/`** unless there's a build step (YAGNI)
- **No `package.json`**, no `node_modules` for static pages
- **Reference cookbook:** Browse `cookbook/` before writing new CSS from scratch

---

## §4 HTML Patterns

### Semantic Elements (always prefer)

```html
<nav>, <main>, <article>, <section>, <aside>, <header>, <footer>
<form>, <fieldset>, <legend>, <label>
<button>, <input>, <select>, <textarea>
<details>/<summary> for accordions (no JS needed)
<dialog> for modals (no JS needed for the element itself)
<picture>/<source> for responsive images
```

### Dialog Element

```html
<dialog id="note-dialog">
  <form method="dialog">
    <h2>Edit Note</h2>
    <textarea></textarea>
    <button>Save</button>
    <button type="button" onclick="this.closest('dialog').close()">Cancel</button>
  </form>
</dialog>
<script>
  document.getElementById('note-dialog').showModal();
</script>
```

### Details/Summary Accordion

```html
<details>
  <summary>Common Question</summary>
  <p>Answer content here...</p>
</details>
```

### Accessible Forms

```html
<form>
  <label for="email">Email</label>
  <input type="email" id="email" autocomplete="email" inputmode="email" required>
  <label for="password">Password</label>
  <input type="password" id="password" autocomplete="current-password" required>
  <button type="submit">Login</button>
</form>
```

### ARIA Attributes

```html
<button aria-expanded="false" aria-label="Toggle menu">☰</button>
<nav aria-label="Main navigation">
  <!-- nav items -->
</nav>
```

### Popover API (no JS needed for open/close)

```html
<button popovertarget="my-popover">Open</button>
<div popover id="my-popover">Popover content</div>
```

### Data Attributes for Dynamic Content

```html
<a href="#" data-name="Twitter" style="--color: #1da1f2">🐦</a>
<a href="#" data-name="GitHub" style="--color: #333">💻</a>
```
```css
a::before { content: attr(data-name); }
```

---

## §5 CSS Patterns

### CSS Custom Properties / Design Tokens

```css
:root {
  --color-primary: oklch(54% 0.23 255);
  --color-bg: #ffffff;
  --color-text: #1a1a1a;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 2rem;
  --radius: 0.5rem;
  --font-body: system-ui, -apple-system, sans-serif;
}
```

### OKLCH Color System

```css
:root {
  --lch-blue: 54% 0.23 255;
  --lch-blue-light: 95% 0.03 255;
  --lch-blue-dark: 80% 0.08 255;

  --color-link: oklch(var(--lch-blue));
  --color-selected: oklch(var(--lch-blue-light));
  --color-selected-dark: oklch(var(--lch-blue-dark));
  --color-link-50: oklch(var(--lch-blue) / 0.5);
}
```

### Relative Colors from Syntax

```css
:root {
  --color-primary: oklch(54% 0.23 255);
}
.btn {
  --btn-hover: oklch(from var(--color-primary) calc(l - 10%) c h);
  --btn-active: oklch(from var(--color-primary) calc(l - 20%) c h);
  background: var(--color-primary);
  &:hover { background: var(--btn-hover); }
  &:active { background: var(--btn-active); }
}
```

### currentColor

```css
.icon {
  color: var(--color-primary);
  filter: drop-shadow(0 0 4px currentColor);
}
.btn {
  color: var(--color-primary);
  border-color: currentColor;
  background: oklch(from currentColor l c h / 0.1);
}
```

### CSS Nesting (no Sass needed)

```css
.card {
  background: var(--color-bg);
  border-radius: var(--radius);

  & h2 { font-size: 1.5rem; }
  & p { color: var(--color-text); }
  &:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
}
```

### @layer for Specificity Management

```css
@layer tokens, base, components, utilities;

@layer tokens {
  :root { --color-primary: blue; }
}
@layer components {
  .btn { background: var(--color-primary); }
}
@layer utilities {
  .hidden { display: none; }
}
```

### Fluid Typography with clamp()

```css
h1 { font-size: clamp(1.5rem, 4vw + 1rem, 3rem); }
body { font-size: clamp(1rem, 0.9rem + 0.5vw, 1.125rem); }
```

### text-wrap: balance

```css
h1, h2, h3 { text-wrap: balance; }
/* Only works for six wrapped lines or fewer */
/* Fixes ugly line breaks on mobile headings */
```

### Responsive Grid — No Media Queries

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}
```

### :has() Parent Selector

```css
.card:has(.badge) { border: 2px solid var(--color-primary); }
nav:has(.active) { background: var(--color-bg); }
```

### :not() Pseudo-class

```css
li:not(:last-child) { border-bottom: 1px solid #eee; }
input:not([type="submit"]) { border: 1px solid #ccc; }
```

### Glassmorphism

```css
.glass {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}
```

### Neumorphism

```css
.neumorphic {
  background: #e0e0e0;
  border-radius: 12px;
  box-shadow: 8px 8px 15px #bebebe, -8px -8px 15px #ffffff;
}
.neumorphic:active {
  box-shadow: inset 4px 4px 8px #bebebe, inset -4px -4px 8px #ffffff;
}
```

### Gradient Text

```css
.gradient-text {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
```

### Scroll Snapping Carousel

```css
.carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  gap: 1rem;
}
.carousel-card {
  flex: 0 0 100%;
  scroll-snap-align: start;
}
@media (max-width: 600px) {
  .carousel-card { flex: 0 0 100%; }
}
```

### ::scroll-button() (CSS-Only Carousel Navigation)

```css
.carousel {
  scroll-snap-type: x mandatory;
}
.carousel::scroll-button(inline-start) { content: "←"; }
.carousel::scroll-button(inline-end) { content: "→"; }
.carousel::scroll-marker-group {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
}
.carousel::scroll-marker { content: ""; width: 10px; height: 10px; border-radius: 50%; }
.carousel::scroll-marker:target-current { background: var(--color-primary); }
/* Buttons auto-disable at scroll boundaries */
/* Clicking a scroll button scrolls ~85% of viewport */
```

### CSS-Only Dropdown with :checked

```css
.dropdown { display: none; }
.dropdown-toggle:checked + .dropdown-label + .dropdown { display: block; }
/* Alternative with :focus-within */
.dropdown-wrapper:focus-within .dropdown { display: block; }
```

### Popover API Styling

```css
[popover] {
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
```

### Anchor Positioning

```css
.trigger { anchor-name: --my-trigger; }
.tooltip {
  position: fixed;
  position-anchor: --my-trigger;
  top: anchor(bottom);
  left: anchor(center);
  translate: -50% 8px;
}
```

### @property for Animating Custom Properties

```css
@property --angle {
  syntax: "<angle>";
  initial-value: 0deg;
  inherits: false;
}
.animated-border {
  --border-color: conic-gradient(from var(--angle), #667eea, #764ba2, #667eea);
  border: 2px solid transparent;
  animation: rotate-border 3s linear infinite;
}
@keyframes rotate-border {
  to { --angle: 360deg; }
}
```

### Border Animations with Conic Gradients

```css
.animated-border {
  position: relative;
  border-radius: 8px;
  overflow: hidden;
}
.animated-border::before {
  content: '';
  position: absolute;
  inset: -2px;
  background: conic-gradient(from var(--angle), #667eea, #764ba2, #667eea);
  border-radius: inherit;
  z-index: -1;
  animation: rotate-border 3s linear infinite;
}
```

### Flexbox Patterns

```css
/* Isolate one item with margin: auto (navbar) */
nav ul { display: flex; gap: 1rem; }
nav ul li:first-child { margin-right: auto; }

/* Floating footer fix */
body {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

/* Equal columns */
.equal-cols { display: flex; gap: 1rem; }
.equal-cols > * { flex: 1; }

/* Increase clickable hitbox */
.clickable { display: flex; align-items: center; }
.clickable a { flex-grow: 1; }

/* 3-card layout: align buttons */
.card { display: flex; flex-direction: column; }
.card-content { flex: 1; }
.card button { margin-top: auto; }
```

### Grid Patterns

```css
/* Full-page dashboard with grid-template-areas */
.dashboard {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }

/* Stack elements (replaces position: absolute) */
.stack { display: grid; }
.stack > * { grid-area: 1 / 1; }
```

### Resize Property

```css
textarea { resize: both; }
textarea { resize: vertical; }
textarea { resize: none; }
```

### @starting-style for Entry Animations

```css
dialog {
  opacity: 1;
  transform: scale(1);
  transition: opacity 0.3s, transform 0.3s, display 0.3s allow-discrete;

  @starting-style {
    opacity: 0;
    transform: scale(0.95);
  }
}
```

### light-dark() for Theme Switching

```css
:root { color-scheme: light dark; }
body {
  background: light-dark(#ffffff, #121212);
  color: light-dark(#1a1a1a, #e0e0e0);
}
```

### Container Queries

```css
.card-container { container-type: inline-size; }
@container (min-width: 400px) {
  .card { display: grid; grid-template-columns: 1fr 2fr; }
}
```

### CSS Shorthand vs Longhand

```css
/* Shorthand — use when clear */
margin: 1rem 2rem;
background: #fff url('img.png') no-repeat center / cover;

/* Longhand — use when you need individual control */
margin-top: 1rem;
```

### CSS @function (Custom Functions)

```css
/* Define a custom function */
@function --transparent(--color, --alpha: 0.2) {
  result: oklch(from var(--color) l c h / var(--alpha));
}

/* Typed parameters */
@function --inner-radius(--outer, --padding) {
  result: max(0px, var(--outer) - var(--padding));
}

/* Use the function */
.card {
  background: --transparent(var(--color-primary), 0.1);
  border: 1px solid --transparent(var(--color-primary), 0.2);
}
```

- Parameters start with `--` like custom properties
- `result` defines the return value (not `return` like JS)
- Last `result` wins (CSS cascade behavior)
- Can include media queries inside functions for responsive logic
- Browser support: limited (Chrome 133+, not production-ready yet)

### CSS Subgrid

```css
/* Parent grid defines rows */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: auto 1fr auto;
  gap: 1rem;
}

/* Child inherits parent's row tracks */
.card {
  grid-row: span 3;
  grid-template-rows: subgrid;
}

/* Override gap inside subgrid items */
.card > * { gap: 0; }
```

- Children inherit parent grid tracks instead of creating their own
- Solves the "misaligned buttons in cards" problem
- Use `span N` instead of hardcoded line numbers for responsive grids
- Gap inheritance: parent gap applies to subgrid items too

### View Transitions API

```css
/* Animate between page states */
::view-transition-old(root) {
  animation: fade-out 0.3s ease;
}
::view-transition-new(root) {
  animation: fade-in 0.3s ease;
}

/* Named transitions for specific elements */
.hero-title {
  view-transition-name: hero;
}
```

```javascript
// Trigger transition
document.startViewTransition(() => {
  updateDOM(); // Your DOM update
});
```

- Animates between page states without a full page reload
- Works with SPA navigation and MPA with `@view-transition`

### sibling-index() and sibling-count()

```css
/* Style based on element's position among siblings */
li {
  opacity: calc(1 - (sibling-index() - 1) * 0.1);
}

/* Create numbered backgrounds */
li::before {
  content: counter(sibling-index);
}

/* Calculate values based on total siblings */
.item {
  width: calc(100% / sibling-count());
}
```

- `sibling-index()` — 1-based position among siblings
- `sibling-count()` — total number of siblings
- Replaces manual counter/JS solutions for nth-child calculations

### corner-shape

```css
/* Control corner shape */
.card {
  border-radius: 1rem;
  corner-shape: round; /* default */
}

/* Superellipse (squircle) corners */
.card {
  border-radius: 1rem;
  corner-shape: superellipse;
}
```

- New CSS property for controlling corner curvature
- `round` (default), `superellipse`, `bevel`
- Creates iOS-style squircle corners natively

### Position Fallbacks (@position-try)

```css
/* Define fallback positions */
@position-try --below-right {
  inset: unset;
  top: anchor(bottom);
  left: anchor(left);
}

@position-try --below-left {
  inset: unset;
  top: anchor(bottom);
  right: anchor(right);
}

/* Menu tries positions in order */
.menu {
  position: absolute;
  position-anchor: --my-button;
  position-area: bottom right;
  position-try-fallbacks: --below-right, --below-left;
}
```

- CSS-only intelligent positioning without JavaScript
- Browser tries each fallback until one fits in viewport
- Works with anchor positioning for dropdowns, tooltips, popovers
- Chrome 129+, limited Firefox/Safari support

### Intersection Observer (Scroll Animations)

```javascript
// Observe elements entering viewport
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
    } else {
      entry.target.classList.remove('visible');
    }
  });
}, { threshold: 0.1 });

// Observe all elements
document.querySelectorAll('.animate').forEach(el => observer.observe(el));
```

```css
.animate {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.5s, transform 0.5s;
}
.animate.visible {
  opacity: 1;
  transform: translateY(0);
}
```

- Detects when elements enter/leave viewport
- Better than scroll events (no performance issues)
- Use for: scroll animations, lazy loading, infinite scroll

### Scroll-Driven Animations (CSS-only)

```css
/* Reveal elements on scroll */
@keyframes fade-in {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

.reveal {
  animation: fade-in linear both;
  animation-timeline: view();
  animation-range: entry 0% cover 40%;
}

/* Progress bar */
.progress {
  animation: grow-width linear;
  animation-timeline: scroll();
}

@keyframes grow-width {
  from { width: 0%; }
  to { width: 100%; }
}
```

- `animation-timeline: view()` — tied to element visibility
- `animation-timeline: scroll()` — tied to page scroll
- No JavaScript needed for scroll-triggered animations

### color-mix()

```css
/* Tint/shade without manual color math */
.btn-hover { background: color-mix(in oklch, var(--primary), black 15%); }
.subtle { background: color-mix(in oklch, var(--bg), var(--text) 5%); }
.border { border-color: color-mix(in oklch, var(--primary), transparent 50%); }
```

### @scope (Scoped Styling)

```css
/* Style only within this component's subtree */
@scope (.card) to (.card-footer) {
  p { color: var(--text); }
  a { color: var(--primary); }
}
```

- Prevents style leakage to siblings or parents
- The `to` clause limits scope to a boundary element
- Chrome 118+, Firefox 128+

### <template> Element

```html
<template id="card-template">
  <article class="card">
    <h3></h3>
    <p></p>
  </article>
</template>
```

```javascript
const template = document.getElementById('card-template');
const clone = template.content.cloneNode(true);
clone.querySelector('h3').textContent = item.title;
container.appendChild(clone);
```

- Efficient DOM cloning without re-parsing HTML
- Content not rendered until added to DOM

---

## §6 JavaScript Patterns

### Vanilla DOM Manipulation

```javascript
const el = document.querySelector('.my-element');
const all = document.querySelectorAll('.items');

const div = document.createElement('div');
div.textContent = 'Hello';
document.body.appendChild(div);

el.addEventListener('click', (e) => {
  e.preventDefault();
  // handle
});
```

### localStorage Persistence

```javascript
const saveData = (key, data) => localStorage.setItem(key, JSON.stringify(data));
const loadData = (key, fallback = []) => {
  try { return JSON.parse(localStorage.getItem(key)) || fallback; }
  catch { return fallback; }
};

// Theme persistence
const saveTheme = (theme) => localStorage.setItem('theme', theme);
const loadTheme = () => localStorage.getItem('theme') || 'light';
```

### Dark Mode Toggle

```javascript
const toggle = document.getElementById('dark-toggle');
toggle.addEventListener('click', () => {
  const isDark = document.documentElement.toggleAttribute('data-theme', 'dark');
  saveTheme(isDark ? 'dark' : 'light');
  toggle.textContent = isDark ? '☀️' : '🌙';
});
if (loadTheme() === 'dark') {
  document.documentElement.setAttribute('data-theme', 'dark');
  toggle.textContent = '☀️';
}
```

### Accessible Navbar with ARIA

```javascript
const menuBtn = document.querySelector('[aria-label="Toggle menu"]');
const nav = document.querySelector('nav');

menuBtn.addEventListener('click', () => {
  const expanded = menuBtn.getAttribute('aria-expanded') === 'true';
  menuBtn.setAttribute('aria-expanded', String(!expanded));
  nav.setAttribute('aria-hidden', String(expanded));
});

document.addEventListener('click', (e) => {
  if (!nav.contains(e.target) && !menuBtn.contains(e.target)) {
    menuBtn.setAttribute('aria-expanded', 'false');
    nav.setAttribute('aria-hidden', 'true');
  }
});
```

### Hash Routing

```javascript
function navigate(hash) { window.location.hash = hash; }

window.addEventListener('hashchange', () => {
  const route = window.location.hash.slice(1) || '/';
  document.querySelectorAll('[data-route]').forEach(el => {
    el.hidden = el.dataset.route !== route;
  });
});
```

### Template Literals for Dynamic HTML

```javascript
const renderItem = (item) => `
  <article class="item" data-id="${item.id}">
    <h3>${item.title}</h3>
    <p>${item.content}</p>
    <button class="edit-btn">Edit</button>
    <button class="delete-btn">Delete</button>
  </article>
`;

container.innerHTML = items.map(renderItem).join('');
```

### ES Modules

```html
<script type="module" src="app.js"></script>
```
```javascript
import { saveData, loadData } from './storage.js';
import { renderItems } from './ui.js';
```

### Form Validation

```javascript
form.addEventListener('submit', (e) => {
  e.preventDefault();
  const email = form.querySelector('#email');
  const password = form.querySelector('#password');

  form.querySelectorAll('.error').forEach(el => el.remove());

  if (!email.value.includes('@')) {
    email.insertAdjacentHTML('afterend', '<span class="error">Invalid email</span>');
    return;
  }
  if (password.value.length < 8) {
    password.insertAdjacentHTML('afterend', '<span class="error">Min 8 characters</span>');
    return;
  }
  // Submit...
});
```

### Async/Await with Fetch

```javascript
// Fetch API with async/await
async function loadData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    return await response.json();
  } catch (error) {
    console.error('Fetch failed:', error);
    return null;
  }
}

// Usage
const data = await loadData('https://api.example.com/data');
```

### Promises (Quick Reference)

```javascript
// Create a promise
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

// Chain promises
fetchUser()
  .then(user => fetchPosts(user.id))
  .then(posts => renderPosts(posts))
  .catch(error => showError(error));

// Parallel execution
const [users, posts] = await Promise.all([
  fetch('/api/users'),
  fetch('/api/posts')
]);
```

### Event Delegation

```javascript
// Handle events on parent, delegate to children
document.querySelector('.list').addEventListener('click', (e) => {
  const item = e.target.closest('.item');
  if (!item) return;

  if (e.target.matches('.delete-btn')) {
    item.remove();
  } else if (e.target.matches('.edit-btn')) {
    editItem(item.dataset.id);
  }
});
```

### AbortController (Cancel Fetches)

```javascript
const controller = new AbortController();

fetch('/api/data', { signal: controller.signal })
  .then(res => res.json())
  .catch(err => {
    if (err.name === 'AbortError') console.log('Cancelled');
  });

// Cancel after 5 seconds
setTimeout(() => controller.abort(), 5000);
```

### structuredClone() (Deep Clone)

```javascript
// Deep clone objects (replaces JSON roundtrip)
const original = { nested: { value: 42 } };
const clone = structuredClone(original);

// Works with Dates, Maps, Sets, TypedArrays
const complex = { date: new Date(), map: new Map([['key', 'value']]) };
const deepClone = structuredClone(complex);
```

### crypto.randomUUID()

```javascript
// Generate unique IDs without libraries
const id = crypto.randomUUID();
// "3b241101-e2bb-4d7a-8702-9e0e0a3f0f87"

// Useful for keys in dynamic lists
const item = { id: crypto.randomUUID(), text: 'New item' };
```

---

## §7 CSS Framework Allowlist

**Eligibility criteria** (all must pass):
- Loaded via single `<link>` tag from CDN
- No JavaScript dependency
- No build step / no Node/npm required
- Semantic HTML-first (minimal or zero class names)
- MIT or permissive license
- < ~60KB gzipped
- Works in modern evergreen browsers

### Category A: Classless (zero classes, pure semantic HTML)

| Framework | Size | Best for |
|-----------|------|----------|
| Pico CSS | ~14KB | Most complete; classless version; 20 themes; dark/light |
| EdibleCSS | ~3.8KB | WCAG AA, truly zero classes |
| Acorn CSS | ~3.8KB | Generative design from 5 variables |
| The New CSS | ~5KB | Addon system; modular; fast |
| Simple.css | ~6KB | Truly minimal classless |
| MVP.css | ~8KB | Quick prototypes |
| Sakura | ~8KB | Minimal, pretty defaults |
| Tacit | ~5KB | Classless for any project |
| Holiday.css | ~5KB | Modern aesthetic, nav/dropdowns |
| new.css | ~4KB | Classic classless |

### Category B: shadcn-aesthetic (no React needed)

| Framework | Size | Best for |
|-----------|------|----------|
| slick-css | ~5.7KB | shadcn style via attribute selectors; tiny |
| shadcn-classless | ~15KB | Zero-class, dialog/nav/accordion support |
| Daft CSS | ~55KB | Closest to shadcn visuals; zero JS; modern CSS only |

### Category C: Minimal-class component frameworks

| Framework | Size | Best for |
|-----------|------|----------|
| µCSS | ~19KB | 20+ themes, button/form/nav components |
| Aesthetium | ~12KB | Attribute-based variants; clean OpenAI-style |
| Spectre.css | ~10KB | Lightweight component framework |

### Category D: Effect libraries (add-ons)

| Framework | Size | Best for |
|-----------|------|----------|
| GlassKit | ~6.5KB | Pure CSS glassmorphism component library |
| signature-css-framework | ~15KB | Glassmorphism + RTL support |

### Allowed JS Enhancement

- HTMX (~14KB) from CDN — for AJAX, polling, form validation, search

---

## §8 Anti-Patterns (What NOT to Do)

| Don't | Instead |
|-------|---------|
| `npm init` / `package.json` for static HTML | Just write HTML files |
| Tailwind CSS for simple webpages | Plain CSS or a classless framework (§7) |
| Bootstrap | Semantic HTML + custom CSS (§4, §5) |
| CSS preprocessor (Sass/PostCSS) | Native CSS nesting (§5) |
| Build tool (Webpack/Vite/Parcel) | No build step — files served directly |
| Premature file splitting | Keep files until they exceed ~400 lines |
| Component abstraction for one instance | Only abstract at 5+ repeated instances |
| TypeScript for simple pages | Plain JavaScript (§6) |
| `src/` / `dist/` folder "for later" | Root-level files until there's a need |
| Utility-first CSS for single-page sites | Semantic CSS with custom properties |
| BEM naming (`.card__title--large`) | Simple selectors (`.card-title`) |
| `<script>` in `<head>` without `defer` | `<script defer>` or place before `</body>` |
| `document.write()` | DOM manipulation methods (§6) |
| `var` in JS | `const` / `let` only |
| `setTimeout` for animations | CSS transitions/animations or `requestAnimationFrame` |
| `innerHTML` with user input | `textContent` or sanitize with DOMPurify |
| onclick in template literals | Event delegation with `data-id` attributes (§6) |

---

## §9 Design Quick Reference

| Property | Value |
|----------|-------|
| Spacing scale | 4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96 (4px base) |
| Body font | 16px, line-height 1.5, max 4 font sizes |
| Touch targets | Min 44×44px, recommended 48×48px |
| Contrast | 4.5:1 normal text, 3:1 large text, 3:1 UI components (WCAG AA) |
| Button hierarchy | Primary (solid, ONE per view), Secondary (outlined), Tertiary (text-only) |
| Dark mode | bg #121212, text #E0E0E0, reduce saturation |
| Form labels | Always above inputs, never placeholders as labels |

---

## §10 Accessibility Minimums

- Semantic HTML = most accessibility for free
- `:focus-visible` rings on all interactive elements
- `prefers-reduced-motion` guard on all animations
- Color never the sole indicator (add icons/text labels)
- Form inputs: always `<label>`, `autocomplete`, `inputmode`
- ARIA attributes updated via JavaScript when state changes
- `<dialog>` for modals (handles focus trap natively)
- Test with voiceover/narrator before shipping

---

## §10b Additional Techniques (from Video Transcripts)

### align-content on Block Elements (Chrome 123+)

```css
/* Center vertically without flexbox/grid */
.center-block {
  text-align: center;
  align-content: center;
  min-height: 200px;
}
```

### Customizable Select (appearance: base-select)

```css
/* Opt into new select model */
select {
  appearance: base-select;
}

/* Style the picker dropdown */
select::picker {
  appearance: base-select;
  background: white;
  border: 1px solid #ccc;
  border-radius: 8px;
}

/* Rotate arrow when open */
select:open::picker-icon {
  transform: rotate(180deg);
  transition: transform 0.3s;
}

/* Style the checkmark */
option::checkmark {
  content: '✓';
  color: green;
}
```

- Chrome 134+, limited Firefox/Safari support
- Falls back to native select styling in unsupported browsers

### display: contents

```css
/* Remove element from layout, children become direct children of parent */
.wrapper {
  display: contents;
}
```

- Useful for maintaining flex/grid layouts with wrapper elements
- Wrapper disappears from layout flow, children inherit parent's grid

### CSS Counters for Numbering

```css
/* Auto-number headings */
:root { counter-reset: heading; }
h2 { counter-increment: heading; }
h2::before {
  content: counter(heading) ". ";
  font-weight: bold;
}
```

### :popover-open Pseudo-class

```css
/* Style popover only when open */
.menu:popover-open {
  display: grid;
}
```

- Browser toggles display automatically — don't set display manually

### Inert Attribute (Accessibility)

```html
<!-- Make element non-interactive and invisible to assistive tech -->
<div inert>
  <button>This button is unreachable</button>
</div>
```

- Prevents focus and click events on disabled content
- Useful for off-screen menus, background content during modals

### Skip Link (Accessibility)

```html
<a href="#main" class="skip-link">Skip to content</a>
<main id="main">...</main>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  padding: 8px;
  background: #000;
  color: white;
  z-index: 100;
  transition: top 0.3s;
}
.skip-link:focus { top: 0; }
```

### Glow Effect with Box-Shadow

```css
.glow {
  box-shadow: 40px 0 100px blue, -40px 0 100px purple;
}
```

### Gradient Fill on Hover

```css
.btn {
  position: relative;
  z-index: 0;
}
.btn::after {
  content: '';
  position: absolute;
  inset: -3px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  z-index: -1;
  border-radius: inherit;
}
.btn:hover::after { z-index: 0; }
```

### Ternary Operator (JS Pattern)

```javascript
// Concise conditional
const result = condition ? valueIfTrue : valueIfFalse;

// In event handlers
darkMode !== 'active' ? enableDark() : disableDark();
```

### Early Return Pattern (JS)

```javascript
// Guard clauses — exit early, keep happy path flat
function processUser(user) {
  if (!user) return;
  if (!user.loggedIn) return;
  if (!user.email) return;
  // happy path here
}
```

### Spread Operator for Immutable Copies

```javascript
// Object copy (breaks reference)
const copy = { ...original };

// Override specific property
const updated = { ...product, price: product.price * 0.5 };

// Array copy
const newArr = [...oldArr];
```

---

## §11 Cookbook Reference

Patterns live in `cookbook/` — each is a standalone HTML file with embedded CSS. Browse before writing new CSS from scratch.

### Layouts
- `cookbook/layouts/responsive-grid/` — Auto-fit grid, no media queries
- `cookbook/layouts/sidebar-layout/` — Sidebar + main content
- `cookbook/layouts/hero-section/` — Full-width hero with content
- `cookbook/layouts/holy-grail/` — Classic header/sidebar/main/footer
- `cookbook/layouts/dashboard/` — grid-template-areas dashboard
- `cookbook/layouts/masonry/` — Pinterest-style masonry grid

### Navigation
- `cookbook/navigation/navbar/` — Flexbox navbar with margin:auto isolation
- `cookbook/navigation/sidebar-menu/` — Animated dropdowns, CSS grid
- `cookbook/navigation/mobile-hamburger/` — ARIA accessible toggle
- `cookbook/navigation/sticky-header/` — position: sticky header
- `cookbook/navigation/breadcrumbs/` — Semantic breadcrumb trail

### Forms
- `cookbook/forms/login-signup/` — Form validation, responsive
- `cookbook/forms/form-validation/` — Front-end validation patterns
- `cookbook/forms/search-bar/` — Search input with icon
- `cookbook/forms/contact-form/` — Multi-field contact form
- `cookbook/forms/multi-step/` — Step-by-step form wizard

### Cards
- `cookbook/cards/profile-card/` — User profile card
- `cookbook/cards/product-card/` — E-commerce product card
- `cookbook/cards/article-card/` — Blog article preview
- `cookbook/cards/pricing-card/` — Pricing tier comparison

### Effects
- `cookbook/effects/glassmorphism/` — backdrop-filter: blur + rgba
- `cookbook/effects/neumorphism/` — Dual box-shadows
- `cookbook/effects/gradient-text/` — background-clip: text
- `cookbook/effects/border-animations/` — @property + conic-gradient
- `cookbook/effects/hover-effects/` — Transform and transition effects
- `cookbook/effects/scroll-snap/` — CSS-only carousel
- `cookbook/effects/scroll-driven-animations/` — animation-timeline: view()
- `cookbook/effects/animated-tooltips/` — ::before/::after with attr()
- `cookbook/effects/triangles/` — Border trick for CSS shapes

### Interactivity
- `cookbook/interactivity/dark-mode/` — CSS vars + localStorage + prefers-color-scheme
- `cookbook/interactivity/todo-app/` — localStorage CRUD
- `cookbook/interactivity/modal-dialog/` — `<dialog>` element
- `cookbook/interactivity/accordion/` — `<details>/<summary>`
- `cookbook/interactivity/carousel/` — Scroll-snap + ::scroll-button
- `cookbook/interactivity/tabs/` — :checked pseudo-class
- `cookbook/interactivity/toast-notifications/` — Auto-dismiss notifications
- `cookbook/interactivity/popover-api/` — HTML popover attribute
- `cookbook/interactivity/css-only-dropdown/` — :focus/:focus-within

### Typography
- `cookbook/typography/fluid-type/` — clamp() for responsive type
- `cookbook/typography/text-balance/` — text-wrap: balance
- `cookbook/typography/decorative-headings/` — Gradient text, writing-mode
- `cookbook/typography/vertical-rhythm/` — Consistent spacing baseline
