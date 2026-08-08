Hello,
Someone asked me if I can develop in python..
I know TS, how different are Python and JS ? Beside syntax ?

If you already know TypeScript (TS) and JavaScript (JS), you can absolutely learn Python—and quickly. The mental models for how code runs, how variables behave, and how applications are structured are incredibly similar.

The short answer to "Can you develop in Python?" is: **"Yes, I have a strong foundation in TypeScript/JavaScript, and the core programming concepts translate directly. I can get up to speed on Python's specific ecosystem within a few days."**

Beyond the obvious syntax differences (like Python using indentation instead of curly braces `{}`), here is how they stack up under the hood where it actually matters for a developer.

## 1. The Type Systems (Where you'll feel it most)

Since you know TypeScript, you are used to **Static Typing** (errors are caught at compile-time). JavaScript uses **Dynamic Typing** (errors happen at runtime).

- **Python is Dynamically but _Strongly_ Typed:** Like JS, variables can change types at runtime. However, unlike JS, Python will _not_ silently try to coerce types. It fails fast.
    
- **Type Hinting:** Modern Python uses a feature called "Type Hinting" that looks a lot like TypeScript. Tools like `mypy` can check these types before runtime, giving you a very TS-like experience.
    

JavaScript

```
// JavaScript (Weakly typed - silently coerces)
"5" - 3; // Returns 2
"5" + 3; // Returns "53"
```

Python

```
# Python (Strongly typed - throws an explicit TypeError)
"5" - 3  # TypeError: unsupported operand type(s) for -: 'str' and 'int'
"5" + 3  # TypeError: can only concatenate str (not "int") to str
```

## 2. Concurrency: Event Loop vs. Threading

How the languages handle heavy lifting or asynchronous tasks is fundamentally different.

- **JavaScript / TS:** Built from day one around an **Event Loop**. It is single-threaded and non-blocking by default. `async/await` is the bread and butter of almost all JS applications (especially in Node.js).
    
- **Python:** Historically relies on multi-threading or multi-processing. It has a notorious mechanism called the **GIL (Global Interpreter Lock)**, which prevents multiple native threads from executing Python code at the same time.
    
    - _Note:_ Python _does_ have modern `async/await` syntax (via the `asyncio` library) that mimics the JS event loop, but it was bolted on later and isn't used universally across every library.
        

## 3. Scope and `this`

Managing variable scope and object context is a notorious headache in JS, but much more straightforward in Python.

- **No `this` puzzles:** Python doesn't have a confusing, context-shifting `this` keyword. Instead, Python object methods explicitly receive the instance as the first argument, conventionally named `self`.
    
- **Block Scope:** JS uses `let` and `const` for block scoping (inside an `if` statement or `for` loop). Python does **not** have block scope; variables declared inside an `if` block or loop are still accessible outside of it, as long as you remain within the same function.
    

## 4. Ecosystem & Tooling Equivalents

When switching to Python, you aren't just learning a language; you're learning an ecosystem. Here is a quick translation guide for the tools you use every day:

|JavaScript / TypeScript Ecosystem|Python Equivalent|Notes|
|---|---|---|
|**npm / yarn / pnpm**|`pip` (or `poetry`, `pixi`)|Package managers. `poetry` behaves closest to npm.|
|**package.json**|`requirements.txt` or `pyproject.toml`|Where dependencies are declared.|
|**Node.js / Bun**|CPython (The standard Python runtime)|The engine executing your code.|
|**Express / NestJS**|**FastAPI** or **Flask**|FastAPI is incredibly popular for TS devs because it uses Python Type Hints natively.|
|**Prettier / ESLint**|`black` / `ruff`|`ruff` is an incredibly fast, modern linter/formatter written in Rust.|

## The Verdict

The hardest part of software engineering is learning _how to think like a developer_—how to structure logic, design APIs, handle databases, and solve algorithmic problems. You already know how to do that.

If you take the gig, start by looking into **FastAPI** for backend development. Because it relies heavily on Python's type hints, it will feel incredibly natural coming from a TypeScript background.

