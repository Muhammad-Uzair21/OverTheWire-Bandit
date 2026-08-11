# Bandit Level 04 → Level 05

## Level Objective

Find the password for the next level stored in the `inhere` directory. The password is contained in the only human-readable file among several files.

## Approach

I first navigated into the `inhere` directory.

Since there were multiple files and I needed to identify the human-readable one, I used the `file` command with a wildcard to inspect all of them at once.

After identifying the correct file, I used `cat` to read its contents.

## Commands Used

```bash
cd inhere
file ./*
cat ./-file07
```

## Explanation

### Identifying File Types

The `file` command examines the contents of a file and attempts to determine its type.

Using:

```bash
file ./*
```

checks all files in the current directory at once.

This is useful when file extensions cannot be trusted or when dealing with multiple files of unknown types.

### Finding Human-Readable Data

The challenge specified that the password was stored in a human-readable file.

The `file` command can identify files containing ASCII text or other readable data, making it easier to narrow down the correct file.

### Handling Filenames Beginning with `-`

The target file was named `-file07`.

Using:

```bash
cat ./-file07
```

explicitly provides the file's path relative to the current directory, preventing `cat` from interpreting the leading hyphen as an option.

## What I Learned

- How to inspect file types using `file`
- How wildcards can be used to inspect multiple files
- How to identify human-readable files
- How to safely reference filenames beginning with `-`

---

**[← Previous Level](./level-03.md) | [Next Level →](./level-05.md)**