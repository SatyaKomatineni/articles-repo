<!-- ********************* -->

# Summarized CSS Guide — Topics, Briefs, and References (with Next.js context)

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

A compact guide that lists what you learned in this thread, briefly explains each topic, and consolidates all relevant references—including links useful in a Next.js App Router context.

* Topics list
* Global vs. Module CSS (Next.js)
* CSS Custom Properties (`--vars`) and `:root`
* Pseudo-classes vs. Pseudo-elements
* `:is()` and `:where()`
* CSS Specificity (how the score is calculated)
* Selectors & Combinators (quick notes)
* Focus Rings (accessible patterns)
* CSS Resets / Normalize / Preflight
* CSS Cascade Layers (`@layer`)
* Next.js: layouts, templates, and where CSS mounts
* Practical patterns & tips
* References

<!-- ********************* -->

# Topics list

<!-- ********************* -->

* Global vs. Module CSS (scopes; where to import in Next.js)
* CSS variables, lookup rules, and why `:root` is common
* Pseudo-classes vs. pseudo-elements
* Grouping selectors with `:is()` and low-specificity scoping with `:where()`
* Specificity scoring and conflict resolution
* Common selectors and combinators
* Accessible focus rings and safe overrides
* Resets vs. normalize vs. Preflight
* Cascade layers and why you don’t always see them
* How layouts/templates affect CSS mounting in Next.js

<!-- ********************* -->

# Global vs. Module CSS (Next.js)

<!-- ********************* -->

* **Global CSS** lives in a file like `app/globals.css` and must be imported **once** (typically in `app/layout.tsx`). It applies across the app.
* **CSS Modules** (`*.module.css`) are imported by components; class names are locally scoped (hashed) to prevent collisions. Specificity rules are unchanged; only names are localized.

<!-- ********************* -->

# CSS Custom Properties (`--vars`) and `:root`

<!-- ********************* -->

* Define tokens with `--name: value;` and read them with `var(--name, fallback)`.
* Lookup follows normal inheritance: element → ancestors → `<html>`/`:root`.
* `:root` is a pseudo-class that matches the top element (in HTML, `<html>`) and has slightly higher specificity than `html`. It’s a convenient place for global design tokens.
* Variables can be scoped anywhere (e.g., on `.card`) to affect only that subtree.

**Example**

```css
:root { --brand: #0ea5e9; }
.card { --pad: 12px; padding: var(--pad); border-color: var(--brand); }
```

<!-- ********************* -->

# Pseudo-classes vs. Pseudo-elements

<!-- ********************* -->

* **Pseudo-classes** (one colon `:`) select elements in a **state**: `:hover`, `:focus-visible`, `:disabled`, `:is()`, `:where()`, `:has()`.
* **Pseudo-elements** (two colons `::`) target **parts** of elements: `::before`, `::after`, `::selection`, `::marker`, `::backdrop`.

<!-- ********************* -->

# `:is()` and `:where()`

<!-- ********************* -->

* Both match **any** of the listed alternatives and help avoid selector repetition.
* **`:is(...)`** contributes the specificity of its **most specific** argument.
* **`:where(...)`** contributes **zero** specificity—ideal for base/structural rules that should be easy to override.

**Example**

```css
/* Group states without repetition */
nav :is(a, button):focus-visible { outline: 2px solid; }
/* Low-specificity base typography */
:where(article, aside) :where(h1, h2, h3) { margin-block: .5rem; }
```

<!-- ********************* -->

# CSS Specificity (how the score is calculated)

<!-- ********************* -->

* Score tuple: **(inline, IDs, classes/attrs/pseudo-classes, elements/pseudo-elements)**.
* Examples: `button:hover` → (0,0,1,1); `button` → (0,0,0,1); `main :is(h1,h2,h3)` → (0,0,0,2); `:where(...)` adds 0.
* Ties go to **later source order**; `!important` overrides normal importance; **@layer** order is considered before specificity.

<!-- ********************* -->

