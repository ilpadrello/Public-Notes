---
title: Error Handling in Python
---
# Python Error Handling: Technical Reference & Cheat Sheet

A concise technical reference for Python exception handling, highlighting key conceptual differences compared to the JavaScript/TypeScript paradigm.

## 1. The Full Structure (`try / except / else / finally`)

Unlike TypeScript's `try / catch / finally`, Python extends flow control with the **`else`** block.

  

Python

```python
try:
    # Code at risk of raising an exception (keep as atomic as possible)
    file = open("data.json", "r")
    data = json.load(file)

except (FileNotFoundError, json.JSONDecodeError) as err:
    # Targeted handling for specific exception types
    print(f"Read/parsing error: {err}")

except Exception as err:
    # Generic catch for unexpected errors
    print(f"Unhandled critical error: {type(err).__name__}")

else:
    # Executed ONLY IF the try block raised NO exceptions
    process_data(data)

finally:
    # Executed ALWAYS, regardless of success or failure (resource cleanup)
    file.close()
```

## 2. Block Granularity (Atomic `try` vs Monolithic `try`)

Python follows the principle _"Flat is better than nested"_. Wrapping an entire function in a massive `try` block is considered an anti-pattern.

  

- **Risks of monolithic `try` blocks:** If a syntax typo, misspelled variable (`NameError`), or unrelated failed call occurs deep inside a large `try` block, it will be mistaken for a business logic failure or silently caught by a generic `except`.
    
      
    
- **Atomic approach + `else`:** The `try` block should isolate **only** the specific I/O operation or instruction at risk. Any code that depends on the success of that operation belongs in the `else` block.

Python

```python
# ❌ BAD PRACTICE (Hides typos and unexpected bugs)
try:
    data = read_file()
    calculate_stats(data) # A NameError here gets caught by the same except!
except Exception:
    print("Error in file")

# ✅ GOOD PRACTICE (Atomic isolation)
try:
    data = read_file()
except IOError:
    print("I/O error while reading file")
else:
    calculate_stats(data) # Executed only if reading succeeded
```

## 3. Exception Classes as First-Class Objects

In Python, **everything is an object**, including classes themselves.

  

- **Type Identifiers:** In `except FileNotFoundError:`, `FileNotFoundError` is not a string or language flag, but a **direct reference to the class object** (inheriting from `BaseException`).
    
      
    
- **Custom Exceptions:** To create a valid exception type for `except`, define a class that inherits from `Exception`:
    
      
    

Python

```python
class BusinessLogicError(Exception):
    """Custom exception for domain logic errors."""
    pass

# Using 'except UndefinedError:' without declaring the class first
# raises a NameError at runtime initialization.
```

## 4. Exception Object Inspection & `traceback` Module

When an exception is caught using `except Exception as err:`, the `err` instance holds the entire execution failure state.

  

|**Property / Method**|**Description**|**JS/TS Equivalent**|
|---|---|---|
|`type(err).__name__`|Exception class name as a string|`err.constructor.name`|
|`str(err)`|Implicit invocation of `err.__str__()` (formatted message)|`err.message`|
|`err.args`|Tuple containing all positional arguments passed to the exception|N/A by default|
|`err.__traceback__`|Reference to execution stack frames|`err.stack` (raw string)|

### Analytical Stack Trace Extraction

Python

```python
import traceback

try:
    compute_value()
except Exception as err:
    # 1. Format full stack trace string (identical to terminal output, ideal for logging/alerts)
    full_stack_str = traceback.format_exc()
    
    # 2. Inspect exact line, file, and function of the primary cause
    tb = traceback.extract_tb(err.__traceback__)
    last_frame = tb[-1]
    
    file_path = last_frame.filename
    line_number = last_frame.lineno
    function_name = last_frame.name
    source_code = last_frame.line
```

## 5. The Role of `__str__()` and Multiple Arguments (`err.args`)

Unlike JavaScript where `new Error("msg")` primarily expects a single string, Python exception constructors accept an **arbitrary number of positional arguments**, stored in the `err.args` tuple.

  

Python

```python
err = ValueError("Resource not found", 404, "/api/v1/users")

print(err.args)        # Output: ('Resource not found', 404, '/api/v1/users')
print(err.args[1])     # Output: 404
```

### `__str__()` Behavior on Exceptions

The `__str__()` method (invoked via `str(err)` or `print(err)`) follows this logic:

  

1. **1 argument provided:** Returns the string conversion of that single argument (e.g., `"Resource not found"`).
    
      
    
2. **N arguments provided:** Returns the string representation of the entire `self.args` tuple (e.g., `"('Resource not found', 404, '/api/v1/users')"`).
    
      
    
3. **0 arguments provided:** Returns an empty string `""`.
    
      
    

## 6. Propagation, Re-raising, and Exception Chaining

### Uncaught Exception Behavior

If a raised exception does not match any `except` block:

  

1. Current block execution terminates immediately.
    
      
    
2. The exception **bubbles up the call stack** searching for handlers in outer scopes.
    
      
    
3. If it reaches the root scope uncaught, the process **terminates** and prints an `Uncaught Traceback`.
    
      
    

### Clean Re-raising (Bare `raise`)

Inside an `except` block, calling `raise` without arguments **re-raises the current active exception**, preserving its original stack trace and context intact.

  

Python

```python
try:
    execute_query()
except DatabaseError as err:
    log_error(err)
    raise  # Re-raises the exact same exception upwards
```

### Exception Chaining (`raise ... from ...`)

To transform a low-level error into a domain-specific exception while retaining the root cause context:

  

Python

```python
try:
    value = int(raw_input)
except ValueError as err:
    # Automatically links 'err' to the __cause__ attribute of the new exception
    raise CustomValidationError("Invalid input format") from err
```

## 7. Common Native Exceptions

|**Exception**|**Typical Context**|
|---|---|
|`ValueError`|Right type, but inappropriate value (e.g., `int("abc")`).|
|`TypeError`|Operation applied to an incompatible data type (e.g., `"a" + 2`).|
|`KeyError`|Key not found inside a dictionary (`dict`).|
|`IndexError`|Sequence index out of bounds.|
|`AttributeError`|Accessing an attribute or method that does not exist on the object.|
|`FileNotFoundError`|Attempting to open a non-existent file on the filesystem.|
|`ZeroDivisionError`|Division or modulo by zero.|
|`NameError`|Accessing a variable that has not been declared in the current scope.|

## 8. The Multiple Uses of the `as` Keyword

The `as` keyword in Python handles assignment and aliasing across three distinct scenarios:

1. **In `except` blocks:** Assigns the caught exception instance to a local variable (`except ValueError as err:`).
2. **In Context Managers (`with`):** Assigns the value returned by `__enter__()` to a variable (`with open("file.txt") as f:`).
3. **In `import` statements:** Creates an alias for an imported module or member (`import numpy as np` or `from datetime import datetime as dt`).