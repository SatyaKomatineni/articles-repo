<!-- ********************* -->

# Next.js Basic App Setup

<!-- ********************* -->

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This document provides a distilled set of instructions for creating a new Next.js application in an empty repository, along with explanations of the options available when running the `create-next-app` command.

* Installation prerequisites
* Creating the app
* Running the development server
* Building for production
* Explanation of `create-next-app` options
* Caution about Tailwind for beginners
* Guidance for AI code generation
* References

<!-- ********************* -->

# Installation prerequisites

<!-- ********************* -->

Ensure you have Node.js version **18.18.0 or newer** installed:

```bash
node -v
```

<!-- ********************* -->

# Creating the app

<!-- ********************* -->

Use the official scaffolding tool:

```bash
npx create-next-app@latest
```

This command will interactively ask you about preferences such as TypeScript, Tailwind, ESLint, etc. You can also skip the prompts and provide options directly:

```bash
npx create-next-app@latest my-app --ts --eslint --app --src-dir --import-alias "@/*"
```

<!-- ********************* -->

# Running the development server

<!-- ********************* -->

Navigate into the project directory:

```bash
cd my-app
```

Start the development server:

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) to see your app.

<!-- ********************* -->

# Building for production

<!-- ********************* -->

When you are ready to build a production version:

```bash
npm run build
npm start
```

<!-- ********************* -->

# Explanation of `create-next-app` options

<!-- ********************* -->

The `create-next-app` tool supports several flags to customize your setup:

* **`--ts`**: Initialize with TypeScript instead of JavaScript.
* **`--eslint`**: Add ESLint configuration for linting.
* **`--tailwind`**: Install and configure Tailwind CSS.
* **`--app`**: Use the App Router (recommended for new projects) rather than the older Pages Router.
* **`--src-dir`**: Create a `src/` directory to hold application code.
* **`--import-alias "@/*"`**: Sets up path aliases so you can import modules using `@/` instead of relative paths like `../../`.
* **`--use-npm`**, **`--use-pnpm`**, **`--use-yarn`**, **`--use-bun`**: Choose which package manager to use.
* **`--empty`**: Create a minimal app without example code.
* **`--example <url or name>`**: Scaffold from a predefined example instead of the default starter.
* **`--no-git`**: Do not initialize a Git repository automatically.

<!-- ********************* -->

# Caution about Tailwind for beginners

<!-- ********************* -->

If you are just getting started with React and Next.js, enabling Tailwind will not harm your setup, but it can add extra complexity. Tailwind introduces a utility-first class syntax (e.g., `px-4 py-2 text-white bg-blue-600`), which may distract from learning React fundamentals like props, state, and component composition. Beginners are encouraged to focus first on React basics using CSS Modules or simple styles, and then gradually adopt Tailwind once they are more comfortable. Tailwind can always be added later without difficulty.

<!-- ********************* -->

# Guidance for AI code generation

<!-- ********************* -->

When using GitHub Copilot or ChatGPT, be explicit about styling requirements so generated components **do not** use Tailwind:

* Ask for **CSS Modules**: *"Generate React components using CSS Modules for styling. No Tailwind classes."*
* Or ask for **plain CSS**: *"Use semantic class names and a separate `.css` file. Avoid Tailwind utilities."*
* Or **inline styles for quick demos**: *"Use inline styles only; no external CSS or Tailwind."*
* Remind the tool to **avoid utility chains**: *"Do not include className strings like `px-4 py-2 ...`."*

**Prompt templates**

```
Create a React function component named Card. Styling requirements:
- No Tailwind or utility-first classes
- Use CSS Modules (Card.module.css)
- Keep class names semantic (card, header, body)
- Include hover and focus states in the module
```

```
I’m using Next.js (App Router). Generate a Button component with variants (primary, secondary):
- No Tailwind
- Use a .module.css file
- Export types for props
- Include example usage in app/page.tsx
```

**Repo/setup cues that help Copilot**

* Don’t install Tailwind in the project (no `tailwind.config.js`, no `@tailwind` directives). Copilot often mirrors the tech it sees.
* Add a short note in `README.md` (e.g., *Styling: CSS Modules only; avoid Tailwind*). Copilot reads visible context.
* Keep a small example module (e.g., `components/Button.tsx` + `Button.module.css`)—Copilot tends to imitate local patterns.

<!-- ********************* -->

# Guidance for AI code generation

<!-- ********************* -->

When using tools like GitHub Copilot or ChatGPT for code generation, you can explicitly instruct them to avoid Tailwind by saying things such as:

* *"Generate a React component without Tailwind classes"*
* *"Use plain CSS Modules for styling, not Tailwind"*
* *"Do not include utility class strings, just provide semantic classNames I can style separately"*

Being explicit in your prompts helps these tools adjust the generated code to match your preferences.

# References

<!-- ********************* -->

* [Next.js Installation Guide](https://nextjs.org/docs/getting-started/installation) – Official documentation on creating and installing a Next.js project.
* [create-next-app Options](https://nextjs.org/docs/app/api-reference/create-next-app) – List of supported flags and options with details.
* [App Router Introduction](https://nextjs.org/docs/app/building-your-application/routing) – Explanation of the modern App Router in Next.js.
