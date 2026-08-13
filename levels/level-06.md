````md
# Bandit Level 06 → Level 07

## Level Objective

Find the password for the next level stored somewhere on the server.

The file is:

- Owned by user `bandit7`
- Owned by group `bandit6`
- Exactly 33 bytes in size

## Approach

Because the challenge stated that the file could be located anywhere on the server, I searched from the root directory and filtered the results using the required user, group, and file-size conditions.

The search also produced permission errors from directories I could not access, so I redirected those errors to `/dev/null` to keep the output clean.

## Commands Used

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
````

## Explanation

### Searching by Ownership

The `-user` option filters files based on their owner.

```bash
-user bandit7
```

The `-group` option filters files based on their group ownership.

```bash
-group bandit6
```

Combining these conditions allows `find` to search specifically for files matching the ownership requirements.

### Searching the Entire Filesystem

Starting `find` with `/` searches from the root directory and allows the command to traverse the entire filesystem hierarchy.

### Suppressing Error Messages

When searching the entire filesystem, `find` may encounter directories that the current user cannot access.

```bash
2>/dev/null
```

File descriptor `2` represents standard error (`stderr`). Redirecting it to `/dev/null` discards those error messages instead of displaying them in the terminal.

## What I Learned

* How to search the filesystem using `find`
* How to filter files by user and group ownership
* How to search from the filesystem root
* What file descriptor `2` represents
* How `/dev/null` can be used to discard unwanted output

---

**[← Previous Level](./level-05.md) | [Next Level →](./level-07.md)**

```
```
