# CLI and Git Cheat Sheet

Quick reference for Linux command-line operations and Git.

> Replace placeholders such as `<PID>`, `<branch>`, and `<url>` with your actual values. Commands that delete files or terminate processes should be used carefully.

---

# Linux CLI Cheat Sheet

## Navigation and files

| Command | What it does |
|---|---|
| `pwd` | Show current directory |
| `ls` | List files and directories |
| `ls -la` | Show all files, including hidden files |
| `cd folder` | Move into a directory |
| `cd ..` | Move to the parent directory |
| `cd ~` | Go to the home directory |
| `mkdir folder` | Create a directory |
| `touch file.txt` | Create an empty file |
| `cp file.txt backup.txt` | Copy a file |
| `mv old.txt new.txt` | Move or rename a file |
| `rm file.txt` | Delete a file |
| `rm -r folder` | Delete a directory and its contents |

### Practice: create a directory and files

```bash
cd ~
mkdir d1
touch d1/a.txt
mkdir d1/d2
touch d1/d2/b.txt
```

## Viewing and searching

| Command | What it does |
|---|---|
| `cat file.txt` | Display file contents |
| `less file.txt` | Read a file page by page |
| `head file.txt` | Show the first 10 lines |
| `tail file.txt` | Show the last 10 lines |
| `tail -f file.log` | Follow a file as it changes |
| `grep "text" file.txt` | Search for text |
| `find . -name "*.py"` | Find Python files |

### Find names containing `java`

From the home directory, search names throughout the filesystem:

```bash
cd ~
sudo find / -iname '*java*' 2>/dev/null
```

- `find /` starts at the filesystem root.
- `-iname` matches names case-insensitively.
- `'*java*'` matches filenames or directory names containing `java`.
- `2>/dev/null` hides permission-denied messages.
- Without `sudo`, some directories may not be searchable.

To search only under your home directory, use `find ~ -iname '*java*'`.

## Permissions

Linux permissions have three classes:

1. **User/owner** (`u`)
2. **Group** (`g`)
3. **Others** (`o`)

Each class has three permission bits:

- Read (`r`) = 4
- Write (`w`) = 2
- Execute (`x`) = 1

Add the values to get the numeric permission for each class. For example, `7 = 4 + 2 + 1 = rwx`, `5 = 4 + 1 = r-x`, and `0 = ---`.

| Permission | Binary | Decimal |
|---|---:|---:|
| `---` | `000` | 0 |
| `--x` | `001` | 1 |
| `-w-` | `010` | 2 |
| `-wx` | `011` | 3 |
| `r--` | `100` | 4 |
| `r-x` | `101` | 5 |
| `rw-` | `110` | 6 |
| `rwx` | `111` | 7 |

### Check and change permissions

```bash
ls -l d1/a.txt
stat -c '%a %A %n' d1/a.txt
chmod 755 d1/a.txt
chmod -R 755 d1/d2
```

- `ls -l` shows symbolic permissions; `stat -c '%a'` shows the numeric mode.
- `755` means owner `rwx` (7), group `r-x` (5), others `r-x` (5).
- `-R` applies the change recursively to the directory and its contents.

**Note:** `chmod -R 755` gives directories and files the same mode. This is the requested practice command, but it also makes every file executable. A common alternative is to set directories to `755` and regular files to `644`.

## Processes and pipes

| Command | What it does |
|---|---|
| `ps aux` | List running processes |
| `ps -ef` | List processes in full format |
| `pgrep -a firefox` | Find Firefox processes and show their command lines |
| `pstree -p` | Display the process tree with PIDs |
| `kill <PID>` | Send the default termination signal (`SIGTERM`) |
| `kill -9 <PID>` | Force-kill a process (`SIGKILL`); use only if needed |

### Start Firefox and find its PID

```bash
firefox &
pgrep -a firefox
```

A **parent process** starts another process; the started process is its **child**. A process's parent PID is shown as `PPID` in `ps -ef`.

### Pipes, `grep`, `awk`, and `xargs`

A pipe (`|`) sends one command's output to the next command.

```bash
ps -ef | grep '[f]irefox'
ps -ef | awk '/[f]irefox/ {print "PID=" $2, "PPID=" $3, $0}'
pgrep -a firefox
```

To terminate Firefox gracefully by matching its process name:

```bash
pkill firefox
```

Or find its PID and pass it to `kill`:

```bash
pgrep -f firefox | xargs -r kill
```

- `awk` can select and print fields from each line.
- `xargs` turns input lines into command arguments.
- `-r` prevents `xargs` from running `kill` if there is no matching PID.

Use `pgrep -a firefox` to inspect the match first. Be cautious with `pgrep -f`, because it can match command lines beyond the browser process itself.

## User, group, and system information

| Command | What it does |
|---|---|
| `whoami` | Print the current username |
| `id` | Show user ID, primary group, and supplementary groups |
| `groups` | Show groups for the current user |
| `uname` | Show kernel name |
| `uname -a` | Show all available system information |
| `uname -m` | Show machine hardware architecture |
| `lscpu` | Show CPU architecture and details |
| `lspci` | List PCI devices |

### Find the audio device/controller

```bash
lspci | grep -i audio
```

This shows PCI devices identified as audio controllers. The actual audio driver/module can be investigated with:

```bash
lspci -k | grep -A 3 -i audio
```

Look for `Kernel driver in use` in the output.

## List home-directory contents by modification time

```bash
ls -lathr ~
```

This includes hidden files (`-a`), uses human-readable sizes (`-h`), sorts by modification time (`-t`), and reverses the order (`-r`). With `-r`, the oldest entries appear first and the newest last.

## Permissions and file safety reminders

- `rm` does not normally move files to a trash folder.
- `chmod` changes permissions; `chown` changes ownership.
- Use `sudo` only when necessary and understand the command before running it.

## Useful shortcuts

| Shortcut | What it does |
|---|---|
| `Ctrl + C` | Interrupt a running command |
| `Ctrl + L` | Clear the terminal |
| `Ctrl + R` | Search command history |
| `Tab` | Autocomplete commands and paths |
| `↑` / `↓` | Browse command history |

---
