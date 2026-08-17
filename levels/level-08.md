
# Bandit Level 08 → Level 09

## Level Objective

Find the password for the next level stored in `data.txt`. The password is the only line that occurs exactly once.

## Approach

I needed to identify the line that appeared only once in the file.

Since `uniq` only detects duplicate lines when they are adjacent, I first sorted the contents of the file and then passed the result to `uniq`.

## Commands Used

```bash
sort data.txt | uniq -u
````

## Explanation

### Sorting the Data

The `sort` command arranges lines of text in a specified order.

Sorting the file first ensures that identical lines are grouped together.

### Using a Pipe

The `|` operator sends the output of one command directly into another command as input.

```bash
sort data.txt | uniq -u
```

Here, the output from `sort` becomes the input for `uniq`.

### Finding Unique Lines

The `-u` option tells `uniq` to display only lines that occur exactly once.

## What I Learned

* How to combine commands using pipes
* Why sorting is important before using `uniq`
* How to identify unique lines with `uniq -u`
* How Linux commands can be chained together to solve a problem

---

**[← Previous Level](./level-07.md) | [Next Level →](./level-09.md)**

