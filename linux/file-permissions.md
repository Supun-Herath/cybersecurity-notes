# File Permissions

> How Linux controls who can read, write and run files, and how to change those permissions.

**Date:** 2026-10-20
**Source(s):** linuxjourney.com, hands-on practice in my terminal
**Tags:** `linux` `permissions` `chmod` `security`

## What it is
Every file and folder on Linux has permissions that decide who can read it, change it, or run it. This is one of the main ways Linux keeps users and programs from interfering with each other, and it is a core security control.

## How it works

### Reading `ls -l`
```
-rwxr-xr-- 1 owner group 120 Oct 20 10:00 script.sh
```
- First character: `-` is a file, `d` is a directory.
- Next 9 characters: three groups of `rwx` for **owner**, **group** and **others**.

### r, w, x
| Letter | On a file | On a directory |
| :-- | :-- | :-- |
| `r` | View contents | List contents |
| `w` | Change contents | Create or delete files inside |
| `x` | Run it | Enter it with `cd` |

### Numbers
r = 4, w = 2, x = 1. Add them for each group (owner, group, others).

| Number | Permissions |
| :-- | :-- |
| 7 | `rwx` |
| 6 | `rw-` |
| 5 | `r-x` |
| 4 | `r--` |
| 0 | `---` |

Example: `chmod 754 file` gives owner `rwx`, group `r-x`, others `r--`.

### Common modes
| Mode | Meaning | Use |
| :-- | :-- | :-- |
| 644 | `rw-r--r--` | Normal files |
| 755 | `rwxr-xr-x` | Scripts and public folders |
| 600 | `rw-------` | Private files |
| 700 | `rwx------` | Private folders |
| 777 | `rwxrwxrwx` | Dangerous, avoid |

### Commands
| Command | Meaning |
| :-- | :-- |
| `chmod 644 file` | Set permissions with numbers |
| `chmod u+x file` | Give the owner execute (symbolic) |
| `chmod g-w file` | Remove write from the group |
| `chmod a+r file` | Give everyone read |
| `sudo chown user:group file` | Change the owner and group |

## Example I ran
```bash
echo "hello" > secret.txt
chmod 000 secret.txt
cat secret.txt
# cat: secret.txt: Permission denied
chmod 644 secret.txt
cat secret.txt
# hello

printf '#!/bin/bash\necho "Hello from my script"\n' > hello.sh
./hello.sh
# Permission denied
chmod +x hello.sh
./hello.sh
# Hello from my script
```

## Mistakes and surprises
- *My script would not run until I added the execute permission.*
- *I mixed up which number goes with which group.*
- *I locked myself out of a folder with chmod 000 and had to fix it with chmod 755.*

## Security angle
- **chmod 777 is dangerous:** anyone on the system can modify the file. Attackers can replace a script with malicious code if it is world-writable.
- **Least privilege:** give only the permissions needed. Private files like SSH keys should be `600`, and SSH refuses to use keys that are too open.
- **Hunting for risky files:** `find / -type f -perm -o+w 2>/dev/null` lists world-writable files, which defenders audit and attackers look for.
- **Sensitive files:** `/etc/shadow` is restricted so normal users cannot read password hashes.
- **Execute permission matters:** a file cannot run unless it has `x`, which limits what an attacker can run from uploaded files.

## Key takeaways
- `ls -l` shows type, owner, group and permissions.
- r=4, w=2, x=1, and `chmod 755` means `rwxr-xr-x`.
- `644` for files, `755` for scripts and folders, `600` for private files.
- `777` is almost always a mistake.
- Permissions on directories mean something different (`x` lets you enter).

## Questions I still have
- What are special permissions like SUID and why do attackers care about them?
- What is `umask` and how does it set default permissions?
- How do groups work, and how do I add a user to one?
