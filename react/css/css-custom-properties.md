<!-- ********************* -->

# CSS Custom Properties: Scope, Lookup, and Common Usage

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

This article provides a concise overview of CSS custom properties (often called CSS variables), how the `var()` function looks them up in the DOM, and why `:root` is commonly used to define them.

* Introduction
* How custom properties are defined
* Lookup rules for `var()`
* Pseudo-classes: what they are
* **Specificity examples**
* Role of `:root` in global scope
* Defining variables outside `:root`
* Common usage patterns
* References

<!-- ********************* -->

# Introduction

<!-- ********************* -->

CSS custom properties allow authors to define reusable values that can be referenced throughout stylesheets. They follow normal CSS cascade rules, meaning their scope and availability depend on where they are defined in the DOM.

<!-- ********************* -->

# How custom properties are defined

<!-- ********************* -->

A custom property:

* Starts with `--`.
* Can hold any valid CSS value.
* Is defined in a rule set just like any other CSS property.

Example:

```css
:root {
  --brand-color: #0ea5e9;
}
```

<!-- ********************* -->

# Lookup rules for `var()`

<!-- ********************* -->

The `var()` function retrieves a custom property’s value based on the cascade:

1. Start with the element being styled.
2. Look for the custom property defined directly on that element.
3. If not found, check its parent, then each ancestor up the DOM tree.
4. The `<html>` element (or `:root`) is the highest ancestor in HTML documents.
5. If no value is found and no fallback is provided, the property becomes invalid.

Example with fallback:

```css
background: var(--brand-color, blue);
```

If `--brand-color` is not found, `blue` is used.

<!-- ********************* -->

# Pseudo-classes: what they are

<!-- ********************* -->

A **pseudo-class** is a selector that matches an element in a **particular state** (e.g., hovered, focused, checked). It begins with a single colon `:` and does not create new elements— it only changes which existing elements are selected based on conditions.

**Examples**

* Interaction: `:hover`, `:active`, `:focus`, `:focus-visible`
* Forms: `:disabled`, `:enabled`, `:checked`, `:indeterminate`, `:placeholder-shown`
* Document/links: `:visited`, `:link`, `:target`
* Tree-structural: `:nth-child(n)`, `:first-child`, `:last-child`, `:only-child`
* Functional: `:not()`, `:is()`, `:where()`, `:has()`

**Specificity quick rules**

* `:not(X)` has the specificity of `X`.
* `:is(A, B, C)` takes the **most specific** argument.
* `:where(...)` contributes **zero** specificity.
* `:has(...)` enables parent/relative selection and is widely supported.

**Pseudo-classes vs. pseudo-elements**

* Pseudo-classes (one colon): state-based selection, e.g., `:hover`, `:root`.
* Pseudo-elements (two colons): select a **part** of an element, e.g., `::before`, `::after`, `::marker`.

<!-- ********************* -->

# Specificity examples

<!-- ********************* -->

This section provides practical specificity comparisons using the tuple format *(inline, IDs, classes/attributes/pseudo-classes, elements/pseudo-elements).*

**Baseline hierarchy**

* Inline style: **(1,0,0,0)**
* ID selector `#id`: **(0,1,0,0)**
* Class/attribute/pseudo-class `.class`, `[type]`, `:hover`: **(0,0,1,0)**
* Element/pseudo-element `button`, `::before`: **(0,0,0,1)**

**Element vs. class**

```css
p { color: green; }          /* 0,0,0,1 */
.note { color: purple; }     /* 0,0,1,0 → wins */
```

**ID beats class**

```css
#title { color: red; }       /* 0,1,0,0 → wins */
.title { color: blue; }      /* 0,0,1,0 */
```

**Pseudo-class with element**

```css
button { color: black; }     /* 0,0,0,1 */
button:hover { color: teal; }/* 0,0,1,1 → wins while hovered */
```

`button:hover` means: select any `<button>` element **that is currently hovered** (pointer is over it).

**`:root` vs `html`**

```css
html  { --brand: #777; }     /* 0,0,0,1 */
:root { --brand: #000; }     /* 0,0,1,0 → higher specificity than `html` */
```

