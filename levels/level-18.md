# Bandit Level 18 → Level 19

## Level Objective

The password for the next level is stored in a file called `readme` in the home directory.

However, someone has modified `.bashrc` to automatically log the user out when connecting through SSH.

## Approach

When I first tried to log into `bandit18`, I was immediately disconnected and received:

```text
Byebye!
```

The challenge mentioned that `.bashrc` had been modified, so the problem wasn't the password itself. The issue was that the normal interactive shell was being terminated as soon as I logged in.

Instead of trying to open a normal interactive shell, I realized I could use SSH to execute a command directly on the remote machine.

Since the password was stored in `readme`, I used `cat readme` as the remote command.

## Commands Used

```bash
ssh bandit18@bandit.labs.overthewire.org cat readme
```

After the command displayed the contents of `readme`, I used the returned password to proceed to the next level.

## Explanation

### Why the Normal SSH Login Failed

Normally, SSH opens an interactive shell after authentication.

In this level, `.bashrc` had been modified to log the user out immediately. This meant that successfully authenticating as `bandit18` was not enough to get a usable shell.

### Executing a Remote Command with SSH

SSH can also execute a specific command on the remote machine instead of starting an interactive shell.

```bash
ssh user@host command
```

In this case:

```bash
ssh bandit18@bandit.labs.overthewire.org cat readme
```

SSH authenticated as `bandit18`, executed `cat readme` on the remote machine, displayed the contents, and then exited.

This avoided the interactive shell that was being terminated by `.bashrc`.

## What I Learned

- How `.bashrc` can affect an interactive shell session
- That successfully logging in through SSH does not always mean you will get a usable shell
- How SSH can execute commands directly on a remote system
- How to work around a broken interactive shell by executing the required command remotely
- How understanding what a command actually does can be more useful than simply trying different commands

## My Takeaway

This level initially looked like an authentication problem because I was being kicked out immediately after entering the password.

The important realization was that **SSH itself was working**. It was the interactive shell that was being terminated.

Once I stopped trying to get a shell and instead asked SSH to run the exact command I needed, the problem became much simpler.

---

**[← Previous Level](./level-17.md) | [Next Level →](./level-19.md)**