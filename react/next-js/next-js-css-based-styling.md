<!-- ********************* -->

# Classical Styling of React Components with CSS Modules in Next.js

<!-- ********************* -->

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This article explains how to style React components in a Next.js project using CSS Modules. CSS Modules are a way to scope CSS locally to a component while still writing familiar CSS.

* What CSS Modules are
* File naming and placement
* How class names are transformed
* Using CSS Modules inside a component
* Conventions for naming files
* Shared styles vs. global CSS
* References

<!-- ********************* -->

# What CSS Modules are

<!-- ********************* -->

A CSS Module is a standard `.css` file with the suffix `.module.css`. When imported into a component, its class names are automatically scoped locally. This avoids style collisions across your project.

Example: `Button.module.css`

```css
.primary {
  padding: 8px 12px;
  border-radius: 4px;
  background: blue;
  color: white;
}
```

When imported into `Button.tsx`, the `.primary` class is transformed into a unique identifier at build time, ensuring it doesn’t collide with other `.primary` classes elsewhere.

<!-- ********************* -->

# File naming and placement

<!-- ********************* -->

There is no strict requirement on file names, but the convention is:

* Use the same base name as the component.
* Place the `.module.css` file next to the component file.

Example project structure:

```
components/
  Button.tsx
  Button.module.css
  Card.tsx
  Card.module.css
```

This convention makes it clear which CSS file belongs to which component.

<!-- ********************* -->

# How class names are transformed

<!-- ********************* -->

At build time, Next.js rewrites class names into unique values:

```css
.primary { background: blue; }
```

becomes something like:

```css
.Button_primary__3H9fs { background: blue; }
```

When imported, you get a JavaScript object mapping:

```ts
styles = {
  primary: "Button_primary__3H9fs"
}
```

This ensures local scoping.

<!-- ********************* -->

# Using CSS Modules inside a component

<!-- ********************* -->

**Button.tsx**

```tsx
import styles from "./Button.module.css";

export function Button({ children }: { children: React.ReactNode }) {
  return <button className={styles.primary}>{children}</button>;
}
```

Usage:

```tsx
<Button>Save</Button>
```

<!-- ********************* -->

# Conventions for naming files

<!-- ********************* -->

* **Convention:** Match component name and CSS Module name (`Button.tsx` ↔ `Button.module.css`).
* **Requirement:** Not mandatory; any name ending in `.module.css` works.
* **Best practice:** Stick with the convention for clarity, unless creating a shared style file.

<!-- ********************* -->

# Shared styles vs. global CSS

<!-- ********************* -->

* **Shared styles:** Create a file like `common.module.css` and import it in multiple components.
* **Global styles:** Use `globals.css` (in the `app/` folder) for resets, typography, or styles applied to the entire app. These are not scoped and affect all components.

**Example of shared styles**

```css
/* common.module.css */
.cardShadow {
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
```

```tsx
import common from "./common.module.css";

export function Card({ children }: { children: React.ReactNode }) {
  return <div className={common.cardShadow}>{children}</div>;
}
```

<!-- ********************* -->

# A Practical global.css and Key Provisions

<!-- ********************* -->

Here is a practical `globals.css` you can include in your Next.js project. It covers resets, theme tokens, base typography, and utilities:

```css
/* 1) CSS Reset */
*, *::before, *::after { box-sizing: border-box; }
* { margin: 0; }
html:focus-within { scroll-behavior: smooth; }
body { min-height: 100vh; text-rendering: optimizeLegibility; -webkit-font-smoothing: antialiased; }
img, picture, video, canvas, svg { display: block; max-width: 100%; }
input, button, textarea, select { font: inherit; }

/* 2) Theme tokens */
:root {
  --color-bg: #0b1220;
  --color-surface: #121a2b;
  --color-border: #2b3752;
  --color-text: #e5eaf3;
  --color-muted: #a7b1c5;
  --color-link: #6ea8fe;
  --color-primary: #5b8cff;
  --color-primary-700: #3f6ff0;
  --color-danger: #ff6b6b;
  --color-success: #5bd6a1;
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --radius-sm: 6px;
  --radius-md: 10px;
  --shadow-1: 0 1px 2px rgba(0,0,0,.12), 0 3px 8px rgba(0,0,0,.14);
}

/* 3) Base typography and layout */
html, body { color: var(--color-text); background: var(--color-bg); }
body { font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial; line-height: 1.6; }
main { max-width: 900px; margin: 0 auto; padding: var(--space-6); }

h1 { font-size: clamp(1.6rem, 3vw, 2.2rem); margin-bottom: var(--space-4); }
h2 { font-size: clamp(1.3rem, 2.5vw, 1.6rem); margin-top: var(--space-6); margin-bottom: var(--space-2); }

/* 4) Links */
a { color: var(--color-link); text-decoration: none; }
a:hover { text-decoration: underline; }

/* 5) Focus styles */
:where(a, button, input, select, textarea):focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

/* 6) Containers */
.section { background: var(--color-surface); border: 1px solid var(--color-border); border-radius: var(--radius-md); box-shadow: var(--shadow-1); padding: var(--space-6); }
.muted { color: var(--color-muted); }

/* 7) Small utilities */
.stack > * + * { margin-top: var(--space-2); }
.row { display: flex; gap: var(--space-2); align-items: center; }
```

### Key provisions explained

* `*` selector applies to all elements; used here to reset margins and set `box-sizing`.
* `*::before, *::after` ensures pseudo-elements also inherit `box-sizing`.
* `:root` is the highest-level element (html); defining variables here makes them globally available.
* `:where(...)` is a forgiving selector that doesn’t increase specificity; used for focus outlines.
* `clamp()` sets a font size that adapts between a min, preferred, and max value.
* `.stack > * + *` is a utility that adds spacing between stacked children without adding margin to the first element.

