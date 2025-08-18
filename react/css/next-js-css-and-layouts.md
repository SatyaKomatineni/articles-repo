<!-- ********************* -->

# Next.js Layouts + Templates + CSS — Mount Diagram (App Router)

<!-- ********************* -->

*Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.*

This guide explains how layouts and templates mount and re-render in the Next.js App Router, and how CSS (global styles, CSS Modules, and libraries like Tailwind) attaches to that render tree.

* Overview
* Layouts: definition and behavior
* Templates: definition and behavior
* Mount vs. re-render across navigation
* CSS in Next.js (App Router)
* CSS mounting & scoping rules
* Mount diagram and timelines
* Minimal working examples
* Common pitfalls and best practices
* FAQ
* References

<!-- ********************* -->

# Overview

<!-- ********************* -->

This section introduces the roles of **layouts** and **templates** in the App Router and how CSS binds to components. After reading this, you should be able to predict which parts of the UI persist, which re-render, and which CSS files load where.

<!-- ********************* -->

# Layouts: definition and behavior

<!-- ********************* -->

Layouts wrap a route segment and persist across navigation **within** that segment. They are ideal for shells like app chrome, navigation bars, persistent sidebars, and shared providers.

Key properties:

* File name: `layout.tsx` (or `.jsx`) placed in a route segment folder like `app/`, `app/dashboard/`, etc.
* Composition: Nested layouts stack—each segment can add a new layout around its children.
* Persistence: A layout **mounts once** and stays mounted as you navigate between sibling pages inside its segment subtree.
* Data fetching: Server Components by default; can fetch data and stream children.

<!-- ********************* -->

# Templates: definition and behavior

<!-- ********************* -->

Templates are like layouts but **re-render on navigation**. Use them when you need a wrapper that resets state between page transitions (e.g., open/close animation boundaries, probabilistic A/B wrappers, or to ensure fresh effects).

Key properties:

* File name: `template.tsx` (or `.jsx`) in the same places you would put `layout.tsx`.
* Re-render semantics: A template **remounts** on every navigation to a route within its segment.
* Use when: You want layout-like structure but not persistence (e.g., page transition animations that should restart).

<!-- ********************* -->

# Mount vs. re-render across navigation

<!-- ********************* -->

Before diving into CSS, understand how React nodes behave during navigation in the App Router:

* **Root layout** (`app/layout.tsx`): persists for the entire app. Imported global CSS should go here.
* **Nested layouts**: persist while navigating among pages under their segment.
* **Templates**: remount on segment navigation.
* **Pages** (`page.tsx`): replace when navigating to a different page within the same segment.
* **Client components inside a persistent layout**: maintain state unless the layout unmounts.

<!-- ********************* -->

# CSS in Next.js (App Router)

<!-- ********************* -->

This section introduces where CSS lives and how it is applied.

* **Global CSS**: `app/globals.css` (or any global import) must be imported **once** at the top of the tree—typically in the **root layout**. Affects the entire app.
* **CSS Modules**: `*.module.css` files imported by components. Styles are locally scoped to the importing component tree.
* **CSS-in-JS**: Libraries like styled-jsx, styled-components, emotion can be used; their scoping and mounting follow their own libraries’ rules but still attach to the React tree where used.
* **Tailwind**: Implemented via global `@tailwind` directives in a global stylesheet (e.g., `globals.css`) and configured with `tailwind.config.{js,ts}`. Classes applied directly in JSX.

<!-- ********************* -->

# CSS mounting & scoping rules

<!-- ********************* -->

This section explains how styles attach to the rendered tree and how they persist through navigation.

* **Where to import**:

  * Global CSS → import once in `app/layout.tsx`.
  * CSS Modules → import where needed (component or page). They load when that component renders.
* **Persistence on navigation**:

  * If a component remains mounted (e.g., in a persistent layout), its CSS Modules remain active without re-injection.
  * If a component unmounts (e.g., a page change), associated CSS Modules can be removed from the active bundle.
* **Precedence (simplified)**:

  * Inline styles > authored CSS specificity order.
  * Between global and module styles, standard CSS specificity applies. CSS Modules don’t magically outrank global CSS; they simply generate unique class names that reduce accidental collisions.
* **Order**:

  * Global CSS from the root is present first; component/module CSS is included as those components render. Build-time splitting ensures only what’s needed ships per route.

<!-- ********************* -->

# Mount diagram and timelines

<!-- ********************* -->

This section visualizes how layouts, templates, pages, and styles mount over time during navigation.

**Tree at initial load**

```
<App Router>
  app/layout.tsx  (RootLayout)  ← mounts once
    └─ app/(marketing)/layout.tsx (MarketingLayout)  ← persists within / (marketing)
        └─ app/(marketing)/page.tsx (HomePage)
```

**Navigation from `/` to `/pricing` under the same segment**