Want a quick cheat sheet on how to translate JS Async/Await or Arrays into Python?

Oui

Can you tell me more about the difference for the event loop vs threading ?

This is one of the most interesting architectural divides in backend engineering. It dictates how you write code, how your application scales, and where it bottlenecks.

At a high level: **JavaScript avoids concurrency by juggling tasks incredibly fast on a single track, while Python (traditionally) runs tasks down multiple physical tracks simultaneously.**

Here is exactly how they handle the work under the hood.

## 1. JavaScript / TypeScript: The Master Juggler (Event Loop)

Node.js is completely single-threaded. It has exactly **one main thread** to execute your JavaScript code.

Instead of waiting around when it hits a slow task (like reading a file or making a database query), JavaScript hands that task off to the underlying operating system and moves instantly to the next line of code. When the OS finishes the slow task, it drops a message into a queue. The **Event Loop** constantly checks this queue and executes the callback (or resolves the Promise) when the main thread is free.

```
[ Call Stack (Main Thread) ] ---> Executes code line-by-line
        |
        +---> Slow I/O Task? ---> Hand off to OS / File System
                                           |
[ Event Loop ] <--- Polling <--- [ Callback Queue ] <--- (Task Done!)
```

- **The Big Benefit:** Incredibly cheap context switching. Because there's only one thread, Node.js can handle tens of thousands of concurrent database or API connections simultaneously without breaking a sweat or consuming massive amounts of memory.
    
- **The Achilles' Heel:** **CPU-heavy tasks.** If you calculate a massive mathematical equation or resize an image on the main thread, the entire server freezes. The event loop cannot tick, and every other user waiting for a response gets blocked.
    

## 2. Python: The Assembly Line (Threading & The GIL)

Python handles multitasking differently. By default, when you want to do two things at once, you spin up a **new operating system thread**. Each thread has its own execution state.

However, Python has a massive architectural quirk called the **GIL (Global Interpreter Lock)**. The GIL is a master lock that ensures **only one thread can execute Python bytecode at any given microsecond**.

```
Thread 1 (Trying to run) ---\
Thread 2 (Trying to run) ----+---> [ GIL (Only 1 wins) ] ---> CPU Executes
Thread 3 (Trying to run) ---/
```

- **For I/O Tasks (Web Requests/Databases):** Threads work perfectly. When Thread 1 hits a database query, it voluntarily releases the GIL. Thread 2 grabs the lock and starts running. To you, it looks like simultaneous multitasking.
    
- **For CPU Tasks (Data Science/Machine Learning):** Threading is useless because of the GIL. If you spin up 4 threads to process a massive dataset, they will fight over the lock, run sequentially, and actually perform _slower_ due to the overhead of switching back and forth.
    
    - _How Python solves this:_ For true CPU concurrency, Python bypasses threading entirely and uses **Multiprocessing**—literally launching entirely separate instances of the Python interpreter, each with its own memory space and its own GIL.
        

## Where Modern Python Meets JavaScript

To make things more interesting, modern Python (via the `asyncio` library) actually added its own built-in **Event Loop** that uses `async` and `await` just like JavaScript.

If you use a modern Python framework like **FastAPI**, you can write code that looks identical to Node.js:

Python

```
# Modern Python (FastAPI) - Non-blocking Event Loop
@app.get("/users")
async def get_users():
    users = await database.fetch_all()  # Yields to the event loop
    return users
```

## Summary: Which one wins where?

- **Choose JS / Async Python** for network-heavy apps (chat apps, REST APIs, microservices). They handle thousands of open connections using minimal memory because they don't spin up heavy threads for each user.
    
- **Choose Multi-process Python** for data manipulation, heavy calculations, or machine learning. It can actually leverage all the cores of your CPU at the exact same time.
    

Want to see a code comparison of how Node.js handles a heavy CPU crash vs how Python prevents it?

Oui

