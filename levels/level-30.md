# Bandit Level 30 → Level 31

## Level Goal

There is a git repository at `ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo` via port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.

From your local machine, clone the repository and find the password for the next level.

---

## Understanding the Task

In this level, neither the `master` branch working tree nor its commit history contains the target password. Additionally, no extra remote branches exist. The objective is to inspect Git **tags** (lightweight or annotated references to specific Git objects/commits).

---

## Where I Got Stuck & Technical Questions

### 1. Empty/Misleading Working Tree & Log
Inspecting `README.md` showed a bait message (`just an empty file... muahahah`), and running `git log -p` revealed only a single commit containing no password leaks.

### 2. No Alternate Branches
Running `git branch -a` showed only `master`.

---

## The Solution Strategy

1. Clone the repository locally.
2. Inspect repository tags using `git tag` to see if specific objects or commits are marked with custom tags.
3. Examine the content of any discovered tags using `git show <tag_name>`.

---

## Execution

### Step 1: Clone the Repository

```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo repo_bandit30
cd repo_bandit30
```

### Step 2: Check File Content, Logs, and Branches

```bash
cat README.md
git log -p
git branch -a
```

* `README.md` output: `just an empty file... muahahah`
* `git branch -a` output: Only `master`

### Step 3: Check Git Tags

To view all tags in the repository:

```bash
git tag
```

Output:

```text
secret
```

### Step 4: Inspect the Tag Content

To view the object or payload pointed to by the tag `secret`:

```bash
git show secret
```

---

## Output

```text
[REDACTED_BANDIT31_PASSWORD]
```

This output printed the password for Level 31.

---

## Commands Used

```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo repo_bandit30

cd repo_bandit30

git tag

git show secret
```

---

## What I Learned

### 1. Git Tags (`git tag`)
Tags are explicit pointers to specific objects (commits, blobs, or annotated tags) in Git history. They are often used for release milestones (e.g., `v1.0`), but can hold arbitrary references or unlinked commits.

### 2. Inspecting Tag References (`git show <tag>`)
The `git show` command outputs the explicit details and stored content associated with any Git reference (commit hash, branch, or tag name).

---

## Key Takeaway

> **When files, commit history, and branches reveal no secrets in a Git repository, check the tag references (`git tag` & `git show <tag>`).**

Data pipeline:

```text
git clone ssh://bandit30-git@...
        ↓
git tag (Discovered "secret")
        ↓
git show secret
        ↓
Level 31 Password retrieved
```