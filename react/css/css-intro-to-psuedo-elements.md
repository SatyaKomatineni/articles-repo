<!-- ********************* -->

# Understanding and Using Pseudo-elements in CSS

<!-- ********************* -->

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This article introduces **pseudo-elements** in CSS, what they are, and how to use them to style parts of elements or insert decorative content.

* What are pseudo-elements?
* Common pseudo-elements
* Pseudo-elements vs. Pseudo-classes
* Practical examples
* The `content` property in pseudo-elements
* Why pseudo-elements are called "boxes"
* Controlling box sizes
* Best practices
* References

<!-- ********************* -->

# What are pseudo-elements?

<!-- ********************* -->

Pseudo-elements are keywords added to selectors that let you style specific parts of an element’s content or insert virtual elements. They are not real DOM elements but behave like additional boxes you can style with CSS.

Syntax: `selector::pseudo-element`

Examples:

```css
p::first-line { font-weight: bold; }
button::before { content: "★ "; color: gold; }
```

<!-- ********************* -->

# Common pseudo-elements

<!-- ********************* -->

* **`::before`**: Inserts generated content before the content of an element.
* **`::after`**: Inserts generated content after the content of an element.
* **`::first-line`**: Styles only the first line of a block of text.
* **`::first-letter`**: Styles the first letter of the element.
* **`::marker`**: Styles the bullet or number of list items.
* **`::selection`**: Styles the portion of text the user has highlighted.

<!-- ********************* -->

# Pseudo-elements vs. Pseudo-classes

<!-- ********************* -->

* **Pseudo-classes** (single `:`) target an element in a special state, e.g. `:hover`, `:focus`, `:first-child`.
* **Pseudo-elements** (double `::`) let you style or generate a part of an element, e.g. `::before`, `::after`, `::first-line`.

Example:

```css
/* Pseudo-class: applies when hovered */
a:hover { color: red; }

/* Pseudo-element: generates content */
a::after { content: " ↗"; }
```

<!-- ********************* -->

# Practical examples

<!-- ********************* -->

**Decorative icons:**

```css
button::before {
  content: "✔ ";
  color: green;
}
```

**Badges or labels:**

```css
.notification::after {
  content: attr(data-count);
  background: red;
  color: white;
  font-size: 0.8rem;
  padding: 0.1rem 0.3rem;
  border-radius: 50%;
  margin-left: 0.3rem;
}
```

**Typography effects:**

```css
p::first-line { text-transform: uppercase; }
p::first-letter { font-size: 200%; font-weight: bold; }
```

**Custom list markers:**

```css
li::marker { color: blue; font-weight: bold; }
```

**Text selection:**

```css
::selection { background: yellow; color: black; }
```

<!-- ********************* -->

# The `content` property in pseudo-elements

<!-- ********************* -->

The `content` property is what actually makes `::before` and `::after` render. It can take different kinds of values:

* **Text strings**: e.g. `content: "★";` to insert a star.
* **Quoted empty string**: e.g. `content: "";` to make the pseudo-element exist without visible text (useful for decoration via `background` or `border`).
* **Attribute values**: e.g. `content: attr(data-count);` to pull from an element’s attribute.
* **Images**: e.g. `content: url(icon.png);` to insert an image.
* **Counters**: e.g. `content: counter(item);` in combination with CSS counters.
* **Special keywords**: `none` (no content), `open-quote` / `close-quote` (quotation marks defined in CSS `quotes` property).

Examples:

```css
button::before { content: "✔ "; }
.notification::after { content: attr(data-count); }
li::before { content: counter(mylist, upper-roman) ". "; }
```

<!-- ********************* -->

# About the `attr()` function

<!-- ********************* -->

The `attr()` function in CSS lets you use the value of an element’s attribute inside a pseudo-element’s `content`. For example:

```css
.notification::after {
  content: attr(data-count);
}
```

If the element is `<div class="notification" data-count="5"></div>`, then the pseudo-element will render `5` after it. This is commonly used for badges, tooltips, or displaying attribute values.

Note: As of now, `attr()` is only widely supported inside `content`. Using it in other properties (like `color` or `width`) is still experimental.

# CSS functions overview

<!-- ********************* -->

CSS has many built-in functions beyond `attr()`. They provide dynamic values for styles. Examples:

