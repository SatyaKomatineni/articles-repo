<!-- ********************* -->
# Scoop: A Lightweight Package Manager for Windows
<!-- ********************* -->

Scoop is a minimalist, command-line package manager for Windows that makes it easy to install, manage, and update developer tools and CLI utilities without needing administrative privileges or graphical installers. It is particularly popular among developers and power users who want a clean, scriptable environment for setting up and maintaining their toolchain.

<!-- ********************* -->
# Table of Contents
<!-- ********************* -->

1. What is Scoop?
2. Why Use Scoop Instead of Manual Installers?
3. Installing Scoop
4. How Scoop Works
5. Installing Tools with Scoop
6. Scoop and Isolation: What Gets Installed and Where
7. What Are Shims?
8. Useful Developer Tools Available via Scoop
9. Comparison: Scoop vs Chocolatey
10. References

<!-- ********************* -->
# What is Scoop?
<!-- ********************* -->

Scoop is a command-line installer for Windows that helps users install programs and developer tools in an easy and repeatable way. Unlike traditional Windows installers, Scoop installs programs into a user's profile directory, avoiding the need for administrator access.

Scoop is ideal for:
- Installing CLI tools like `fzf`, `git`, `node`, `python`, and `jq`
- Automating development environment setup
- Keeping tools up to date with one command

<!-- ********************* -->
# Why Use Scoop Instead of Manual Installers?
<!-- ********************* -->

Manual `.exe` and `.msi` installers are familiar but tedious for bulk installation or automation. Scoop solves that by offering:

- **Scriptable installs** for batch setup
- **Silent installations** with no dialogs or wizards
- **User-level installation**, requiring no admin rights
- **Easy upgrades** with `scoop update *`
- **Predictable paths** and no registry bloat

| Task                        | Manual Installer | Scoop                          |
|-----------------------------|------------------|--------------------------------|
| Install one CLI tool        | ✅ Easy           | ✅ Just as easy                |
| Install multiple tools      | ❌ Tedious        | ✅ One-liner                   |
| Requires admin rights       | ✅ Usually        | ❌ Not needed                 |
| Update all tools            | ❌ Manual         | ✅ `scoop update *`           |
| System clutter              | ⚠️ Possible       | ✅ Isolated in user folder     |

<!-- ********************* -->
# Installing Scoop
<!-- ********************* -->

To install Scoop, open PowerShell and run:

```powershell
iwr -useb get.scoop.sh | iex
```

This installs Scoop in your user profile directory: `C:\Users\<you>\scoop`.

No admin rights required. Once installed, the `scoop` command is available in your terminal.

<!-- ********************* -->
# How Scoop Works
<!-- ********************* -->

Scoop downloads application binaries from GitHub and other sources, verifies them, and places them in `C:\Users\<you>\scoop\apps`. It also sets up lightweight "shims" (tiny launchers) in `C:\Users\<you>\scoop\shims`, which is automatically added to your PATH.

Each installed app has a versioned directory under:
```plaintext
C:\Users\<you>\scoop\apps\<tool>\<version>
```

To install a tool:
```powershell
scoop install fzf
```

To uninstall:
```powershell
scoop uninstall fzf
```

To upgrade everything:
```powershell
scoop update *
```

<!-- ********************* -->
# Installing Tools with Scoop
<!-- ********************* -->

Example: Installing `fzf` (fuzzy finder for the terminal)
```powershell
scoop install fzf
```

This fetches the Windows-compatible `fzf.exe` and places it in:
```plaintext
C:\Users\<you>\scoop\apps\fzf\current\fzf.exe
```

No extra dependencies are installed, and no Unix shell or emulation layer is introduced. This makes Scoop very lightweight and non-intrusive.

<!-- ********************* -->
# Scoop and Isolation: What Gets Installed and Where
<!-- ********************* -->

When you install a package with Scoop:
- Only that package is installed — no surprise dependencies
- Everything is stored in your user profile directory
- No changes are made to the Windows registry
- Admin rights are never needed

### Important Paths:
- App binaries: `C:\Users\<you>\scoop\apps`
- Executable shims: `C:\Users\<you>\scoop\shims`
- Config and buckets: `C:\Users\<you>\scoop\apps\scoop`

You can remove Scoop cleanly by deleting the `scoop` folder if needed.

<!-- ********************* -->
# What Are Shims?
<!-- ********************* -->

Shims in Scoop are small executable wrappers that act as proxies to the actual program binaries installed under the `apps` directory. When you install a program using Scoop, it creates a shim in `C:\Users\<you>\scoop\shims`, which is part of your system's PATH.

This allows you to run any installed tool globally, just by typing its name, without needing to specify the full path. For example, if you install `fzf`, Scoop places `fzf.exe` in the shims folder, and you can launch it from anywhere in the terminal by typing:
```powershell
fzf
```

Shims also help version management — updating or rolling back tools is as simple as changing what the shim points to.

<!-- ********************* -->
# Useful Developer Tools Available via Scoop
<!-- ********************* -->

Beyond `fzf`, Scoop makes it easy to install many lightweight and powerful developer utilities. Here are a few notable ones:

- **bat** — A syntax-highlighting version of `cat` for reading files
- **ripgrep (rg)** — A fast search tool (like `grep` but faster and smarter)
- **fd** — A user-friendly alternative to `find`, written in Rust
- **jq** — A lightweight and flexible command-line JSON processor
- **dust** — Better disk usage visualization (alternative to `du`)
- **delta** — Beautiful `git diff` viewer with syntax highlighting
- **httpie** — Command-line HTTP client with a clean UI for APIs
- **exa** — A modern replacement for `ls` with color and Git info

These tools can greatly enhance productivity for scripting, debugging, and working with code and data directly in the terminal.

<!-- ********************* -->
# Comparison: Scoop vs Chocolatey
<!-- ********************* -->

| Feature                      | Scoop                          | Chocolatey                     |
|-----------------------------|--------------------------------|--------------------------------|
| Admin rights needed         | ❌ No                           | ✅ Yes (usually)               |
| Installs CLI tools          | ✅ Excellent                    | ✅ Good                        |
| Installs GUI apps           | ⚠️ Limited                     | ✅ Yes                         |
| Registry changes            | ❌ None                         | ✅ Sometimes                   |
| Good for developers         | ✅ Yes                          | ✅ Yes                         |
| Where apps go               | User profile                   | Program Files / System-wide   |

Use **Scoop** for developer tooling, scripting, and portability. Use **Chocolatey** for traditional software (editors, browsers, system apps).

<!-- ********************* -->
# References
<!-- ********************* -->

1. [Scoop Homepage](https://scoop.sh/)
   - Official site with install instructions and overview.

2. [Scoop GitHub Repository](https://github.com/ScoopInstaller/Scoop)
   - Source code and community issue tracking.

3. [Scoop Directory Structure Explained](https://github.com/ScoopInstaller/Scoop/wiki/Directory-Structure)
   - Covers where everything lives in your file system.

4. [Awesome Scoop List](https://github.com/ScoopInstaller/Extras)
   - A curated list of extra tools and buckets for Scoop.

5. [Chocolatey vs Scoop Comparison (Blog)](https://dev.to/aaronksaunders/why-i-like-scoop-over-chocolatey-4a3h)
   - Real-world perspective comparing both tools.

