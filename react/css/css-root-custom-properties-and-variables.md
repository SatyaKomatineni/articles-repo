<!-- ********************* -->

# \:root & CSS Custom Properties — A Practical Introduction

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

A concise guide to the `:root` selector and CSS custom properties (variables): what they are, how `var()` looks up values through the cascade, how to scope/override tokens, common theming patterns, and where to place them in a Next.js app.

* Overview
* What is `:root`?
* Custom properties: basics (syntax & rules)
* Lookup & inheritance model (`var()`)
* Scoping & overrides (classes, attributes, components)
* Theming patterns (dark mode, prefers‑color‑scheme)
* Dynamic updates with JavaScript
* Recipes (copy‑ready)
* Next.js context (where to put global tokens)
* Pitfalls & FAQs
* References

<!-- ********************* -->

# Overview

<!-- ********************* -->

CSS custom properties—often called *CSS variables*—let you define reusable design tokens (colors, spacing, radii) that participate in normal CSS inheritance. The `:root` selector is a convenient, global place to declare defaults.

<!-- ********************* -->

# What is `:root`?

<!-- ********************* -->

* `:root` is a **pseudo‑class** that matches the document’s top element (in HTML, that is `<html>`).
* Defining variables on `:root` makes them **globally available** via inheritance.
* **Specificity note**: `:root` has slightly higher specificity than the `html` element selector, which can help predictable overrides when both appear.

```css
/* Global tokens */
:root {
  --brand: #0ea5e9;
  --radius: 12px;
}
```

<!-- ********************* -->

# Custom properties: basics (syntax & rules)

<!-- ********************* -->

* Names start with `--` and can hold any valid CSS value.
* Read values with `var(--token, optional-fallback)`.
* They are **inherited** by default and can be **overridden** anywhere in the cascade.

```css
.card {
  border-radius: var(--radius, 8px);
  border: 1px solid var(--brand);
}
```

<!-- ********************* -->

# Lookup & inheritance model (`var()`)

<!-- ********************* -->

`var(--token)` resolves by walking **up the DOM tree** from the element:

1. Use a value set **on the element** if present.
2. Otherwise, check its **ancestors** in order.
3. Finally reach `<html>`/`:root`.
4. If no value is found and no fallback is provided, the declaration becomes invalid (the property is ignored).

```css
/* Fallback example */
.button { background: var(--btn-bg, #222); }
```

<!-- ********************* -->

# Scoping & overrides (classes, attributes, components)

<!-- ********************* -->

You can define variables on any selector (class, attribute, element). Descendants inherit the closest value.

```css
/* Scoped to a component subtree */
.card { --pad: 12px; }
.card .content { padding: var(--pad); }

/* Attribute-scoped tokens */
[data-theme="brand"] { --brand: #0ea5e9; }
[data-theme="alt"]   { --brand: #7c3aed; }
```

This plays well with CSS Modules—define a variable on a component’s root class, consume it inside descendants.

<!-- ********************* -->

# Theming patterns (dark mode, prefers‑color‑scheme)

<!-- ********************* -->

**Class/attribute toggle** (manual):

```css
:root { --bg: #fff; --fg: #111; }
.dark  { --bg: #111; --fg: #eee; }
body { background: var(--bg); color: var(--fg); }
```

**Automatic preference**:

```css
:root { --bg: #fff; --fg: #111; }
@media (prefers-color-scheme: dark) {
  :root { --bg: #111; --fg: #eee; }
}
```

**Hybrid** (preference + user toggle wins):

```css
/* 1) Base + system preference */
:root { --bg: #fff; --fg: #111; }
@media (prefers-color-scheme: dark) {
  :root { --bg: #111; --fg: #eee; }
}
/* 2) User override via attribute/class */
[data-theme="light"] { --bg: #fff; --fg: #111; }
[data-theme="dark"]  { --bg: #111; --fg: #eee; }
```

<!-- ********************* -->

# Dynamic updates with JavaScript

<!-- ********************* -->

Update tokens at runtime without touching CSS files:

```js
// Set a global token
document.documentElement.style.setProperty('--brand', '#1e40af');

// Scope to a component root
const card = document.querySelector('.card');
card?.style.setProperty('--pad', '16px');
```

<!-- ********************* -->

# Recipes (copy‑ready)

<!-- ********************* -->

**Semantic color system**

```css
:root {
  /* roles */
  --color-bg:    oklch(1 0 0);
  --color-fg:    oklch(0.22 0 0);
  --color-ring:  Highlight; /* uses system color in HC modes */
}
@media (prefers-color-scheme: dark) {
  :root { --color-bg: oklch(0.16 0 0); --color-fg: oklch(0.97 0 0); }
}
```

**Accessible focus ring (low specificity) + token override**

```css
:where(a, button, input, select, textarea, summary):focus-visible {
  outline: 2px solid var(--ring, var(--color-ring));
  outline-offset: 2px;
}
/* Component adjusts only the token */
.button { --ring: #0ea5e9; }
```

**Spacing scale**

```css
:root { --space-1: 4px; --space-2: 8px; --space-3: 12px; --space-4: 16px; }
.stack > * + * { margin-block-start: var(--space-3); }
```

**Component‑local tokens (CSS Modules idea)**

```css
/* Card.module.css */
.root { --card-pad: 12px; --card-radius: 10px; }
.content { padding: var(--card-pad); border-radius: var(--card-radius); }
```

<!-- ********************* -->

# Next.js context (where to put global tokens)

<!-- ********************* -->

* Import your global stylesheet **once** in the **root layout** (`app/layout.tsx`), and declare app‑wide tokens in `:root` there.
* Use CSS Modules for component‑local variables and styles (`*.module.css`).
* Keep globals small: tokens, base typography, resets; push specifics into components.

```tsx
// app/layout.tsx
import "./globals.css";
export default function RootLayout({ children }) {
  return (
    <html lang="en"><body>{children}</body></html>
  );
}
```

<!-- ********************* -->

# Pitfalls & FAQs

<!-- ********************* -->

* **Do variables override by specificity?** They follow normal cascade: closest definition wins; selector specificity and order still matter.
* **Do CSS Modules change specificity?** No—only class names are scoped. Specificity rules stay the same.
* **`html` vs `:root`?** In HTML they match the same element; `:root` has slightly higher specificity.
* **Performance?** Custom properties are efficient; they’re resolved at computed‑value time. Avoid excessively deep overrides if not needed.
* **Math with variables?** Use `calc()` with `var()`, e.g., `calc(var(--space-2) * 2)`.

<!-- ********************* -->

# References

<!-- ********************* -->

* [MDN — Using CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties): Syntax, inheritance, and practical examples.
* [MDN — `:root` pseudo‑class](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Aroot): Definition, relation to `<html>`, and specificity notes.
* [MDN — `var()` CSS function](https://developer.mozilla.org/en-US/docs/Web/CSS/var): Function syntax and fallback behavior.
* [W3C — CSS Custom Properties for Cascading Variables Level 1](https://www.w3.org/TR/css-variables-1/): Formal definition of custom properties and `var()` lookup rules.
* [Next.js — Styling (App Router)](https://nextjs.org/docs/app/building-your-application/styling): Where to import global CSS and how to use CSS Modules.
