<!-- ********************* -->
# How to Run Python Launch Configurations in VS Code
<!-- ********************* -->

## Question

> If I want to run Python launch configurations for the following:
>
> 1. Just run
> 2. Run with debug
> 3. Run with debug with additional command line inputs I can supply during run as -param1 etc on the command line
>
> What is my best approach for doing that with or without launch.json, preferably if possible with just the menu options? If I have to use launch.json, give me what that process looks like, step by step, and with examples of the launch.json files.

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