# Selectors & Combinators (quick notes)

<!-- ********************* -->

* **Descendant** `A B` (any depth) vs. **Child** `A > B` (direct only) vs. **Siblings** `A + B` (adjacent), `A ~ B` (general).
* Attributes: `[type="text"]`, starts-with `^=`, ends-with `$=`, contains `*=`.
* Structural: `:first-child`, `:last-child`, `:nth-child()`, `:not()`, `:has()`.
* Handy: `:scope` (current query root), `:target` (URL fragment).

<!-- ********************* -->

# Focus Rings (accessible patterns)

<!-- ********************* -->

* Prefer `:focus-visible` to show rings when they’re expected (keyboard users), and keep a clear indicator.
* Keep base rings low specificity so components can adjust without `!important`.

**Base + component override**

```css
:where(a, button, input, select, textarea, summary):focus-visible {
  outline: 2px solid var(--ring, Highlight);
  outline-offset: 2px;
}
.btn { --ring: #0ea5e9; }            /* change only the ring color */
.btn:focus-visible { outline-offset: 3px; }
```

<!-- ********************* -->

# CSS Resets / Normalize / Preflight

<!-- ********************* -->

* **Reset (aggressive)** removes most UA defaults; you reintroduce what you need.
* **Normalize (lightweight)** aligns inconsistencies but keeps sensible defaults.
* **Preflight (Tailwind)** is an opinionated normalize plus base rules.
* Use low-specificity patterns (e.g., `:where(...)`) so components can override easily.

**Modern minimal reset (snippet)**

```css
*, *::before, *::after { box-sizing: border-box; }
:where(body,h1,h2,h3,h4,h5,h6,p,figure,blockquote,dl,dd) { margin: 0; }
html:focus-within { scroll-behavior: smooth; }
:where(img,picture,video,canvas,svg) { display: block; max-width: 100%; }
:where(input,button,textarea,select) { font: inherit; }
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```

<!-- ********************* -->

# CSS Cascade Layers (`@layer`)

<!-- ********************* -->

* Layers are **named buckets** evaluated **before** specificity: importance → **layer** → specificity → source order.
* Typical order: `@layer reset, base, components, utilities;` (leftmost lowest priority).
* Unlayered author CSS outranks layered rules of the same importance; keep project CSS in named layers for predictability. Many stacks (Tailwind, CSS-in-JS) already manage order for you.

**Pattern**

```css
@layer reset, base, components, utilities;
@layer reset { /* UA neutralization / normalize */ }
@layer base { :where(h1,h2,h3) { margin: 0; } }
@layer components { .card { padding: 1rem; } }
@layer utilities { .text-center { text-align: center; } }
```

<!-- ********************* -->

# Next.js: layouts, templates, and where CSS mounts

<!-- ********************* -->

* **Root layout** (`app/layout.tsx`) persists for the app lifetime; import **global CSS** here.
* **Nested layouts** persist within their segments; good for shells and providers.
* **Templates** remount on navigation within a segment; use for animation/reset boundaries.
* **Pages** swap on navigation; CSS Modules used by a page unload when the page unmounts.

<!-- ********************* -->

# Practical patterns & tips

<!-- ********************* -->

* Keep base rules **low specificity** (use `:where`) and let components express intent.
* Avoid IDs inside `:is()` unless you want sticky rules.
* Prefer **variable overrides** (`--token`) over selector arm-wrestling.
* Use **layers** to avoid cross-file specificity wars, especially with vendor CSS.
* In Next.js, keep global CSS limited and lean; favor component-scoped CSS Modules.

<!-- ********************* -->

# References

<!-- ********************* -->

