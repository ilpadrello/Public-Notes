# Python Virtual Environments (`.venv`): Technical Reference

A technical reference covering virtual environment isolation, activation mechanisms, module execution flags, and editable package installations.

## 1. Why You Need a `.venv` Folder (Dependency Isolation)

In Python, installing packages globally (`/usr/lib/python3.x/site-packages` or `/usr/local/lib`) creates a single shared environment across your entire system. A local `.venv` directory isolates dependencies per project.

- **Prevents Version Conflicts:** Allows Project A to use `requests==2.25.0` while Project B uses `requests==2.31.0` on the same machine without collisions.
    
- **Avoids Breaking System Tools:** Many operating systems rely on system-level Python packages. Modifying global packages can break OS utilities (governed by [PEP 668](https://peps.python.org/pep-0668/) and `EXTERNALLY-MANAGED` flags).
    
- **Reproducibility:** Keeps project dependencies self-contained so you can commit a `pyproject.toml` or `requirements.txt` reflecting exact dependency trees.
    
- **No Root/Sudo Required:** Packages are written to a user-owned local directory (`.venv/lib/python3.x/site-packages`), eliminating the need for `sudo pip install`.
    

## 2. Activating the Environment (`source .venv/bin/activate`)

### Why `source` is Required

Running a script as `./.venv/bin/activate` creates a **child process subshell**. Any environment variables modified inside that child process disappear as soon as the script finishes executing.

Using `source` (or `.` in POSIX shells) executes the script commands directly inside your **current shell session**.

Bash

```
# macOS / Linux
source .venv/bin/activate

# Windows (PowerShell)
.venv\Scripts\Activate.ps1
```

### What `activate` Does Under the Hood

1. **Prepends to `$PATH`:** Places `.venv/bin` at the front of your shell's `PATH` environment variable. When you type `python` or `pip`, the shell hits `.venv/bin/python` first.
    
2. **Sets `VIRTUAL_ENV`:** Sets `VIRTUAL_ENV="/path/to/project/.venv"` so tools (IDEs, linters, prompt themes) know an environment is active.
    
3. **Modifies Shell Prompt (`PS1`):** Prepends `(.venv)` to your terminal prompt.
    
4. **Unsets `PYTHONHOME`:** Clears global Python override variables if present.
    

### How Python Locates Packages

When `.venv/bin/python` executes, Python checks the path of its own binary (`sys.executable`). Because it resides inside `.venv/bin/`, Python automatically sets `sys.path` to search `.venv/lib/python3.x/site-packages` for imports—even without running `source`.

## 3. Why Use the `-m` Flag (`python -m <module>`)

The `-m` (module-name) flag searches `sys.path` for the specified module name and executes its `__main__.py` file as a script.

Bash

```
# Explicit module execution
python -m pip install requests
python -m venv .venv
python -m pytest
```

### The "Which Pip?" Ambiguity Trap

Executing `pip install` directly relies entirely on your shell's current `$PATH` lookup:

Bash

```
# ❌ RISKY: Which pip is this running?
pip install requests
```

If your shell environment isn't activated correctly, `pip` might resolve to `/usr/bin/pip` or a global Python installation, silently installing packages outside your `.venv`.

### Why `python -m pip` is Safe

Running `python -m pip` explicitly ties execution to the exact Python binary instance invoked:

Bash

```
# ✅ SAFE: Guarantees packages install into .venv
.venv/bin/python -m pip install requests
```

|**Syntax**|**Execution Mechanics**|**Target Environment**|
|---|---|---|
|`pip install <pkg>`|Uses whatever `pip` binary the shell `$PATH` finds first.|Ambiguous / System-dependent|
|`python -m pip install <pkg>`|Uses the `pip` library bound to the explicit `python` binary.|Guaranteed local `.venv`|

## 4. The `-e` Flag: Editable Mode (`pip install -e .`)

> **Clarification:** The `-e` flag belongs to **`pip install`** (`pip install -e .`), not `python -m venv`.

When developing a Python project or library locally, you want installed dependencies and binary entry points inside `.venv`, but you need your source code to remain in your project root directory.

Bash

```
# Create the virtual environment
python -m venv .venv

# Activate it
source .venv/bin/activate

# Install the current directory project in Editable mode
pip install -e .
```

### How `pip install -e .` Works Under the Hood

Instead of copying your package's source code into `.venv/lib/python3.x/site-packages/`, pip creates a **direct link** (via a `.pth` path configuration file or an import hook):

1. **Source Code Location:** Your `.py` source files stay in your project root (`./src/my_package` or `./my_package`).
    
2. **Package Linking:** Pip writes a `.pth` file inside `.venv/lib/python3.x/site-packages/` pointing to your local project folder path.
    
3. **Binaries & Entry Points:** Executable CLI scripts defined in `pyproject.toml` (e.g., `[project.scripts]`) are compiled and installed into `.venv/bin/`.
    
4. **Dependencies:** External libraries listed in `dependencies` are downloaded and installed into `.venv/lib/python3.x/site-packages/`.
    

### Why This Matters for Development

- Any changes you make to your local Python source files take effect **immediately** without running `pip install .` again.
    
- Executables and CLI tools created by your package run within the context of `.venv/bin/`.
    

## Summary Command Sequence

Bash

```
# 1. Create virtual environment
python3 -m venv .venv

# 2. Activate environment in current shell session
source .venv/bin/activate

# 3. Upgrade pip using the explicit module flag
python -m pip install --upgrade pip

# 4. Install local package in editable mode with dependencies
pip install -e .
```