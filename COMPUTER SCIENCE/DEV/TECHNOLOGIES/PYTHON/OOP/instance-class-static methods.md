|**Method Type**|**First Parameter**|**Access Level**|**Primary Use Case**|**TypeScript Parallel**|
|---|---|---|---|---|
|**Instance Method**|`self`|Instance & Class state|Mutating or reading instance data|Standard method (`this.prop`)|
|**`@classmethod`**|`cls`|Class state only|Alternative / Factory constructors|Static factory (`User.fromJSON()`)|
|**`@staticmethod`**|_None_|Isolated (No `self` or `cls`)|Pure utility methods grouped inside a class namespace|Pure `static` utility function|

### Code Blueprint

```python
class User:
    species = "Homo sapiens"  # Class attribute

    def __init__(self, name: str, role: str):
        self.name = name      # Instance attribute
        self.role = role

    # 1. INSTANCE METHOD
    def get_profile(self) -> str:
        # Accesses both instance attributes (self.name) and class attributes (self.species)
        return f"{self.name} ({self.role}) - {self.species}"

    # 2. CLASS METHOD
    @classmethod
    def create_admin(cls, name: str) -> "User":
        # Receives class 'cls' dynamically (supports inheritance)
        return cls(name=name, role="admin")

    # 3. STATIC METHOD
    @staticmethod
    def is_valid_role(role: str) -> bool:
        # No access to self or cls. Pure function in a namespace.
        return role in ["admin", "editor", "guest"]


# Usage
user = User("Alice", "editor")

# Instance method call
print(user.get_profile())  # "Alice (editor) - Homo sapiens"

# Factory method using @classmethod
admin = User.create_admin("Bob")
print(admin.role)          # "admin"

# Utility call using @staticmethod
print(User.is_valid_role("admin"))  # True
```

### Crucial Mental Model Shifts for TS Developers

1. **No Constructor Overloading $\rightarrow$ Use `@classmethod`**
    
    TypeScript lets you overload constructor signatures. Python only permits a single `__init__` method per class. To support multiple ways of instantiating an object (e.g., `from_json()`, `from_dict()`, `from_db_row()`), Python uses `@classmethod` as alternative factory constructors.
    
1. **Inheritance Safety with `cls(...)`**
    
    In `@classmethod`, `cls` is passed dynamically at runtime. If a subclass `SuperUser(User)` inherits `create_admin()`, calling `SuperUser.create_admin("Alice")` receives `cls = SuperUser` and returns a `SuperUser` instance, not a base `User`.
    
2. **`static` in TS vs `@staticmethod` in Python**
    
    In TypeScript, inside a `static` method, `this` refers to the Class constructor function. In Python, `@staticmethod` receives **no implicit arguments whatsoever**. If your static method needs to access class attributes or call other class methods, use `@classmethod` instead.

## Better look at @class methods:

To understand `@classmethod`, it helps to compare how TypeScript and Python handle **alternative object creation** and **class-level behaviors**.

  

In TypeScript, you can overload the constructor signature or use static factory methods:


```typescript
// TypeScript
class User {
  name: string;
  role: string;

  constructor(name: string, role: string) {
    this.name = name;
    this.role = role;
  }

  // Static factory method in TS
  static fromJSON(jsonString: string): User {
    const data = JSON.parse(jsonString);
    return new User(data.name, data.role); // Explicitly names "User"
  }
}
```

In Python, **there is no constructor overloading**. You can only have one `__init__` per class. This is where `@classmethod` shines as the primary tool for alternative constructors, dynamic instantiation, and class-level state mutation.

### Key Use Cases for `@classmethod`

#### 1. Alternative Constructors (Factory Pattern)

Since `__init__` can only take one set of positional arguments, `@classmethod` gives you multiple clean ways to construct an object from different data sources (`from_dict`, `from_json`, `from_db_row`, `from_timestamp`).

```python
import json
from datetime import date

class User:
    def __init__(self, name: str, birth_year: int):
        self.name = name
        self.birth_year = birth_year

    # Factory 1: Construct from JSON string
    @classmethod
    def from_json(cls, json_str: str) -> "User":
        data = json.loads(json_str)
        # cls(...) expands to User(...) dynamically
        return cls(name=data["name"], birth_year=data["birth_year"])

    # Factory 2: Construct from age instead of birth year
    @classmethod
    def from_age(cls, name: str, age: int) -> "User":
        current_year = date.today().year
        return cls(name=name, birth_year=current_year - age)

# Usage
u1 = User("Alice", 1990)                             # Standard constructor
u2 = User.from_json('{"name": "Bob", "birth_year": 1995}') # Factory from JSON
u3 = User.from_age("Charlie", 30)                   # Factory from Age
```

#### 2. Polymorphic Inheritance Safety (Why `cls` Matters)

Why pass `cls` as the first argument instead of hardcoding `User(...)` inside the method? **Subclassing.**

  

When a subclass inherits a `@classmethod`, `cls` automatically refers to the **subclass at runtime**, creating instances of the derived class without overriding the factory method.

  

Python

```
class User:
    def __init__(self, name: str):
        self.name = name

    @classmethod
    def from_dict(cls, data: dict):
        # Using cls(...) instead of User(...)!
        return cls(name=data["name"])

class AdminUser(User):
    def __init__(self, name: str):
        super().__init__(name)
        self.is_admin = True

# Usage with inheritance:
data = {"name": "Dave"}

# User.from_dict receives cls = User -> returns User instance
regular_user = User.from_dict(data)
print(type(regular_user))  # <class '__main__.User'>

# AdminUser.from_dict receives cls = AdminUser -> returns AdminUser instance!
admin_user = AdminUser.from_dict(data)
print(type(admin_user))   # <class '__main__.AdminUser'>
print(admin_user.is_admin) # True
```

If `from_dict` had used `return User(name=data["name"])`, calling `AdminUser.from_dict(data)` would have mistakenly returned a base `User` instance, breaking polymorphism.

  

#### 3. Reading/Mutating Class-Level State

Because `cls` represents the class object itself, a `@classmethod` can inspect or alter class attributes shared across all instances.

  

Python

```
class Config:
    ENVIRONMENT = "development"
    ALLOWED_HOSTS = ["localhost"]

    @classmethod
    def set_production_mode(cls):
        cls.ENVIRONMENT = "production"
        cls.ALLOWED_HOSTS = ["api.example.com"]

# Change configuration globally across the entire application:
Config.set_production_mode()
print(Config.ENVIRONMENT)  # "production"
```

### `@classmethod` vs `@staticmethod` Summary

- **Use `@classmethod`** when the logic needs to create an instance of the class (`cls(...)`), access class attributes, or support inheritance safely.
    
      
    
- **Use `@staticmethod`** when the function is a pure utility that doesn't need information from either `self` (the instance) or `cls` (the class), but logically belongs inside the class namespace.