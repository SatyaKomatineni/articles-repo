<!-- ********************* -->
# Creating and Using the `lightinclude` PowerShell Module
<!-- ********************* -->

Fundamental to computer languages is to use functions. Then functions broken down into libraries or/and modules.

In some languages this is very easy to do, for they are built ground up with that idea. 

High level languages are good examples of this. 

Especially Java. 

However in scripting languages this can be tricky. Perhaps because, the scripts are expected to be just one script file.

But that is short-sighted if you become even slightly sane.

In PS this is solved through creating proper "modules" out of reusable functions. But that takes some work.

I really wanted a way for sharing functions much more easily on an adhoc basis where I don't want to take the extra step of formal modules.

In that effort this article is to help with a "formal module" that can let me use "informal just directories and files" as the containers for the reusable functions.

Read ahead now.

There are 2 approaches presented.

1. Create a module with single function called `use` for importing functions for local files and directories
2. Or just 2 or 3 lines of code you can use at the top of your script to do the same
3. Both approaches use an environment variable pointing to a directory where the reusable functions and scripts are available

This article explains how to build a lightweight, user-defined PowerShell module named `lightinclude`, which simplifies the inclusion of shared `.ps1` script files using a simple `use` function. This module supports caching to ensure that each file is loaded only once and relies on an environment variable to locate shared scripts.

<!-- ********************* -->
# Table of Contents
<!-- ********************* -->

1. Overview
2. Motivation and Use Cases
3. Understanding PowerShell Module Files
4. Module Structure
5. Creating the Module Files
6. Installation Instructions
7. Using the Module
8. Direct Inline Use Without Installing the Module
9. References

<!-- ********************* -->
# Overview
<!-- ********************* -->

`lightinclude` is a minimalist PowerShell module for including `.ps1` scripts using dot-sourcing in a safe and convenient way. It introduces a `use` function that locates shared scripts using an environment variable (`MY_PS_LIB`), checks for existence, caches them to prevent repeat execution, and includes them into the current scope.

<!-- ********************* -->
# Motivation and Use Cases
<!-- ********************* -->

In complex scripting environments, it is common to organize helper functions into reusable script files. PowerShell supports modular design, but modules are heavier than needed for simple `.ps1` libraries. `lightinclude` fills this gap by:

- Simplifying reuse of script files across multiple scripts
- Avoiding double inclusion
- Centralizing shared script locations via an environment variable
- Providing safety checks with minimal syntax overhead

<!-- ********************* -->
# Understanding PowerShell Module Files
<!-- ********************* -->

A PowerShell module is a collection of related functions packaged for reuse. Modules typically consist of two primary files:

### `.psm1` – PowerShell Module File
- This file contains the actual function implementations.
- You place your exported functions and any logic here.
- It can be as simple as a few functions or a full script library.

### `.psd1` – PowerShell Module Manifest
- This metadata file describes the module.
- It declares the module’s name, version, author, compatible PowerShell version, exported functions, and more.
- It's not required for simple modules but highly recommended for reuse, tooling support, and publishing.

### `README.md` – Markdown Documentation File
- This optional file documents the purpose, usage, and examples for the module.
- It's especially useful when sharing on GitHub or publishing to the PowerShell Gallery, where it will be rendered as the front page.
- It provides installation instructions, usage patterns, and any relevant details for developers and users.

### Naming Conventions
- The `.psm1` and `.psd1` files should have the **same base name** as the folder that contains them.
  - For example, if the module folder is named `lightinclude`, then the files should be `lightinclude.psm1` and `lightinclude.psd1`.
- This ensures that PowerShell recognizes and autoloads the module correctly.

<!-- ********************* -->
# Module Structure
<!-- ********************* -->

Here’s the structure of the `lightinclude` module:

```plaintext
lightinclude/
├── lightinclude.psm1   # Core logic
├── lightinclude.psd1   # Manifest metadata
└── README.md           # Usage and documentation
```

<!-- ********************* -->
# Creating the Module Files
<!-- ********************* -->

### `lightinclude.psm1`

```powershell
function use {
    param($n)
    if (-not $env:MY_PS_LIB) {
        Write-Warning "Missing env: MY_PS_LIB"
        exit
    }
    $p = Join-Path $env:MY_PS_LIB $n
    if (-not (Test-Path $p)) {
        Write-Warning "Missing file: $p"
        exit
    }
    if (-not $script:__use_cache) { $script:__use_cache = @{} }
    if ($script:__use_cache[$p]) { return }
    . $p
    $script:__use_cache[$p] = $true
    Write-Verbose "Loaded $p"
}

Export-ModuleMember -Function use
```

