# Bandit Level 26 → Level 27

## Level Goal

Good job getting a shell! Now gain access to the next level...

---

## Background & Context

In the previous level (Bandit Level 25 → 26), we broke out of the restricted `/usr/bin/showtext` login shell using the `more` pager and the `vi` editor. That breakout dropped us into an interactive `/bin/bash` shell running as user `bandit26`.

Once inside the shell, the goal was to elevate privileges or execute a command as `bandit27` to retrieve the password for the next level.

---

## Inspecting the Environment

Upon listing the contents of `bandit26`'s home directory, an executable binary named `bandit27-do` was present:

```bash
ls -la /home/bandit26
```

Checking its permissions revealed the SUID bit was set:

```text
-rwsr-x--- 1 bandit27 bandit26 14864 May 2 2020 bandit27-do
```

### Understanding SUID

* **Owner:** `bandit27`
* **Group:** `bandit26`
* **Permissions (`-rwsr-x---`):** The `s` in the owner field indicates the **SUID (Set Owner User ID)** bit is active.

When an executable with SUID set is run, it executes with the privileges of the file's **owner** (`bandit27`) rather than the user running it (`bandit26`).

---

## Execution

The `bandit27-do` binary acts as a wrapper that runs any command passed to it as user `bandit27`. 

To read the password file for Level 27, I passed the `cat` command targeted at `/etc/bandit_pass/bandit27` as an argument to `bandit27-do`:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

---

## Output

```text
[REDACTED_BANDIT27_PASSWORD]
```

This output printed the password for Level 27.

---

## Commands Used

```bash
ls -la

./bandit27-do cat /etc/bandit_pass/bandit27
```

---

## What I Learned

### 1. SUID Binaries (`chmod u+s`)
SUID binaries allow users to temporarily execute specific commands with elevated privileges (in this case, running commands as `bandit27` while logged in as `bandit26`).

### 2. Privilege Wrapper Execution
Custom binaries structured like `bandit27-do` function similarly to `sudo`, executing user-supplied parameters under the security context of the binary's owner.

---

## Key Takeaway

> **Always inspect home directories for SUID binaries (`-rwsr-x---`), as they provide a direct mechanism to execute elevated commands across user boundaries.**

Execution flow:

```text
Interactive shell as bandit26
        ↓
Execute ./bandit27-do (SUID binary owned by bandit27)
        ↓
Executes: cat /etc/bandit_pass/bandit27 (as bandit27)
        ↓
Level 27 Password displayed
```