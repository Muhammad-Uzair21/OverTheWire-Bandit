# Bandit Level 05 → Level 06

## Level Objective

Find the password stored in a file somewhere under the `inhere` directory.

The correct file is:

- Human-readable
- Exactly 1033 bytes in size
- Not executable

## Approach

Since the password could be located inside any of the nested directories under `inhere`, I used `find` to search recursively and filter the results according to the conditions given by the challenge.

Once I identified the matching file, I used `cat` to read its contents.

## Commands Used

```bash
cd inhere
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2
```

## Explanation

### Recursive Searching

The `find` command searches through a directory and its nested subdirectories.

Starting the search with:

```bash
find .
```

tells `find` to search from the current directory.

### Filtering by File Type

```bash
-type f
```

restricts the results to regular files.

### Filtering by File Size

```bash
-size 1033c
```

searches for files that are exactly 1033 bytes in size.

The `c` represents bytes.

### Negating a Condition

```bash
! -executable
```

excludes files that have executable permissions.

By combining these conditions, I could narrow down the search to the file matching all the requirements.

## What I Learned

- How to search recursively with `find`
- How to filter files by type and size
- How to check for executable files using `-executable`
- How to negate a condition using `!`
- How multiple conditions can be combined to narrow down search results

---

**[← Previous Level](./level-04.md) | [Next Level →](./level-06.md)**