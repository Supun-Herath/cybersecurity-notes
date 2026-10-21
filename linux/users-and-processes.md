# Users and Processes

> How to see who I am, what is running on the system, and how to stop a process.

**Date:** 2026-10-21
**Source(s):** linuxjourney.com, hands-on practice in my terminal
**Tags:** `linux` `users` `processes` `security`

## What it is
Linux is a multi-user system, and every program that runs is a process owned by a user. Knowing who I am, what is running, and who owns it is basic to administering and investigating a system.

## How it works

### Users
| Command | What it does |
| :-- | :-- |
| `whoami` | Shows my username |
| `id` | Shows my user ID, group ID and groups |
| `sudo command` | Runs one command with admin rights |
| `su username` | Switches to another user |

The admin account is **root** (user ID 0). It can do anything.

### Processes
| Command | What it does |
| :-- | :-- |
| `ps aux` | Lists all running processes |
| `top` | Live view of processes (`q` to quit) |
| `kill PID` | Asks a process to stop |
| `kill -9 PID` | Forces a process to stop |

A **PID** is a process ID, a number that identifies a running process.

### System info
| Command | What it does |
| :-- | :-- |
| `uname -a` | System and kernel information |
| `df -h` | Disk space |
| `free -h` | Memory usage |

### Important files
- `/etc/passwd`: lists user accounts (name, user ID, home folder, shell). Readable by everyone.
- `/etc/shadow`: stores password hashes. Readable only by root.

## Example I ran
```bash
whoami
id
sleep 300 &
ps aux | grep sleep
kill <PID>
cat /etc/shadow
# cat: /etc/shadow: Permission denied
```

## Mistakes and surprises
- *grep showed up in its own ps results.*
- *I forgot the PID changes every time I start a process.*
- *cat /etc/shadow was denied because only root can read it.*

## Security angle
- **Least privilege:** use a normal user day to day and `sudo` only when needed, so a mistake or attack does less damage.
- **Spotting suspicious processes:** `ps aux` and `top` help find unknown programs, odd names, or high CPU use that could signal malware or a crypto miner.
- **`/etc/passwd` is readable by all:** attackers use it to list user accounts, so it contains no passwords.
- **`/etc/shadow` is protected:** it holds password hashes, which attackers try to crack if they can read them.
- **Root is the main target:** gaining root access means full control of the system.

## Key takeaways
- `whoami` and `id` tell me who I am and what groups I'm in.
- `sudo` gives temporary admin rights, and root is the all-powerful account.
- `ps aux` and `top` show running processes, and `kill PID` stops one.
- `/etc/shadow` is root-only because it contains password hashes.

## Questions I still have
- What is the difference between a process, a service and a daemon?
- How do I see which user owns a suspicious process?
- How do I see who has logged in recently?