Is it necessary to use the asyncio lib with fastAPI ? Or is it possible to do the same thing without this library ? I'm worried that, since is a library, and not native architectural way, it is slow to use this library

It is **not necessary** to explicitly import or manually manage the `asyncio` library yourself when using FastAPI. FastAPI handles all the event loop orchestration behind the scenes.

Furthermore, you can write standard, synchronous Python code (without `async/await`) inside FastAPI, and it will still handle multiple requests concurrently.

Regarding your performance concern: **`asyncio` is not a slow, third-party add-on—it is a core part of Python's standard library** (built directly into the language runtime, just like the event loop is built into Node.js).

Here is exactly how FastAPI handles both approaches, and why you don't need to worry about performance.

## How FastAPI Handles Sync vs. Async (No Imports Needed)

FastAPI gives you total flexibility. You can define your API routes in two different ways, and FastAPI treats them completely differently under the hood to ensure your app stays fast.

### Approach A: The JavaScript Way (`async def`)

If you define your route with `async def`, FastAPI runs it directly on its main event loop—exactly like Node.js.

Python

```
@app.get("/data")
async def get_data():
    # FastAPI runs this on the main event loop.
    # It expects you to use 'await' for non-blocking I/O tasks.
    result = await call_database() 
    return result
```

### Approach B: The Traditional Way (`def`)

If you drop the `async` keyword, FastAPI doesn't just block your server. Instead, it automatically hands this function off to an internal **thread pool**.

Python

```
@app.get("/data")
def get_data():
    # FastAPI says: "Oh, this is synchronous code."
    # It automatically moves this execution off the main loop into a separate thread.
    result = call_blocking_database_or_api() 
    return result
```

This means if you don't want to deal with `async/await` syntax yet, you don't have to! You can just write standard Python functions (`def`), and FastAPI safely prevents them from freezing your server by running them in threads.

## Why `asyncio` Isn't a Slow Wrapper

It is completely natural to worry that a library layer might introduce lag, but `asyncio` is an official built-in module optimized at the language level.

In fact, FastAPI doesn't even use the standard Python event loop implementation directly. Under the hood, it uses an incredibly fast server gateway called **Uvicorn**, which uses **`uvloop`**—a drop-in replacement for `asyncio` written in **Cython/C**.

> **Performance Reality:** Because of `uvloop`, FastAPI's async architectural layer matches or sometimes even outperforms Node.js in raw I/O throughput benchmarks.

## The Rule of Thumb for a TS Developer in FastAPI

Since you are coming from TypeScript, you don't need to change your mental model at all. You can use `async def` exactly like you would use `async function` in JS.

The only rule to remember: **Only use `async def` if the libraries you are using inside that function support `await`.**

- If you are using a modern async database driver (like `asyncpg` for Postgres) → Use **`async def`**.
    
- If you are using an older, blocking database driver (like standard `psycopg2` or `sqlite3`) → Use standard **`def`** and let FastAPI handle the threading for you.
    

So If understand correctly :

I can just not worry about async await, fast api will do that for me (and since not all libraries uses those it is bettere this wat).

  

There are two way for running concurrent code:

1 The internal Lock: inven in multi-threading, the lock lets only one "code" at the time and if there is something that would "lock" the code, it release the lock for other code to run.

  

2 The internal loop event like JS, that let me decide when I want to be async or not.

  

Am I correct ?

You have grasped the core concepts beautifully. You are **95% of the way there**, and your practical takeaway about FastAPI is spot on.

Let's do a quick alignment on the details to make sure you have a perfect mental model.

## 1. Your FastAPI Takeaway: Spot On ✓

> _"I can just not worry about async await, fastapi will do that for me (and since not all libraries use those it is better this way)."_

**Exactly.** This is a brilliant practical approach. If you just use standard `def`, FastAPI spins up a thread pool behind the scenes. This is incredibly safe because 90% of traditional Python libraries (like standard database connectors or file readers) are synchronous. If you used `async def` with a blocking library, you would accidentally freeze the whole server. Sticking to `def` avoids that trap entirely.

