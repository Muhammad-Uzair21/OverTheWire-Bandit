# Bandit Level 13 → Level 14

## Level Objective

The password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by user `bandit14`.

Instead of being given the password directly, this level provides a private SSH key that can be used to log into the next level.

## Approach

This level was a bit different from the previous ones.

I was given a private SSH key instead of the next password, so the goal was to figure out how to use that key to authenticate as `bandit14`.

I initially tried using the key directly from the Bandit server:

```bash
ssh -i sshkey.private bandit14@localhost -p 2220
```

However, I ran into an error related to the key being restricted from use on the local host.

I then tried changing the permissions of the private key with `chmod 600`, but I couldn't change them because the file was not owned by my current user, `bandit13`.

After getting stuck for a while, I changed my approach.

I copied the contents of the private SSH key to my own computer, saved it as a private key file, and used it from my local machine to SSH into `bandit14`.

That worked.

## Commands Used

On my local computer:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

After successfully logging in as `bandit14`, I could access the password file:

```bash
cat /etc/bandit_pass/bandit14
```

## Explanation

### Using an SSH Private Key

SSH keys can be used for authentication instead of passwords.

The `-i` option tells `ssh` which private key file to use:

```bash
ssh -i sshkey.private ...
```

### Why the Initial Approach Failed

I initially tried to use the private key directly from the Bandit environment, but the connection was restricted.

I also considered changing the permissions of the key with:

```bash
chmod 600 sshkey.private
```

However, I could not modify the permissions because I did not own the file as `bandit13`.

### Using the Key from My Own Machine

Instead of continuing to fight the restrictions on the Bandit server, I copied the private key to my own computer and used it there for the SSH connection.

This allowed me to authenticate as `bandit14` and access the password file that was restricted to that user.

## What I Learned

- How SSH private keys can be used for authentication
- How the `-i` option specifies an SSH identity file
- Why file ownership and permissions matter when working with private keys
- How error messages can reveal restrictions and point toward the actual problem
- Sometimes the solution isn't another command on the same machine — changing where you perform the operation can be the answer

## Reflection

This was the first Bandit level where I genuinely got stuck for a while.

The earlier levels mostly felt like learning a command and applying it to the problem. This one forced me to think about **who I was, what permissions I had, where the key could be used, and what SSH was actually doing**.

The important part wasn't just finding the command. It was figuring out why my first approach wasn't working and changing my approach accordingly.

---

**[← Previous Level](./level-12.md) | [Next Level →](./level-14.md)**