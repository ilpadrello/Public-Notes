This is part of the bash Operators, but I put it here to better understand it.

These are three **ways to feed input to a command**, but they work differently.

---

## **A. `<` — redirect a file as stdin**

`sort < file.txt`

- The command reads **from `file.txt` instead of keyboard**
- Equivalent to: `sort file.txt` (for most commands)
- Useful for commands that **always read stdin**

---

## **B. `<<` — here-document (multi-line inline input)**

`cat <<EOF Hello World EOF`

- Everything between `<<EOF` and the ending `EOF` is **fed as stdin** to the command
- Multi-line input **embedded directly in the script**
- Common in scripting for config generation

`grep foo <<EOF foo bar foobar EOF`

- `grep foo` sees `foo\nbar\nfoobar` as input

---

## **C. `<<<` — here-string (single string as stdin)**

`grep foo <<< "hello foo world"`

- Feeds the **string** as stdin to the command
- Handy for **short inline strings** without a file

`wc -w <<< "hello world"`

- Counts words in the string `"hello world"` → outputs `2`
- Basically a shorthand for `echo "..." | command`, but **no subshell** involved

## Why this end-of-command syntax ? Why don't just pipe ?

# **1️⃣ Using `< file` vs `cat file | command`**

### Example:

`sort < file.txt`

vs

`cat file.txt | sort`

Both **work**, but there are subtle differences.

---

## **A. Performance & efficiency**

- `sort < file.txt` → **single process** (`sort`) reads directly from file
- `cat file.txt | sort` → **two processes**:
    1. `cat` reads file
    2. `sort` reads from `cat`
- For **small scripts**, the difference is negligible.
- For **big files or many processes**, `<` is **faster and lighter**.

---

## **B. Avoiding useless `cat` (“UUOC”)**

- Shell gurus call this the **UUOC**: **Useless Use Of Cat**
- Example:

`cat file.txt | grep foo`

- Can be rewritten as:

`grep foo < file.txt`

- Cleaner, less overhead, no extra process.

---

## **C. Redirection is POSIX idiomatic**

- Many commands are written to **read stdin if no filename is given**
- Using `<` is considered **canonical shell scripting style**
- Pipes are **better for chaining commands**, not feeding a single file

---

## **D. Subshell / pipe caveats**

- `cat file.txt | while read line; do ...; done`
    
    - The `while` loop often runs in a **subshell**
        
    - Variables set inside are lost outside
        
- `<` avoids this problem:
    

`while read line; do     LAST="$line" done < file.txt echo "$LAST"  # works fine`

---

## **E. When to use a pipe**

- When **output of one command** is feeding **another command**:

`grep foo file.txt | sort | uniq`

- Here, `|` is required because the **first command generates dynamic data**.

---

### ✅ TL;DR

|Approach|When to use|Pros|Cons|
|---|---|---|---|
|`< file.txt`|Single command reading a file|Efficient, no extra process, no subshell|Only works with commands that read stdin|
|`cat file.txt|command`|Chaining or when file name not accepted|Useful for multiple commands in a chain|

---

### **Rule of thumb**

- **Single command → `< file`**
- **Command chain → `|`**

> This is why shell scripts (prefer `<` for loops, redirection, and `<<` for here-documents — it’s **safe, predictable, and fast**.