## 2. The Internal Lock (Threading): 90% Correct

> _"In multi-threading, the lock lets only one 'code' at a time and if there is something that would 'lock' the code, it releases the lock for other code to run."_

You understand the behavior perfectly, but let's tweak one word so you don't say the wrong thing in an interview:

- **The Correction:** It's not when code "locks" that it releases the GIL; it's when the code **waits (or blocks)** on something outside of Python (like waiting for a database to reply, a file to read, or a network request to finish).
    
- **How it works:** While Python is waiting for the hard drive or internet, the thread literally has nothing to do. So, it **releases the lock** so another thread can use the CPU.
    

## 3. The Event Loop (The JS Way): 100% Correct

> _"The internal loop event like JS, that let me decide when I want to be async or not."_

**Spot on.** When you use `async def` in FastAPI, you are explicitly telling the engine: _"I am taking control of the steering wheel. I will tell you exactly when to pause and switch tasks by using the `await` keyword."_

### The Cheat Sheet Summary for Your New Project

If you build an app tomorrow, here is your decision matrix:

|If the library you are using...|Write your FastAPI function as...|Under the hood, FastAPI will...|
|---|---|---|
|Is traditional / synchronous (e.g., standard `requests`, `boto3`, old ORMs)|`def my_route():`|Launch it in a separate thread. Safe and concurrent.|
|Is modern / asynchronous (e.g., `httpx`, `asyncpg`, `tortoise-orm`)|`async def my_route():`|Run it directly on the fast Event Loop.|

You've got a fantastic handle on this. You are definitely ready to tell that person you can develop in Python!

Cool, thank you...

