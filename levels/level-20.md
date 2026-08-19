# Bandit Level 20 → Level 21

## Level Goal

There is a setuid binary called `suconnect` in the home directory. It connects to `localhost` on a port specified as a command-line argument.

The program expects to receive the **password from the previous level (Bandit 20)** through that connection. If the password is correct, it sends back the **password for Bandit 21**.

## Understanding the Problem

`suconnect` acts as a **client**, so we need another program listening on a port for it to connect to.

We can use `nc` (netcat) to create a simple TCP listener.

```text
Terminal 1                         Terminal 2

nc -l 3000                         ./suconnect 3000
      │                                  │
      └──────── TCP connection ──────────┘
```

## Step 1 — Start a Netcat Listener

In the first terminal:

```bash
nc -l 3000
```

This opens port `3000` and waits for an incoming connection.

## Step 2 — Connect Using `suconnect`

Open another SSH session as `bandit20` and run:

```bash
./suconnect 3000
```

This connects `suconnect` to the netcat listener running on port `3000`.

## Step 3 — Send the Previous Password

Once the connection is established, enter the **Bandit 20 password** into the terminal running `nc`.

`suconnect` receives the password and checks it.

If the password is correct, `suconnect` sends the **Bandit 21 password** back through the connection.

The next password will appear in the `nc` terminal.

## Why Earlier Attempts Failed

Running:

```bash
./suconnect 3000
```

without a listener resulted in:

```text
Could not connect
```

because nothing was listening on port `3000`.

However, this worked:

```bash
./suconnect 2220
```

and returned:

```text
Read: SSH-2.0-OpenSSH_10.2p1
ERROR: This doesn't match the current password!
```

This proved that `suconnect` successfully connected to port `2220`, but an SSH server was running there. It received the SSH banner instead of the Bandit 20 password.

## Commands Used

**Terminal 1:**

```bash
nc -l 3000
```

**Terminal 2:**

```bash
./suconnect 3000
```

Then enter the Bandit 20 password into the `nc` terminal.

## Key Concept

This level demonstrates basic **TCP client-server communication**:

- `nc -l 3000` → creates a TCP listener.
- `./suconnect 3000` → connects to that listener.
- Bandit 20 password → sent through the connection.
- `suconnect` → verifies the password.
- Correct password → Bandit 21 password is transmitted back.

The main lesson is that `suconnect` doesn't ask for the previous password interactively. We provide it **through the network connection**.