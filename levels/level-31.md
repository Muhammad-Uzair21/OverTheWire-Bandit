# Bandit Level 31 → Level 32

## Level Goal

There is a git repository at `ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo` via port `2220`. The password for the user `bandit31-git` is the same as for the user `bandit31`.

From your local machine, clone the repository and find the password for the next level.

---

## Understanding the Task

Unlike previous levels that focused on searching history or references, this level requires pushing a new commit containing a specific file (`key.txt`) with predefined content (`May I come in?`) to the remote `master` branch. A server-side `pre-receive` hook validates the commit and returns the next password if the file requirements are met.

---

## Where I Got Stuck & Technical Questions

### 1. `.gitignore` Rule Blocking `key.txt`
Inspecting `.gitignore` revealed rules that prevented standard `git add key.txt` from staging the file. Trying to stage it required using the force flag (`git add -f`).

### 2. PowerShell Output Encoding (UTF-16 vs. UTF-8)
Initially creating `key.txt` using PowerShell redirect (`echo 'May I come in?' > key.txt`) saved the file encoded in **UTF-16 LE** by default. When pushed, the remote Git server's hook rejected the commit with the error:

```text
remote: Wrong! The file you submitted is UTF-16, not UTF-8/ASCII
! [remote rejected] master -> master (pre-receive hook declined)
```

To resolve this, I deleted `key.txt` and recreated it using `Set-Content` to enforce proper encoding before staging, committing, and pushing again.

---

## The Solution Strategy

1. Clone the repository and inspect `README.md` and `.gitignore`.
2. Create `key.txt` with the exact text `May I come in?` using UTF-8 encoding.
3. Force-stage the file using `git add -f key.txt` to bypass `.gitignore`.
4. Commit and push to `origin master` to trigger the remote server's validation script.

---

## Execution

### Step 1: Clone the Repository

```bash
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo repo_bandit31
cd repo_bandit31
```

### Step 2: Create the File in PowerShell

```powershell
Set-Content -Path key.txt -Value "May I come in?"
```

### Step 3: Force-Stage and Commit

```bash
git add -f key.txt
git commit -m "Add key.txt"
```

### Step 4: Push to Remote

```bash
git push origin master
```

---

## Output

```text
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 320 bytes | 320.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: [REDACTED_BANDIT32_PASSWORD]
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
```

---

## Commands Used

```powershell
# Clone repository
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo repo_bandit31
cd repo_bandit31

# Create key.txt with correct encoding
Set-Content -Path key.txt -Value "May I come in?"

# Force stage past .gitignore and commit
git add -f key.txt
git commit -m "Add key.txt"

# Push to trigger server validation
git push origin master
```

---

## What I Learned

### 1. PowerShell Output Encoding Differences
In Windows PowerShell, using `>` or standard redirection creates UTF-16 LE files by default. Many Linux server scripts strictly expect UTF-8/ASCII formatting, which makes commands like `Set-Content` preferable for controlling text output formats.

### 2. Overriding `.gitignore` (`git add -f`)
When `.gitignore` rules prevent staging specific files, the `-f` (force) flag allows tracking the file explicitly without altering the `.gitignore` configuration.

### 3. Server-Side Git Hooks (`pre-receive`)
Remote repositories can execute automated scripts (hooks) during `git push` events to enforce policy checks, validate payload contents, or dynamically output responses based on submitted commits.

---

## Key Takeaway

> **Pay attention to file encoding when generating files in Windows shell environments for Linux-hosted services, and use `git add -f` to bypass `.gitignore` constraints when required.**

Data pipeline:

```text
git clone ssh://bandit31-git@...
        ↓
Set-Content -Path key.txt -Value "May I come in?"
        ↓
git add -f key.txt && git commit
        ↓
git push origin master
        ↓
Server pre-receive hook validation pass
        ↓
Level 32 Password retrieved
```