* **CSS variables & `:root`**

  * MDN — Using CSS custom properties: [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_cascading\_variables/Using\_CSS\_custom\_properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties) — Syntax, inheritance, fallbacks, and examples.
  * W3C — CSS Custom Properties for Cascading Variables Level 1: [https://www.w3.org/TR/css-variables-1/](https://www.w3.org/TR/css-variables-1/) — Formal behavior of custom properties and `var()`.
  * MDN — `:root` pseudo-class: [https://developer.mozilla.org/en-US/docs/Web/CSS/%3Aroot](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Aroot) — `:root` semantics and specificity note.
  * MDN — `var()` CSS function: [https://developer.mozilla.org/en-US/docs/Web/CSS/var](https://developer.mozilla.org/en-US/docs/Web/CSS/var) — Function syntax and fallback rules.

* **Specificity & selectors**

  * MDN — CSS Specificity: [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_cascade/Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity) — Specificity algorithm and examples.
  * web.dev — Specificity: [https://web.dev/learn/css/specificity](https://web.dev/learn/css/specificity) — Practical overview and worked examples.
  * MDN — `:is()`: [https://developer.mozilla.org/en-US/docs/Web/CSS/%3Ais](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Ais) — Matching semantics and “most specific argument.”
  * MDN — `:where()`: [https://developer.mozilla.org/en-US/docs/Web/CSS/%3Awhere](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Awhere) — Zero-specificity behavior and use cases.
  * MDN — `:not()`: [https://developer.mozilla.org/en-US/docs/Web/CSS/%3Anot](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Anot) — Argument-specific specificity.
  * MDN — `:has()`: [https://developer.mozilla.org/en-US/docs/Web/CSS/%3Ahas](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Ahas) — Parent/relative selection and specificity behavior.

* **Resets / Normalize / Preflight**

  * Meyer Reset: [https://meyerweb.com/eric/tools/css/reset/](https://meyerweb.com/eric/tools/css/reset/) — Classic aggressive reset.
  * Normalize.css: [https://github.com/necolas/normalize.css](https://github.com/necolas/normalize.css) — Lightweight baseline that preserves sensible defaults.
  * Tailwind CSS — Preflight: [https://tailwindcss.com/docs/preflight](https://tailwindcss.com/docs/preflight) — Opinionated normalize and base rules in Tailwind.
  * MDN — User Agent Style Sheets: [https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade#user-agent\_stylesheets](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade#user-agent_stylesheets) — Background on browser defaults.

* **Cascade layers**

  * MDN — `@layer`: [https://developer.mozilla.org/en-US/docs/Web/CSS/@layer](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer) — Syntax, ordering, and imports into layers.
  * MDN — The Cascade: [https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade) — How layers fit with importance/specificity/source order.
  * web.dev — Cascade Layers: [https://web.dev/learn/css/cascade-layers/](https://web.dev/learn/css/cascade-layers/) — Strategies, pitfalls, and examples.
  * W3C — CSS Cascading & Inheritance Level 5 (Layering): [https://www.w3.org/TR/css-cascade-5/#layering](https://www.w3.org/TR/css-cascade-5/#layering) — Formal definition of layering.

* **HTML semantics (context for elements like `<main>`, `<nav>`)**

  * MDN — HTML elements reference: [https://developer.mozilla.org/en-US/docs/Web/HTML/Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element) — Canonical element list and compatibility.
  * MDN — HTML semantics: [https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics\_in\_html](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html) — Why semantics matter.
  * WHATWG — HTML Living Standard: [https://html.spec.whatwg.org/multipage/](https://html.spec.whatwg.org/multipage/) — Up‑to‑date HTML spec.
  * MDN — `<dialog>`: [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog) — Methods, usage, and `::backdrop`.
  * MDN — `<details>` & `<summary>`: [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details) — Native disclosure element guidance.

* **Next.js (styling & routing context)**

  * Next.js — Styling (App Router): [https://nextjs.org/docs/app/building-your-application/styling](https://nextjs.org/docs/app/building-your-application/styling) — Global CSS, CSS Modules, and libraries.
  * Next.js — Routing, layouts, and templates: [https://nextjs.org/docs/app/building-your-application/routing](https://nextjs.org/docs/app/building-your-application/routing) — App Router concepts, nested layouts, templates, and pages.
