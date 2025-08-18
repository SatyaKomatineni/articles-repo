<!-- ********************* -->

# Lesser‑Used HTML Elements — Quick Reference & When To Use Them

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

A compact, printable cheat sheet of less‑used but useful HTML elements. It includes a quick copy list and brief “when to use” notes for the top ten.

* Quick copy list
* Top 10 elements — when to use them
* Accessibility notes
* References

<!-- ********************* -->

# Quick copy list

<!-- ********************* -->

```html
<main>               <!-- primary page content -->
<nav>                <!-- site/section navigation -->
<article>            <!-- self-contained content unit -->
<section>            <!-- thematic grouping -->
<aside>              <!-- complementary/related content -->
<figure>             <!-- media + caption container -->
<figcaption>         <!-- caption for a <figure> -->
<header>             <!-- intro/header for page/section -->
<footer>             <!-- footer for page/section -->
<address>            <!-- contact info for nearest article/section -->

<time>               <!-- machine-readable dates/times -->
<data>               <!-- machine-readable value for content -->
<mark>               <!-- highlight text (relevance) -->
<dfn>                <!-- term being defined -->
<abbr>               <!-- abbreviation (use title attr) -->
<cite>               <!-- title of a work -->
<q>                  <!-- short inline quotation -->
<ruby>               <!-- ruby annotation container -->
<rt>                 <!-- ruby text (pronunciation) -->
<rp>                 <!-- ruby fallback parentheses -->
<bdi>                <!-- bidirectional isolation -->
<bdo>                <!-- bidirectional override -->
<wbr>                <!-- optional line break opportunity -->
<kbd>                <!-- user input (keyboard) -->
<samp>               <!-- sample output from a program -->
<var>                <!-- variable name -->
<del>                <!-- deletion (diff/edits) -->
<ins>                <!-- insertion (diff/edits) -->

<details>            <!-- disclosure widget -->
<summary>            <!-- summary/handle for <details> -->
<dialog>             <!-- modal/dialog element -->
<meter>              <!-- scalar measurement (e.g., score) -->
<progress>           <!-- task progress -->
<output>             <!-- calc/result output -->

<picture>            <!-- art-directed responsive images -->
<source>             <!-- media source for <picture>/<audio>/<video> -->
<track>              <!-- captions/subtitles for media -->
<map>                <!-- image map container -->
<area>               <!-- clickable region in an image map -->

<datalist>           <!-- autocomplete options for inputs -->
<fieldset>           <!-- group form controls -->
<legend>             <!-- caption for a fieldset -->
<optgroup>           <!-- group options in a select -->

<colgroup>           <!-- group table columns -->
<col>                <!-- column definition in tables -->

<template>           <!-- inert DOM fragment for cloning -->
<slot>               <!-- web component content slot -->
<noscript>           <!-- fallback content if scripting disabled -->
```

<!-- ********************* -->

# Top 10 elements — when to use them

<!-- ********************* -->

* **`<main>`** — Use once per page to wrap the *primary* content (skip headers/nav/footers). Helps screen readers jump to content.
* **`<nav>`** — Wrap major navigation blocks (site menu, in‑page TOC). Use `aria-label` when multiple navs exist.
* **`<article>`** — A portable, self‑contained unit: blog post, forum post, card that could stand alone or be syndicated.
* **`<section>`** — Thematic grouping within a page/article. Prefer it when you might add a heading; don’t use purely for styling.
* **`<aside>`** — Complementary content related to the nearest section/article (pull quotes, sidebars, related links).
* **`<figure>` + `<figcaption>`** — Pair media (image, code listing, chart) with a caption that travels with it.
* **`<time>`** — Mark up dates/times with `datetime` for machine readability and better SEO/feeds, e.g. `datetime="2025-08-18"`.
* **`<details>` + `<summary>`** — Native disclosure widget for FAQs or collapsible sections, keyboard‑ and a11y‑friendly by default.
* **`<dialog>`** — Standards‑based modal/dialog; call `dialog.showModal()`/`close()`; supports the `::backdrop` pseudo‑element.
* **`<template>`** — Inert HTML fragment not rendered until cloned; great for client‑side templating or repeating UI blocks.

<!-- ********************* -->

# Accessibility notes

<!-- ********************* -->

* Use semantic elements to expose **landmarks** and **structure** to assistive tech (e.g., `<main>`, `<nav>`, `<header>`, `<footer>`).
* For multiple navs, add an **accessible name**: `<nav aria-label="Primary">…</nav>`.
* `<details>/<summary>` work out‑of‑the‑box; don’t replace them with custom JS unless you keep keyboard and focus behavior.
* For `<dialog>`, ensure focus trapping and an obvious close action; style `::backdrop` for contrast.
* Prefer `<figure>/<figcaption>` instead of bare `<img>` + `<p>` when the caption is essential context.
* For `<time>`, always include a machine‑readable `datetime` attribute.

<!-- ********************* -->

# References

<!-- ********************* -->

* [MDN — HTML elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element): Canonical list of elements with examples and compatibility tables.
* [MDN — HTML semantics guide](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html): Why semantic elements matter and how to apply them.
* [WHATWG — HTML Living Standard](https://html.spec.whatwg.org/multipage/): Formal, up‑to‑date specification for HTML elements and behavior.
* [MDN — `<dialog>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog): Usage, methods (`showModal`/`close`), and `::backdrop` styling.
* [MDN — `<details>` & `<summary>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details): Native disclosure element patterns and accessibility notes.
