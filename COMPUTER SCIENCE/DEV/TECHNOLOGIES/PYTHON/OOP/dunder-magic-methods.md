---
title: Dunder (Magic) Methods
---

Dunder (double underscore) methods allow custom classes to hook directly into Python's native syntax and language features—such as string formatting, operator overloading, array indexing, and context management.

Unlike TypeScript, where operator overloading does not exist and custom iteration relies on `[Symbol.iterator]`, Python exposes these hooks as special method names surrounded by double underscores (`__name__`).

**1. String Representation: `__repr__` vs `__str__`**

In TypeScript, objects rely on `.toString()`. Python separates user-facing strings from developer debugging strings:

- **`__repr__` (Developer):** Unambiguous representation used in logs, debuggers, and REPLs. Ideologically, it should look like valid Python code to recreate the object.
- **`__str__` (User):** Readable representation triggered by `print(obj)` or `str(obj)`. If omitted, Python falls back to `__repr__`.


```python
class Money:
    def __init__(self, amount: float, currency: str):
        self.amount = amount
        self.currency = currency

    def __repr__(self) -> str:
        return f"Money(amount={self.amount!r}, currency={self.currency!r})"

    def __str__(self) -> str:
        return f"{self.amount:.2f} {self.currency}"

m = Money(42.5, "EUR")
print(str(m))   # "42.50 EUR"   (__str__)
print(repr(m))  # "Money(amount=42.5, currency='EUR')" (__repr__)
```

**2. Operator Overloading**
Python lets you define how operators (`+`, `-`, `==`, `<`, etc.) behave when applied to your objects.

```python
class Money:
    def __init__(self, amount: float, currency: str = "EUR"):
        self.amount = amount
        self.currency = currency

    # Overloading `+` (m1 + m2)
    def __add__(self, other: "Money") -> "Money":
        if not isinstance(other, Money) or self.currency != other.currency:
            raise ValueError("Cannot add different currencies or non-Money objects")
        return Money(self.amount + other.amount, self.currency)

    # Overloading `==` (m1 == m2)
    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Money):
            return False
        return self.amount == other.amount and self.currency == other.currency

    # Overloading `<` (m1 < m2)
    def __lt__(self, other: "Money") -> bool:
        return self.amount < other.amount

m1 = Money(10, "EUR")
m2 = Money(20, "EUR")

print(m1 + m2)   # 30.00 EUR  (invokes __add__)
print(m1 == m2)  # False     (invokes __eq__)
print(m1 < m2)   # True      (invokes __lt__)
```

_Common Operator Dunders:_

  

- `__add__` (`+`), `__sub__` (`-`), `__mul__` (`*`), `__truediv__` (`/`)
- `__eq__` (`==`), `__ne__` (`!=`), `__lt__` (`<`), `__gt__` (`>`)

**3. Container & Sequence Emulation**
You can make any custom object act like a list, dictionary, or iterable by implementing sequence dunders:

```python
class Inventory:
    def __init__(self, items: list[str]):
        self._items = items

    # Enables len(inventory)
    def __len__(self) -> int:
        return len(self._items)

    # Enables bracket lookup: inventory[0]
    def __getitem__(self, index: int) -> str:
        return self._items[index]

    # Enables 'for item in inventory:'
    def __iter__(self):
        return iter(self._items)

    # Enables 'if "apple" in inventory:'
    def __contains__(self, item: str) -> bool:
        return item in self._items

inv = Inventory(["apple", "banana", "cherry"])

print(len(inv))            # 3
print(inv[1])              # "banana"
print("apple" in inv)      # True

for item in inv:           # Iterates smoothly!
    print(item)
```

**4. Callable Objects: `__call__`**
Implementing `__call__` lets an instance of a class be invoked as if it were a function `obj()`. This is useful for stateful functions or strategy pattern instances.

```python
class Multiplier:
    def __init__(self, factor: int):
        self.factor = factor

    def __call__(self, x: int) -> int:
        return x * self.factor

double = Multiplier(2)
print(double(5))  # 10 (instance called like a function!)
```

**5. Context Managers: `__enter__` & `__exit__`**
Powers Python's `with` statement for resource setup and automatic cleanup (like DB connections, file handles, or mutex locks).

```python
class Timer:
    def __enter__(self):
        import time
        self.start = time.perf_counter()
        return self  # Value assigned to 'as' target

    def __exit__(self, exc_type, exc_val, exc_tb):
        import time
        elapsed = time.perf_counter() - self.start
        print(f"Elapsed time: {elapsed:.4f} seconds")

# Automatic setup on enter, cleanup on exit:
with Timer():
    sum(range(1_000_000))
```

