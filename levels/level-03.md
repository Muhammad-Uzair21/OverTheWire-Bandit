# Bandit Level 03 → Level 04

## Level Objective

Find the password for the next level, which is stored in a hidden file inside the `inhere` directory.

## Approach

I first navigated into the `inhere` directory using `cd`.

I then used `ls -a` to reveal hidden files, since hidden files are not shown by a regular `ls` command.

After identifying the hidden file, I used `cat` with `--` to read its contents.

## Commands Used

```bash
cd inhere
ls -a
cat -- ...Hiding-From-You
```

## Explanation

### Navigating Directories

The `cd` command is used to change the current working directory.

```bash
cd inhere
```

moves into the `inhere` directory.

### Finding Hidden Files

In Linux, files and directories whose names begin with `.` are hidden by default.

The `-a` option tells `ls` to show all entries, including hidden files.

```bash
ls -a
```

### Using `--` to End Options

The `--` tells the command that everything following it should be treated as an argument rather than an option.

This is useful when working with filenames that could otherwise be interpreted as command-line options.

## What I Learned

- How to navigate directories using `cd`
- How Linux handles hidden files
- How to reveal hidden files with `ls -a`
- The purpose of `--` when handling command arguments

---

**[← Previous Level](./level-02.md) | [Next Level →](./level-04.md)**