<!-- ********************* -->

# Global CSS baseline for Next.js

<!-- ********************* -->

Below is a practical `app/globals.css` you can drop into a new Next.js app. It provides:

* A tiny reset
* Design tokens via CSS variables (colors, spacing, radius, shadows)
* Base typography/layout
* Accessible focus outlines
* A few light utility classes

```css
/* 1) CSS Reset-ish (small, modern) */
*, *::before, *::after { box-sizing: border-box; }
* { margin: 0; }
html:focus-within { scroll-behavior: smooth; }
body { min-height: 100vh; text-rendering: optimizeLegibility; -webkit-font-smoothing: antialiased; }
img, picture, video, canvas, svg { display: block; max-width: 100%; }
input, button, textarea, select { font: inherit; }

/* 2) Theme tokens (colors, spacing, radius, shadows) */
:root {
  /* colors */
  --color-bg: #0b1220;          /* page background */
  --color-surface: #121a2b;     /* cards, sections */
  --color-border: #2b3752;      /* subtle borders */
  --color-text: #e5eaf3;        /* primary text */
  --color-muted: #a7b1c5;       /* secondary text */
  --color-link: #6ea8fe;        /* links */
  --color-primary: #5b8cff;     /* brand */
  --color-primary-700: #3f6ff0; /* brand darker */
  --color-danger: #ff6b6b;
  --color-success: #5bd6a1;

  /* spacing & radius */
  --space-1: 0.25rem; /* 4px  */
  --space-2: 0.5rem;  /* 8px  */
  --space-3: 0.75rem; /* 12px */
  --space-4: 1rem;    /* 16px */
  --space-6: 1.5rem;  /* 24px */
  --radius-sm: 6px;
  --radius-md: 10px;
  --shadow-1: 0 1px 2px rgba(0,0,0,.12), 0 3px 8px rgba(0,0,0,.14);
}

/* 3) Base typography and layout */
html, body { color: var(--color-text); background: var(--color-bg); }
body { font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji", "Segoe UI Emoji"; line-height: 1.6; }
main { max-width: 900px; margin: 0 auto; padding: var(--space-6); }

h1 { font-size: clamp(1.6rem, 3vw, 2.2rem); margin-bottom: var(--space-4); }
h2 { font-size: clamp(1.3rem, 2.5vw, 1.6rem); margin-top: var(--space-6); margin-bottom: var(--space-2); }
p + p { margin-top: var(--space-2); }

/* 4) Links */
a { color: var(--color-link); text-decoration: none; }
a:hover { text-decoration: underline; }

/* 5) Focus styles (keyboard a11y) */
:where(a, button, input, select, textarea, summary):focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

/* 6) Containers */
.section { background: var(--color-surface); border: 1px solid var(--color-border); border-radius: var(--radius-md); box-shadow: var(--shadow-1); padding: var(--space-6); }
.muted { color: var(--color-muted); }

/* 7) Small utilities */
.stack > * + * { margin-top: var(--space-2); }   /* vertical rhythm */
.row { display: flex; gap: var(--space-2); align-items: center; }
```

<!-- ********************* -->

# Explaining the key selectors and tokens

<!-- ********************* -->

This section clarifies the less-obvious CSS constructs used above.

**Universal selector `*`**

* `*` matches **every element**; `*::before, *::after` also target pseudo-elements. We use it to set `box-sizing: border-box` globally (padding/border included in width/height) and to zero out default margins.

**Pseudo-elements `::before` / `::after`**

* Virtual elements attached to a real element, often used for icons/decorative lines. Including them with `box-sizing` keeps layout math consistent.

**Root scope `:root`**

* Selects the document root (the `<html>` element). It’s the perfect place to define **CSS custom properties** (variables) like `--color-primary`. These variables can be read anywhere with `var(--color-primary)`.

**CSS variables `var(--token)`**

* Tokens such as `--space-4` or `--color-text` make spacing/colors consistent. Change them once in `:root` and the entire app updates.

**Functional sizing `clamp(min, preferred, max)`**

* Used on headings so font sizes scale with viewport width, but never below/above reasonable bounds.

**Adjacent-sibling combinator `p + p`**

* Targets a `<p>` that directly follows another `<p>` to add vertical spacing only **between** paragraphs (no extra space before the first one).

**Focus management `:focus-visible`**

* Shows focus outlines only when appropriate (e.g., keyboard navigation), reducing noisy outlines for mouse users while remaining accessible.

**Selector list with `:where(...)`**

* `:where(a, button, input, ...)` groups multiple selectors without adding specificity. Great for defining a single focus style for many controls.

**Smooth scrolling `html:focus-within`**

* When any element inside `<html>` has focus, the page uses `scroll-behavior: smooth`. Improves keyboard/skip-link navigation.

**Utility classes `.stack` and `.row`**

* `.stack > * + *` applies top margin to every sibling **after** the first, giving consistent vertical rhythm.
* `.row` is a small horizontal flex layout with gap and centered alignment.

**Containers `.section`, muted text `.muted`**

* `.section` is a neutral card-like container using tokens for background, border, radius, and shadow.
* `.muted` downtones text using `--color-muted`.

**A note on specificity**

* Using low-specificity selectors (`:where(...)`, class names) keeps styles easy to override inside CSS Modules when needed.

<!-- ********************* -->

# References

<!-- ********************* -->

* [Next.js CSS Modules Documentation](https://nextjs.org/docs/app/building-your-application/styling/css-modules) – Official guide on using CSS Modules in Next.js.
* [React Docs: Styling](https://react.dev/learn/styling) – Overview of styling approaches in React, including CSS Modules.
