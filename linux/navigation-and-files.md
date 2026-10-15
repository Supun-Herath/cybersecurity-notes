# Linux Navigation and Files

> The basic commands for moving around the file system and managing files from a Linux terminal.

**Date:** 2026-10-15
**Source(s):** TryHackMe - Linux Fundamentals Part 1
**Tags:** `linux` `terminal` `basics`

## What it is
The terminal is a text-based way to control a computer. Instead of clicking folders and files, you type commands. Most servers and security tools run on Linux, so being comfortable in the terminal is a core skill in cybersecurity.

## How it works
Linux organizes everything in a tree of folders, starting from the root `/`. Your own files usually live in your home folder (`~`). At any moment the terminal is "inside" one folder, called the current working directory, and commands act relative to it.

| Command | What it does | Example |
| :-- | :-- | :-- |
| `pwd` | Prints the folder I'm currently in | `pwd` |
| `ls` | Lists files and folders | `ls` |
| `ls -la` | Lists everything, including hidden files, with details | `ls -la` |
| `cd` | Changes to another folder | `cd Documents` |
| `cd ..` | Goes up one folder | `cd ..` |
| `cd ~` | Goes to my home folder | `cd ~` |
| `mkdir` | Creates a folder | `mkdir practice` |
| `mkdir -p` | Creates nested folders in one go | `mkdir -p one/two/three` |
| `touch` | Creates an empty file | `touch a.txt` |
| `cp` | Copies a file | `cp a.txt c.txt` |
| `mv` | Moves or renames a file | `mv b.txt d.txt` |
| `rm` | Deletes a file permanently | `rm c.txt` |
| `rmdir` | Deletes a folder permanently | `rm one` |


**Useful ideas:**
- A path starting with `/` is **absolute** (from the root). A path without it is **relative** (from where I am now).
- `.` means "this folder" and `..` means "the parent folder".
- Files and folders starting with `.` are **hidden** and only show with `ls -a`.
- Pressing **Tab** auto-completes names, and the **Up arrow** brings back previous commands.

## Example I ran
```bash
pwd
ls
cd ~
mkdir practice
cd practice
touch a.txt b.txt
ls
cp a.txt c.txt
mv b.txt d.txt
ls -la
mkdir -p one/two/three
cd one/two/three
pwd
cd ../../..
rm c.txt
ls
```

What happened:
- `pwd` showed my current folder, and `ls` listed what was in it.
- `mkdir` and `touch` created the `practice` folder and two empty files.
- `cp` made a copy named `c.txt`, and `mv` renamed `b.txt` to `d.txt`.
- `ls -la` showed file details such as permissions, owner, size and date.
- `rm c.txt` removed the file immediately, with no confirmation and no recycle bin.


## Mistakes and surprises
- I tried `cd` into a folder that didn't exist and got "No such file or directory". I fixed it by checking the name with `ls`.
- Linux is **case sensitive**, so `Practice` and `practice` are different folders.
- create a nested folders like `mkdir -p one/two/three` and try to delete it from home directory gives me an error rm: cannot remove 'one' is a directory. so I learn `rm` command only remove files. If you want remove a folder use `rmdir`

## Security angle
- **Hidden files:** files starting with `.` often hold configuration, history or credentials. Attackers look for them, and defenders should check them too.
- **Permanent deletion:** `rm` has no undo. A careless command (especially `rm -rf`) can destroy data, so I only run it in a practice folder.
- **Knowing where you are:** `pwd` and `ls` before a destructive command prevents mistakes in the wrong folder.
- **Paths matter:** in security, knowing where files live (like home folders and system folders) helps with investigating a system.

## Key takeaways
- `pwd`, `ls` and `cd` are how I find my way around.
- `mkdir`, `touch`, `cp`, `mv` and `rm` create, copy, move and delete things, and `rm` is permanent.
- Hidden files (starting with `.`) exist and are worth checking with `ls -la`.

## Questions I still have
- What do the columns in `ls -la` mean exactly? (This leads into Day 4: permissions.)
- How do I search for a file when I don't know which folder it's in? (Day 3.)
