# Searching Files and Pipes

> How to read files, search inside them, find files, and chain commands together in the Linux terminal.

**Date:** 2026-10-16
**Source(s):** linuxjourney.com, hands-on practice in my terminal
**Tags:** `linux` `grep` `find` `pipes` `terminal` `AI`

## What it is
On Linux, most things are plain text files: logs, configuration files, lists. These commands let me read those files, search inside them, find files by name, and combine small commands into longer ones. In security, this is how an analyst searches a huge log for one suspicious event.

## How it works

### Reading files
| Command | What it does | Example |
| :-- | :-- | :-- |
| `cat` | Prints a whole file | `cat fruits.txt` |
| `less` | Scrolls through a long file (`q` to quit) | `less fruits.txt` |
| `head -n 3` | Shows the first 3 lines | `head -n 3 fruits.txt` |
| `tail -n 3` | Shows the last 3 lines | `tail -n 3 fruits.txt` |
| `wc -l` | Counts lines | `wc -l fruits.txt` |

`tail` matters for logs because new entries are added at the end.

### Searching inside files with `grep`
`grep` prints every line that contains the text I search for.

| Option | Meaning | Example |
| :-- | :-- | :-- |
| (none) | Match exact text, case sensitive | `grep "apple" fruits.txt` |
| `-i` | Ignore upper/lower case | `grep -i "banana" fruits.txt` |
| `-n` | Show line numbers | `grep -n "e" fruits.txt` |
| `-v` | Show lines that do NOT match | `grep -v "apple" fruits.txt` |
| `-c` | Count matching lines | `grep -c "apple" fruits.txt` |
| `-r` | Search all files in a folder | `grep -r "apple" ~/practice` |

If nothing matches, `grep` prints nothing. That is not an error.

### Finding files with `find`
`grep` searches inside files. `find` searches for the files themselves.

| Command | Meaning |
| :-- | :-- |
| `find ~/practice -name "*.txt"` | All files ending in `.txt` |
| `find ~/practice -iname "FRUITS.TXT"` | Same, ignoring case |
| `find ~/practice -type f` | Only files |
| `find ~/practice -type d` | Only folders |
| `find / -name "*.txt" 2>/dev/null` | Search the whole system and hide "Permission denied" errors |

`2>/dev/null` sends error messages to `/dev/null`, which discards them.

### Pipes
A pipe (`|`) sends the output of the command on the left into the command on the right.

```bash
cat fruits.txt | grep "a" | wc -l
```
1. `cat` prints the file.
2. `grep "a"` keeps lines containing "a".
3. `wc -l` counts what is left.

### Redirection
| Symbol | Meaning |
| :-- | :-- |
| `>` | Save output to a file, overwriting it |
| `>>` | Add output to the end of a file |

## Example I ran
My practice file:
```bash
printf "apple\nbanana\ncherry\napple pie\nBanana split\ndate\nelderberry\nfig\ngrape\nhoneydew\n" > fruits.txt
```

Commands and what they showed:
```bash
grep "apple" fruits.txt
# apple
# apple pie

grep -i "banana" fruits.txt
# banana
# Banana split

grep -v "apple" fruits.txt
# everything except the two apple lines

cat fruits.txt | grep "a" | wc -l
# 6
```

## Mistakes and surprises

- *grep "banana" missed "Banana split" until I added -i, because grep is case sensitive.*
- *I opened a file with less and could not get out until I pressed q.*
- *find / printed lots of "Permission denied" lines until I added 2>/dev/null.*

## Security angle
- **Log analysis:** `grep` is how analysts search logs for failed logins, IP addresses or keywords. For example, `grep "Failed password" /var/log/auth.log | wc -l` counts failed login attempts on a Linux server (log file names vary).
- **Newest events first:** `tail` shows the most recent log entries.
- **Hunting for files:** `find` can locate configuration files or files with sensitive names. Attackers use it after getting into a system, and defenders use it to find files that should not be exposed.
- **Filtering noise:** `grep -v` removes normal lines so unusual ones stand out.
- **Case matters:** a search without `-i` can miss results, so I should always think about what my search might skip.

## Key takeaways
- `grep` searches inside files, and `find` searches for files.
- A pipe lets me chain small commands into a powerful one.
- `grep -i` ignores case and `grep -v` inverts the match.
- `>` overwrites a file, but `>>` adds to it.
- `tail` and `grep` are core tools for reading logs.

## Questions I still have
- How does `grep` handle patterns more complex than plain words (regular expressions)?
- How do I search for files modified in the last day with `find`?
- How do `sort` and `uniq` help count repeated entries in a log?
