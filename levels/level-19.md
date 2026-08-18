# Bandit Level 19 → Level 20

## Level Objective

Gain access to the next level by using the setuid binary located in the home directory.

The password for the next level can be found in the usual location:

```text
/etc/bandit_pass
```

after using the setuid binary.

## Approach

I first listed the contents of the home directory:

```bash
ls
```

This revealed a binary called `bandit20-do`.

The challenge said to execute it without arguments to find out how to use it, so I ran:

```bash
./bandit20-do
```

It explained that the binary could be used to run a command as another user.

I initially tried to read `/etc/bandit_pass` directly:

```bash
./bandit20-do cat /etc/bandit_pass
```

but got:

```text
cat: /etc/bandit_pass: Is a directory
```

I then tried adding `/` and even tried `ls` as if it were a file, but realized that `/etc/bandit_pass` is a directory containing individual password files.

I used the setuid binary to list its contents:

```bash
./bandit20-do ls /etc/bandit_pass/
```

This showed files for the different Bandit users, including `bandit20`.

Finally, I used the binary to run `cat` on that specific file:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

This returned the password for the next level.

## Commands Used

```bash
ls

./bandit20-do

./bandit20-do cat /etc/bandit_pass

./bandit20-do ls /etc/bandit_pass/

./bandit20-do ls /etc/bandit_pass/bandit20

./bandit20-do cat /etc/bandit_pass/bandit20
```

## Explanation

### What is Setuid?

**Setuid** (Set User ID) is a Linux permission mechanism that allows an executable to run with the permissions of the file's owner rather than the permissions of the user executing it.

This can allow a program to perform actions that the current user normally would not have permission to perform.

In this level, the `bandit20-do` binary has setuid behavior that allows it to execute commands with the privileges of another user.

### Using `./bandit20-do`

The `./` means:

> Execute the file named `bandit20-do` from the current directory.

Running:

```bash
./bandit20-do
```

without arguments showed:

```text
Run a command as another user.
Example: ./bandit20-do whoami
```

This told me that I could place another command after the binary.

For example:

```bash
./bandit20-do ls /etc/bandit_pass/
```

means that `bandit20-do` executes `ls /etc/bandit_pass/` using its elevated privileges.

I could then use the same method with `cat`:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

### Understanding `/etc/bandit_pass`

My initial mistake was treating:

```text
/etc/bandit_pass
```

as if it were the password file.

It is actually a **directory** containing separate password files for the different Bandit levels.

That's why:

```bash
./bandit20-do cat /etc/bandit_pass
```

returned:

```text
Is a directory
```

Listing the directory showed the individual files, including:

```text
bandit20
```

I could then read the specific file using the setuid binary.

## What I Learned

- What the setuid mechanism does in Linux
- How `./` is used to execute a program from the current directory
- How a setuid binary can execute commands with different privileges
- How to distinguish between a directory and a file
- How to inspect a directory before trying to read something inside it
- How command chaining through a privileged binary can be used to access files that the current user normally could not read

## My Takeaway

This level was mostly about understanding **what the provided binary was actually giving me**.

I made a few wrong attempts by treating `/etc/bandit_pass` as a file. Once I listed the directory and saw the individual password files, the solution became straightforward:

```text
bandit20-do
    ↓
run command with elevated privileges
    ↓
ls /etc/bandit_pass/
    ↓
find bandit20
    ↓
cat /etc/bandit_pass/bandit20
```

---

**[← Previous Level](./level-18.md) | [Next Level →](./level-20.md)**