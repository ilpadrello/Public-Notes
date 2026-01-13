
## Summary (TL;DR)

- Generate keys with: `ssh-keygen -t ed25519 -f ~/.ssh/servers/vps_ed25519`
- Keep the **private key** secret
- Copy the **public key** to servers
- Prefer **ed25519**
- Use subfolders + SSH config for sanity (also mental)
## What are SSH keys ?

SSH keys are used for **authentication** — proving who you are — without sending a password.

SSH keys rely on **asymmetric cryptography**:

- A **private key** (secret)
- A **public key** (shared)

The private key is used to decrypt the message, while the public key is used to encrypt the message.

---

## 3. The command to generate SSH keys

### The modern, recommended command

`ssh-keygen -t ed25519 -f ~/.ssh/vps_ed25519 -C "vps access key"`

### What this does

- Generates a **key pair**
- Stores it in a **custom filename**
- Uses the ed25519 (a **modern cipher**)
- Adds a comment for identification

You will be prompted for:
- An optional **passphrase** (recommended) that will be asked each time you want to use the key.

---

## Explanation of the main flags

### `-t` → key type (cipher)

`-t ed25519`

Specifies the cryptographic algorithm.

---

### `-f` → output file

`-f ~/.ssh/vps_ed25519`

Controls where the key files are stored.

This produces:

`vps_ed25519        (private) vps_ed25519.pub    (public)`

---

### `-C` → comment

`-C "vps access key"`

A human-readable label.  
Useful when you manage multiple keys.

---

## Storing keys in subfolders (recommended)

You can organize keys by purpose:

`mkdir -p ~/.ssh/servers ssh-keygen -t ed25519 -f ~/.ssh/servers/vps_ed25519 -C "debian vps"`

Result:

`~/.ssh/servers/vps_ed25519 ~/.ssh/servers/vps_ed25519.pub`

This keeps things clean and avoids confusion.

---

## Registering the key with SSH (client-side)

If you use non-default paths, tell SSH which key to use.

### Option A — SSH config (best)

Edit `~/.ssh/config`:

`Host my-vps     HostName 203.0.113.10     User allacatalla     IdentityFile ~/.ssh/servers/vps_ed25519`

Now you can connect with:

`ssh my-vps`

---

### Option B — one-time command

`ssh -i ~/.ssh/servers/vps_ed25519 allacatalla@203.0.113.10`

---

## Installing the public key on the server

### Automatic (recommended)

`ssh-copy-id -i ~/.ssh/servers/vps_ed25519.pub allacatalla@server_ip`

This appends the key to:

`~/.ssh/authorized_keys`

---

### Manual (when needed)

On the server:

`mkdir -p ~/.ssh chmod 700 ~/.ssh nano ~/.ssh/authorized_keys chmod 600 ~/.ssh/authorized_keys`

Paste the **content** of `.pub` (one line).

---

## Which ciphers can I use (and which should I)?

### Recommended today (2025)

#### ✅ **ed25519** — BEST DEFAULT

`ssh-keygen -t ed25519`

**Why**

- Strong security
- Fast
- Short keys
- Resistant to implementation mistakes

This is the **best choice for almost everyone**.

---

### ⚠️ RSA — legacy, still acceptable with large keys

`ssh-keygen -t rsa -b 4096`

**Use only if**

- You need compatibility with very old systems

Downsides:

- Larger keys
- Slower
- More error-prone historically

---

### ❌ DSA / ECDSA

Avoid unless you know exactly why you need them.

---

## 9. Passphrase: should you use one?

### Yes, unless:

- The key is **strictly ephemeral**
- Used inside a locked-down automation environment

A passphrase:

- Encrypts the private key at rest
- Protects you if the file is stolen

You can combine this with `ssh-agent` to avoid retyping it.

---
