---
title: Python Modules
---
## Summary Matrix

| **File / Dunder** | **Primary Purpose**                               | **Triggered By**                              |
| ----------------- | ------------------------------------------------- | --------------------------------------------- |
| `filename.py`     | Defines a single module named `filename`.         | `import filename`                             |
| `__init__.py`     | Marks directory as a package; defines public API. | `import package` or `from package import ...` |
| `__main__.py`     | Package execution entry point.                    | `python -m package` or `python package/`      |
| `__name__`        | Dunder string set to `"__main__"` or module path. | Evaluated dynamically at runtime.             |
| `__pycache__/`    | Directory storing compiled `.pyc` bytecode.       | Automatic caching on import.                  |

# Python Modules & Packages: Technical Reference

A technical reference covering module semantics, package structures, import resolution, and the roles of `__init__.py` and `__main__.py`.
## 1. Files as Modules

In Python, **every single `.py` file is automatically a module**. There is no `module` keyword or explicit declaration needed.

- **Module Name:** Derived directly from the file name. A file named `database.py` becomes a module named `database`.
- **Execution on Import:** The top-level code inside a `.py` file executes **once** the first time it is imported.
- **Namespaces:** Importing a module creates an isolated namespace. Global variables inside `database.py` do not pollute the importing file's scope.

Python

```python
# database.py
connection_string = "postgres://..."  # Top-level code executes on import

def connect():
    pass

# main.py
import database

database.connect()            # Accessed via module namespace
print(database.connection_string)
```

## 2. Entry Points & The `__name__ == "__main__"` Idiom

When Python executes a `.py` file, it automatically injects several special dunder variables into the module's global namespace, including `__name__`.

### Value of `__name__`

- **Executed Directly (`python script.py`):** Python sets `__name__ = "__main__"`.
- **Imported as a Module (`import script`):** Python sets `__name__ = "script"` (its actual module name).

### The Idiom

This pattern allows a file to act as **both** an importable library module and an executable standalone script:

Python

```python
# math_utils.py

def add(a, b):
    return a + b

# Executed ONLY if run directly via terminal, ignored when imported
if __name__ == "__main__":
    print("Running self-tests for math_utils...")
    assert add(2, 3) == 5
    print("Tests passed!")
```

## 3. Packages and `__init__.py`
A **Package** is simply a directory containing Python modules. It allows organizing modules into hierarchical namespaces (`package.submodule`).

```
my_package/
├── __init__.py
├── auth.py
└── database.py
```

### The Role of `__init__.py`

Including an `__init__.py` file marks a directory as a regular Python package. When the package or any of its submodules are imported, `__init__.py` **executes first automatically**.
#### Primary Uses of `__init__.py`:

1. **Exposing a Clean Public API (Re-exporting):** Allows consumers to import directly from the package root instead of deep nested paths.

```python
# my_package/__init__.py
from .auth import login
from .database import connect

# consumer.py (clean import path)
from my_package import login, connect
```

2. **Defining `__all__`:** Controls what gets imported when a user uses wildcard imports (`from my_package import *`).

Python

```python
# my_package/__init__.py
__all__ = ["login", "connect"]  # Only these symbols will be exported
```

3. **Package-Level Initialization:** Running setup code (configuring loggers, loading config variables) when the package is loaded.

> **Implicit Namespace Packages (PEP 420):** In Python 3.3+, directories _without_ an `__init__.py` are treated as "namespace packages." They allow a single package namespace to be split across multiple file-system directories or ZIP files. However, standard application packages should still always include `__init__.py`.  

## 4. Runnable Packages & `__main__.py`

While `__init__.py` runs when a package is **imported**, `__main__.py` executes when a package is **executed directly as a CLI or script**.

```
my_app/
├── __init__.py
├── __main__.py
└── server.py
```

### How `__main__.py` Operates

Executing a directory/package via CLI triggers `__main__.py`:

```bash
# Both of these commands execute my_app/__main__.py
python -m my_app
python my_app/
```

```python
# my_app/__main__.py
from .server import start_server

def main():
    print("Starting my_app CLI...")
    start_server()

if __name__ == "__main__":
    main()
```

### Standard Library / Tool Examples

- `python -m pip` $\rightarrow$ Executes `pip/__main__.py`
- `python -m http.server` $\rightarrow$ Executes `http/server.py` (or `http/__main__.py`)
- `python -m pytest` $\rightarrow$ Executes `pytest/__main__.py`
## 5. Import System & Path Resolution

### Absolute vs. Relative Imports

Plaintext

```
my_package/
├── __init__.py
├── utils/
│   ├── __init__.py
│   └── helpers.py
└── core/
    ├── __init__.py
    └── engine.py
```

- **Absolute Imports (Preferred):** Specifies the full path from the project root or `site-packages`.

```python
# Inside engine.py
from my_package.utils.helpers import format_date
```

- **Explicit Relative Imports:** Uses leading dots to reference modules relative to the current module's position.

```python
# Inside engine.py
from ..utils.helpers import format_date  # '..' goes up one level
from . import config                    # '.' refers to current directory
```

> **Warning:** Relative imports depend on the current module's `__name__`. If you attempt to execute a file containing relative imports directly as a script (`python my_package/core/engine.py`), Python will throw:
> 
> `ImportError: attempted relative import with no known parent package`
### How Python Finds Modules (`sys.path`)

When you write `import foo`, Python searches paths in `sys.path` in the following exact order:

1. The directory of the script being executed (or current working directory).
2. Directories listed in the `PYTHONPATH` environment variable.
3. Standard library modules.
4. Installed third-party packages inside `.venv/lib/python3.x/site-packages/`.

## 6. Module Caching (`sys.modules` & `__pycache__`)

### A. Singleton Caching (`sys.modules`)

Python modules are **cached singletons**. When a module is imported for the first time:

1. Python creates the module object and executes its top-level code.
2. Python stores the reference in `sys.modules` (a dictionary of loaded modules).
3. Any subsequent `import` statement for that module anywhere in the project simply returns the cached reference from `sys.modules` without re-executing the code.

### B. Bytecode Compilation (`__pycache__`)

When a Python file is imported, Python compiles its source code into intermediate **bytecode** (`.pyc` files) and stores it inside a `__pycache__/` folder.

- **Purpose:** Speeds up startup time on subsequent runs by skipping compilation if the `.py` source file hasn't modified.
- **Execution Speed:** Bytecode does **not** make Python execution faster at runtime; it only speeds up the module load time.