```
RootLayout   : stays mounted
MarketingLayout: stays mounted
HomePage     : unmounts
PricingPage  : mounts
Templates    : if present at the segment, they remount on every navigation
```

**ASCII timeline (mount vs. remount)**

```
Time →  t0            t1 navigation to /pricing       t2 navigation to /contact
        ───────────────────────────────────────────────────────────────────────
RootLayout       [mounted──────────────────────────────────────────────────]
MarketingLayout  [mounted──────────────────────────────────────────────────]
Template?        [mounted]→(unmount)→[mounted]→(unmount)→[mounted] ...
Page             [HomePage]→(unmount)→[PricingPage]→(unmount)→[ContactPage]
CSS (global)     [present──────────────────────────────────────────────────]
CSS (modules)    [Home modules]→(drop)→[Pricing modules]→(drop)→[Contact]
```

<!-- ********************* -->

# Minimal working examples

<!-- ********************* -->

This section provides short, copy-ready snippets.

**Root layout: `app/layout.tsx`**

```tsx
// app/layout.tsx
import "./globals.css"; // global CSS imported once here
import type { ReactNode } from "react";

export default function RootLayout({ children }: { children: ReactNode }) {
  return (
    <html lang="en">
      <body>
        <header>App Shell</header>
        {children}
      </body>
    </html>
  );
}
```

**Nested layout (persistent shell): `app/dashboard/layout.tsx`**

```tsx
// app/dashboard/layout.tsx
import type { ReactNode } from "react";

export default function DashboardLayout({ children }: { children: ReactNode }) {
  return (
    <section>
      <aside>Sidebar</aside>
      <main>{children}</main>
    </section>
  );
}
```

**Template (remount on navigation): `app/dashboard/template.tsx`**

```tsx
// app/dashboard/template.tsx
import type { ReactNode } from "react";

export default function DashboardTemplate({ children }: { children: ReactNode }) {
  // This wrapper remounts on every navigation under /dashboard
  return <>{children}</>;
}
```

**Page using a CSS Module: `app/dashboard/page.tsx`**

```tsx
// app/dashboard/page.tsx
import styles from "./page.module.css";

export default function DashboardPage() {
  return <h1 className={styles.title}>Dashboard</h1>;
}
```

**CSS Module file: `app/dashboard/page.module.css`**

```css
.title {
  font-size: 1.5rem;
  font-weight: 600;
}
```

**Global stylesheet: `app/globals.css`**

```css
:root { --brand: #0ea5e9; }
html, body { margin: 0; font-family: system-ui, sans-serif; }
a { color: var(--brand); }
```

<!-- ********************* -->

# Common pitfalls and best practices

<!-- ********************* -->

This section highlights mistakes to avoid and practices to adopt.

* **Import global CSS only once** (root layout). Importing global CSS elsewhere causes build errors.
* **Pick layout vs. template intentionally**: use layouts for persistence (stateful shells), templates for remount boundaries (reset animations/effects).
* **Avoid unintended state resets**: Putting stateful client components in a template will reset them on each navigation.
* **Use CSS Modules for component-level styles** to prevent collisions and ship only what’s needed per route.
* **Keep providers high** (e.g., theme, auth) so they persist across navigation; otherwise they’ll remount.

<!-- ********************* -->

# FAQ

<!-- ********************* -->

This section answers quick questions you might have while wiring a project.

* **Where should Tailwind be configured?** In `tailwind.config.{js,ts}`; add `@tailwind base; @tailwind components; @tailwind utilities;` to your global CSS (imported by the root layout).
* **Do CSS Modules leak globally?** No. Class names are hashed and scoped to the importing component.
* **Do templates affect performance?** They can, since they remount; use them only where you need reset boundaries or fresh wrappers.
* **Can I have both a layout and a template in the same segment?** Yes. The layout persists; the template remounts.
* **Why did my sidebar state reset?** Likely because it lives under a template or a layout that unmounted due to leaving its segment.

<!-- ********************* -->

# References

<!-- ********************* -->

* [Next.js — Layouts and Pages (App Router)](https://nextjs.org/docs/app/getting-started/layouts-and-pages): How to create layouts and pages in the App Router and how layouts persist across navigation.
* [Next.js — `template.js` file convention](https://nextjs.org/docs/app/api-reference/file-conventions/template): What templates are, how they remount on navigation, and when to use them.
* [Next.js — Styling (App Router)](https://nextjs.org/docs/14/app/building-your-application/styling): Overview of styling approaches (Global CSS, CSS Modules, Tailwind, CSS‑in‑JS, Sass) and where to configure them.
* [Next.js — Loading UI & Streaming (App Router)](https://nextjs.org/docs/14/app/building-your-application/routing/loading-ui-and-streaming): How `loading.js` works, Suspense boundaries, and progressive streaming across route segments.
* [Next.js — Project Structure](https://nextjs.org/docs/app/getting-started/project-structure): Folder and file conventions for organizing an App Router project.
