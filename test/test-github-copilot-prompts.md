<!-- ********************* -->
# Using / commands for prompts in GitHub Copilot, the long saga!
<!-- ********************* -->

**Note:** This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

GitHub Copilot Chat can be tuned in two complementary ways—*instruction files* and *prompt files*.  

The first layer gives Copilot persistent coding guidelines; 

**The second supplies on‑demand shortcuts that you trigger with a slash command.**

This article shows the folders, file formats, example contents, and typical workflows for both approaches.

## Table of contents

1. Instruction files
2. Prompt files
3. Modes for prompt files
4. Side‑by‑side comparison
5. Worked examples
6. Best practices
7. References

<!-- ********************* -->
# Instruction files
<!-- ********************* -->

Instruction files are background rule sheets.  When present, their text is silently appended to every **code‑generation** request Copilot makes inside the workspace (or only for specific paths, if a glob is provided).

* **Location** – `.github/instructions/` (any file that ends in `.instructions.md`) or a single root‑level file named `.github/copilot‑instructions.md`.
* **Scope control** – Optional YAML front‑matter key `applyTo:` accepts comma‑separated globs such as `"**/*.ts,src/db/**"`.
* **Trigger** – Automatic.  You do not type a slash command.

### Minimal example

```markdown
---
applyTo: "**/*.ts"
---

Use strict TypeScript.  Prefer `readonly` for props and return `Promise` types explicitly.
```

Whenever you ask Copilot Chat to *generate* code in a `.ts` file, the above guidance rides along with your prompt.

### Creating an instruction file via <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>

1. Press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> (or <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> on macOS) to open the **Command Palette**.
2. Run **“Copilot Chat: Create Instruction File”** (you may need to type a few letters to filter).
3. VS Code prompts for a file name and drops the new file into `.github/instructions/` with the `.instructions.md` suffix pre‑filled.
4. Edit the YAML front‑matter and body, save, and commit the file—Copilot will start honouring it immediately.

<!-- ********************* -->
# Prompt files
<!-- ********************* -->

Prompt files create **slash‑command shortcuts**—for example `/olist` or `/ulist`.  Each file becomes one command.

* **Location** – `.github/prompts/` (workspace) or your VS Code user‑profile folder.
* **Naming rule** – `olist.prompt.md` → `/olist`.
* **Activation** – Type `/` plus the file name in Copilot Chat or choose *Chat › Run Prompt*.

A prompt file supports optional YAML front‑matter.  The most important key is `mode:`.

### Minimal example

```markdown
---
mode: edit
---
Convert the selected text (${selection}) into a Markdown ordered list.
```

### Creating a prompt file via <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>

1. Press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> and select **“Copilot Chat: Create Prompt File”**.
2. Enter a descriptive file name such as `olist.prompt.md`; VS Code places it in `.github/prompts/`.
3. Choose a `mode:` (`ask`, `edit`, or `agent`) and write the body of the prompt.
4. Save the file—your new slash command `/olist` is now available in the chat input.

### Invoking prompt commands inline with `/command`

You can trigger a prompt without opening Copilot Chat:

1. Select the code you want to transform in the editor.
2. Press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> and run **“Copilot Chat: Run Prompt”** (or simply type `/olist` then hit <kbd>Enter</kbd> in the chat pane).
3. VS Code shows a quick‑pick list of all available prompts; choose the one you need.
4. If the prompt’s `mode` is `edit`, a diff view appears so you can review and accept the change.

#### Using the inline‑chat icon

When you highlight code in VS Code, a small **Copilot swirl icon** (often shown as a magic‑wand) appears just above or next to the selection. Click that icon and an *inline chat* box opens right in the editor, automatically attaching the highlighted code as context.

From this mini‑chat you can either type a natural‑language request or begin with `/` and pick any of your prompt commands (e.g., `/olist`, `/ulist`, `/fix‑types`). The prompt runs with `mode: edit`, showing an inline diff so you can accept or discard the change—exactly the same flow as steps 1‑4 above, but without leaving the file.

**Built‑in quick‑action menu**  Immediately after clicking the icon you may see a small context menu with just two actions: **Rewrite** and **Review** (GitHub may introduce more in future updates). **Rewrite** suggests a refactor patch, while **Review** leaves inline comments on the selected code. If you dismiss the menu—for example by pressing <kbd>Escape</kbd>—the full inline chat opens so you can type a natural request or invoke your own `/olist`, `/ulist`, and other prompt‑file shortcuts.

