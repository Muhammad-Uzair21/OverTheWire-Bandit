# Bandit Level 29 → Level 30

## Level Goal

There is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` via port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

From your local machine, clone the repository and find the password for the next level.

---

## Understanding the Task

In this level, checking the commit history on the default `master` branch only reveals sanitized placeholders or previous level credentials (`bandit29`). The objective is to identify and inspect non-default Git **branches** existing on the remote repository.

---

## Where I Got Stuck & Technical Questions

### 1. Misleading Commit Diffs on `master`
Running `git log -p` on the `master` branch showed a commit with `fix info leak` where a password was changed to `xxxxxxxxxx`. However, the original password exposed in that diff was for `bandit29` (the current level), not `bandit30`.

### 2. Hidden Remote Branches
Running `git branch` initially listed only the local `master` branch. The target data was stored on a separate development branch on the remote server (`origin`) that had not been checked out locally yet.

### 3. Exiting Text Pager Views
When reviewing long outputs like `git log -p`, the screen locked into a pager (`less`/`more`), requiring pressing **`q`** to safely return to the active shell prompt.

---

## The Solution Strategy

1. Clone the repository and inspect all branches (both local and remote) using `git branch -a`.
2. Locate non-standard remote branches (such as `remotes/origin/dev`).
3. Switch (checkout) to the development branch to expose its unique file tracking state and read the stored password.

---

## Execution

### Step 1: Clone the Repository

On my local machine, I cloned a clean copy of the Level 29 repository:

```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo repo_bandit29
cd repo_bandit29
```

### Step 2: List All Remote and Local Branches

To view every branch tracked on the remote server:

```bash
git branch -a
```

Output:

```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
```

### Step 3: Switch to the `dev` Branch

I checked out the `dev` branch to view its active workspace:

```bash
git checkout dev
```

### Step 4: Retrieve the Password

Once on the `dev` branch, I inspected `README.md`:

```bash
cat README.md
```

---

## Output

```text
# credentials

- username: bandit30
- password: [REDACTED_BANDIT30_PASSWORD]
```

This output successfully printed the password for Level 30.

---

## Commands Used

```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo repo_bandit29

cd repo_bandit29

git branch -a

git checkout dev

cat README.md
```

---

## What I Learned

### 1. Branch Inspection (`git branch -a`)
By default, Git clones check out the main branch (`master` or `main`). Running `git branch -a` reveals all remote tracking branches (`remotes/origin/*`) available on the remote repository.

### 2. Branch Switching (`git checkout <branch>`)
Git allows projects to maintain completely isolated development contexts. Switching branches alters the local working tree to reflect the exact state of files tracked under that specific ref.

### 3. Pager Control Basics
Interactive terminal pagers (`less`/`more`) used by tools like `git log` can be exited cleanly at any time by pressing **`q`**.

---

## Key Takeaway

> **Information in a Git repository isn't limited to the `master` branch. Always check all remote branches (`git branch -a`) when auditing version control repositories.**

Data pipeline:

```text
git clone ssh://bandit29-git@...
        ↓
git branch -a (Identify remotes/origin/dev)
        ↓
git checkout dev
        ↓
cat README.md
        ↓
Level 30 Password retrieved
```