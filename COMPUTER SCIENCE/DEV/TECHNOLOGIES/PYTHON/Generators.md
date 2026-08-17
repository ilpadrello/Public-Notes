# Python Generators & The Iterator Protocol: Technical Reference

A technical summary of Python generators, lazy evaluation, and the underlying Iterator Protocol.

  

## 1. Generator Functions vs. Generator Objects

In Python, defining a function with the `yield` keyword turns it into a **Generator Function**. Calling a generator function **does not execute its code immediately**; instead, it returns a **Generator Object**.

  

- **Generator Function (The Factory):** The function definition containing `yield`.
    
      
    
- **Generator Object (The Stream/Book):** The stateful iterator object returned when calling the generator function.
    
      
    

Python

```
def my_generator():
    yield "Page 1"
    yield "Page 2"

# Calling the function returns a Generator Object, code execution has NOT started yet
gen = my_generator() 
print(type(gen))  # <class 'generator'>
```

## 2. Execution Flow & Lazy Evaluation

Generators employ **lazy evaluation** (_call-by-need_). They compute values on demand rather than allocating memory for an entire collection upfront.

  

1. Execution starts only when `next(gen)` is invoked for the first time.
    
      
    
2. The code runs until it encounters a `yield` statement.
    
      
    
3. The function **pauses**, saves its entire execution state (local variables, stack position), and returns the yielded value.
    
      
    
4. Subsequent calls to `next(gen)` **resume** execution immediately after the `yield` statement.
    
      
    

Python

```
# Infinite sequence consuming O(1) RAM
def infinite_counter():
    count = 0
    while True:
        yield count
        count += 1
```

## 3. The Iterator Protocol: `next()` Under the Hood

The `next()` built-in function triggers the generator object's internal `__next__()` method.

  

### How `for` loops work under the hood

A standard Python `for` loop is syntactic sugar over an explicit `while` loop using `next()` and catching the `StopIteration` exception:

  

Python

```
# What you write:
for item in my_generator():
    print(item)

# What Python executes under the hood:
_iterator = iter(my_generator())  # Obtains the iterator object
while True:
    try:
        item = next(_iterator)    # Advances state and retrieves value
        print(item)
    except StopIteration:
        break                     # Clean exit when generator is exhausted
```

## 4. Single-Use Exhaustion & State Isolation

### A. Generator Objects are Single-Use

Once a generator object yields all its items and raises `StopIteration`, it is **exhausted**. Calling `next()` or running a `for` loop on an exhausted generator produces no items.

  

Python

```
gen = (x for x in [1, 2])
list(gen)  # [1, 2]
list(gen)  # []  (Exhausted!)
```

### B. Calling `iter(gen)` Returns the Same Instance

Calling `iter()` on an existing generator object returns **itself** (`iter(gen) is gen`). You cannot create two independent pointers from a single generator instance.

  

### C. Creating Independent Streams

To iterate over the same generated sequence twice, either:

  

1. **Re-invoke the Generator Function:** Creates a new generator instance with its own state.
    
      
    
2. **Use `itertools.tee(gen, n)`:** Duplicates an iterator into $n$ independent streams using an internal memory buffer.
    
      
    

## 5. What You Might Have Missed: Key Generator Concepts

### A. Consumers & Sinks

Any function or construct that requests items sequentially until completion consumes (drains) a generator:

  

|**Consumer Category**|**Examples**|**Behavior**|
|---|---|---|
|**Collection Constructors**|`list(gen)`, `tuple(gen)`, `set(gen)`, `dict(gen)`|Drains generator into RAM.|
|**Reductions / Aggregations**|`sum(gen)`, `max(gen)`, `min(gen)`|Processes elements in $O(1)$ extra space.|
|**Short-Circuiting Logics**|`any(gen)`, `all(gen)`|Drains **only** until condition is met, pausing execution.|
|**String Joining**|`" ".join(gen)`|Streams strings directly without intermediate list.|
|**Unpacking**|`print(*gen)`|Unpacks elements one by one into positional arguments.|

### B. Generator Expressions

A concise syntax for creating generators on the fly, using parentheses `()` instead of brackets `[]` (List Comprehension):

  

Python

```
# List Comprehension: Allocates full list in RAM immediately
list_comp = [x * 2 for x in range(1_000_000)]

# Generator Expression: Allocates O(1) memory, computes on demand
gen_exp = (x * 2 for x in range(1_000_000))
```

### C. Generator Pipelines (Chaining)

Generators can be chained together so that data flows item-by-item through processing stages without intermediate arrays:

  

Python

```
raw_lines = ("  alice,25  ", "  ", "  bob,17  ")

# Stage 1: Clean whitespace
cleaned = (line.strip() for line in raw_lines if line.strip())

# Stage 2: Parse CSV
parsed = (line.split(",") for line in cleaned)

# Stage 3: Convert types
users = ((name, int(age)) for name, age in parsed)

# Item-by-item execution: memory stays low regardless of stream size
for name, age in users:
    print(name, age)
```

## 6. `range()` vs. True Generators

`range()` in Python 3 is lazy, but it is **not** a generator object:

  

- **`range()` is a Lazy Sequence:** It supports `len()`, indexing (`range(10)[2]`), and multiple passes because it calculates values algorithmically on demand based on `(start, stop, step)`.
    
      
    
- **Generators are One-Shot Streams:** They do not support `len()` or indexing (`gen[0]`), because past values are discarded and future values do not exist in memory yet.
    

## Quick Reference Summary Table

|**Feature**|**Regular Function**|**Generator Function**|
|---|---|---|
|**Keyword**|`return`|`yield`|
|**Execution**|Runs to completion once invoked.|Pauses at `yield` and waits for `next()`.|
|**Return Value**|Computed data structure or `None`.|`generator` object.|
|**State Memory**|Frame destroyed on return.|Frame frozen in memory between yields.|
|**Memory Footprint**|$O(N)$ for $N$ items.|$O(1)$ constant RAM.|