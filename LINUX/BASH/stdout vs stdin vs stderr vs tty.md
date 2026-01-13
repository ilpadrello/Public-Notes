# **The 3 standard streams**

Every Unix process has **3 standard streams**, plus sometimes a **tty**:

|Stream|File descriptor|Purpose|
|---|---|---|
|**stdin**|`0`|Standard **input** — where the process reads data from|
|**stdout**|`1`|Standard **output** — normal output of the process|
|**stderr**|`2`|Standard **error output** — error messages, diagnostics|

> Think of `stdin` as "where input comes from," `stdout` as "where normal output goes," `stderr` as "where errors go."

---

## **A. stdin (Standard Input)**

- Default: the **keyboard** if you run interactively
- Can be redirected: `< file.txt`
- Example:

`# reads from keyboard (interactive) read -p "Name? " NAME echo "Hello $NAME"  # reads from file read NAME < file.txt`

---

## **B. stdout (Standard Output)**

- Default: **your terminal screen**
- Can be redirected: `>` or `>>`
- Example:

`echo "hello world" > output.txt  # write to file ls / > files.txt                 # save directory listing`

- Pipe `|` also reads **stdout** of left-hand command

`ls / | wc -l  # stdout of ls becomes stdin of wc`

---

## **C. stderr (Standard Error)**

- Default: also your **terminal screen**
- Used for **errors or diagnostics**
- Can be redirected separately from stdout:

`ls /notexist        # prints error to terminal (stderr) ls /notexist 2> err.txt  # save error messages to a file ls / > files.txt 2> err.txt  # save normal output separately`

- Can merge with stdout:
    

`ls /notexist &> all.txt  # both stdout and stderr to file`

---

# **2️⃣ TTY (Teletype / Terminal)**

- **Not a stream** per se, but a **device**
- Usually `/dev/tty`
- Represents the **actual terminal** of the user
- Used when you want to interact with the user **even if stdin is redirected**

Example:

`read -p "Password: " PASS < /dev/tty`

- Even if your script is reading input from a file or pipe, `/dev/tty` ensures the prompt goes to the **real terminal**
    

Another example: `age` asks for passphrase:

`Enter passphrase (leave empty to autogenerate a secure one):`

- It reads from **tty**, not stdin, so you can’t just `echo "mypassword" | age` — unless you use `expect` or `--passphrase-file`.

---

# **3️⃣ How they connect in scripts**

- `< file` → stdin
- `>` → stdout
- `2>` → stderr
- `|` → stdout of left command → stdin of right command
- `/dev/tty` → direct terminal input/output

---

### **Visual mental model**

```
		 +------------------+ 
stdin <--|   process        |--> stdout 
stderr --|                  | 
tty <----|                  |
         +------------------+
```

- **stdin** = data read
- **stdout** = normal output
- **stderr** = error output
- **tty** = user’s terminal, can override stdin/stdout

---

# **4️⃣ Examples**

```bash
# stdout vs stderr 
ls / > out.txt        # normal output in file 
ls /notexist 2> err.txt  # errors in file  
# merge them 
ls / &> all.txt  
# use tty for secure prompt 
read -s -p "Password: " PASS < /dev/tty
```

---

### ✅ TL;DR

- `stdin` = where input comes from
- `stdout` = normal output
- `stderr` = error messages
- `tty` = the **actual terminal** (bypasses stdin/stdout redirection)
- Pipes `|` connect **stdout → stdin**
- Redirections `<, >, >>, 2>` control where streams go

