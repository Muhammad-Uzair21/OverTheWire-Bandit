# Bandit Level 14 → Level 15

## Level Objective

Retrieve the password for the next level by submitting the current level's password to port `30000` on `localhost`.

## Approach

My first thought was to use `scp`, since I was working with a password and another Bandit level:

```bash
scp -P 30000 "CURRENT_PASSWORD" bandit15@localhost
```

However, this failed because `scp` is used for securely copying files, while this challenge requires sending a password to a service listening on a specific port.

I then used `nc` (Netcat) to connect to port `30000` on `localhost`:

```bash
nc localhost 30000
```

After the connection was established, I entered the current level's password and pressed Enter. The service returned the password for the next level.

## Commands Used

```bash
nc localhost 30000
```

## Explanation

### Connecting to a Local Port

`nc`, short for **Netcat**, is a networking utility that can establish connections and send or receive data over TCP or UDP.

In this case:

```bash
nc localhost 30000
```

connects to port `30000` on the local machine.

### Why `scp` Wasn't the Right Tool

`scp` is designed for securely copying files between systems.

This challenge wasn't asking me to transfer a file. It was asking me to **submit data to a service listening on a specific port**, which is why `nc` was more appropriate.

### Passing the Password

I initially tried:

```bash
nc localhost 30000 CURRENT_PASSWORD
```

This produced an error because `nc` interpreted the password as another command-line argument rather than data to send.

The correct approach was to connect first:

```bash
nc localhost 30000
```

and then enter the password interactively.

## What I Learned

- What `nc` (Netcat) is used for
- How to connect to a service running on a specific port
- The difference between file-transfer tools like `scp` and networking tools like `nc`
- How command-line arguments differ from data entered into an interactive connection
- How reading the wording of a challenge carefully can point toward the right tool

---

**[← Previous Level](./level-13.md) | [Next Level →](./level-15.md)**