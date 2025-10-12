<!-- ************************************************************ -->
# Getting Started with OpenAI CLI (via uv)
<!-- ************************************************************ -->
Satya Komatineni: 10/12/2025

Note: This article was generated with the assistance of ChatGPT and may contain automated content. Please verify critical details.

This guide explains how to use the **OpenAI Python package**—which provides both a **command-line interface (CLI)** and a **Python SDK library**—through **uv**, a modern and fast Python tool manager.

The OpenAI package serves two distinct use cases:

* As a **CLI tool**, it lets you run OpenAI commands directly from the terminal for quick testing, automation, and data management.
* As a **Python SDK**, it allows developers to integrate OpenAI’s API seamlessly into their applications and scripts.

This document shows how to install, configure, and use both modes with uv, and how to keep environments clean by separating **global tools** from **project libraries**.

---

* Installing via uv
* Recommended Directory Structure
* Choosing Between Project and Global Installation
* Authentication (Windows / Mac / Linux)
* CLI vs Python SDK
* Basic Usage Examples
* Common Commands
* Output and Debugging Options
* References

---

<!-- ************************************************************ -->
# Installing via uv
<!-- ************************************************************ -->

If **uv** is already installed on your system, you can use it to install and manage the OpenAI CLI easily.

### Install the OpenAI CLI globally

```bash
uv tool install openai
```

This makes the `openai` command available system-wide, isolated from your project environments.

### Update or remove later

```bash
uv tool upgrade openai
uv tool uninstall openai
```

### Install OpenAI as a library inside a project

If you’re developing a Python app that imports the OpenAI SDK, use:

```bash
uv add openai
```

This adds the SDK as a dependency in your project’s environment.

---

<!-- ************************************************************ -->
# Recommended Directory Structure
<!-- ************************************************************ -->

How you structure your workspace depends on whether you’re using OpenAI as a **Python SDK library** or as a **command-line tool**.

### When using OpenAI as a Library (SDK)

If you plan to write Python code that imports OpenAI classes and methods, create a dedicated project folder managed by uv:

```bash
mkdir my-openai-app
cd my-openai-app
uv init
uv add openai
```

This approach isolates dependencies and keeps your SDK version consistent within the project.

### When using OpenAI as a CLI Tool

If you primarily execute commands directly from the terminal, no project folder is required. Install the CLI globally so it’s available everywhere:

```bash
uv tool install openai
```

You can then use commands like `openai models.list` or `openai api chat_completions.create` from any directory.

### Summary

| Use Case      | Installation             | Where it runs           | Best for                                      |
| ------------- | ------------------------ | ----------------------- | --------------------------------------------- |
| SDK (Library) | `uv add openai`          | Inside a project folder | Application or script development             |
| CLI (Tool)    | `uv tool install openai` | System-wide             | Ad hoc queries, automation, quick API testing |

This separation ensures you maintain clean project environments while having global access to the OpenAI CLI when needed.# Choosing Between Project and Global Installation

If you primarily write Python code, install the OpenAI SDK locally within your project using `uv add openai`.

If you primarily use the **CLI** for API calls, use `uv tool install openai` for a global, isolated install.

### Recommended Approach

| Use Case                | Command                  | Description                                         |
| ----------------------- | ------------------------ | --------------------------------------------------- |
| CLI for everyday tasks  | `uv tool install openai` | Installs a global CLI tool, accessible system-wide. |
| SDK for coding projects | `uv add openai`          | Adds the library to a local project managed by uv.  |
| Temporary tool use      | `uvx openai --version`   | Runs the CLI once without permanent installation.   |

---

<!-- ************************************************************ -->
# Authentication (Windows / Mac / Linux)
<!-- ************************************************************ -->

### Step 1: Get Your API Key

Create or view your API key at [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys).

### Step 2: Set Environment Variable

**Windows (PowerShell):**

```bash
setx OPENAI_API_KEY "sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

**macOS / Linux:**

```bash
export OPENAI_API_KEY="sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

