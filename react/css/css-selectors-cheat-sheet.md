<!-- ********************* -->

# CSS Selectors Cheat Sheet

<!-- ********************* -->

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This document provides a compact cheat sheet of CSS selectors, followed by short examples and explanations for each. It also highlights functional selectors like `:is()` and `:where()`, which are powerful but less obvious.

* Universal (`*`): Selects all elements.
* Descendant (`A B`): B inside A (any depth).
* Child (`A > B`): B is a direct child of A.
* Adjacent sibling (`A + B`): B immediately follows A.
* General sibling (`A ~ B`): B is any sibling after A.
* Selector list (`A, B`): A or B (applies to both).
* Functional pseudo-class `:is(A, B, C)`: Matches any of the selectors inside (shorter code).
* Functional pseudo-class `:where(A, B, C)`: Like `:is()`, but adds zero specificity.
* Attribute `[attr]`: Elements with the attribute.
* Attribute `[attr="value"]`: Attribute equals value.
* Attribute `[attr~="value"]`: Attribute contains value in a space-separated list.
* Attribute `[attr|="value"]`: Attribute starts with value (or value-… with hyphen).
* Attribute `[attr^="value"]`: Attribute starts with value (prefix match).
* Attribute `[attr$="value"]`: Attribute ends with value (suffix match).
* Attribute `[attr*="value"]`: Attribute contains value (substring match).

<!-- ********************* -->

# Examples and Explanations

<!-- ********************* -->

**Universal (`*`)**

```css
* { margin: 0; padding: 0; }
```

Resets margin and padding for all elements.

**Descendant (`A B`)**

```css
div p { color: blue; }
```

All `<p>` elements inside a `<div>`.

**Child (`A > B`)**

```css
ul > li { list-style: square; }
```

Styles only `<li>` that are direct children of `<ul>`.

**Adjacent sibling (`A + B`)**

```css
h1 + p { font-weight: bold; }
```

Styles a `<p>` immediately after an `<h1>`.

**General sibling (`A ~ B`)**

```css
h1 ~ p { color: gray; }
```

All `<p>` siblings after an `<h1>`.

**Selector list (`A, B`)**

```css
h1, h2 { color: darkred; }
```

Applies same style to both `<h1>` and `<h2>`.

**Functional pseudo-class `:is()`**

```css
:is(h1, h2, h3) { margin-bottom: 0.5rem; }
```

Applies the same style to `<h1>`, `<h2>`, and `<h3>`. This is similar to using `h1, h2, h3 { ... }`, but `:is()` can be more useful when combining with complex selectors because it keeps the selector shorter and manages specificity consistently. For example, instead of writing `section h1, section h2, section h3 { ... }`, you can write `section :is(h1, h2, h3) { ... }`. Specificity refers to how the browser decides which CSS rule takes precedence when multiple rules target the same element.

**Functional pseudo-class `:where()`**

```css
article :where(h1, h2, h3) { font-weight: bold; }
```

Works like `:is()`, but with **zero specificity** (it does not add weight to the selector). In practice this means styles applied with `:where()` are very easy to override, because the browser gives them the lowest priority when resolving conflicts. By contrast, `:is()` uses the specificity of the most specific selector inside it, so its rules can "lock in" more strongly and may require more specific selectors to override.
