````md
# Bandit Level 07 → Level 08

## Level Objective

Find the password for the next level stored in the file `data.txt` next to the word `millionth`.

## Approach

Instead of manually reading through the entire file, I searched its contents for the specific word given in the challenge.

## Commands Used

```bash
grep "millionth" data.txt
````

## Explanation

### Searching File Contents

The `grep` command searches text for a specific string or pattern and displays the matching lines.

In this case:

```bash
grep "millionth" data.txt
```

searches `data.txt` for the word `millionth`.

### Filtering Large Files

`grep` is useful when working with large amounts of text because it allows specific information to be extracted without manually reading the entire file.

## What I Learned

* How to search file contents using `grep`
* How to search for specific strings
* How filtering can make working with large amounts of text more efficient

---

**[← Previous Level](./level-06.md) | [Next Level →](./level-08.md)**

```
```
