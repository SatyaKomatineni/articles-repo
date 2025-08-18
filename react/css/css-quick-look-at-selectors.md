<!-- ********************* -->

# CSS Selectors — Quick Copy Sheet

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

A compact list of commonly used CSS selectors. One selector per line with a short comment so you can copy fast.

* Elements
* Classes & IDs
* Attribute selectors
* Pseudo-classes (state & UI)
* Structural pseudo-classes
* Pseudo-elements
* Document/root
* Combinators
* Grouping
* Common patterns

<!-- ********************* -->

# Elements

<!-- ********************* -->

```css
html                 /* root element */
body                 /* document body */
main                 /* main content */
header               /* page/section header */
footer               /* page/section footer */
h1                   /* heading level 1 */
h2                   /* heading level 2 */
h3                   /* heading level 3 */
p                    /* paragraph */
a                    /* link */
img                  /* image */
button               /* button */
input                /* form input */
select               /* select box */
textarea             /* multi-line input */
ul                   /* unordered list */
li                   /* list item */
```

<!-- ********************* -->

# Classes & IDs

<!-- ********************* -->

```css
.card                /* class */
.card.primary        /* class with modifier */
#app                 /* id */
```

<!-- ********************* -->

# Attribute selectors

<!-- ********************* -->

```css
input[type="text"]           /* exact match */
a[target="_blank"]           /* opens new tab */
a[href^="https://"]          /* starts with */
a[href$=".pdf"]              /* ends with */
a[href*="docs"]              /* contains */
input[aria-invalid="true" i] /* case-insensitive match */
```

<!-- ********************* -->

# Pseudo-classes (state & UI)

<!-- ********************* -->

```css
a:hover              /* mouse over */
button:active        /* active press */
input:focus          /* focused */
:focus-visible       /* keyboard-visible focus */
:focus-within        /* element containing a focused descendant */
:disabled            /* disabled controls */
:enabled             /* enabled controls */
:checked             /* checked checkbox/radio */
:indeterminate       /* tri-state checkbox */
:required            /* required input */
:optional            /* optional input */
:read-only           /* readonly input */
:placeholder-shown   /* input shows placeholder */
:valid               /* passes validation */
:invalid             /* fails validation */
:target              /* URL fragment target */
```

<!-- ********************* -->

# Structural pseudo-classes

<!-- ********************* -->

```css
:first-child         /* first child of parent */
:last-child          /* last child of parent */
:only-child          /* only child of parent */
:nth-child(2)        /* 2nd child */
:nth-child(odd)      /* odd children */
:nth-child(3n+1)     /* every 3rd, starting at 1 */
:nth-of-type(2)      /* 2nd element of same type */
:is(h1, h2, h3)      /* any of listed selectors (normal specificity) */
:where(h1, h2, h3)   /* any of listed selectors (zero specificity) */
:not(.active)        /* not matching .active */
:has(> img)          /* element that has a direct child img */
```

<!-- ********************* -->

# Pseudo-elements

<!-- ********************* -->

```css
::before             /* generated content before */
::after              /* generated content after */
::placeholder        /* placeholder text styling */
::selection          /* selected text */
::marker             /* list item marker/bullet */
::file-selector-button /* file input button */
::backdrop           /* backdrop of a modal dialog */
```

<!-- ********************* -->

# Document/root

<!-- ********************* -->

```css
:root                /* top-level element (html) */
:scope               /* current query scope */
```

<!-- ********************* -->

# Combinators

<!-- ********************* -->

```css
article p            /* descendant (p anywhere inside article) */
article > p          /* direct child */
h2 + p               /* adjacent sibling (p immediately after h2) */
h2 ~ p               /* general siblings (any p after h2) */
```

<!-- ********************* -->

# Grouping

<!-- ********************* -->

```css
h1, h2, h3           /* group multiple selectors */
```

<!-- ********************* -->

# Common patterns

<!-- ********************* -->

```css
nav :is(a, button):focus-visible   /* shared focus ring targets */
:where(article, aside) :where(h1, h2, h3)  /* low-specificity typography base */
.card :not(:last-child)            /* all children except last */
main :has(> .error)                /* main that contains a direct .error child */
```
