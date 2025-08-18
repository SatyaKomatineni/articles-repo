<!-- ********************* -->

# CSS Specificity — What it is, how it’s calculated, and its impacts

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

A concise guide to CSS specificity: what it is, how the browser calculates it, how it interacts with the cascade and source order, and practical strategies to avoid fights with your styles.

* Overview
* Specificity vs. cascade, source order, and `!important`
* How specificity is calculated (the score)
* What counts toward specificity (with examples)
* `:is()`, `:where()`, `:not()`, and `:has()`
* What does **not** affect specificity
* Worked examples (step‑by‑step calculations)
* Practical impacts and strategies
* References

<!-- ********************* -->

# Overview

<!-- ********************* -->

**Specificity** is the priority score CSS uses when multiple rules match the same element. Higher specificity wins; ties are resolved by **later source order**. Understanding the score lets you design selectors that are easy to override (or intentionally hard to override when needed).

<!-- ********************* -->

# Specificity vs. cascade, source order, and `!important`

<!-- ********************* -->

* **Cascade**: considers **origin** (user agent, user, author), **importance** (`!important`), **specificity**, and **source order**.
* **Specificity**: numeric weight based on the selector’s parts.
* **Source order**: when specificity ties, the later rule wins.
* **`!important`**: elevates a declaration within the same origin above others. Use sparingly—it can make code hard to maintain.

<!-- ********************* -->

# How specificity is calculated (the score)

<!-- ********************* -->

Most explanations use a 4‑part tuple: **(inline styles, IDs, classes/attributes/pseudo‑classes, elements/pseudo‑elements)**.

Typical values:

* Inline style → **(1,0,0,0)**
* `#id` → **(0,1,0,0)**
* `.class`, `[attr]`, `:hover` → **(0,0,1,0)**
* `h1`, `button`, `::before` → **(0,0,0,1)**

When combining selectors, **sum the columns**. Combinators (space, `>`, `+`, `~`) don’t add specificity.

<!-- ********************* -->

# What counts toward specificity (with examples)

<!-- ********************* -->

* **ID selectors**: `#site` → (0,1,0,0)
* **Class/attribute/pseudo‑class**: `.card`, `[type="button"]`, `:focus-visible` → (0,0,1,0)
* **Type/pseudo‑element**: `main`, `h2`, `::marker` → (0,0,0,1)
* **Inline styles**: `style="…"` on the element → (1,0,0,0)

Examples:

```css
/* ID beats class */
#title { color: red; }                /* 0,1,0,0 */
.title { color: blue; }               /* 0,0,1,0 */

/* Class beats element */
p { color: green; }                   /* 0,0,0,1 */
.note { color: purple; }              /* 0,0,1,0 */

/* Pseudo-class adds like a class */
button:hover { background: black; }   /* 0,0,1,1 */
button { background: gray; }          /* 0,0,0,1 */
```

<!-- ********************* -->

# `:is()`, `:where()`, `:not()`, and `:has()`

<!-- ********************* -->

* **`:is(A, B, C)`**: contributes the specificity of its **most specific argument**. It’s great for avoiding repetition, but be careful not to include IDs unless you intend an ID‑level rule.
* **`:where(A, B, C)`**: always contributes **zero specificity**, no matter what’s inside. Use it to keep base/structural rules easy to override.
* **`:not(X)`**: takes the specificity of its **argument `X`**; the `:not()` itself adds nothing extra.
* **`:has(Y)`**: like `:is()`/`:not()`, it takes the specificity of its **most specific argument**. Powerful, but avoid heavy, page‑wide patterns like `body:has(...)` for performance.

Examples:

```css
/* Group headings without repetition */
main :is(h1, h2, h3) { margin-block: 0; }  /* (0,0,0,2) */

/* Keep base rules cheap to override */
:where(article) :where(h1, h2, h3) { line-height: 1.2; }  /* adds 0 */

/* :not() uses its argument's specificity */
button:not(.primary) { color: gray; }     /* 0,0,1,1 */

/* :has() specificity follows its most specific argument */
section:has(> h2) { padding-top: 1rem; }  /* 0,0,1,1 */
```

<!-- ********************* -->

# What does **not** affect specificity

<!-- ********************* -->

* Combinators: descendant (space), child (`>`), adjacent sibling (`+`), general sibling (`~`).
* Universal selector `*`.
* Property values, order of declarations **inside** a rule.
* Media queries (they conditionally apply rules; they don’t change the rule’s specificity).

<!-- ********************* -->

# Worked examples (step‑by‑step calculations)

<!-- ********************* -->

**1) `main :is(h1, h2, h3)`**

* `main` → (0,0,0,1)
* `:is(h1, h2, h3)` → most specific inside is a type selector → (0,0,0,1)
* **Total** → **(0,0,0,2)**

**2) `main :is(.title, h1)`**

* `main` → (0,0,0,1)
* `:is(.title, h1)` → most specific inside is `.title` → (0,0,1,0)
* **Total** → **(0,0,1,1)**

**3) `:where(#app, h1) p`**

* `:where(#app, h1)` → always (0,0,0,0)
* `p` → (0,0,0,1)
* **Total** → **(0,0,0,1)**

**4) `#app .card:hover h2`**

* `#app` → (0,1,0,0)
* `.card` + `:hover` → (0,0,2,0)
* `h2` → (0,0,0,1)
* **Total** → **(0,1,2,1)**

**5) Tie broken by source order**

```css
.title { color: teal; }  /* earlier */
.title { color: navy; }  /* later → wins on tie */
```

<!-- ********************* -->

# Practical impacts and strategies

<!-- ********************* -->

* **Prefer low‑specificity base rules**. Use `:where()` for structural scoping so later component classes can override without `!important`.
* **Avoid IDs in selectors** for theme/component styles; they create hard‑to‑override rules.
* **Group repeated tails** with `:is()` but keep arguments modest (avoid IDs).
* **Use component classes** (BEM/utility approaches) to keep specificity predictable.
* **Know your tools**:

  * **CSS Modules**: class names are local, but specificity rules are unchanged (a local `.title` is still (0,0,1,0)).
  * **Tailwind/utility CSS**: many single‑class rules—specificity stays low and consistent.
  * **Cascade layers (`@layer`)**: control the order that groups of rules are considered before specificity. Put resets/base in a lower‑priority layer.
* **Debug with DevTools**: Inspect an element to see which rules matched and which were overridden, along with their specificity.

<!-- ********************* -->

# References

<!-- ********************* -->

* [MDN — CSS Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity): Authoritative explanation of the specificity algorithm with examples and edge cases.
* [MDN — `:is()`](https://developer.mozilla.org/en-US/docs/Web/CSS/:is): Defines matching semantics and the “most specific argument” specificity rule.
* [MDN — `:where()`](https://developer.mozilla.org/en-US/docs/Web/CSS/:where): Explains zero‑specificity behavior and when to use it for safe scoping.
* [MDN — `:not()`](https://developer.mozilla.org/en-US/docs/Web/CSS/:not): Shows that `:not()` takes the specificity of its argument.
* [MDN — `:has()`](https://developer.mozilla.org/en-US/docs/Web/CSS/:has): Specificity behavior and examples; includes notes on support and usage.
* [web.dev — Specificity](https://web.dev/learn/css/specificity): Practical overview with worked examples, including `:is()` vs. `:where()`.
