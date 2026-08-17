---
title: Python Classes Decorators
---
# List:
- **@property**
- **@classmethod**
- **@staticmethod**
- **@dataclass**
- **@abstractmethod**
- **@cached_property**
- **@override** and **@final**

# @Property
In TypeScript, accessors use explicit `get` and `set` keywords. In Python, getters and setters are managed through the `@property` decorator and its resulting `@<attr>.setter` decorator.


```typescript
// TypeScript
class BankAccount {
  private _balance: number = 0;

  get balance(): number {
    return this._balance;
  }

  set balance(value: number) {
    if (value < 0) throw new Error("Invalid balance");
    this._balance = value;
  }
}

const acc = new BankAccount();
acc.balance = 100; // Triggers setter
console.log(acc.balance); // Triggers getter
```

```python
# Python
class BankAccount:
    def __init__(self, balance: int = 0):
        self._balance = balance  # Internal convention variable

    # 1. GETTER (Defines the property name)
    @property
    def balance(self) -> int:
        return self._balance

    # 2. SETTER (Must match the exact getter method name)
    @balance.setter
    def balance(self, value: int) -> None:
        if value < 0:
            raise ValueError("Invalid balance")
        self._balance = value

    # 3. DELETER (Python-specific feature for `del acc.balance`)
    @balance.deleter
    def balance(self) -> None:
        print("Resetting balance")
        del self._balance

acc = BankAccount()
acc.balance = 100        # Automatically invokes @balance.setter
print(acc.balance)       # Automatically invokes @property getter
del acc.balance          # Automatically invokes @balance.deleter
```

**Key Differences for TypeScript Developers**

- **Method Naming:** In Python, both the getter, setter, and deleter methods MUST share the exact same method name (`balance` in the example above).
- **The Decorator Chain:** The `@property` decorator turns the first method into a property object. That property object then exposes the `@<name>.setter` and `@<name>.deleter` decorators for the setter and deleter.
- **Deleters (`@<name>.deleter`):** TypeScript accessors have no concept of a deleter. In Python, intercepting `del object.attribute` allows for explicit cleanup or resets.
- **Read-Only Properties:** In TypeScript, omitting a setter makes a property read-only at compile time. In Python, defining only `@property` makes it read-only at runtime: attempting `acc.balance = 100` throws `AttributeError: can't set attribute`.

**The "Pythonic" Refactoring Advantage**

In TypeScript, developers often write preemptive getters and setters just in case validation logic is needed in the future.

In Python, the standard practice is to **start with plain public attributes**:

```python
class User:
    def __init__(self, name: str):
        self.name = name # Plain public attribute
```

If you later need validation, you can convert `name` to a `@property` without changing the external API. All existing caller code (`user.name = "Alice"`) continues to work seamlessly without refactoring.

# `@dataclass` (Data Modeling & Boilerplate Reduction)

In TypeScript, you often define data contracts with `interface` or `type`. In Python, defining a class to hold data requires writing boilerplate `__init__`, `__repr__`, and `__eq__` methods manually. `@dataclass` generates all of them automatically based on type annotations.

```python
from dataclasses import dataclass, field

@dataclass
class User:
    id: int
    name: str
    role: str = "guest"                          # Default value
    tags: list[str] = field(default_factory=list) # Mutable default factory

# Automatically generates:
# - __init__(self, id, name, role="guest", tags=None)
# - __repr__ -> "User(id=1, name='Alice', role='admin', tags=[])"
# - __eq__ (compares all field values, not memory references!)

u1 = User(1, "Alice", "admin")
u2 = User(1, "Alice", "admin")

print(u1 == u2)  # True (Value equality out of the box!)
```

# `@abstractmethod` (Abstract Classes & Contracts)

TypeScript has the built-in `abstract` keyword. Python uses the `abc` (Abstract Base Classes) module and the `@abstractmethod` decorator to enforce interfaces at runtime.

```python
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount: float) -> bool:
        """Subclasses MUST implement this method."""
        pass

class StripeProcessor(PaymentProcessor):
    def process_payment(self, amount: float) -> bool:
        print(f"Processing ${amount} via Stripe")
        return True

# Attempting to instantiate the abstract class directly throws TypeError:
# p = PaymentProcessor() -> TypeError: Can't instantiate abstract class PaymentProcessor
```

# `@cached_property` (Lazy Evaluation & Performance)

Like `@property`, but it computes the return value **only once** on first access and caches the result directly on the instance `__dict__`. Subsequent accesses run in $O(1)$ time without re-executing the method.

```python
from functools import cached_property
import time

class AnalyticsReport:
    def __init__(self, raw_data: list[int]):
        self.raw_data = raw_data

    @cached_property
    def summary_stats((self) -> dict:
        print("Computing expensive aggregation...")
        time.sleep(2)  # Simulating heavy computation or DB query
        return {"total": sum(self.raw_data), "avg": sum(self.raw_data) / len(self.raw_data)}

report = AnalyticsReport([10, 20, 30, 40])

# First access: executes method (~2 seconds)
print(report.summary_stats)

# Second access: retrieved instantly from instance cache
print(report.summary_stats)
```

# `@override` and `@final` (Type Checker Guardrails)

From the `typing` module, these behave identically to TypeScript's `override` and `readonly`/sealed patterns for static analysis tools like `mypy` or Pyright.

```python
from typing import override, final

class Parent:
    def fetch_data(self):
        pass

class Child(Parent):
    @override
    def fetch_data(self):  # Ensures method actually exists in Parent class
        print("Overridden implementation")

    @final
    def critical_security_check(self):
        # Prevents any subclass of Child from overriding this method
        pass
```