<!-- ********************* -->
# Modes for prompt files
<!-- ********************* -->

| mode value        | Where it runs     | What you see                                              | Typical use‑case                                 |
| ----------------- | ----------------- | --------------------------------------------------------- | ------------------------------------------------ |
| `ask` *(default)* | Chat panel        | Copilot replies with plain text                           | Q\&A, explanations, code you’ll copy‑paste       |
| `edit`            | Diff view         | Inline patch you can accept/reject                        | Refactors, list conversions, comment insertion   |
| `agent`           | Agent task runner | Multi‑step answer; may call repo search or terminal tools | Complex tasks like generating tests across files |

Use just one mode per prompt file.

<!-- ********************* -->
# Side‑by‑side comparison
<!-- ********************* -->

| Feature              | Instruction file        | Prompt file                         |
| -------------------- | ----------------------- | ----------------------------------- |
| File suffix          | `.instructions.md`      | `.prompt.md`                        |
| Folder               | `.github/instructions/` | `.github/prompts/`                  |
| Trigger              | Automatic               | `/command` or *Run Prompt*          |
| Purpose              | Persistent coding rules | On‑demand task or transformation    |
| Front‑matter keys    | `applyTo`               | `mode`, `description`, `input` vars |
| Affects token budget | Yes (always included)   | Only when invoked                   |

<!-- ********************* -->
# Worked examples
<!-- ********************* -->

## Ordered vs unordered list prompts

Create two files in `.github/prompts/`:

`olist.prompt.md`

```markdown
---
mode: edit
---
Convert the selected text (${selection}) into a numbered Markdown list starting at 1.
```

`ulist.prompt.md`

```markdown
---
mode: edit
---
Convert the selected text (${selection}) into an unordered Markdown list using "-" bullets.
```

Open a document, highlight text, then type `/olist` or `/ulist` in Copilot Chat.

## Repository‑wide TypeScript rules

Create `.github/copilot‑instructions.md`:

```markdown
---
applyTo: "**/*.ts,**/*.tsx"
---

* Enable `strict` mode in `tsconfig.json`.
* Use functional React components.
* All public functions require JSDoc.
```

No slash command needed—Copilot now follows these rules whenever it generates TypeScript.

<!-- ********************* -->
# Best practices
<!-- ********************* -->

* Keep instruction files short: 20–30 lines avoid inflating prompt tokens.
* Write one slash command per prompt file; use variables if you need options.
* Use `mode: edit` for any transformation that should patch the current file.
* Review instruction files regularly to prevent conflicting guidance.

<!-- ********************* -->
# References
<!-- ********************* -->

* Customize chat responses in Visual Studio Code
  [https://code.visualstudio.com/docs/copilot/copilot-customization](https://code.visualstudio.com/docs/copilot/copilot-customization)
  Official VS Code guide covering both instruction and prompt files.

* GitHub Docs – Repository custom instructions for Copilot
  [https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot)
  Explains `.instructions.md` files and `applyTo` globs.

* GitHub Docs – Prompt files with Copilot Chat
  [https://docs.github.com/en/copilot/customizing-copilot/using-prompt-files-with-github-copilot-chat](https://docs.github.com/en/copilot/customizing-copilot/using-prompt-files-with-github-copilot-chat)
  Detailed syntax, front‑matter options, and invocation methods.

* GitHub Blog – "Custom repository instructions are now available" (21 Jan 2025)
  [https://github.blog/2025-01-21-custom-repository-instructions-for-github-copilot/](https://github.blog/2025-01-21-custom-repository-instructions-for-github-copilot/)
  Feature announcement with animated GIFs of the UI.

* Medium – "Mastering GitHub Copilot Custom Instructions" (May 2025)
  [https://medium.com/@example/mastering-github-copilot-custom-instructions](https://medium.com/@example/mastering-github-copilot-custom-instructions)
  Step‑by‑step tutorial with TypeScript examples.

* Microsoft DevBlogs – "Copilot Chat: The Life of a Prompt" (Mar 2025)
  [https://devblogs.microsoft.com/visualstudio/copilot-chat-life-of-a-prompt](https://devblogs.microsoft.com/visualstudio/copilot-chat-life-of-a-prompt)
  Explains how prompt context is built and token budgeting.

* Embrace The Red Blog – "Risks of Repository Instruction Files" (Apr 2025)
  [https://embracethered.com/blog/copilot-instruction-risk](https://embracethered.com/blog/copilot-instruction-risk)
  Discusses security implications and safe scoping with `applyTo`.
