# Bandit Level 10 → Level 11

## Level Objective

Find the password for the next level stored in `data.txt`, which contains Base64-encoded data.

## Approach

The contents of `data.txt` were encoded using Base64. I used the `base64` utility with its decode option to convert the encoded data back into readable text.

## Commands Used

```bash
base64 -d data.txt
````

## Explanation

### Base64 Decoding

Base64 is an encoding scheme used to represent binary data as ASCII text.

It is **encoding, not encryption**, meaning it does not provide confidentiality and can be decoded without a secret key.

### The `-d` Flag

The `-d` option tells the `base64` utility to decode the input rather than encode it.

```bash
base64 -d data.txt
```

## What I Learned

* What Base64 encoding is
* The difference between encoding and encryption
* How to decode Base64 data from the command line
* How command-line options can change the behavior of a utility

---

**[← Previous Level](./level-09.md) | [Next Level →](./level-11.md)**

