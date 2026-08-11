# Bandit Level 00 → Level 01

## Level Objective

Log into the Bandit Level 0 server using SSH and find the password for the next level.

## Approach

I first used `ls` to see what files were available in the home directory.

The `readme` file contained the password for the next level, so I used `cat` to read its contents.

## Commands Used

```bash
ls
cat readme
```

## Explanation

### Listing Files

The `ls` command lists the files and directories in the current directory.

### Reading a File

The `cat` command displays the contents of a file directly in the terminal.

## What I Learned

- How to list files using `ls`
- How to read a file using `cat`
- Basic interaction with files in the Linux terminal

---

**[Next Level →](./level-01.md)**