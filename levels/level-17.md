# Bandit Level 17 → Level 18

## Level Objective

There are two files in the home directory:

- `passwords.old`
- `passwords.new`

The password for the next level is in `passwords.new` and is the only line that has changed between the two files.

## Approach

Since the challenge said that only one line had changed between the two files, I needed a way to compare them line by line.

I used the `diff` command to compare `passwords.new` with `passwords.old`.

The output showed the changed line, which contained the password for the next level.

## Commands Used

```bash
diff passwords.new passwords.old
```

## Explanation

### Comparing Files with `diff`

The `diff` command compares two files line by line and displays the differences between them.

```bash
diff passwords.new passwords.old
```

Since the challenge specified that only one line had changed, the output directly revealed the line that was different.

The order of the files matters when interpreting `diff` output, as it shows changes needed to transform the first file into the second.

## What I Learned

- How to compare files using `diff`
- How to identify changes between two versions of a file
- How reading the exact wording of a challenge can point directly toward the appropriate Linux utility
- A practical use case for `diff` when comparing different versions of a file

---

**[← Previous Level](./level-16.md) | [Next Level →](./level-18.md)**