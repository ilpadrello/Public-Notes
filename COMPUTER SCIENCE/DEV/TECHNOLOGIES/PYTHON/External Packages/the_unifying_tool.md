---
title: The unifying tool
---
# The unifying tool
`uv` is built to handle both `requirements.txt` and `pyproject.toml` projects seamlessly side by side.

You do not need to convert your legacy projects. `uv` automatically adapts its behavior based on what configuration files exist in your project root.

**1. Legacy Projects (`requirements.txt`)**

For older repositories that don't have a `pyproject.toml`, `uv` provides a fast drop-in replacement for `pip` under the `uv pip` interface:

- **Create environment:** `uv venv`
- **Install dependencies:** `uv pip install -r requirements.txt`
- **Freeze dependencies:** `uv pip freeze > requirements.txt`
- **Run scripts:** `uv run python script.py`

**2. Modern Projects (`pyproject.toml`)**
For projects using `pyproject.toml`, you drop the `pip` keyword and use `uv`'s top-level project interface:

- **Install/Sync environment:** `uv sync`
- **Add new package:** `uv add requests`
- **Run scripts or entry points:** `uv run main.py`

**Bridging Both Formats**

If you need to bridge the gap between team members using different tools or deploy to systems that only accept `requirements.txt`, `uv` translates between them instantly:

|**Action**|**Command**|
|---|---|
|**Import legacy `requirements.txt` into `pyproject.toml`**|`uv add -r requirements.txt`|
|**Export `uv.lock` out to a standard `requirements.txt`**|`uv export --format requirements-txt -o requirements.txt`|
|**Compile locked `requirements.txt` from `.in` file**|`uv pip compile requirements.in -o requirements.txt`|

You can adopt `uv` as your global CLI immediately across all your WSL projects regardless of how old or new their configuration structure is.