<!-- ********************* -->
# How to Run Python Launch Configurations in VS Code
<!-- ********************* -->

> **Note:** This document was generated using ChatGPT. Not all sections have been individually verified. Please use discretion and consult official documentation as needed.

## Question

If I want to run Python launch configurations for the following:

 1. Just run
 2. Run with debug
 3. Run with debug with additional command line inputs I can supply during run as -param1 etc on the command line

 What is my best approach for doing that with or without launch.json, preferably if possible with just the menu options? If I have to use launch.json, give me what that process looks like, step by step, and with examples of the launch.json files.

---

## Using VS Code Without `launch.json`

VS Code offers several built-in ways to run and debug Python scripts without creating a custom `launch.json` file.

### Just Run (No Debug)
- Open the `.py` file
- Click the **"Run Python File in Terminal"** button in the top-right corner
- Or right-click in the editor → **"Run Python File in Terminal"**
- Or press `Ctrl + F5`

### Run With Debugger
- Open the `.py` file
- Go to **Run > Start Debugging**
- Or press `F5`

This launches the file with the debugger attached. No arguments are passed, and no configuration is required.

### Run With Debugger + Command-Line Arguments
This is **not possible** through the menu alone. It requires a `launch.json` file.

---

## Using `launch.json` for Full Control

You can configure three distinct behaviors using VS Code's `launch.json` file.

### Step-by-Step:

