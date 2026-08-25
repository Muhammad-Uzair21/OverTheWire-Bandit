# OverTheWire Bandit: 100% Completion Writeups

> Mastering Linux CLI, privilege escalation, version control, and shell breakouts—one level at a time.

This repository contains my complete writeups and documentation for **[OverTheWire Bandit](https://overthewire.org/wargames/bandit/)**, a beginner-to-intermediate wargame focused on practical Linux administration, security auditing, and command-line problem solving.

## Overview

Bandit provides a hands-on environment for exploring Linux systems, uncovering misconfigurations, and mastering command-line utilities. Rather than memorizing syntax, completing all 33 levels required developing an analytical security mindset:

- Inspecting raw file formats, hidden directories, and system file structures.
- Auditing local permissions, cron jobs, and setuid/setgid binaries.
- Interacting directly with network services (Netcat, OpenSSL, SSH port forwarding).
- Reversing and traversing Git repositories (commit diffs, tags, hooks, remotes).
- Bypassing restricted shells and custom execution environments.

---

## Repository Structure

Each level features a dedicated writeup structured as follows:

- **Level Goal** — Challenge requirements and context.
- **Understanding the Task** — Breakdown of the underlying concept.
- **Where I Got Stuck & Technical Questions** — Challenges faced (e.g., shell encodings, Git remotes).
- **Solution Strategy & Execution** — Step-by-step commands executed.
- **Key Takeaways & Lessons Learned** — Theoretical and practical takeaways.

> **Note:** In accordance with OverTheWire guidelines, raw level passwords and credentials are strictly excluded from this repository.

---

## Progress: 33 / 33 Levels Completed

| Range | Status | Key Focus |
|-------|--------|-----------|
| **Level 00 → 10** | 🟢 100% Completed | Navigation, Hidden Files, Base64, Sorting, Human-Readable Strings |
| **Level 11 → 20** | 🟢 100% Completed | ROT13, Multi-Layer Archives, SSH Keys, Port Scanning, SSL/TLS Tunnels |
| **Level 21 → 30** | 🟢 100% Completed | Cron Jobs, Setuid/Setgid Exploitation, Shell Injections, Git History & Tags |
| **Level 31 → 33** | 🟢 100% Completed | Git Pre-Receive Hooks, PowerShell Encodings, Restricted Shell Breakouts |

---

## Level Directory

| Level | Link | Primary Concept / Skill |
|-------|------|-------------------------|
| **00 → 01** | [Level 00](./levels/level-00.md) | SSH basics & reading files (`cat`) |
| **01 → 02** | [Level 01](./levels/level-01.md) | Hyphen-prefixed filenames (`./-`) |
| **02 → 03** | [Level 02](./levels/level-02.md) | Spaces in filenames & escaping |
| **03 → 04** | [Level 03](./levels/level-03.md) | Hidden files (`ls -a`) |
| **04 → 05** | [Level 04](./levels/level-04.md) | Human-readable files (`file`) |
| **05 → 06** | [Level 05](./levels/level-05.md) | Advanced file searching (`find`) |
| **06 → 07** | [Level 06](./levels/level-06.md) | Searching across system paths by user/group |
| **07 → 08** | [Level 07](./levels/level-07.md) | Pattern matching (`grep`) |
| **08 → 09** | [Level 08](./levels/level-09.md) | Unique lines filtering (`sort \| uniq -u`) |
| **09 → 10** | [Level 09](./levels/level-10.md) | Extracting strings from binaries (`strings`) |
| **10 → 11** | [Level 10](./levels/level-10.md) | Base64 decoding (`base64 -d`) |
| **11 → 12** | [Level 11](./levels/level-11.md) | Cipher translation (`tr`) |
| **12 → 13** | [Level 12](./levels/level-12.md) | Multi-stage archive decompression (`xxd`, `tar`, `gzip`, `bzip2`) |
| **13 → 14** | [Level 13](./levels/level-13.md) | SSH private key authentication (`ssh -i`) |
| **14 → 15** | [Level 14](./levels/level-14.md) | Submitting data to local network ports (`nc`) |
| **15 → 16** | [Level 15](./levels/level-15.md) | Encrypted network communication (`openssl s_client`) |
| **16 → 17** | [Level 16](./levels/level-16.md) | Port scanning & SSL key extraction (`nmap`, `openssl`) |
| **17 → 18** | [Level 17](./levels/level-17.md) | File diffing (`diff`) |
| **18 → 19** | [Level 18](./levels/level-18.md) | Bypassing interactive SSH `.bashrc` restrictions |
| **19 → 20** | [Level 19](./levels/level-19.md) | Setuid binary execution (`setuid`) |
| **20 → 21** | [Level 20](./levels/level-20.md) | Local port listening & backgrounding (`nc -l`) |
| **21 → 22** | [Level 21](./levels/level-21.md) | Inspecting system cron tasks (`/etc/cron.d`) |
| **22 → 23** | [Level 22](./levels/level-22.md) | Deconstruct shell scripts & variable expansions |
| **23 → 24** | [Level 23](./levels/level-23.md) | Writing cron-executed privilege escalation scripts |
| **24 → 25** | [Level 24](./levels/level-24.md) | Socket brute-forcing via shell scripting |
| **25 → 26** | [Level 25](./levels/level-25.md) | Escaping `more` pager to drop into `vim` shell |
| **26 → 27** | [Level 26](./levels/level-26.md) | Setuid privilege escalation via `vim` shell command |
| **27 → 28** | [Level 27](./levels/level-27.md) | Cloning remote Git repositories over SSH |
| **28 → 29** | [Level 28](./levels/level-28.md) | Analyzing historic Git commit diffs (`git log -p`) |
| **29 → 30** | [Level 29](./levels/level-29.md) | Inspecting remote Git branches (`git branch -a`) |
| **30 → 31** | [Level 30](./levels/level-30.md) | Extracting secrets from Git tags (`git tag`, `git show`) |
| **31 → 32** | [Level 31](./levels/level-31.md) | Git `pre-receive` hooks, `.gitignore` bypass (`-f`), UTF-8 encoding |
| **32 → 33** | [Level 32](./levels/level-32.md) | Restricted shell (`uppershell`) breakout via `$0` expansion |
| **33 → 34** | [Level 33](./levels/level-33.md) | Final Level / Completion |

---

## Concepts & Tooling Breakdown

```text
       ┌────────────────────────────────────────────────────────┐
       │              BANDIT SKILL MATRIX ARCHITECTURE           │
       └───────────────────────────┬────────────────────────────┘
                                   │
      ┌────────────────────────────┼────────────────────────────┐
      ▼                            ▼                            ▼
┌──────────────┐           ┌──────────────┐           ┌──────────────────┐
│  Linux CLI   │           │ Networking & │           │    Git & Code    │
│  & Scripting │           │ Encryption   │           │    Auditing      │
├──────────────┤           ├──────────────┤           ├──────────────────┤
│ • find/grep  │           │ • SSH / SCP  │           │ • Commit Logs    │
│ • xxd/tar    │           │ • OpenSSL    │           │ • Remote Branches│
│ • cron jobs  │           │ • Netcat     │           │ • Git Tags       │
│ • Bash loops │           │ • Nmap       │           │ • Pre-Receive    │
└──────────────┘           └──────────────┘           └──────────────────┘
```
---

### Core Technologies Used

- **System Administration:** `find`, `grep`, `sort`, `uniq`, `tar`, `gzip`, `bzip2`, `xxd`, `cron`
- **Networking & Security:** `ssh`, `nc` (Netcat), `nmap`, `openssl`, system file permissions (`setuid`/`setgid`)
- **Version Control:** `git` (cloning, branch traversal, commit diffing, tag extraction, hook triggers)
- **Shells & Editors:** `bash`, `sh`, `vim`, `less`, `more`

---

## Disclaimer & Recommendations

This repository serves strictly as a **personal learning reference**.

If you are attempting Bandit yourself, avoid reading solutions beforehand. The primary value of wargames comes from independent research, reading man pages, debugging execution failures, and mastering the Linux CLI through hands-on troubleshooting.

---

### Resources

- [OverTheWire Wargames Main Page](https://overthewire.org/)
- [Bandit Official Level Documentation](https://overthewire.org/wargames/bandit/)