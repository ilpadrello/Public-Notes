---
title: OOP Basic Stuff
---
In Python, instance attributes are initialized dynamically inside the constructor using an explicit `self` reference, rather than being declared at the top of the class body.

Python

```python
class Person:
    # Public class attribute (shared across all instances, like static in TS)
    species = "Homo sapiens"

    # Constructor method (__init__)
    def __init__(self, name: str, ssn: str):
        # Public instance variable
        self.name = name
        
        # Private instance variable (double underscore triggers name mangling)
        self.__ssn = ssn
        
        # Protected instance variable (single underscore convention: "internal use")
        self._internal_id = 101

    # Public method
    def greet(self) -> str:
        self.__log_access("greet")
        return f"Hello, I'm {self.name}"

    # Private method
    def __log_access(self, action: str) -> None:
        print(f"[Audit] {self.name} performed {action}")
```

### Key Differences from TypeScript

- **Explicit `self` vs `this`:** Python requires `self` as the first parameter in every instance method declaration. When calling the method (`p.greet()`), Python passes the instance automatically.
- **Class Body vs `__init__`:** Variables declared directly inside the `class` body are **class attributes** (shared static values). Instance attributes MUST be attached to `self` inside `__init__` or another method to be independent.
- **Privacy by Convention:** Python lacks `public`, `private`, or `protected` keywords:  
    - **Public (`name`):** Accessible everywhere.
    - **Protected (`_internal_id`):** Single underscore is a universal convention telling developers "treat this as internal." Python will not stop external access.
    - **Private (`__ssn`):** Double underscore triggers **name mangling**. Python renames `p.__ssn` to `p._Person__ssn` under the hood to avoid collision during inheritance.

**What is Name Mangling?**
In TypeScript, `private` is checked by the compiler, while `#field` uses JS hard runtime privacy. Python takes a different approach: **it transforms the variable name under the hood.**

When Python sees an attribute starting with `__` (double underscore), it automatically prepends `_ClassName` to the attribute name.

```pyhon
class Account:
    def __init__(self, balance: int):
        self.__balance = balance  # Mangled to _Account__balance

acc = Account(100)

# Trying to access it directly FAILS:
# print(acc.__balance)  
# -> AttributeError: 'Account' object has no attribute '__balance'

# But accessing the mangled name WORKS:
print(acc._Account__balance)  # 100
```

**Why does Python do this?**
Name mangling isn't designed to stop someone from reading private data (Python's philosophy is "we are all consenting adults here"). It exists to **prevent accidental collisions during inheritance**.

```pyhon
class Parent:
    def __init__(self):
        self.__id = "parent_123"  # Saved as _Parent__id

class Child(Parent):
    def __init__(self):
        super().__init__()
        self.__id = "child_456"   # Saved as _Child__id

c = Child()
# Without mangling, Child's __id would silently overwrite Parent's __id!
# With mangling, both co-exist safely on the object:
print(c._Parent__id)  # "parent_123"
print(c._Child__id)   # "child_456"
```
## **Class Attributes & The Assignment Trap**

Yes, class attributes are shared across all instances, but reassigning them on an instance creates a common trap for developers coming from TypeScript.


```pyhon
class User:
    role = "guest"       # Class attribute (shared)
    roles_list = []      # Mutable class attribute (shared)

u1 = User()
u2 = User()

# Scenario 1: Reassigning via an instance (SHADOWING)
u1.role = "admin"        # Python creates an *instance* attribute on u1!
print(u1.role)           # "admin" (from u1's instance dict)
print(u2.role)           # "guest" (still reading from User.role)
print(User.role)         # "guest"

# Scenario 2: Reassigning via the Class itself
User.role = "member"
print(u2.role)           # "member"
print(u1.role)           # "admin" (u1's instance attribute still shadows it)

# Scenario 3: Mutating an in-place object (list/dict)
u1.roles_list.append("editor")
print(u2.roles_list)     # ["editor"] - both pointing to the same memory address!
```

- **Reassigning via instance (`u1.attr = val`):** Python doesn't mutate the class attribute. It creates a brand-new instance attribute on `u1` that **shadows** (hides) the class attribute for `u1`.
- **Mutating in place (`u1.list.append()`):** Because `u1.list` resolves to the shared class list in memory, modifying it affects all instances.
