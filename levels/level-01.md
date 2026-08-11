# Bandit Level 01 → Level 02

## Level Objective

Find the password for the next level, which is stored in a file named `-` in the home directory.

## Approach

I used `ls` to identify the file.

Since the filename is a single hyphen (`-`), I used `./-` to explicitly reference the file instead of allowing the command to interpret `-` as a special argument.

## Commands Used

```bash
ls
cat ./-
```

## Explanation

### Handling Filenames Beginning with `-`

A hyphen is commonly used by command-line utilities to introduce options or flags.

Using `./-` explicitly specifies the file's path relative to the current directory, so `cat` treats it as a filename.

## What I Learned

- How command-line tools interpret arguments
- Why filenames beginning with `-` can cause unexpected behavior
- How `./` can be used to explicitly reference a file in the current directory.

---

**[← Previous Level](./level-00.md) | [Next Level →](./level-02.md)**