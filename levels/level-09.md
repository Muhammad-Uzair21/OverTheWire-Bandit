````md
# Bandit Level 09 → Level 10

## Level Objective

Find the password for the next level inside `data.txt`.

The password is stored as a human-readable string within the binary data and is preceded by several `=` characters.

## Approach

Because `data.txt` contained binary data, I first used `strings` to extract the human-readable text from it.

I then piped that output into `grep` to filter for lines beginning with `=`.

## Commands Used

```bash
strings data.txt | grep "^="
````

## Explanation

### Extracting Printable Text

The `strings` command searches binary or raw data for sequences of printable characters and displays them as text.

This is useful when a file contains a mixture of binary data and human-readable strings.

### Combining `strings` and `grep`

I used a pipe to pass the output of `strings` directly to `grep`.

```bash
strings data.txt | grep "^="
```

The `^` character is a regular-expression anchor that represents the beginning of a line.

Therefore, `^=` matches lines that begin with `=`.

## What I Learned

* How `strings` can extract human-readable data from binary files
* How to combine `strings` and `grep`
* The basic use of `^` as a regular-expression anchor
* How filtering can make noisy command output easier to analyze

---

**[← Previous Level](./level-08.md) | [Next Level →](./level-10.md)**

```
```
