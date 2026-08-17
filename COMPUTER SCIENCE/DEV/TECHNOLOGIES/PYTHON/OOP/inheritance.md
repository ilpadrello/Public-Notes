---
title: Inheritance
---
Python supports **multiple inheritance** (`class Child(ParentA, ParentB)`). To handle lookup order and prevent duplicate method execution in complex hierarchies, Python uses **Method Resolution Order (MRO)** powered by the C3 Linearization algorithm.

|**Feature**|**TypeScript**|**Python**|
|---|---|---|
|**Inheritance Model**|Single class inheritance (`extends`)|Multiple class inheritance (`class Child(ParentA, ParentB)`)|
|**Lookup Mechanism**|Single prototype chain|C3 Linearization Algorithm (MRO)|
|**`super()` Target**|Strictly points to the single parent class|Points to the **next class in the MRO chain** (dynamically resolved)|
|**Inspection**|N/A (Types erased at compile time)|`Class.mro()` or `Class.__mro__` at runtime|

**The Diamond Problem & Cooperative `super()`**
In Python, `super()` does **not** simply mean "call my parent"—it means **"call the next class in this object's MRO sequence."**


```python
class Base:
    def action(self):
        print("Base")

class A(Base):
    def action(self):
        print("A entry")
        super().action()
        print("A exit")

class B(Base):
    def action(self):
        print("B entry")
        super().action()
        print("B exit")

class C(A, B):  # Inherits from BOTH A and B
    def action(self):
        print("C entry")
        super().action()
        print("C exit")
```

**Inspecting MRO for Class `C`:**

```python
print(C.mro())
# Output: [<class 'C'>, <class 'A'>, <class 'B'>, <class 'Base'>, <class 'object'>]
```

When you execute `c = C()` and call `c.action()`, Python walks through the linear MRO chain step by step:

Python

```
c = C()
c.action()

# Output:
# C entry
# A entry
# B entry    <-- super() inside A called B, NOT Base!
# Base
# B exit
# A exit
# C exit
```

**Why `super()` in Class `A` Called Class `B`**

When `super().action()` runs inside class `A`:

1. Python checks the MRO of the instance `c` (`C -> A -> B -> Base -> object`).
    
2. The class _after_ `A` in the sequence is `B`.
    
3. Therefore, `super()` inside `A` delegates execution directly to `B.action()`.

This design pattern is called **Cooperative Multiple Inheritance**. For it to work safely, all methods sharing a name in a hierarchy should maintain matching parameter signatures and consistently invoke `super()`.

### The order in which you list parent classes in the class declaration directly determines their precedence in the Method Resolution Order (MRO).

When you change the definition from `class C(A, B)` to `class C(B, A)`, Python alters the C3 linearization sequence accordingly:

Python

```
class C(B, A):  # B is listed BEFORE A
    def action(self):
        print("C entry")
        super().action()
        print("C exit")
```

### Updated MRO Chain

Inspecting `C.mro()` now yields: `[C, B, A, Base, object]`

### Updated Execution Flow

Python

```
c = C()
c.action()

# Output:
# C entry
# B entry    <-- super() in C delegates to B first
# A entry    <-- super() in B delegates to A
# Base       <-- super() in A delegates to Base
# A exit
# B exit
# C exit
```

### The Rule of Thumb

Python evaluates inheritance left-to-right:

1. **Left-to-Right Priority:** Subclasses and earlier listed parents take precedence over later listed parents.
    
2. **Depth vs Breadth:** Python ensures a common ancestor (like `Base`) is never evaluated before its derived children (`A` or `B`), regardless of declaration order.