* **`url()`** – Reference an image or font file, e.g. `background-image: url('bg.png');`
* **`calc()`** – Perform calculations, e.g. `width: calc(100% - 50px);`
* **`var()`** – Use custom properties (CSS variables), e.g. `color: var(--main-color);`
* **`rgb()` / `hsl()`** – Define colors in numeric formats, e.g. `color: rgb(255, 0, 0);`
* **`min()` / `max()` / `clamp()`** – Responsive sizing, e.g. `font-size: clamp(1rem, 2vw, 2rem);`
* **`counter()`** – Insert counter values, e.g. `content: counter(item);`

These functions make CSS more powerful and expressive.

# Why pseudo-elements are called "boxes"

<!-- ********************* -->

In CSS, every element is modeled as a **box** (the box model: content, padding, border, margin). Pseudo-elements like `::before` and `::after` are also treated as boxes by the rendering engine. They participate in layout just like real elements, even though they are not present in the HTML.

For example:

* A `button::before` with `content: "★";` creates a small inline box before the button’s text.
* You can style that box with background, padding, borders, dimensions, positioning.
* Because it’s a box, it follows normal CSS flow, can be absolutely positioned, or styled like any other element.

So the term “box” reflects that pseudo-elements generate rectangular regions in the layout tree, subject to the box model.

<!-- ********************* -->

# Controlling box sizes

<!-- ********************* -->

```css
*, *::before, *::after { box-sizing: border-box; }
```

**About the commas:** The commas separate selectors. This means the rule applies to each selector independently (all elements, all `::before`, all `::after`). It is equivalent to writing three separate rules. In other words, the comma functions like an OR operator in CSS selector lists—so the same declaration block applies to each selector individually.

This common global rule ensures consistent sizing across elements and pseudo-elements.

**What `box-sizing` does**

In CSS, every element is considered a box according to the **box model**, which has four parts:

* **Content box**: the actual text, image, or content.

* **Padding box**: space between content and border.

* **Border box**: the border wrapping around the padding and content.

* **Margin box**: space outside the border, separating elements.

* `content-box` (default): width/height apply only to content. Padding and borders add extra size.

* `border-box`: width/height include content + padding + border. Easier to predict total size.

**Why apply to `*`, `*::before`, `*::after`**

* `*` selects all elements.
* `*::before` and `*::after` ensure pseudo-elements also follow the same sizing.
* The commas mean “OR,” so the rule applies to all three selectors.

Example:

```css
/* With content-box (default) */
.box1 {
  width: 200px;
  padding: 10px;
  border: 5px solid red;
  /* total width = 230px */
}

/* With border-box */
.box2 {
  box-sizing: border-box;
  width: 200px;
  padding: 10px;
  border: 5px solid red;
  /* total width = 200px */
}
```

This reset is widely recommended to avoid layout surprises when adding padding or borders.

<!-- ********************* -->

# Best practices

<!-- ********************* -->

* Always use `::before` and `::after` with a `content` property (even if empty) to make them render.
* Don’t overuse pseudo-elements for content critical to the document’s meaning (screen readers may not interpret them as actual content).
* Use them for decoration, icons, minor labels, or typographic effects.
* Keep accessibility in mind—important information should exist in the DOM, not just in pseudo-elements.

<!-- ********************* -->

# References

<!-- ********************* -->

* [MDN: Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements) – Detailed reference of all pseudo-elements.
* [MDN: Content property](https://developer.mozilla.org/en-US/docs/Web/CSS/content) – How to insert generated content with `content`.
* [CSS Tricks: Pseudo-elements](https://css-tricks.com/almanac/selectors/a/after-and-before/) – Practical guide and examples for `::before` and `::after`.
* [MDN: CSS attr() function](https://developer.mozilla.org/en-US/docs/Web/CSS/attr) – Explains the `attr()` function and its limitations.
* [MDN: CSS functions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions) – Comprehensive list of CSS functions like `calc()`, `var()`, `url()`, `clamp()`, etc.

<!-- ********************* -->

* [MDN: Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements) – Detailed reference of all pseudo-elements.
* [MDN: Content property](https://developer.mozilla.org/en-US/docs/Web/CSS/content) – How to insert generated content with `content`.
* [CSS Tricks: Pseudo-elements](https://css-tricks.com/almanac/selectors/a/after-and-before/) – Practical guide and examples for `::before` and `::after`.
