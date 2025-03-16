<!-- ********************* -->
# A Comprehensive Guide to Typer in Python
<!-- ********************* -->

## 1. Introduction
Typer is a modern Python library for building command-line interface (CLI) applications with minimal code. It is built on top of Click and takes advantage of Python's type hints to provide automatic argument validation and documentation generation. 

Unlike traditional CLI libraries, Typer simplifies development while offering powerful features like autocompletion and built-in help messages.

<!-- ********************* -->
# 2. Key Features of Typer
<!-- ********************* -->

- **Type Hinting Integration**: Uses Python type hints for argument parsing and validation.
- **Automatic Documentation**: Generates help messages automatically.
- **Autocompletion**: Provides shell autocompletion support.
- **Ease of Use**: Allows for clean and readable CLI application development.
- **Built on Click**: Extends Click's functionality while offering an improved developer experience.

<!-- ********************* -->
# 3. Installing Typer
<!-- ********************* -->
To install Typer, use:
```bash
pip install typer[all]
```
This includes optional dependencies required for shell autocompletion.

<!-- ********************* -->
# 4. Writing a Simple Typer Application
<!-- ********************* -->

Below is a basic example of using Typer to create a CLI application:

```python
import typer

app = typer.Typer()

@app.command()
def greet(name: str, age: int):
    """Greets a user with their name and age."""
    typer.echo(f"Hello {name}, you are {age} years old.")

if __name__ == "__main__":
    app()
```

### Running the Script
```bash
python script.py greet Alice 25
```
**Output:**
```
Hello Alice, you are 25 years old.
```

<!-- ********************* -->
# 5. Competitors of Typer
<!-- ********************* -->

Typer competes with several other CLI libraries in Python, each with its strengths:

- **argparse**: The built-in Python library for argument parsing, but lacks modern syntax and autocompletion.
- **Click**: Provides decorators for CLI development but requires more boilerplate than Typer.
- **docopt**: Uses docstrings for argument parsing but is less flexible than Typer.
- **Fire (Google’s Python Fire)**: Automatically converts functions into CLIs but offers minimal control.

### How Typer Differs
Typer stands out because it integrates **Python type hints** for automatic argument validation and documentation. It also provides an easier syntax compared to Click while maintaining similar capabilities.

<!-- ********************* -->
# 6. Advanced Typer Example
<!-- ********************* -->

Below is a more complex example demonstrating multiple commands and argument types:

```python
import typer

app = typer.Typer()

@app.command()
def add(x: int, y: int):
    """Adds two numbers."""
    typer.echo(f"Result: {x + y}")

@app.command()
def subtract(x: int, y: int):
    """Subtracts two numbers."""
    typer.echo(f"Result: {x - y}")

if __name__ == "__main__":
    app()
```

### Running the Script
```bash
python script.py add 10 5
```
**Output:**
```
Result: 15
```

```bash
python script.py subtract 10 5
```
**Output:**
```
Result: 5
```

<!-- ********************* -->
# 7. Decorators in Python
<!-- ********************* -->

Typer uses **decorators**, such as `@app.command()`, to define CLI commands. A decorator in Python is a function that modifies another function’s behavior without changing its structure. It is commonly used in frameworks like Flask and Click to register functions dynamically. Decorators improve code reusability and readability by allowing developers to attach behaviors to functions in a clean way.

<!-- ********************* -->
# 8. Conclusion
<!-- ********************* -->

Typer is a powerful and modern library that simplifies CLI application development in Python. With its **type hint integration**, **automatic documentation**, and **ease of use**, it is an excellent choice for both beginners and experienced developers. While alternatives like Click, argparse, docopt, and Fire have their own merits, Typer offers a perfect balance of **usability** and **flexibility**.

<!-- ********************* -->
# 9. References
<!-- ********************* -->

Here are some useful references for learning more about Typer:

1. **[Typer Official Documentation](https://typer.tiangolo.com/)**
   - This is the official documentation for Typer, covering installation, usage, and advanced features.

2. **[Click Documentation](https://click.palletsprojects.com/)**
   - Since Typer is built on Click, understanding Click’s features can help in building more complex applications.

3. **[Python argparse Documentation](https://docs.python.org/3/library/argparse.html)**
   - A good reference if you want to compare Typer with Python’s built-in argparse module.

4. **[GitHub Repository for Typer](https://github.com/tiangolo/typer)**
   - The official GitHub repo where you can find the source code, issues, and contributions.

5. **[Python Fire Documentation](https://github.com/google/python-fire)**
   - If you're interested in Google Fire, this repo explains how it differs from Typer in CLI development.

6. **[Python Decorators Guide](https://realpython.com/primer-on-python-decorators/)**
   - A detailed guide on Python decorators, explaining how they work and how they are used in frameworks like Typer.

