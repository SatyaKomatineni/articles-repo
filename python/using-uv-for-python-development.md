<!-- ********************* -->
# How to use UV to manage Python environments
<!-- ********************* -->
Metadata: 4/10/2025 (Very Prliminary - A get started document)

Many new python tools and tooling are using a new rust based package manager called "uv". This is an introductory article for myself, to undersand it, how to use it, and how do I understand it conceptually, and get started using it. A set of references are at the end to dig deeper.

**Basics**

1. `uv` is a high-performance, Rust-based package manager for Python 
2. Simplifies dependency management without the need for virtual environments (`venv`). 
3. Fast installs, native lockfile support
4. Modern isolation similar to `npm` or `pnpm` in the JavaScript world. 

**This guide explains how to use `uv` as the primary tool for managing Python environments and integrating it with VS Code.**

Note: Much of this is gathered via ChatGPT. Some I verified and some I haven't . Proceed with caution.

Note: "uv", from what I read, is expected to be used without the usual project based virtual environments. However it can be used in along with .venv, apparently for backward compatibility. ** This article, intentionally, does not cover that scenario **.

- [How to use UV to manage Python environments](#how-to-use-uv-to-manage-python-environments)
- [Installing `uv`](#installing-uv)
  - [Windows](#windows)
  - [macOS/Linux](#macoslinux)
- [Setting Up a Project Without `venv`](#setting-up-a-project-without-venv)
  - [Project Initialization](#project-initialization)
  - [How Does `uv` Know Where the Environment Is?](#how-does-uv-know-where-the-environment-is)
  - [Where Are Packages Stored?](#where-are-packages-stored)
  - [Useful Commands](#useful-commands)
- [Using VS Code with `uv`](#using-vs-code-with-uv)
  - [Purpose of Configuring `uv` in VS Code](#purpose-of-configuring-uv-in-vs-code)
  - [Configuring VS Code Settings](#configuring-vs-code-settings)
  - [Running Python Scripts in VS Code](#running-python-scripts-in-vs-code)
  - [Configuring Debugging with `uv`](#configuring-debugging-with-uv)
- [Comparing `uv` vs `venv`](#comparing-uv-vs-venv)
  - [When to Prefer uv](#when-to-prefer-uv)
  - [When to Prefer venv](#when-to-prefer-venv)
- [Nuances of Running Scripts with `uv`](#nuances-of-running-scripts-with-uv)
  - [Running from the Project Root](#running-from-the-project-root)
  - [Running from a Subdirectory within the Project](#running-from-a-subdirectory-within-the-project)
  - [Running from Outside the Project](#running-from-outside-the-project)
  - [Best Practice](#best-practice)
- [Understanding uv conceptually!](#understanding-uv-conceptually)
- [How `uv` Handles Direct vs Transitive Dependencies](#how-uv-handles-direct-vs-transitive-dependencies)
  - [How Does It Work?](#how-does-it-work)
  - [What is Locking?](#what-is-locking)
  - [File Responsibilities](#file-responsibilities)
- [About `pyproject.toml`](#about-pyprojecttoml)
- [References](#references)



<!-- ********************* -->
# Installing `uv`
<!-- ********************* -->

## Windows

```powershell
winget install --id=astral-sh.uv -e
```

## macOS/Linux

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Verify installation:

```bash
uv --version
```

<!-- ********************* -->
# Setting Up a Project Without `venv`
<!-- ********************* -->

uv is best worked without using virtual environments.

## Project Initialization

```bash
mkdir myproject
cd myproject
uv init
uv pip install <library-name>
```

This creates a `pyproject.toml`, resolves the dependency, and stores everything in a global cache with per-project metadata in `.uv/`.

## How Does `uv` Know Where the Environment Is?

`uv` detects the root of a project by looking for a `pyproject.toml` file or `.uv/` directory. Always run `uv run` from within the project root or its subdirectories.

## Where Are Packages Stored?

- **Windows**: `C:\Users\<User>\AppData\Local\uv\packages`
- **macOS/Linux**: `~/.cache/uv/packages`

## Useful Commands

List installed dependencies:

```bash
uv pip freeze
```

Run a Python script:

```bash
uv run python script.py
```

<!-- ********************* -->
# Using VS Code with `uv`
<!-- ********************* -->

With `uv`, integrating VS Code requires a few specific settings so that `uv` runs correctly inside VS Code terminals, run commands, and debuggers.

## Purpose of Configuring `uv` in VS Code

By default, VS Code expects a Python interpreter path — either from the system or a `venv`. Since `uv` wraps Python execution dynamically, you instruct VS Code to run Python via `uv run python` to ensure the correct project environment is picked up automatically.

## Configuring VS Code Settings

Create or update `.vscode/settings.json` in your project folder:

```json
{
    "python.defaultInterpreterPath": "uv run python",
    "python.terminal.activateEnvironment": false
}
```

This tells VS Code to:

- Run Python using `uv run python`
- Skip trying to activate any `venv` (since `uv` handles isolation)

## Running Python Scripts in VS Code

Now, when you right-click any `.py` file and select "Run Python File in Terminal," or use the `Run` button in the editor, VS Code will run:

```bash
uv run python <your-script>.py
```

## Configuring Debugging with `uv`

For debugging support with `uv`, configure `.vscode/launch.json` as below:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python Debug with uv",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "justMyCode": true,
            "python": "uv run python"
        }
    ]
}
```

Now you can:

- Run Python files normally
- Debug Python files using F5
- Avoid managing or activating any `venv`

VS Code and `uv` together provide a modern, frictionless Python development experience.

<!-- ********************* -->
# Comparing `uv` vs `venv`
<!-- ********************* -->

While uv provides a modern, simplified, and faster experience for managing Python environments, there are still scenarios where using venv may be necessary or beneficial. Understanding the key differences helps decide when and why to use one over the other.

## When to Prefer uv

1. You want automatic environment handling without manual activation.
2. You want fast dependency resolution and installation.
3. You want native lockfile support for reproducibility.
4. You're starting a new project without legacy tool dependencies.

## When to Prefer venv

1. Working with legacy tools or libraries expecting a venv structure.
2. Integration with systems or CI/CD pipelines that are designed around venv activation.
3. Projects that expect standard site-packages based isolation.

** Tip: ** You can even use uv inside a venv to combine the benefits of both approaches — fast dependency management with compatibility. However that aspect is not covered in this article.

| Feature                           | `uv` (No venv)        | Traditional `venv` + `pip`                  |
| --------------------------------- | --------------------- | ------------------------------------------- |
| Requires virtualenv?              | ❌ No                  | ✅ Yes                                       |
| Dependency isolation              | ✅ Built-in            | ✅ Via `venv`                                |
| Performance                       | ⚡ Very fast           | 🐢 Slower                                   |
| Setup complexity                  | 🚀 Simple (`uv init`) | 🧱 Requires `python -m venv` and activation |
| Lockfile support                  | ✅ `uv.lock`           | ❌ Manual management needed                  |
| Environment activation needed     | ❌ No                  | ✅ Yes                                       |
| Works like `npm`/`pnpm`           | ✅ Yes                 | ❌ No                                        |
| Compatibility with legacy tooling | ⚠ Partial             | ✅ Full                                      |

<!-- ********************* -->
# Nuances of Running Scripts with `uv`
<!-- ********************* -->

Understanding how `uv` locates and manages project environments when running Python scripts is essential for a smooth workflow.

## Running from the Project Root

If you are in the root directory of your project (where `pyproject.toml` exists), simply run:

```bash
uv run python script.py
```

This works seamlessly because `uv` finds the environment definition right there.

## Running from a Subdirectory within the Project

If your working directory is a subfolder inside your project:

```bash
cd myproject/subfolder
uv run python ../script.py
```

`uv` will search parent directories until it finds the nearest `pyproject.toml` and `.uv/` and will use that environment.

## Running from Outside the Project

If you are in a completely different location:

```bash
cd ~
uv run python ~/myproject/script.py
```

- `uv` will start from the location of the script (`~/myproject/script.py`) and search upwards for `pyproject.toml`.
- If found, it will use that environment.
- If not found, it will default to the global context or error out.

## Best Practice

Although `uv` can resolve the environment from the script path, it is still good practice to `cd` into the project root or a subdirectory of the project before running scripts to avoid ambiguity and ensure clarity in larger, multi-project setups.

<!-- ********************* -->
# Understanding uv conceptually!
<!-- ********************* -->

`uv` changes the traditional way of thinking about Python development and environment management. Rather than explicitly creating, activating, and managing virtual environments, `uv` encourages thinking at the level of a project boundary.

Each project folder becomes a self-contained universe defined by:

- Its `pyproject.toml` file (declaring what you care about)
- Its `uv.lock` file (capturing what is actually used)
- Its `.uv/` directory (internal bookkeeping for `uv`)

This is similar to the experience in other ecosystems like Node.js with `npm` or `pnpm`, where the presence of a `package.json` defines a project boundary.

**Mental Model for a Python Developer using `uv`**

```text
- Is this script part of a uv project? → Just run it with uv
- What dependencies does this project need? → Declare them in pyproject.toml
- Do I need to activate environments manually? → No, uv resolves and runs in project context
```

This makes day-to-day workflows simpler, faster, and more reproducible. Developers can focus on writing Python code without worrying about environment setup intricacies.

Run any script like:

```bash
uv run python script.py
```

Run any script like:

```bash
uv run python script.py
```

<!-- ********************* -->
# How `uv` Handles Direct vs Transitive Dependencies
<!-- ********************* -->

When you install a package using `uv pip install <library-name>`, `uv` manages dependencies at two levels:

1. Direct Dependencies — explicitly installed by you.
2. Transitive (Indirect) Dependencies — automatically installed because they are required by your direct dependencies.

## How Does It Work?

- Only the direct dependencies you install go into `pyproject.toml`.
- All transitive dependencies are captured and locked in the `uv.lock` file.

This separation keeps `pyproject.toml` clean and human-friendly, while ensuring reproducibility via the lockfile.

## What is Locking?

Locking refers to capturing the exact versions of all dependencies — both direct and transitive — that `uv` resolved at install time. This is written into `uv.lock`.

This ensures:

- The same environment on all developer machines.
- Reproducible builds in CI/CD.
- No surprises from upstream dependency changes.

Whenever you:

- Install a new package → `uv.lock` is automatically updated.
- Remove a package → `uv.lock` is automatically updated.
- Manually edit `pyproject.toml` → Run `uv pip compile` to update `uv.lock`.

## File Responsibilities

| File             | Contains                                               | Managed by                      |   |
| ---------------- | ------------------------------------------------------ | ------------------------------- | - |
| `pyproject.toml` | Direct dependencies only                               | `uv pip install`                |   |
| `uv.lock`        | Direct + all transitive dependencies (locked versions) | Auto-update or `uv pip compile` |   |

<!-- ************************************************************* -->
# About `pyproject.toml` 
<!-- ************************************************************* -->

The `pyproject.toml` file is the heart of every Python project managed by `uv`. It defines your project’s metadata and explicitly lists the top-level (direct) dependencies that you care about.

Unlike `requirements.txt`, which can be large and messy listing every single installed package, `pyproject.toml` stays clean and focused on just the libraries you directly installed.

When you run:

```bash
uv pip install <library-name>
```

`uv` adds that package to `pyproject.toml` under `[project.dependencies]`.

**Why is this Useful?**

- Clean project definition
- Easy to review & audit
- Version control-friendly

The full dependency tree (including all transitive dependencies required by your libraries) is automatically captured in the `uv.lock` file.

This provides:

- Guaranteed reproducibility
- Predictable environments across all machines
- Confidence that your app won't suddenly break because an indirect dependency changed upstream

Example `pyproject.toml`:

```toml
[project]
name = "myproject"
version = "0.1.0"
description = "Example Python project using uv"
authors = ["Your Name <you@example.com>"]

[project.dependencies]
requests = "*"
numpy = "*"
flask = "*"
```

<!-- ********************* -->
# References
<!-- ********************* -->

1. [Astral's Official ](https://astral.sh/uv/)[`uv`](https://astral.sh/uv/)[ Documentation](https://astral.sh/uv/) — The primary source of truth for `uv`. Explains installation, usage, dependency management, and advanced features.

2. [Astral's Blog on ](https://astral.sh/blog/uv)[`uv`](https://astral.sh/blog/uv) — Provides the vision behind `uv`, its design goals, performance improvements, and roadmap compared to traditional Python tooling.

3. [VS Code Python Environments](https://code.visualstudio.com/docs/python/environments) — Official VS Code documentation explaining how Python interpreters and environments are detected and configured within the editor.

4. [VS Code Python Debugging Guide](https://code.visualstudio.com/docs/python/debugging) — Details on how to configure debugging workflows in VS Code, including environment settings and custom launch configurations.

5. [PEP 621 - Storing project metadata in ](https://peps.python.org/pep-0621/)[`pyproject.toml`](https://peps.python.org/pep-0621/) — The official Python Enhancement Proposal that defines how `pyproject.toml` should structure project metadata like name, version, dependencies, and more.

6. [Python Packaging User Guide - Dependency Management](https://packaging.python.org/en/latest/discussions/dependency-management/) — Explains the philosophy of dependency management in Python, including direct vs transitive dependencies, and why lockfiles matter.



