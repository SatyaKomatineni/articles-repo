<!-- ********************* -->
# Naming for Python Packages, Modules, and Files
<!-- ********************* -->

Proper naming in Python projects is essential for readability, maintainability, and consistency. Python follows a clear set of naming conventions for packages, modules, and other elements, primarily guided by [PEP 8](https://peps.python.org/pep-0008/#package-and-module-names).

<!-- ********************* -->
# Package Naming Conventions
<!-- ********************* -->

Python packages should:

- Be named in **all lowercase**
- Be **short and meaningful**
- **Avoid underscores** (if possible)

**Examples:**
- `utils`
- `data`
- `models`

Avoid using names that clash with standard libraries or built-ins (like `string`, `types`, etc.).

<!-- ********************* -->
# Module Naming Conventions
<!-- ********************* -->

Modules (i.e., `.py` files) should:

- Use **lowercase** letters
- May use **underscores** for readability

**Examples:**
- `data_loader.py`
- `parse_json.py`
- `config.py`

<!-- ********************* -->
# Other Naming Guidelines
<!-- ********************* -->

| Element        | Naming Style     | Examples                    |
|----------------|------------------|-----------------------------|
| **Class**      | CamelCase         | `DataLoader`, `UserModel`   |
| **Function**   | lowercase + `_`   | `load_file()`, `parse_text()` |
| **Variable**   | lowercase + `_`   | `file_name`, `user_id`      |
| **Constant**   | UPPERCASE         | `DEFAULT_PORT`, `MAX_SIZE`  |

<!-- ********************* -->
# Avoid These Pitfalls
<!-- ********************* -->

- Do **not** use dashes (`-`) in package or module names — they’re invalid in Python identifiers.
- Avoid capital letters in file or folder names.
- Don’t reuse Python built-in names (e.g., `list.py`, `string.py`).
- Avoid excessively long or deeply nested names unless necessary.

<!-- ********************* -->
# How Naming Impacts Imports
<!-- ********************* -->

Your folder and file names become part of the import path. For example:

```
my_project/
└── data/
    ├── __init__.py
    └── processor.py
```

```python
from data.processor import clean_data
```

So naming folders and files according to convention ensures clean, predictable imports.

<!-- ********************* -->
# References
<!-- ********************* -->

1. [PEP 8 - Package and Module Names](https://peps.python.org/pep-0008/#package-and-module-names): Describes the recommended naming style for packages and modules.

2. [Python Module and Import System](https://docs.python.org/3/tutorial/modules.html): Covers how modules and imports work in Python, and how naming affects usage.

