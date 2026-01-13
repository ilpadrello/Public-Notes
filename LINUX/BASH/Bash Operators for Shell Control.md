## 1️⃣ Pipe `|`

**Purpose:** take the **stdout of the left command** and feed it as **stdin to the right command**.

### Example:

`echo "hello world" | wc -w`

- `echo "hello world"` → prints `"hello world"`
- `|` → sends that output to `wc -w`
- `wc -w` → counts words → outputs `2`

**Key notes:**

- Each side of the pipe **runs in a subshell**
- Variables set in a pipe **do not propagate back to the parent shell**

---

## 2️⃣ Background `&`

**Purpose:** run a command **in the background**, asynchronously.

### Example:

`sleep 5 & echo "This prints immediately"`

- `sleep 5` → runs in the background
- Shell **does not wait**, immediately runs the next command

**Use case:** running multiple jobs at once, e.g., starting servers in a script.

---

## 3️⃣ AND `&&`

**Purpose:** run the next command **only if the previous command succeeded** (exit code 0).

### Example:

`mkdir /tmp/test && echo "Directory created"`

- If `mkdir` succeeds → `echo` runs
- If `mkdir` fails → `echo` is skipped

**Good for chaining commands safely.**

---

## 4️⃣ Redirect `>`

**Purpose:** write **stdout** to a file (overwrite).

### Example:

`echo "Hello" > file.txt`

- `file.txt` now contains `Hello`
- If the file existed → it is **overwritten**

---

## 5️⃣ Append `>>`

**Purpose:** write **stdout** to a file (append).

### Example:

`echo "Hello again" >> file.txt`

- Adds `"Hello again"` at the end of `file.txt`
- Existing content is **kept**

---

## 6 Redirect stderr `2>`

Redirect stderr **overwrite**

### Example: 
`ls /notexist 2> err.txt` Only error goes to file

---
## 6 Append stderr `2>>`

Redirect stderr **append**
### Example: 
`ls /notexist 2>> err.txt` Append errors at the end of the file


## ✅ Visual summary

| Operator | Purpose                             | Example                        | Notes               |
| -------- | ----------------------------------- | ------------------------------ | ------------------- |
| \|       | Pipe stdout → stdin                 |                                |                     |
| `&`      | Background                          | `sleep 5 &`                    | Runs asynchronously |
| `&&`     | AND, next runs if previous succeeds | `mkdir /tmp/test && echo "ok"` | Checks exit code    |
| `>`      | Redirect stdout, overwrite file     | `echo hi > file`               | File replaced       |
| `>>`     | Redirect stdout, append to file     | `echo hi >> file`              | File preserved      |

---

### Quick tip: combining them

You can mix them:

`grep "error" /var/log/syslog | tee errors.log >> all_logs.log &`

- `grep` → finds lines
- `| tee errors.log` → saves and prints
- `>> all_logs.log` → appends all output
- `&` → runs in background
- 
Combine `&&` and `||` to mimic a `if`

```bash
command && echo "success" || echo "fail"
```

