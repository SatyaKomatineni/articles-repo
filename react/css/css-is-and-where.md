<!-- ********************* -->

# CSS `:is()` and `:where()` — A Practical Introduction

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

This introduction focuses on what `:is()` and `:where()` do, how they simplify selectors, when to use each, and a few practical recipes. Specificity is mentioned briefly for context.

* Overview
* What these selectors do
* Matching semantics and forgiving selector lists
* When to use `:is()` vs. `:where()`
* Recipes (copy‑ready)
* Specificity (quick note)
* Browser support & pitfalls
* References

<!-- ********************* -->

# Overview

<!-- ********************* -->

Both `:is()` and `:where()` take a **selector list** and match if **any** selector in the list matches at that position. They let you avoid repeating long prefixes while keeping your CSS clear and maintainable.

<!-- ********************* -->

# What these selectors do

<!-- ********************* -->

* **`:is(A, B, C)`**: Matches elements that would match **any** of `A`, `B`, or `C`. Keeps **normal strength** (specificity equals the **most specific** argument).
* **`:where(A, B, C)`**: Matches the same set, but contributes **zero specificity**. Use it for **base/structural rules** you want to be easy to override.

Example (targets the same elements):

```css
/* Without is/where */
header h1, header h2, header h3 { margin: 0; }

/* With :is */
header :is(h1, h2, h3) { margin: 0; }

/* With :where (same matches, lower weight) */
header :where(h1, h2, h3) { margin: 0; }
```

<!-- ********************* -->

# Matching semantics and forgiving selector lists

<!-- ********************* -->

Selectors inside `:is(...)` and `:where(...)` are parsed as a **forgiving selector list**. If one item is invalid or unsupported, it’s ignored and the rest still apply. This enables progressive enhancement without throwing away the whole rule.

```css
/* Older browser doesn’t support :focus-visible? The anchor still matches. */
:is(a, :focus-visible) { outline: 2px solid; }
```

<!-- ********************* -->

# When to use `:is()` vs. `:where()`

<!-- ********************* -->

Use this simple mental model:

* **Use `:is()`** when you want **grouping without reducing strength**.

  * Group states without repetition.
  * Keep rules “normal” so they don’t get accidentally overridden.

* **Use `:where()`** when you want **grouping that stays easy to override**.

  * Base typography and structural scopes.
  * Accessibility baselines (focus rings) that components may adjust.

Examples:

```css
/* 1) Group interactive targets for a shared state */
nav :is(a, button):focus-visible { outline: 2px solid; }

/* 2) Base typography in regions, easy to override */
:where(article, aside) :where(h1, h2, h3) { margin-block: .5rem; }

/* 3) Scoped theme tokens via variables */
.card { --pad: 12px; }
.card :where(p, ul, ol) { padding-inline: var(--pad); }
```

<!-- ********************* -->

# Recipes (copy‑ready)

<!-- ********************* -->

**DRY headings under a long ancestor**

```css
.layout > .content :is(h1, h2, h3, h4) { line-height: 1.2; }
```

**Shared focus ring (low‑specificity base)**

```css
:where(a, button, input, select, textarea, summary):focus-visible {
  outline: 2px solid var(--ring, Highlight);
  outline-offset: 2px;
}
/* Component override without selector fights */
.button { --ring: #0ea5e9; }
```

**“Either element” with a shared child structure**

```css
:is(.card, .panel) > header > :is(h2, h3) { margin: 0; }
```

**Progressive enhancement (forgiving list)**

```css
/* If :has() isn’t supported, the class still matches */
:is(.has-alert, :has(.alert)) { border: 1px solid currentColor; }
```

<!-- ********************* -->

# Specificity (quick note)

<!-- ********************* -->

* `:is(...)` takes the specificity of its **most specific argument** (it does not add them up).
* `:where(...)` contributes **zero** specificity (only the rest of the selector counts).
* Keep **IDs out of `:is()`** unless you intend very strong rules; prefer classes/elements.

<!-- ********************* -->

# Browser support & pitfalls

<!-- ********************* -->

* Both selectors have **broad support** in modern browsers. Older IE is not supported.
* Mixing layered and unlayered CSS can affect which rules win; layers are evaluated **before** specificity.
* Don’t rely on `:where()` to force wins—it’s designed to be **easy to override**. Use it intentionally for base rules.

<!-- ********************* -->

# References

<!-- ********************* -->

* [MDN — `:is()` pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Ais): Definition, syntax, examples, and notes on matching/forgiving lists.
* [MDN — `:where()` pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/%3Awhere): Definition, syntax, zero‑specificity behavior, and examples.
* [MDN — Selector list & forgiving lists](https://developer.mozilla.org/en-US/docs/Web/CSS/Selector_list): How forgiving selector lists work with `:is()`/`:where()`.
* [W3C — Selectors Level 4](https://www.w3.org/TR/selectors-4/): Formal definitions for modern selector features, including `:is()`/`:where()`.
* [web.dev — New CSS functional pseudo-class selectors (`:is()` and `:where()`)](https://web.dev/articles/css-is-and-where): Practical introduction and patterns.