### `lightinclude.psd1`

```powershell
@{
    RootModule        = 'lightinclude.psm1'
    ModuleVersion     = '1.0.0'
    GUID              = '8f967efa-b07d-49e7-b2f5-2a6c9c0210e9'
    Author            = 'Satya Komatineni'
    Description       = 'Minimalist module for script inclusion using dot-sourcing with caching.'
    PowerShellVersion = '5.1'
    FunctionsToExport = @('use')
    CmdletsToExport   = @()
    VariablesToExport = @()
    AliasesToExport   = @()
    PrivateData       = @{}
}
```

### `README.md`

```markdown
# lightinclude

`lightinclude` is a lightweight PowerShell module that allows you to include other `.ps1` scripts using a simple `use` command. It supports caching so each script is only loaded once per session.

## Features
- Dot-sources other scripts from a shared folder
- Uses the environment variable `MY_PS_LIB` to locate them
- Prevents double inclusion with caching
- Emits verbose output when loaded
```

<!-- ********************* -->
# Installation Instructions
<!-- ********************* -->

### Option 1: Copy into a known PowerShell module path

1. Create a folder named `lightinclude` under one of your module paths:
   ```plaintext
   $env:USERPROFILE\Documents\PowerShell\Modules\lightinclude
   ```

2. Copy the `lightinclude.psm1`, `lightinclude.psd1`, and `README.md` files into it.

3. Import the module in your session or script:
   ```powershell
   Import-Module lightinclude
   ```

### Option 2: Use from any folder by full path

If you don’t want to copy the module into your profile directory, just import directly:

```powershell
Import-Module "C:\path\to\lightinclude\lightinclude.psd1"
```

### Option 3: Temporarily add to `$env:PSModulePath`

If you want to refer to the module by name without copying it, you can temporarily add it to your session’s module search path:

```powershell
$env:PSModulePath += ";C:\path\to\lightinclude"
Import-Module lightinclude
```

This works only for the current session.

### Option 4: Permanently add to your PowerShell profile

To make the module discoverable in all future sessions, add this to your PowerShell profile:

```powershell
notepad $PROFILE
```

Then add the following line to your profile script:

```powershell
$env:PSModulePath += ";C:\path\to\lightinclude"
```

Now `Import-Module lightinclude` will work in all sessions.

<!-- ********************* -->
# Using the Module
<!-- ********************* -->

### Step 1: Set your shared scripts directory
```powershell
$env:MY_PS_LIB = "C:\Users\me\common-scripts"
```

### Step 2: Include scripts with `use`
```powershell
use "math-utils.ps1"
use "string-utils.ps1" -Verbose
```

If the file has already been loaded, `use` will silently skip it. You can use `-Verbose` to confirm that the script was loaded.

If the environment variable is missing or the script does not exist, the function exits and shows a warning.

<!-- ********************* -->
# Direct Inline Use Without Installing the Module
<!-- ********************* -->

If you don't want to install or import the module at all, you can define the `use` function directly at the top of your script. Here's a minimal version that includes all core behavior without caching:

```powershell
function use { param($n) . (Join-Path $env:MY_PS_LIB $n) }
```

If you want to include basic safety checks:

```powershell
function use {
    param($n)
    if (-not $env:MY_PS_LIB) { Write-Warning "Missing MY_PS_LIB"; exit }
    $p = Join-Path $env:MY_PS_LIB $n
    if (-not (Test-Path $p)) { Write-Warning "Missing: $p"; exit }
    . $p
}
```

This is ideal when you want script reuse without any external module setup.

<!-- ********************* -->
# References
<!-- ********************* -->

1. [PowerShell Docs – about_Modules](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_modules)
   - Explains how PowerShell modules work, where to place them, and how to use manifests.

2. [PowerShell Docs – Import-Module](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/import-module)
   - Details on loading modules dynamically at runtime.

3. [PowerShell Automatic Variables](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_automatic_variables)
   - Describes variables like `$script:` and `$env:` scopes.

4. [PowerShell Profile Paths and Module Paths](https://learn.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module)
   - Shows where modules should be stored for auto-discovery.

5. [PowerShell Gallery – Best Practices](https://docs.microsoft.com/powershell/scripting/gallery/psgallery/creating-and-publishing-a-module)
   - Overview of how to create, test, and publish your own module.