Restart your terminal after setting the variable.

Verify:

```bash
echo $env:OPENAI_API_KEY    # PowerShell
echo $OPENAI_API_KEY         # macOS/Linux
```

---

<!-- ************************************************************ -->
# CLI vs Python SDK
<!-- ************************************************************ -->

Once the `openai` package is installed via uv, you can use it in two ways:

### 1. As a Command-Line Tool (CLI)

After installation, run commands directly from your terminal:

```bash
openai api chat_completions.create -m gpt-5 -g '{"messages":[{"role":"user","content":"Hello!"}]}'
```

No Python imports are required — the CLI executes independently.

### 2. As a Python Library (SDK)

If you’re writing Python code that makes API calls directly:

```python
from openai import OpenAI
client = OpenAI()
response = client.chat.completions.create(model="gpt-5", messages=[{"role": "user", "content": "Hello!"}])
print(response.choices[0].message.content)
```

Use this approach inside project directories managed by uv.

---

<!-- ************************************************************ -->
# Basic Usage Examples
<!-- ************************************************************ -->

### Run a Chat Completion

```bash
openai api chat_completions.create -m gpt-5 -g '{"messages":[{"role":"user","content":"Write a haiku about the ocean."}]}'
```

### Cleaner Output (requires jq)

```bash
openai api chat_completions.create -m gpt-5 -g '{"messages":[{"role":"user","content":"Explain MCP servers in one line."}]}' | jq -r '.choices[0].message.content'
```

### Create an Assistant

```bash
openai assistants.create -n "QuickTutor" -m gpt-5 -t "Helps explain Spanish grammar."
```

### List All Assistants

```bash
openai assistants.list
```

---

<!-- ************************************************************ -->
# Common Commands
<!-- ************************************************************ -->

| Purpose                | Command Example                                                            |
| ---------------------- | -------------------------------------------------------------------------- |
| List available models  | `openai models.list`                                                       |
| Upload a file          | `openai files.create -p "training.jsonl"`                                  |
| Run an evaluation      | `openai evals.run --model gpt-5 --eval my_eval.yaml`                       |
| Simple text completion | `openai api completions.create -m gpt-3.5-turbo-instruct -p "Hello world"` |
| Check CLI version      | `openai --version`                                                         |
| Get help for a command | `openai api chat_completions.create --help`                                |

---

<!-- ************************************************************ -->
# Output and Debugging Options
<!-- ************************************************************ -->

### Show Detailed Logs

```bash
setx OPENAI_LOG "debug"
```

### Save Output to a File

```bash
openai api chat_completions.create -m gpt-5 -g '{"messages":[{"role":"user","content":"Summarize this."}]}' > result.json
```

### Format JSON Output

```bash
openai models.list | jq '.'
```

---

<!-- ************************************************************ -->
# Relationship Between SDK and CLI
<!-- ************************************************************ -->

When you install the `openai` Python package with uv, you get both the **Python SDK** and the **command-line interface**. The CLI is a convenient wrapper built on the same SDK.

* `uv tool install openai` provides the CLI globally.
* `uv add openai` adds the SDK locally to your project.

This design allows you to choose between **writing code** and **issuing direct commands** without managing multiple installs.

---

<!-- ************************************************************ -->
# References
<!-- ************************************************************ -->

* [OpenAI CLI Documentation](https://platform.openai.com/docs/cli)
  Official documentation with full command list and examples.

* [OpenAI API Keys](https://platform.openai.com/api-keys)
  Generate and manage your personal API keys.

* [jq JSON Processor](https://stedolan.github.io/jq/)
  Tool for formatting and filtering JSON output in terminal.

* [uv Documentation](https://docs.astral.sh/uv/)
  Official documentation for uv tool and package management.

* [Python Virtual Environments Guide](https://docs.python.org/3/library/venv.html)
  Learn how to isolate Python dependencies using `venv`.
