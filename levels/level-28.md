# Bandit Level 28 → Level 29

## Level Goal

There is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo` via port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

From your local machine, clone the repository and find the password for the next level.

---

## Understanding the Task

Unlike previous levels where password info was hidden inside git history diffs, this challenge explores how secrets or sensitive data can be inadvertently committed to version control and modified across different commits.

---

## Where I Got Stuck & Technical Questions

### 1. Placeholder File Content
When checking the initial files after cloning, `README.md` contained placeholder text (`password: xxxxxxxxxx`). Running standard file inspection did not yield the actual password.

### 2. Differentiating Between Revisions
Viewing git logs required tracking changes specifically made to files where sensitive credentials were wiped or sanitized.

---

## Execution

### Step 1: Clone the Repository

On my local machine, I cloned the repository for `bandit28`:

```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo repo_bandit28
cd repo_bandit28
```

### Step 2: Inspect Repository Files

Checking `README.md`:

```bash
cat README.md
```

The output showed placeholders for the password.

### Step 3: Inspect Commit Diffs

I used `git log -p` to inspect the patch history of the repository:

```bash
git log -p
```

---

## Output

Reviewing the commit history showed a previous commit where the actual password for `bandit29` was added before being sanitized/replaced with `xxxxxxxxxx`:

```text
commit [COMMIT_HASH]
Author: Morla Porla <morla@overthewire.org>
Date:   [DATE]

    add credentials

diff --git a/README.md b/README.md
index 7ba2d2f..42331d9 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level28 of bandit.
 ## credentials

 - username: bandit28
-- password: [REDACTED_BANDIT29_PASSWORD]
+- password: xxxxxxxxxx
```

---

## Commands Used

```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo repo_bandit28

cd repo_bandit28

git log -p
```

---

## What I Learned

### 1. Git Commit Immutability
Commits in Git are immutable history records. Modifying or redacting sensitive information in a later commit does not remove it from the historical log unless the repository history is explicitly rewritten or purged.

### 2. Tracking File Modifications (`git log -p`)
`git log -p` allows reviewing specific diffs across all commits, making it easy to identify sensitive data additions and subsequent deletions or edits.

---

## Key Takeaway

> **Sanitizing a file in a new commit does not remove sensitive information from Git history. Always inspect commit diffs to locate historic credentials.**

Data pipeline:

```text
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
        ↓
cd repo_bandit28
        ↓
git log -p
        ↓
Identify historic commit containing credentials
        ↓
Level 29 Password retrieved
```