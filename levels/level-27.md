# Bandit Level 27 → Level 28

## Level Goal

There is a git repository at `ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo` via port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`. 

From your local machine, clone the repository and find the password for the next level.

---

## Understanding the Task

This level transitions from standard shell privilege escalation to **version control inspection**. 

The goal requires cloning a remote Git repository hosted on the OverTheWire server onto a local machine via SSH on a non-standard port (`2220`), then inspecting the repository history for the password.

---

## Where I Got Stuck & Technical Questions

### 1. Specifying Custom SSH Ports in Git
Standard Git syntax (`git clone user@host:path`) uses the default SSH port `22`. When cloning over SSH with a non-standard port (like `2220`), standard Git syntax throws a connection error unless formatted properly.

I needed to use explicit SSH URL syntax:
```text
ssh://user@host:PORT/path/to/repo
```

---

## Execution

### Step 1: Create a Local Working Directory

On my local machine, I created a temporary directory to keep things organized:

```bash
mkdir -p ~/bandit_levels/level27
cd ~/bandit_levels/level27
```

### Step 2: Clone the Remote Repository

I ran `git clone` using the full SSH URL including port `2220`:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

When prompted for the password, I entered the password for user `bandit27` obtained in the previous level.

### Step 3: Inspect the Cloned Repository

After cloning, I moved into the repository folder:

```bash
cd repo
ls -la
```

Inspecting `README.md` contained placeholder text:

```text
The password to the next level is: REAL_PASSWORD_IS_NOT_HERE
```

### Step 4: Examine Git Commit History

To see what changes were made in previous commits, I used `git log -p` to view the full patch diffs across commit history:

```bash
git log -p
```

---

## Output

```text
commit 07e47efd037149a0d844c803099908cf02476b70
Author: Official Bandit <bandit@overthewire.org>
Date:   Thu May 7 20:14:54 2020 +0000

    initial commit

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..7333642
--- /dev/null
+++ b/README.md
@@ -0,0 +1,6 @@
+The password to the next level is: [REDACTED_BANDIT28_PASSWORD]
```

The diff revealed the initial commit where the actual password for Level 28 was originally added before being modified or overwritten.

---

## Commands Used

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo

cd repo

git log -p
```

---

## What I Learned

### 1. Cloning Repositories Over Non-Standard SSH Ports
When SSH is running on a port other than 22, Git requires explicit URL syntax (`ssh://user@host:PORT/path`) or setting `core.sshCommand="ssh -p PORT"`.

### 2. Inspecting Commit History with Diffs (`git log -p`)
* `git log` shows the commit history log (messages, authors, dates, hashes).
* The `-p` (or `--patch`) flag displays the exact file changes (diffs) introduced in each commit.

### 3. Hardcoded Secrets in Version Control
Sensitive credentials deleted or modified in standard file views often remain permanently recorded in the Git history (`.git` folder) unless whole commits are purged or rewritten.

---

## Key Takeaway

> **Never assume removing a secret from a file hides it; Git commit history retains a full history of all past changes.**

Data pipeline:

```text
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
        ↓
Clone repository locally
        ↓
git log -p (View commit diffs)
        ↓
Locate initial commit diff in README.md
        ↓
Level 28 Password retrieved
```