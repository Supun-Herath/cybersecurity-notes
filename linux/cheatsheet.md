# Linux Cheat Sheet

A quick reference for the commands I have learned and practiced.

## Navigation and files
| Command | What it does |
| :-- | :-- |
| `pwd` | Show current folder |
| `ls -la` | List all files with details (including hidden) |
| `cd folder` / `cd ..` / `cd ~` | Enter a folder / go up / go home |
| `mkdir name` / `mkdir -p a/b/c` | Make a folder / nested folders |
| `touch file` | Create an empty file |
| `cp a b` | Copy a file |
| `mv a b` | Move or rename a file |
| `rm file` | Delete a file (permanent) |

## Reading and searching
| Command | What it does |
| :-- | :-- |
| `cat file` | Print a file |
| `less file` | Scroll through a file (`q` to quit) |
| `head -n 5 file` / `tail -n 5 file` | First / last lines |
| `wc -l file` | Count lines |
| `grep "text" file` | Find lines containing text |
| `grep -i` / `-n` / `-v` / `-c` | Ignore case / line numbers / invert / count |
| `find path -name "*.txt"` | Find files by name |

## Pipes and redirection
| Syntax | What it does |
| :-- | :-- |
| `cmd1 \| cmd2` | Send output of cmd1 into cmd2 |
| `cmd > file` | Save output to a file (overwrites) |
| `cmd >> file` | Add output to the end of a file |
| `2>/dev/null` | Hide error messages |

## Permissions
| Command | What it does |
| :-- | :-- |
| `ls -l` | Show permissions, owner and group |
| `chmod 755 file` | Set permissions with numbers (r=4, w=2, x=1) |
| `chmod u+x file` | Give the owner execute |
| `sudo chown user:group file` | Change owner and group |

Common modes: `644` files, `755` scripts and folders, `600` private files, never `777`.

## Users and processes
| Command | What it does |
| :-- | :-- |
| `whoami` / `id` | Who I am / my IDs and groups |
| `sudo cmd` | Run one command as admin |
| `ps aux` | List processes |
| `top` | Live process view (`q` to quit) |
| `kill PID` / `kill -9 PID` | Stop a process / force stop |

## System info
| Command | What it does |
| :-- | :-- |
| `uname -a` | System and kernel information |
| `df -h` | Disk space |
| `free -h` | Memory usage |

## Networking
| Command | What it does |
| :-- | :-- |
| `ip a` | Show interfaces and addresses |
| `ping -c 4 host` | Test if a host replies |
| `traceroute host` | Show the path to a host |
| `dig host` | DNS lookup |
| `curl -I url` | Show HTTP headers |
| `ss -tuln` | List listening ports |

## Safety rules
- Use `rm` carefully since there is no undo. Never run `rm -rf` unless I am sure.
- Avoid `chmod 777`.
- Use `sudo` only when needed.
- Remove IP addresses, MAC addresses, hashes and flags before publishing notes.
- Only test my own machines and legal practice platforms.

## Links to my notes
- [Navigation and files](navigation-and-files.md)
- [Searching and pipes](searching-and-pipes.md)
- [File permissions](file-permissions.md)
- [Users and processes](users-and-processes.md)
- [Network commands](network-commands.md)
