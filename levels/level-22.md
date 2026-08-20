# Bandit Level 22 → Level 23

## Level Goal

A program is running automatically at regular intervals from `cron`. Look in `/etc/cron.d/` for the configuration and see what command is being executed.

---

## Finding the Cron Job

First, I checked the cron configuration:

```bash
cat /etc/cron.d/cronjob_bandit23
```

Output:

```text
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

This tells me that `/usr/bin/cronjob_bandit23.sh` is executed every minute as the user `bandit23`.

I then inspected the script:

```bash
cat /usr/bin/cronjob_bandit23.sh
```

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

---

## Understanding the Script

The first important line is:

```bash
myname=$(whoami)
```

`whoami` tells us which user is executing the script.

Since the cron job runs the script as `bandit23`, this means:

```text
myname = bandit23
```

The next line was the part I needed to understand:

```bash
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
```

Breaking it down:

```text
echo I am user $myname
        ↓
I am user bandit23
        ↓
md5sum
        ↓
MD5 hash + "-"
        ↓
cut -d ' ' -f 1
        ↓
MD5 hash only
```

The resulting hash is stored in `mytarget`.

---

## The Part I Had to Learn: `cut`

I already understood the pipe:

```bash
|
```

which passes the output of one command into another command.

The part I had to learn was:

```bash
cut -d ' ' -f 1
```

I checked what `md5sum` produces on its own:

```bash
echo "I am user bandit23" | md5sum
```

The output looks like:

```text
8ca319486bfbbc3663ea0fbe81326349  -
```

There are two fields:

```text
8ca319486bfbbc3663ea0fbe81326349  -
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^  ^
              hash                 input
```

`cut` is used to extract a specific field from this output.

### `-d ' '`

The `-d` option specifies the **delimiter**.

```bash
-d ' '
```

means that a space is being used to separate the fields.

### `-f 1`

The `-f` option means **field**.

```bash
-f 1
```

means:

> Give me the first field.

Therefore:

```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```

returns only:

```text
8ca319486bfbbc3663ea0fbe81326349
```

So:

```text
mytarget = 8ca319486bfbbc3663ea0fbe81326349
```

---

## MD5 Is Not Being Decoded

One thing I learned from this level is that MD5 is a **hash**, not an encoding.

I initially thought I might need to somehow decode the MD5 hash to discover the destination.

That's not what is happening.

The script already tells us exactly what input is being hashed:

```text
I am user bandit23
```

So instead of reversing the hash, I simply reproduce the same calculation:

```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```

Because the same input produces the same MD5 hash, I can calculate the filename myself.

---

## Finding the Password File

The final line of the script is:

```bash
cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

We established:

```text
myname   = bandit23
mytarget = 8ca319486bfbbc3663ea0fbe81326349
```

Therefore, the script effectively does:

```bash
cat /etc/bandit_pass/bandit23 > /tmp/8ca319486bfbbc3663ea0fbe81326349
```

So I can read the file:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

This gives the password for Level 23.

---

## My Initial Test With `bandit22`

I also tried:

```bash
echo "I am user bandit22" | md5sum | cut -d ' ' -f 1
```

This produced a different hash, which pointed to a different file and therefore gave me another password.

The important detail is:

```bash
myname=$(whoami)
```

The script is **not** using my current login username (`bandit22`) to determine the target.

The cron configuration says:

```text
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh
```

Therefore, the cron daemon executes the script as `bandit23`.

Inside the script:

```bash
whoami
```

returns:

```text
bandit23
```

not:

```text
bandit22
```

So the correct calculation is:

```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```

The `bandit22` hash is therefore irrelevant to this level.

---

## Commands Used

```bash
cat /etc/cron.d/cronjob_bandit23

cat /usr/bin/cronjob_bandit23.sh

echo "I am user bandit23" | md5sum

echo "I am user bandit23" | md5sum | cut -d ' ' -f 1

cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

---

## What I Learned

### 1. Cron jobs run commands as a specified user

The cron configuration showed that the script runs as:

```text
bandit23
```

This was important because `whoami` inside the script therefore returns `bandit23`.

### 2. `$(...)` is command substitution

```bash
myname=$(whoami)
```

runs `whoami` and stores its output in `myname`.

### 3. Pipes connect commands

```bash
command1 | command2 | command3
```

passes the output of one command into the next.

### 4. `md5sum` creates a deterministic hash

```bash
echo "I am user bandit23" | md5sum
```

produces the same MD5 hash every time for the same input.

### 5. `cut` extracts fields

```bash
cut -d ' ' -f 1
```

means:

* `-d ' '` → use a space as the delimiter
* `-f 1` → select the first field

So it extracts the hash from the output of `md5sum`.

### 6. I don't need to reverse the hash

The important trick was to reproduce the exact calculation:

```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```

That gives me the filename where the cron job placed the password.

---

## Key Takeaway

The main lesson from this level for me was:

> **When analyzing a script, don't just look at what each command does individually. Follow the data through the entire pipeline.**

In this case:

```text
whoami
  ↓
bandit23
  ↓
"I am user bandit23"
  ↓
md5sum
  ↓
8ca319486bfbbc3663ea0fbe81326349
  ↓
/tmp/8ca319486bfbbc3663ea0fbe81326349
  ↓
bandit23 password
```

The specific part I had to look up and understand was:

```bash
cut -d ' ' -f 1
```

because I hadn't previously used `cut` to select fields from command output.