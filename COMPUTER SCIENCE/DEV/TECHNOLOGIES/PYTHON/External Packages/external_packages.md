---
title: External Packages
---
Coming from Node.js, the biggest shift is that environment isolation isn't implicit. While Node automatically resolves packages locally from `node_modules`, Python defaults to a shared system environment—meaning you explicitly isolate dependencies per project using virtual environments.

|**Node.js Concept**|**Python Equivalent**|**Standard Tool**|
|---|---|---|
|**Package Registry**|PyPI (Python Package Index)|`pypi.org`|
|**Project Config**|`package.json`|`pyproject.toml`|
|**Lock File**|`package-lock.json` / `pnpm-lock.yaml`|`uv.lock` or `poetry.lock`|
|**Local Dependencies**|`node_modules/`|`.venv/` (Virtual Environment)|
|**Global CLI Runner**|`npx`|`uvx` (or `pipx`)|
|**Package Manager**|`npm` / `pnpm` / `yarn`|**`uv`** (modern default) or `poetry` / `pip`|

**Key Differences to Keep in Mind**
- **Explicit Environments (`.venv`):** Python stores project dependencies inside a local `.venv` folder. Rather than Node searching folder trees for `node_modules`, Python relies on executing scripts via the environment's interpreter so packages don't collide with system Python.
- **Unified Modern Tooling (`uv`):** Historically, Python was fragmented—you used `pyenv` for Python versions, `venv` for virtual environments, `pip` for packages, and `pip-tools` for locking dependencies. Today, **`uv`** (built in Rust) handles Python versioning, virtualenvs, lockfiles, and package management in a single fast tool that feels almost identical to `pnpm` or `bun`.
- **Config Standardization (`pyproject.toml`):** Modern Python uses `pyproject.toml` as its single config manifest for dependencies, dev tooling (formatters, linters), and build configuration.

**Workflow Comparison**

Bash

```
# Node.js (pnpm workflow)
pnpm init
pnpm add express
pnpm run start

# Modern Python (uv workflow)
uv init my-app
uv add fastapi
uv run main.py
```

# UV
Uv is the tool that let you handle your python code as easy as possible : 

**1.Install uv in WSL :**Run directly in your WSL terminal.

Install `uv` using the official installation script:

Bash

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart your shell or reload your environment so the `uv` binary is available in your `PATH`:

Bash

```
source $HOME/.cargo/env # or source ~/.bashrc
```

**2.Create and enter a new project directory :**

Initialize a new project folder. `uv init` sets up the basic directory structure, creates a `pyproject.toml` configuration file, and creates a sample `hello.py` file:

Bash

```
uv init my-project
cd my-project
```

**3.Add a package (e.g., Requests or FastAPI) :**

Add your dependencies. `uv` will automatically create a `.venv` virtual environment if one doesn't exist, install the package, and create/update `uv.lock`:

Bash

```
uv add requests
```

**4.Run your Python script :**

Use `uv run` to execute your script inside the project's virtual environment without needing to manually source or activate `.venv`:

Bash

```
uv run hello.py
```

**Useful Commands for Node.js Users**

- **Add dev dependencies:** `uv add --dev pytest` _(equivalent to `npm i -D`)_
- **Install dependencies from existing lockfile:** `uv sync` _(equivalent to `npm ci` or `pnpm i`)_
- **Run a global tool without installing:** `uvx ruff check .` _(equivalent to `npx`)_

## Project.toml
This is like the package.json file of node.js, you keep here de dependencies, the dev dependencies and also the scripts that you want to use for the project.

*Not Simply a Python format*: 
`pyproject.toml` is **not unique to `uv`**—it is the official standard configuration file for the entire modern Python ecosystem (defined in Python standards PEP 518 and PEP 621).

Just like `package.json` is shared across `npm`, `pnpm`, `yarn`, and `bun`, `pyproject.toml` is used across virtually all modern Python package managers, linters, and build tools.

**How `pyproject.toml` works across tools**

1. **Standardized Section (`[project]`):** Universal metadata defined by Python standards. Any modern tool reads this section to find your dependencies, Python version requirements, and CLI scripts (`project.scripts`).
    
2. **Tool-Specific Sections (`[tool.<name>]`):** Allows individual tools to store custom configurations inside the exact same file without conflicting.
    

**Tools That Share `pyproject.toml`**

|**Category**|**Tools using pyproject.toml**|
|---|---|
|**Package / Project Managers**|`uv`, `Poetry`, `PDM`, `Hatch`, `Flit`, `Setuptools`|
|**Formatters & Linters**|`Ruff`, `Black`, `isort`|
|**Testing & Quality Tools**|`pytest`, `mypy`, `coverage`|

For example, a single `pyproject.toml` can hold configuration for multiple tools simultaneously:

Ini, TOML

```
[project]
name = "hello-world"
version = "0.1.0"
dependencies = ["requests>=2.30.0"]

[tool.uv]
# uv-specific settings live here

[tool.pytest.ini_options]
# pytest settings live here

[tool.ruff]
# Ruff linter settings live here
```

**What IS unique to each package manager?**

The **lock file**. While `pyproject.toml` is universal, each tool uses its own lock file format:

- **`uv`** uses `uv.lock`
- **Poetry** uses `poetry.lock`
- **PDM** uses `pdm.lock`

# Pyproject.toml
No, **`pyproject.toml` is not just for `uv`**—it is the universal standard configuration file for the entire modern Python ecosystem, defined in official Python standards (specifically PEP 518 and PEP 621).

It plays the exact same role as `package.json` in Node.js, which is shared across `npm`, `pnpm`, `yarn`, and `bun`.

### How standard vs. tool-specific sections work

`pyproject.toml` uses namespaces so multiple tools can use the exact same file without interfering with each other:

1. **Standard `[project]` section:** Any modern Python tool (including `uv`, `poetry`, `pdm`, `hatch`, or `pip`) reads this standard section for project metadata, dependencies, and CLI scripts.
2. **`[tool.*]` sections:** Each tool gets its own dedicated section for custom configuration.

```toml
# Standard section — read by ANY Python tool/package manager
[project]
name = "hello-world"
version = "0.1.0"
dependencies = [
    "requests>=2.31.0",
]

[project.scripts]
hello-world = "hello_world:main"

# Tool-specific sections
[tool.uv]
dev-dependencies = ["pytest"] # UV settings

[tool.ruff]
line-length = 88              # Ruff linter settings

[tool.pytest.ini_options]
testpaths = ["tests"]         # Pytest test runner settings
```

### Comparison across ecosystems

|**Feature**|**Node.js**|**Modern Python**|
|---|---|---|
|**Universal Manifest**|`package.json`|`pyproject.toml`|
|**Supported Tools**|`npm`, `pnpm`, `yarn`, `bun`|`uv`, `poetry`, `pdm`, `hatch`, `pip`|
|**Formatters/Linters config**|`.eslintrc`, `prettier.config.js` or `package.json`|`pyproject.toml` (`[tool.ruff]`, `[tool.black]`)|
|**Test Runner config**|`jest.config.js` or `package.json`|`pyproject.toml` (`[tool.pytest.ini_options]`)|
### What _is_ unique to each tool?

The **lock file**. Just like `npm` uses `package-lock.json` and `pnpm` uses `pnpm-lock.yaml`, Python package managers each maintain their own lock file format:

- **`uv`** creates `uv.lock`
- **Poetry** creates `poetry.lock`
- **PDM** creates `pdm.lock`

If you switch from `uv` to another tool in the future, your `pyproject.toml` will still work—you would only need to generate that tool's specific lock file!