# Bandit Level 02 → Level 03

## Level Objective

Find the password for the next level, which is stored in a file with spaces and leading hyphens in its filename.

## Approach

The filename contained spaces and began with hyphens, so I used a relative path and wrapped the filename in quotes.

## Commands Used

```bash
cat "./--spaces in this filename--"
```

## Explanation

### Handling Filenames Beginning with `-`

Filenames beginning with hyphens can be interpreted as command-line options.

Using `./` makes it clear that the argument is a path to a file in the current directory.

### Handling Spaces

Spaces normally separate arguments on the command line. Wrapping the complete filename in quotes tells the shell to treat it as a single argument.

## What I Learned

- How to work with filenames containing spaces
- How to handle filenames beginning with hyphens
- How quoting affects command-line arguments

---

**[← Previous Level](./level-01.md) | [Next Level →](./level-03.md)**