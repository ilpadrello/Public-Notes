---
title: How to make a singleton
---
### 1. The Classic OOP Approach (`__new__`)

In Python, `__new__` is the actual allocation method that creates the object in memory _before_ `__init__` initializes it. You can override `__new__` to intercept allocation and return an existing instance.

Python

```
class Database:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            # Allocate the single instance using superclass (object)
            cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        # NOTE: __init__ is called EVERY time Database() is invoked!
        # Use a guard if initialization logic must only run once.
        if not hasattr(self, "_initialized"):
            self.connection_string = "postgres://..."
            self._initialized = True

# Usage (looks like standard instantiation):
db1 = Database()
db2 = Database()

print(db1 is db2)  # True (same memory location)
```

> **Catch:** Even though `__new__` returns the cached instance, Python will still automatically call `__init__` on that instance every time you call `Database()`. That is why the `_initialized` guard flag is required inside `__init__`.
### 2. The "Pythonic" Way: Module-Level Singletons

In Python, **modules themselves are singletons**. When a module is imported anywhere in an application, Python executes it once and caches the resulting module object in `sys.modules`. Subsequent imports reuse that same cached instance.

```python
# db.py
class _DatabaseConnection:
    def __init__(self):
        self.connection_string = "postgres://..."

    def query(self, sql: str):
        print(f"Executing: {sql}")

# Instantiated ONCE when db.py is imported
db = _DatabaseConnection()
```

Python

```
# app.py
from db import db

db.query("SELECT * FROM users")
```

Because Python developers lean into module caching, creating an explicit Singleton class is often considered over-engineering in Python unless you specifically need subclassing or lazy instantiation.