---
title: understanding the problem
---
Python's dependency management feels fragmented compared to Node.js primarily because Python was created in 1991! 18 years before Node.js existed and at a time when package registries, lockfiles, and centralized package managers didn't exist in web development.

When Node.js arrived in 2009, `npm` was designed alongside it with a single, unified standard (`package.json`) from Day 1. Python, by contrast, had to evolve its package ecosystem incrementally over three decades while maintaining backward compatibility.

**The Evolution of Python Packaging**

| **Era**    | **Primary Tool / File**    | **Core Problem It Had**                                                                                                        |
| ---------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **1998**   | `setup.py`                 | Required executing arbitrary Python code just to read a project's dependencies or metadata.                                    |
| **2008**   | `pip` + `requirements.txt` | Handled downloading packages, but `requirements.txt` lacked lockfile determinism, environment management, or project metadata. |
| **2017**   | `Pipenv` + `Pipfile`       | Attempted to combine locking and environments, but suffered from severe performance and resolution bugs.                       |
| **2019**   | `Poetry` + `poetry.lock`   | Introduced clean `pyproject.toml` standards and deterministic locking, but ran into speed and standards-compliance issues.     |
| **Modern** | `uv` + `pyproject.toml`    | Built in Rust; unifies Python versioning, virtual environments, dependency resolution, and fast locking.                       |

**Why Python Fragmented**
1. **No Bundled Package Manager:** For many years, Python shipped without a built-in package manager. `pip` was only added to Python's standard distribution in 2014 (Python 3.4). Before that, community tools filled the vacuum in competing ways.
2. **C Extensions & Native Code:** Python is heavily used in data science, AI, and system tooling. Python packages often need to compile C, C++, or Fortran code on installation. Node's `node_modules` initially focused on pure JavaScript, making cross-platform resolution significantly simpler.
3. **The `requirements.txt` Trap:** `requirements.txt` was never a build system manifest—it was merely a list of arguments passed to `pip install`. Because it didn't separate top-level dependencies from transitive dependencies or lock hash signatures, developers had to build external tools to make installs reproducible.

**Where Python Stand Today**
The Python Steering Council introduced standards (**PEP 518** and **PEP 621**) to fix this fragmentation. Today, `pyproject.toml` is the official, universal standard manifest for all Python projects, playing the exact same role as `package.json`.

While different tools still use their own lockfile formats (`uv.lock`, `poetry.lock`, `pdm.lock`), tools like `uv` have effectively consolidated the developer workflow so you no longer need `pyenv`, `pip`, `virtualenv`, and `pip-tools` running separately.