1. Open the **Run & Debug panel** (`Ctrl + Shift + D`)
2. Click **"create a launch.json file"** → choose **Python**
3. VS Code creates a `.vscode/launch.json` file
4. Replace or add the following configurations:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Run (No Debug)",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "noDebug": true
    },
    {
      "name": "Python: Debug",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal"
    },
    {
      "name": "Python: Debug with Args",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "args": "${command:pickArgs}"
    }
  ]
}
```

### How to Use:
- Open a `.py` file
- Go to the **Run & Debug panel**
- In the dropdown, select the configuration:
  - `Python: Run (No Debug)`
  - `Python: Debug`
  - `Python: Debug with Args` (will prompt for command-line arguments)
- Click ▶️ to run

---

## Terminal Directory Behavior and Virtual Environment Activation

When you run or debug a Python file using any of the methods above, VS Code will generally open the terminal in the folder associated with the file. In a multi-root workspace, this means the **root folder that contains the file** — each root folder acts like its own self-contained project or repo.

However, the default working directory in the terminal can vary depending on your workspace settings.

By default:
- If no `launch.json` is defined, the terminal usually opens at the **main workspace root**.
- If a `launch.json` exists, it may honor the folder where the script resides, unless you specify a `"cwd"` explicitly.

To ensure consistent behavior:
- You can set the terminal to always start in the project root using:
  ```json
  "terminal.integrated.cwd": "${workspaceFolder}"
  ```
- Or, in `launch.json`, include:
  ```json
  "cwd": "${workspaceFolder}"
  ```

### Workspace Variables and Multi-root Workspaces
In multi-root workspaces, `${workspaceFolder}` refers to the **first folder listed** in the workspace. This can lead to confusion when running files from other folders. To dynamically match the folder containing the current file, use:

```json
"terminal.integrated.cwd": "${fileWorkspaceFolder}"
```

This ensures that the terminal opens in the correct folder relative to the file being run. You can also explicitly name a root folder if needed:

```json
"terminal.integrated.cwd": "${workspaceFolder:MyProjectFolder}"
```

This is especially useful when maintaining consistency across multiple root folders, each with their own project-specific environment.

### Virtual Environment Activation
If you rely on a virtual environment (e.g., `.venv`):

- VS Code can automatically activate it when opening a new terminal, if this is enabled:
  ```json
  "python.terminal.activateEnvironment": true
  ```
- However, if you’ve already opened a terminal or are running scripts via a launch config, **you must ensure that the terminal's working directory is in the folder where the `.venv` lives**.

You can enforce this with:
```json
"cwd": "${fileWorkspaceFolder}"
```
in each launch configuration.

If your virtual environment is not activated or your working directory is not at the root of your project, Python might not find the correct interpreter or dependencies. In multi-root workspaces, this also means each root folder should be configured to manage its own environment cleanly.

### What Happens If a Terminal Is Already Open?
If a terminal is already open when you run or debug a Python file:
- VS Code may reuse the **currently active terminal** instead of opening a new one.
- If the active terminal belongs to a different workspace folder (e.g., you last ran something from `Project-A`, and now you're trying to run from `Project-B`), it may:
  - Run the script in the wrong working directory
  - Fail to activate the correct `.venv`
  - Misbehave due to stale context (e.g., old environment variables or lingering processes)

To avoid this:
- Either make sure the terminal you're using belongs to the correct project folder
- Or close existing terminals before switching projects in a multi-root workspace
- You can also enable a new terminal to open automatically by setting `"console": "integratedTerminal"` and avoiding `"python.terminal.activateEnvInCurrentTerminal": true` in your settings

Being aware of which terminal is active and how it’s configured can help prevent subtle bugs in multi-root workspaces.

### Recommended Manual Practice
If you're managing this behavior manually (and prefer to avoid custom automation), follow this practical routine:

1. **Before running from a new project**, explicitly switch to the root directory of that project in the terminal.
2. **If using a virtual environment**, manually activate the `.venv` for that project. If another environment is already active, deactivate it first if needed.
3. **Then run or debug** the desired Python file using either the Run icon, context menu, or launch configuration.

This manual approach is fully valid as long as you stay mindful. The primary risks are:
- Forgetting to change directories or activate the appropriate environment
- Running a script with incorrect paths, Python versions, or environment variables

If you're careful and consistent, this approach works well, especially in multi-root or multi-project environments where each folder has its own isolated setup.

---

## How to Reload VS Code After Changing `launch.json`

If you make edits to your `launch.json` file and want to ensure VS Code picks up the changes:

- Press `Ctrl + Shift + P` to open the Command Palette
- Type and select **“Reload Window”**
- This refreshes the entire workspace, reloading the `launch.json` and other settings

Alternatively, simply:
- Close and reopen the Run & Debug panel
- Or reselect the updated configuration from the dropdown menu

Most changes in `launch.json` are picked up immediately, but reloading helps if something seems out of sync.

---

## Summary Table

| Use Case | Best Method |
|----------|-------------|
| Just run | Use "Run Python File in Terminal" (top-right button or right-click) |
| Debug without arguments | Press `F5` or select "Start Debugging" |
| Debug with CLI arguments | Requires `launch.json` with `args` or `${command:pickArgs}` |
| Ensure working directory is correct | Set `"cwd": "${fileWorkspaceFolder}"` in `launch.json` or settings.json |
| Use virtual environment | Make sure venv is created and terminal starts in project root |
| Multi-root workspace handling | Ensure each root folder is configured with its own settings and environment |
| Terminal already open | Check which terminal is active, close or switch it before running from another project |
| Manual terminal prep (recommended) | Switch to project root, activate `.venv`, then run the file |

---

## References

1. **VS Code Python Debugging Docs**  
   https://code.visualstudio.com/docs/python/debugging  
   Covers basic and advanced debugging scenarios for Python in VS Code.

2. **launch.json Attributes Reference**  
   https://code.visualstudio.com/docs/editor/debugging#_launch-configurations  
   Explains all options available in `launch.json`, including `args`, `program`, `noDebug`, and more.

3. **VS Code Python Extension (GitHub)**  
   https://github.com/microsoft/vscode-python  
   Source code and issues related to Python extension behavior.

4. **Python Extension for Visual Studio Code (Marketplace)**  
   https://marketplace.visualstudio.com/items?itemName=ms-python.python  
   Official extension page with features and install details.

5. **PickArgs Command Integration**  
   https://github.com/microsoft/vscode-python/issues/16432  
   Community and VS Code team discussion around `${command:pickArgs}` and its use for dynamic arguments.

6. **VS Code Variables Reference**  
   https://code.visualstudio.com/docs/reference/variables-reference  
   Describes available variables like `${workspaceFolder}` and `${fileWorkspaceFolder}` for launch and task configs.

