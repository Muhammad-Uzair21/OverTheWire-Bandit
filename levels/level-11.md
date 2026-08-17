# Bandit Level 11 → Level 12

## Level Objective

Find the password for the next level stored in `data.txt`, where all lowercase (`a-z`) and uppercase (`A-Z`) letters have been rotated by 13 positions (ROT13).

## Approach

The contents of `data.txt` were encoded using ROT13.

I used `tr` to translate each letter to the character 13 positions away in the alphabet, reversing the ROT13 transformation.

## Commands Used

```bash
cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M'
```

## Explanation

### ROT13 Cipher

ROT13 is a simple substitution cipher that replaces each letter with the letter 13 positions after it in the alphabet.

Because the alphabet contains 26 letters, applying ROT13 twice returns the original text.

### Character Translation with `tr`

The `tr` command translates or deletes characters from standard input.

```bash
tr 'a-zA-Z' 'n-za-mN-ZA-M'
```

The first character set represents the lowercase and uppercase alphabets, while the second represents the same alphabets shifted by 13 positions.

## What I Learned

- What the ROT13 substitution cipher is
- Why ROT13 can be reversed using the same transformation
- How to use `tr` for character translation
- How character ranges can be used to transform text from the command line

---

**[← Previous Level](./level-10.md) | [Next Level →](./level-12.md)**