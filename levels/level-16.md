# Bandit Level 16 → Level 17

## Level Objective

Retrieve the credentials for the next level by submitting the current password to a port on `localhost` in the range `31000` to `32000`.

First, identify which ports have servers listening, then determine which of those services use SSL/TLS.

Only one service provides the credentials for the next level. The others simply echo whatever is sent to them.

## Approach

This level required a few steps.

First, I scanned the specified port range using `nmap` to find the open ports.

```bash
nmap -p 31000-32000 localhost
```

This returned five open ports:

```text
31046
31518
31691
31790
31960
```

I then tested the open ports with `openssl s_client` to determine which ones supported SSL/TLS.

The ports `31518` and `31790` successfully established TLS connections.

At first, both appeared to behave similarly and produced `KEYUPDATE` messages. The challenge had specifically warned about this, so I used the `-quiet` option with `openssl s_client` and tested the services by submitting the current password.

The service on port `31518` behaved like an echo service.

The service on port `31790`, however, returned a private SSH key. This was the credential needed for the next level.

I then created a temporary directory, saved the private key to a file, restricted its permissions, and used it to authenticate as `bandit17`.

## Commands Used

### Scanning the Port Range

```bash
nmap -p 31000-32000 localhost
```

### Testing TLS Services

```bash
openssl s_client -connect localhost:31518
openssl s_client -connect localhost:31790
```

### Connecting Quietly

```bash
openssl s_client -connect localhost:31790 -quiet
```

### Creating a Temporary Workspace

```bash
mkdir /tmp/bandit16
cd /tmp/bandit16
```

### Saving the Private Key

```bash
nano sshkey.private
```

I pasted the private SSH key returned by the service into the file.

### Restricting Key Permissions

```bash
chmod 600 sshkey.private
```

### Logging into the Next Level

```bash
ssh -i sshkey.private bandit17@localhost -p 2220
```

## Explanation

### Scanning Ports with `nmap`

The challenge specified the port range `31000` to `32000`, so I used `nmap` to identify which ports were open.

```bash
nmap -p 31000-32000 localhost
```

This avoids having to test every port manually.

### Identifying TLS Services

I used `openssl s_client` to test whether the open ports supported SSL/TLS.

```bash
openssl s_client -connect localhost:PORT
```

A successful TLS connection showed information such as the negotiated TLS version and cipher.

Two of the open ports supported TLS, so I then had to determine which one was the actual credential service.

### Handling `KEYUPDATE`

The TLS services produced `KEYUPDATE` messages while I was interacting with them.

The challenge specifically warned about this behavior. Using:

```bash
openssl s_client -connect localhost:31790 -quiet
```

made the interaction easier to work with.

### Finding the Correct Service

The challenge stated that most services would simply echo the submitted password.

After testing the TLS-enabled services, port `31790` returned an SSH private key instead of simply echoing the password.

That indicated that it was the service providing the credentials for the next level.

### Using the SSH Private Key

I saved the returned key into `sshkey.private` and restricted its permissions:

```bash
chmod 600 sshkey.private
```

SSH private keys should not be left readable by other users.

I could then use the key with the `-i` option:

```bash
ssh -i sshkey.private bandit17@localhost -p 2220
```

This authenticated me as `bandit17` without requiring the account password.

## What I Learned

- How to scan a range of ports using `nmap`
- How to identify TLS-enabled services using `openssl s_client`
- How to distinguish an echo service from a service that provides credentials
- What `KEYUPDATE` messages can look like during a TLS connection
- How SSH private keys can be used for authentication
- Why private SSH keys require restricted file permissions
- How to combine network enumeration, service identification, and authentication techniques to solve a challenge

---

**[← Previous Level](./level-15.md) | [Next Level →](./level-17.md)**