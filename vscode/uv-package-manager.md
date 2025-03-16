<!-- ********************* -->
# Installing, Initializing, and Using `uv` in a Virtual Environment (`venv`) with VS Code (Windows)
<!-- ********************* -->

## **1. Introduction**
`uv` is a modern package manager for Python that is faster and more efficient than `pip`. While `uv` does not require virtual environments (`venv`), you may want to use `venv` for compatibility with existing projects and tooling. This guide will walk you through installing `uv`, setting it up within a `venv`, and configuring **VS Code** to run and debug Python scripts seamlessly on Windows.

<!-- ********************* -->
# 2. Installing `uv` on Windows
<!-- ********************* -->

### **2.1 Install `uv` Using `winget`**
The easiest way to install `uv` on Windows is through **Windows Package Manager (`winget`)**:
```powershell
winget install --id=astral-sh.uv -e
```

Verify the installation:
```powershell
uv --version
```
If the output shows a version number, `uv` is installed correctly.

<!-- ********************* -->
# 3. Creating and Initializing a Virtual Environment with `uv`
<!-- ********************* -->

### **3.1 Create a Virtual Environment**
Run the following command to create a virtual environment (`venv`) inside your project directory:
```powershell
python -m venv .venv
```

### **3.2 Activate the Virtual Environment**
```powershell
.\.venv\Scripts\activate
```

### **3.3 Install `uv` Inside the Virtual Environment**
After activating the virtual environment, install `uv` using `winget` (optional) or `pip`:
```powershell
winget install --id=astral-sh.uv -e
```
Or using `pip`:
```powershell
pip install uv
```

### **3.4 Initialize a `uv` Project**
```powershell
uv init
```
This creates a `pyproject.toml` file to track dependencies.

### **3.5 Install Dependencies with `uv`**
```powershell
uv pip install requests
```
This installs `requests` inside the virtual environment and updates `pyproject.toml` + `uv.lock`.

<!-- ********************* -->
# 4. Configuring VS Code for `uv`
<!-- ********************* -->

To make VS Code use `uv` inside the virtual environment, follow these steps:

### **4.1 Select the Virtual Environment as the Python Interpreter**
1. Open **Command Palette** (`Ctrl + Shift + P`).
2. Search for **"Python: Select Interpreter"** and click it.
3. Select the Python interpreter inside your virtual environment:
   ```plaintext
   .venv\Scripts\python.exe
   ```

### **4.2 Modify VS Code Settings**
To ensure `uv` is used correctly, add the following in **VS Code settings (`settings.json`)**:
```json
{
    "python.defaultInterpreterPath": "uv run python",
    "python.terminal.activateEnvironment": false
}
```

<!-- ********************* -->
# 5. Running Python Scripts in VS Code with `uv`
<!-- ********************* -->

Once configured, you can run Python scripts inside VS Code:
```powershell
uv run python script.py
```
To make **VS Code automatically use `uv`**, follow these steps:
1. Open **Command Palette** (`Ctrl + Shift + P`).
2. Search for **"Preferences: Open Keyboard Shortcuts"**.
3. Search for **"Run Python File in Terminal"**.
4. Change the command from:
   ```plaintext
   python ${file}
   ```
   to:
   ```plaintext
   uv run python ${file}
   ```
Now, pressing `Ctrl + F5` will execute Python scripts using `uv`.

<!-- ********************* -->
# 6. Debugging Python Code with `uv` in VS Code
<!-- ********************* -->

To debug Python files using `uv`, modify the **VS Code debugging configuration (`launch.json`)**:

1. Open **Run & Debug** (`Ctrl + Shift + D`).
2. Click **"Create a launch.json file"**.
3. Select **Python**.
4. Modify `launch.json`:
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
5. Save the file and restart VS Code.

Now, pressing `F5` will start debugging with `uv` inside the virtual environment.

<!-- ********************* -->
# 7. Summary of Best Practices
<!-- ********************* -->
| Task                          | Command / Configuration |
|--------------------------------|------------------------|
| **Install `uv` globally**       | `winget install --id=astral-sh.uv -e` |
| **Create a virtual environment** | `python -m venv .venv` |
| **Activate virtual environment** | `.\.venv\Scripts\activate` |
| **Initialize `uv` project**     | `uv init` |
| **Install dependencies**        | `uv pip install requests` |
| **Run Python scripts with `uv`** | `uv run python script.py` |
| **Set Python interpreter in VS Code** | `"python.defaultInterpreterPath": "uv run python"` |
| **Debug with `uv` in VS Code** | Modify `launch.json` |

<!-- ********************* -->
# 8. References
<!-- ********************* -->
1. [Astral's Official `uv` Documentation](https://astral.sh/uv/) - Detailed guide on `uv`, including installation, configuration, and best practices.
2. [Configuring VS Code for Python](https://code.visualstudio.com/docs/python/environments) - Microsoft’s documentation on configuring Python environments, including using custom package managers like `uv`.
3. [Using `uv` in Python Projects](https://astral.sh/blog/uv) - Astral’s blog post explaining how `uv` integrates with modern Python workflows, including its use in VS Code.
4. [VS Code Debugging for Python](https://code.visualstudio.com/docs/python/debugging) - Microsoft's documentation on debugging Python applications in VS Code, useful for integrating `uv` in a development workflow.

This setup ensures that `uv` works seamlessly within `venv` while fully integrating with VS Code for running and debugging Python scripts on Windows.

Would you like any additional customizations? 🚀