I have more questions for you ! (sorry I'm curious)...

How do I have to tell Python that "this code will take time to respond" ?

For example, a lib that call the database, how would the lib say that ?

Never apologize for being curious—this is exactly how the best engineers learn!

The way a library tells Python "this is going to take a moment, go do something else" depends entirely on whether you are using the **Event Loop** approach (`async def`) or the **Threading** approach (`def`).

Here is how the library communicates that behind the scenes in both worlds.

## 1. In the Async World: The `await` keyword

In an asynchronous library, the code uses a mechanism called a **Generator** under the hood.

When a database library wants to send a query over the network, it doesn't just send it and sit there. It uses the `yield` or `return` control mechanism via the **`await`** keyword.

Python

```
# Inside a modern async database library (simplified)
async def fetch_user_data(user_id):
    # 1. Prepare the SQL query
    query = f"SELECT * FROM users WHERE id = {user_id}"
    
    # 2. Tell the Event Loop: "I am sending this to the network. Pause me here!"
    # The 'await' keyword is the literal signal to Python to pause this function.
    socket_response = await network_socket.send_and_receive(query)
    
    # 3. When the network responds, the Event Loop wakes this function back up
    return parse_json(socket_response)
```

**How Python knows:** The `await` keyword is the explicit signal. It tells Python's event loop: _"Pause this specific function right here, bookmark our spot, and go run other code. Wake us up when the database responds on this network socket."_

## 2. In the Sync/Threading World: System Calls (C-Extensions)

If you are using a traditional, synchronous library (no `async/await`), how does the library tell Python to release the GIL (Global Interpreter Lock) so other threads can run?

It happens at the **Operating System level**, usually written in C.

Most heavy-lifting Python libraries (like database drivers or file readers) are actually wrappers around fast C code. When a library makes a network call to a database, it makes an operating system call (a "system call").

C

```
/* Inside a traditional C-based Python database driver (like psycopg2) */
Py_BEGIN_ALLOW_THREADS  // <--- This is the magic signal!

/* 
   The C code now tells the Python interpreter: 
   "Hey, I am about to wait for the database over the network. 
   I am officially releasing the GIL so other Python threads can use the CPU."
*/

status = connect_to_postgres_database(connection_string);

Py_END_ALLOW_THREADS    // <--- The task is done. Block until we can grab the GIL back.
```

**How Python knows:** The library developers explicitly wrap their slow network/file operations in special macros (`Py_BEGIN_ALLOW_THREADS`). This tells the Python runtime: _"This thread is about to go to sleep waiting for the internet. Take the lock away from us and give it to another thread."_

## Summary

- **In Async (`async def`):** **You and the library** explicitly tell Python by writing the word `await`.
    
- **In Sync (`def`):** **The library authors** wrote low-level C code that automatically tells Python, _"We are waiting on I/O, give the CPU lock to someone else."_
    

This is why, in your FastAPI app, you don't have to manually tell Python anything! The libraries you import have already done the heavy lifting for you.

Ok, understood...

In a way this is true also for Javascript :D

  

Ok, so next question :

You told me, that if I want to do 4 cpu heavy work at the same time, Python will create 3 more Python code executioners and will run the code.

  

If I do not use async await, do I have to tell python to do that manually ?

how does that work ?

Yes, exactly! You have to tell Python to do this manually.

If you just write standard synchronous code inside a `def` function in FastAPI, FastAPI will spin up **threads** to handle concurrency. But remember: because of that internal lock (the GIL), **threads cannot do CPU-heavy work at the same time.** They will just queue up and wait for each other, running one after the other.

To get true, simultaneous CPU-heavy processing on all 4 cores of your processor, you have to explicitly tell Python to use **Multiprocessing** (creating those separate executioners).

Here is exactly how you write that code in Python, and how it works under the hood.

## The Code: How you tell Python manually

Python has a built-in module called `multiprocessing`. The easiest and most modern way to use it is with a tool called `ProcessPoolExecutor` from the `concurrent.futures` library.

Here is how you would use it inside a FastAPI endpoint:

Python

```
from concurrent.futures import ProcessPoolExecutor
from fastapi import FastAPI

app = FastAPI()
# 1. Create a pool of separate Python "executioners" (processes)
# This stays alive and ready to do heavy work
executor = ProcessPoolExecutor(max_workers=4)


# This is the heavy math function (CPU-bound)
def heavy_calculation(number):
    # Imagine a massive loop or data processing here
    result = sum(i * i for i in range(number))
    return result


@app.get("/compute")
def run_compute():
    # 2. Tell Python to ship this heavy work off to the separate executioners
    # .submit() hands the function and arguments to one of the 4 processes
    future1 = executor.submit(heavy_calculation, 10_000_000)
    future2 = executor.submit(heavy_calculation, 20_000_000)

    # 3. Get the results when they are done
    result1 = future1.result()
    result2 = future2.result()

    return {"result1": result1, "result2": result2}
```

## How it works behind the scenes (The Pipeline)

When you run `executor.submit()`, Python does something completely different from JavaScript or standard threading:

1. **Cloning:** The main Python process literally "forks" or spawns entirely new, separate copies of the Python interpreter into your operating system's memory.
    
2. **No Lock Sharing:** Each of these new processes gets its _own_ memory space and its _own_ GIL. Because they aren't sharing a lock, the operating system can spread them across your CPU's physical cores.
    
3. **Serialization (The Catch):** Because they don't share memory, the main process has to take your inputs (like the number `10_000_000`), convert them into binary data (a process called _Pickling_ in Python), and send them across a hidden pipeline to the worker process.
    
4. **Execution & Return:** The worker process does the heavy math using 100% of a CPU core, converts the answer back to binary, and shoots it back down the pipeline to your main FastAPI app.
    

## How this compares to JavaScript / Node.js

Since you know JS, this `ProcessPoolExecutor` is the exact equivalent of **Worker Threads** (specifically using the `worker_threads` module) or `child_process.fork()` in Node.js.

Just like Node.js needs cluster modules or worker pools to leverage multiple CPU cores for heavy computation, Python needs multiprocessing.

The biggest takeaway for you: **FastAPI handles standard thread concurrency automatically, but it will never automatically split heavy CPU tasks into multiple processes. For that, you use `ProcessPoolExecutor`.