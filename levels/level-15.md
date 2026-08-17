# Bandit Level 15 → Level 16

## Level Objective

Retrieve the password for the next level by submitting the current level's password to port `30001` on `localhost` using SSL/TLS encryption.

## Approach

This level was similar to the previous one, where I had to submit the current password to a service running on a local port.

The difference was that this service required an encrypted SSL/TLS connection, so `nc` alone was not sufficient.

I used `openssl s_client` to establish a TLS connection to port `30001` on `localhost`. Once the connection was established, I entered the current level's password and received the password for the next level.

## Commands Used

```bash
openssl s_client -connect localhost:30001
```

After the TLS connection was established, I entered the current level's password and pressed Enter.

## Explanation

### SSL/TLS Connection

SSL/TLS provides encryption for data transmitted between a client and a server.

Unlike the previous level, where a plain TCP connection using `nc` was enough, this service required the connection to use TLS.

### Using `openssl s_client`

`openssl s_client` is a command-line tool that can establish and interact with SSL/TLS connections.

```bash
openssl s_client -connect localhost:30001
```

The `-connect` option specifies the host and port to connect to.

In this case:

- `localhost` refers to the local machine
- `30001` is the port running the service

Once the TLS handshake was completed, I could submit the password through the encrypted connection.

## What I Learned

- The difference between a plain TCP connection and a TLS-encrypted connection
- How to establish a TLS connection using `openssl s_client`
- How ports can expose different network services
- How encryption requirements can determine which networking tool to use
- The practical difference between the approach used in Level 14 and Level 15

---

**[← Previous Level](./level-14.md) | [Next Level →](./level-16.md)**