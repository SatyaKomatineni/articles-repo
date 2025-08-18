<!-- ********************* -->

# Next.js Global CSS and Component Styling (CSS Modules)

<!-- ********************* -->

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

A concise, practical `global.css` for a typical web app, plus a few example components (Link, Button, Form controls, and List) using **CSS Modules**, and a `page.tsx` that composes them.

* Project structure
* `app/globals.css` (global baseline)
* Components

  * `LinkText` (semantic link wrapper)
  * `Button` (primary/secondary variants)
  * `Field` (label + input/select with error state)
  * `SimpleList` (unstyled list with spacing)
* `app/page.tsx` bringing everything together
* References

<!-- ********************* -->

# Project structure

<!-- ********************* -->

```
my-app/
  app/
    globals.css
    layout.tsx
    page.tsx
  components/
    Button.tsx
    Button.module.css
    LinkText.tsx
    LinkText.module.css
    Field.tsx
    Field.module.css
    SimpleList.tsx
    SimpleList.module.css
```

<!-- ********************* -->

# app/globals.css

<!-- ********************* -->

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

# Components

<!-- ********************* -->

Below are minimal, composable components styled with **CSS Modules**.

<!-- ********************* -->

# LinkText

<!-- ********************* -->

**components/LinkText.module.css**

```css
.link {
  color: var(--color-link);
  text-decoration: none;
}
.link:hover { text-decoration: underline; }
```

**components/LinkText.tsx**

```tsx
import Link from "next/link";
import styles from "./LinkText.module.css";

export function LinkText({ href, children }: { href: string; children: React.ReactNode }) {
  return (
    <Link className={styles.link} href={href}>
      {children}
    </Link>
  );
}
```

<!-- ********************* -->

# Button

<!-- ********************* -->

**components/Button.module.css**

```css
.base {
  display: inline-flex; align-items: center; justify-content: center;
  gap: var(--space-2);
  padding: 0.5rem 0.75rem;
  border-radius: var(--radius-sm);
  border: 1px solid transparent;
  cursor: pointer;
}
.primary { background: var(--color-primary); color: white; }
.primary:hover { background: var(--color-primary-700); }
.secondary { background: transparent; color: var(--color-text); border-color: var(--color-border); }
.secondary:hover { background: rgba(255,255,255,0.04); }
.disabled { opacity: 0.6; cursor: not-allowed; }
```

**components/Button.tsx**

```tsx
import styles from "./Button.module.css";

type Variant = "primary" | "secondary";

type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> & {
  variant?: Variant;
};

export function Button({ variant = "primary", className, disabled, ...rest }: ButtonProps) {
  const cls = [styles.base, styles[variant], disabled ? styles.disabled : "", className]
    .filter(Boolean)
    .join(" ");
  return <button className={cls} disabled={disabled} {...rest} />;
}
```

<!-- ********************* -->

# Field (label + input/select)

<!-- ********************* -->

**components/Field.module.css**

```css
.field { display: grid; gap: var(--space-1); }
.label { font-weight: 600; }
.input, .select {
  padding: 0.5rem 0.6rem;
  background: var(--color-surface);
  color: var(--color-text);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
}
.help { font-size: 0.9rem; color: var(--color-muted); }
.error { color: var(--color-danger); font-size: 0.9rem; }
.inputError, .selectError { border-color: var(--color-danger); }
```

**components/Field.tsx**

```tsx
import styles from "./Field.module.css";

export function TextField({ id, label, help, error, ...rest }: {
  id: string;
  label: string;
  help?: string;
  error?: string;
} & React.InputHTMLAttributes<HTMLInputElement>) {
  return (
    <div className={styles.field}>
      <label className={styles.label} htmlFor={id}>{label}</label>
      <input id={id} className={`${styles.input} ${error ? styles.inputError : ""}`} {...rest} />
      {help && !error && <div className={styles.help}>{help}</div>}
      {error && <div className={styles.error}>{error}</div>}
    </div>
  );
}

export function SelectField({ id, label, children, help, error, ...rest }: {
  id: string;
  label: string;
  help?: string;
  error?: string;
  children: React.ReactNode;
} & React.SelectHTMLAttributes<HTMLSelectElement>) {
  return (
    <div className={styles.field}>
      <label className={styles.label} htmlFor={id}>{label}</label>
      <select id={id} className={`${styles.select} ${error ? styles.selectError : ""}`} {...rest}>
        {children}
      </select>
      {help && !error && <div className={styles.help}>{help}</div>}
      {error && <div className={styles.error}>{error}</div>}
    </div>
  );
}
```

<!-- ********************* -->

# SimpleList

<!-- ********************* -->

**components/SimpleList.module.css**

```css
.list { list-style: none; padding: 0; margin: 0; }
.list > li { padding: var(--space-2) 0; border-bottom: 1px solid var(--color-border); }
.list > li:last-child { border-bottom: none; }
```

**components/SimpleList.tsx**

```tsx
import styles from "./SimpleList.module.css";

export function SimpleList({ items }: { items: React.ReactNode[] }) {
  return (
    <ul className={styles.list}>
      {items.map((it, i) => <li key={i}>{it}</li>)}
    </ul>
  );
}
```

<!-- ********************* -->

# app/page.tsx

<!-- ********************* -->

```tsx
import { Button } from "@/components/Button";
import { LinkText } from "@/components/LinkText";
import { TextField, SelectField } from "@/components/Field";
import { SimpleList } from "@/components/SimpleList";

export default function Page() {
  return (
    <main>
      <section className="section stack">
        <h1>Demo: Global CSS + CSS Modules</h1>
        <p className="muted">This page shows components styled with CSS Modules, using global tokens and base styles.</p>

        <h2>Buttons</h2>
        <div className="row">
          <Button variant="primary">Primary</Button>
          <Button variant="secondary">Secondary</Button>
          <Button variant="primary" disabled>Disabled</Button>
        </div>

        <h2>Links</h2>
        <p>
          Visit <LinkText href="https://nextjs.org">Next.js</LinkText> for docs.
        </p>

        <h2>Form</h2>
        <div className="stack">
          <TextField id="email" type="email" label="Email" placeholder="you@example.com" help="We never share your email." />
          <TextField id="name" type="text" label="Name" placeholder="Ada Lovelace" error="Please enter your name." />
          <SelectField id="role" label="Role" defaultValue="dev">
            <option value="dev">Developer</option>
            <option value="pm">Product Manager</option>
            <option value="design">Designer</option>
          </SelectField>
        </div>

        <h2>List</h2>
        <SimpleList items={[
          <span>Item One</span>,
          <span>Item Two</span>,
          <span>Item Three</span>
        ]} />
      </section>
    </main>
  );
}
```

<!-- ********************* -->

# References

<!-- ********************* -->

* [Next.js: Styling with CSS Modules](https://nextjs.org/docs/app/building-your-application/styling/css-modules) – Official guide for CSS Modules in the App Router.
* [MDN: \:focus-visible](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible) – Why focus-visible improves keyboard accessibility.
* [Modern CSS Reset (inspiration)](https://piccalil.li/blog/a-modern-css-reset/) – Ideas for light-weight resets.
