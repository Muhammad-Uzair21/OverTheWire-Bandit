# Bandit Level 32 → Level 33

## Level Goal

After all the Git challenges, this level tests restricted shell breakout techniques. Upon logging in via SSH, the user is dropped into a restricted shell environment that prevents normal command execution. The goal is to break out of this environment to read the password for Level 33.

---

## Understanding the Task

When logging into `bandit32`, the default login shell is a binary called `uppershell`. This restricted shell automatically converts any command typed by the user into uppercase characters before execution (e.g., `ls` becomes `LS`, `cat` becomes `CAT`), causing all standard shell commands to fail with "command not found".

---

## Where I Got Stuck & Technical Questions

### 1. Command Case Conversion
Standard Unix commands are lower-case sensitive. Because `uppershell` converted all inputs to uppercase, standard syntax could not be executed directly.

### 2. Identifying Shell Escape Vectors
To break out of `uppershell`, I needed an execution vector that doesn't rely on standard lowercase commands. Using the positional parameter `$0` invokes the underlying executing binary/shell context directly before `uppershell` processes or converts the string.

---

## The Solution Strategy

1. Log into `bandit32` via SSH.
2. Execute `$0` at the `WELCOME TO THE UPPERCASE SHELL` prompt to spawn standard `/bin/sh`.
3. Verify effective user privileges (`bandit33`) and read the password file located at `/etc/bandit_pass/bandit33`.

---

## Execution

### Step 1: SSH into Bandit 32

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

### Step 2: Escape the Uppercase Shell

At the `>>` prompt presented by `uppershell`:

```text
WELCOME TO THE UPPERCASE SHELL
>> $0
$
```

Running `$0` spawned a standard Bourne shell (`/bin/sh`) prompt (`$`).

### Step 3: Verify User and Read Password

```bash
$ whoami
bandit33

$ cat /etc/bandit_pass/bandit33
```

---

## Output

```text
[REDACTED_BANDIT33_PASSWORD]
```

This output successfully printed the password for Level 33.

---

## Commands Used

```bash
# Escape the restricted shell
$0

# Verify privilege
whoami

# Retrieve password
cat /etc/bandit_pass/bandit33
```

---

## What I Learned

### 1. Positional Parameter Zero (`$0`)
In shell environments, `$0` evaluates to the name or path of the shell/program currently being executed. Invoking `$0` inside a wrapper or restricted script executes the parent shell directly, bypassing input processing logic.

### 2. Restricted Shell Vulnerabilities
Custom restricted shells (like `uppershell`) that run setuid/setgid elevated privileges must strictly sanitize environment variables, parameter expansion, and child process execution paths. Failing to do so allows simple breakouts into full interactive shells.

---

## Key Takeaway

> **Restricted shells can often be escaped by leveraging shell variables like `$0` or built-in handlers that bypass input transformation filters.**

Data pipeline:

```text
SSH into bandit32
        ↓
Land in uppershell (converts input to UPPERCASE)
        ↓
Type $0 (Executes parent /bin/sh directly)
        ↓
Gain interactive $ shell as bandit33
        ↓
cat /etc/bandit_pass/bandit33
        ↓
Level 33 Password retrieved
```