**`:is()` takes the most specific argument**

```css
nav :is(a:hover, button:focus) { outline: 2px solid; }
/* Specificity = nav (0,0,0,1) + most specific inside (0,0,1,1) = (0,0,1,2) */
```

**`:where()` adds zero specificity**

```css
nav :where(a) { color: inherit; } /* Specificity is just nav → (0,0,0,1) */
```

**`:not(X)` takes specificity of X**

```css
button:not(.primary) { color: gray; } /* 0,0,1,1 */
```

**Tie-breaker is source order**

```css
.card { color: black; }
.card { color: brown; } /* later rule with same specificity wins */
```

**Inline style beats author CSS**

```html
<button style="color: orange">…</button> <!-- (1,0,0,0) -->
```

Use `!important` sparingly; it overrides normal specificity within the same origin and can make maintenance difficult.

<!-- ********************* -->

# Role of `:root` in global scope

<!-- ********************* -->

* `:root` is a **pseudo-class** that matches the top-level element of the document (the `<html>` element in HTML documents).
* Defining variables on `:root` gives them **global scope** via inheritance; any descendant can read them with `var()` unless a nearer definition overrides them.
* `:root` has slightly **higher specificity** than `html`, which can help with predictable overrides.

```css
:root { --brand: #0ea5e9; }
html  { --brand: #0ea5e9; }
```

If both exist, standard CSS rules apply: later source order or higher specificity wins.

<!-- ********************* -->

# Defining variables outside `:root`

<!-- ********************* -->

This section shows how variables can be defined on **any selector**, not just `:root`. Variables defined on a class, ID, or element apply to that element and **cascade to its descendants**.

**Scoped to a class**

```css
.panel {
  --panel-bg: #f6f9fc;
}
.panel .button {
  background: var(--panel-bg);
}
```

Only buttons **inside** `.panel` inherit `--panel-bg`.

**Theme toggles with classes or attributes**

```css
:root { --text: #111; --bg: #fff; }
[data-theme="dark"] { --text: #eee; --bg: #111; }

body { color: var(--text); background: var(--bg); }
```

Toggling `data-theme="dark"` on `<html>` or `<body>` swaps the variables for the whole subtree.

**Component-local variables (CSS Modules idea)**

```css
/* Card.module.css */
.root { --card-pad: 12px; }
.content { padding: var(--card-pad); }
```

```tsx
// Card.tsx
import styles from "./Card.module.css";

export default function Card({ children }) {
  return (
    <section className={styles.root}>
      <div className={styles.content}>{children}</div>
    </section>
  );
}
```

Here `--card-pad` lives on the `.root` element and is available to its descendants like `.content`.

**Overriding closer to the element**

```css
:root { --accent: #0ea5e9; }
.section { --accent: #1e40af; }
.section .cta { background: var(--accent); } /* uses #1e40af */
```

The class-level value overrides the root value **within that subtree**.

<!-- ********************* -->

# Common usage patterns

<!-- ********************* -->

* **Theme colors and typography**:

```css
:root {
  --color-primary: #0ea5e9;
  --font-family-sans: system-ui, sans-serif;
}
```

* **Dark mode overrides**:

```css
:root {
  --bg-color: white;
}
.dark-mode {
  --bg-color: black;
}
```

* **Responsive scaling**:

```css
:root {
  --spacing-unit: 8px;
}
.card {
  padding: calc(var(--spacing-unit) * 2);
}
```

<!-- ********************* -->

# References

<!-- ********************* -->

* [MDN — Using CSS custom properties (variables)](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascading_variables/Using_CSS_custom_properties): Guide to declaring variables, using `var()`, `:root` scoping, inheritance, and fallbacks.
* [W3C — CSS Custom Properties for Cascading Variables Level 1](https://www.w3.org/TR/css-variables-1/): Formal spec for custom properties and the `var()` function, including cascade/lookup rules.
* [MDN — `:root` pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Aroot): Explains that `:root` matches `<html>` in HTML, with higher specificity than `html`.
* [MDN — `var()` CSS function](https://developer.mozilla.org/en-US/docs/Web/CSS/var): Reference for the `var()` function syntax and fallback behavior.
