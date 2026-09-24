# My Linux and Bandit Cheat Sheet

I use this as a detailed reference while working through OverTheWire Bandit. It explains the Linux and terminal commands, important options, and ideas I need to finish the game. It does not contain passwords or complete level solutions.

Replace anything in `<angle brackets>` with my own value. Anything in `[square brackets]` is optional.

## How I Solve a Problem

1. Read the goal carefully and write down every clue.
2. Decide what kind of information I need: a filename, file type, text, permission, user, process, network service, or Git history.
3. Choose the smallest command that can answer that question.
4. Check the command's help if I do not understand an option.
5. Run the command and read the output before changing anything.
6. Save the next password in my private notes, not in a public walkthrough.
7. Explain what I learned after the level works.

## Command Basics

Most commands follow this shape:

```bash
command [options] [arguments]
```

- The command is the program I want to run.
- An option changes how the command behaves. Options often start with `-` or `--`.
- An argument is the file, folder, text, host, or value the command should use.
- A path tells Linux where something is located.

Examples:

```bash
ls -la /tmp
head -n 5 notes.txt
find . -type f -name "*.log"
```

Quotes keep spaces and special characters together. I use `--` when I need to tell a command that the remaining text is a filename rather than an option:

```bash
cat -- "-notes"
```

`./-notes` also works because it gives the file an explicit path.

## My First Checks

```bash
pwd
ls
ls -la
file <name>
```

- `pwd` prints my current directory.
- `ls` lists the current directory.
- `ls -a` includes hidden names beginning with `.`.
- `ls -l` shows a long listing with permissions, owner, group, size, and time.
- `ls -la` combines both useful options.
- `file` checks the contents of a file and reports its type. It does not trust the filename extension.

Useful navigation commands:

```bash
cd <folder>
cd ..
cd ~
cd -
```

- `cd <folder>` enters a folder.
- `cd ..` goes up one directory.
- `cd ~` goes to my home directory.
- `cd -` returns to the previous directory.

## Creating, Copying, and Reading Files

```bash
mkdir <folder>
mktemp -d
cp <source> <destination>
mv <old-name> <new-name>
cat <file>
head -n <number> <file>
```

- `mkdir` creates a directory.
- `mktemp -d` creates a uniquely named temporary directory.
- `cp` copies a file. `cp -r` copies a directory and its contents.
- `mv` moves or renames a file or directory.
- `cat` prints a complete file.
- `head -n 5` prints the first five lines.

I use `cat` for short files. For a long file, I use `less` or `more` so the output does not disappear above the terminal:

```bash
less <file>
more <file>
```

I press `q` to quit either viewer. In `less`, I can press `/`, type a search word, and press Enter.

## Permissions and Ownership

I inspect permissions with:

```bash
ls -l <file>
```

A listing may begin like this:

```text
-rwxr-x---  user  group  123  Sep 24 12:00 script.sh
```

The first character tells me the type:

- `-` is a regular file.
- `d` is a directory.
- `l` is a symbolic link.

The next nine characters are three groups:

```text
-rwxr-x---
  user group other
```

Each group can contain:

- `r`: read. For a file, I can view its contents.
- `w`: write. For a file, I can change its contents.
- `x`: execute. For a file, I can run it. For a directory, I can enter it and access items when other permissions allow it.
- `-`: that permission is missing.

The groups mean:

1. `user`: the file owner.
2. `group`: members of the file's group.
3. `other`: everyone else.

### Numeric `chmod` Permissions

The numeric values are:

| Value | Permission |
| --- | --- |
| `4` | read (`r`) |
| `2` | write (`w`) |
| `1` | execute (`x`) |
| `0` | no permission |

I add values within each group:

| Number | Letters | Calculation |
| --- | --- | --- |
| `7` | `rwx` | `4 + 2 + 1` |
| `6` | `rw-` | `4 + 2` |
| `5` | `r-x` | `4 + 1` |
| `4` | `r--` | `4` |
| `0` | `---` | `0` |

There are normally three digits: owner, group, and other.

```bash
chmod 600 mykey
```

`600` means:

- Owner: `6`, so read and write.
- Group: `0`, so no permissions.
- Other: `0`, so no permissions.

This is a common permission for a private SSH key. SSH may reject a key that other users can read.

More examples:

| Command | Result |
| --- | --- |
| `chmod 644 <file>` | Owner reads/writes; group and other read only |
| `chmod 700 <folder>` | Owner can read, write, and enter; nobody else has access |
| `chmod 755 <program>` | Owner can read/write/execute; others can read/execute |
| `chmod 400 <key>` | Owner can read; nobody can write or access it |
| `chmod 600 <file>` | Only the owner can read and write |

### Symbolic `chmod`

I can change one permission without replacing all permissions:

```bash
chmod u+x <file>
chmod g-w <file>
chmod o-r <file>
chmod a+r <file>
```

- `u` means user or owner.
- `g` means group.
- `o` means other.
- `a` means all three groups.
- `+` adds a permission.
- `-` removes a permission.
- `=` sets permissions exactly.

## Finding Files with `find`

I use `find` when the problem gives me clues about a name, type, size, owner, group, or permission.

```bash
find <starting-place> [filters] [action] 2>/dev/null
```

Starting places:

- `.` means the current directory.
- `/` means the whole filesystem.
- `<path>` means one specific directory.

Common filters:

```bash
-name "<pattern>"
-type f
-type d
-size <number><unit>
-user <user>
-group <group>
-readable
-writable
-executable
! -executable
```

- `-name` matches a filename. Wildcards should be quoted, such as `"*.log"`.
- `-type f` finds regular files.
- `-type d` finds directories.
- `-size` checks size.
- `-user` checks the owner.
- `-group` checks the group.
- `-readable`, `-writable`, and `-executable` check my access.
- `!` means not.

Size units:

- `c`: bytes.
- `k`: kilobytes.
- `M`: megabytes.
- `G`: gigabytes.

`-size 1000c` means exactly 1000 bytes. `-size +5M` means larger than five megabytes. `-size -5M` means smaller than five megabytes.

Examples:

```bash
find . -type f -name "*.txt"
find / -type f -user alex -size 1000c 2>/dev/null
find . -type f ! -executable
find . -type f -size 1033c -readable
```

`2>/dev/null` sends error output to `/dev/null`, which discards it. This hides permission errors while searching.

## Finding Text with `grep`

```bash
grep "<pattern>" <file>
grep -i "<pattern>" <file>
grep -n "<pattern>" <file>
grep -v "<pattern>" <file>
grep -c "<pattern>" <file>
grep -A <number> "<pattern>" <file>
grep -B <number> "<pattern>" <file>
grep -C <number> "<pattern>" <file>
grep -r "<pattern>" <folder>
```

- No option shows matching lines.
- `-i` ignores upper and lower case.
- `-n` includes line numbers.
- `-v` shows lines that do not match.
- `-c` counts matching lines.
- `-A` shows lines after a match.
- `-B` shows lines before a match.
- `-C` shows lines before and after a match.
- `-r` searches through a directory.

Examples:

```bash
grep "password" notes.txt
grep -n "error" app.log
grep -r "bandit" .
```

## Sorting, Counting, and Extracting Text

`sort` puts lines in order. `uniq` compares neighbouring lines, so I normally sort first.

```bash
sort <file>
sort <file> | uniq -u
sort <file> | uniq -d
sort <file> | uniq -c
```

- `uniq -u` prints lines that appear once.
- `uniq -d` prints lines that repeat.
- `uniq -c` prints a count beside each line.

Another useful tool:

```bash
strings <binary>
diff <file1> <file2>
```

- `strings` extracts readable text from binary data.
- `diff` shows how two files differ.

## Pipes and Redirection

A pipe sends standard output from one command to standard input of another:

```bash
<command 1> | <command 2>
```

Examples:

```bash
strings data.bin | grep "password"
cat data.txt | sort | uniq -u
history | grep "ssh"
```

Redirection controls where output and input go:

```bash
<command> > <file>
<command> >> <file>
<command> < <file>
<command> 2> <error-file>
<command> 2>/dev/null
```

- `>` writes standard output and replaces the destination.
- `>>` adds standard output to the end of the destination.
- `<` gives standard input from a file.
- `2>` redirects standard error.
- `/dev/null` discards whatever is sent there.

A single `>` can erase a file, so I check the command before using it.

## Shell Variables and Command Substitution

A variable stores a value:

```bash
name="Bandit"
echo "$name"
```

- Assignment has no spaces around `=`.
- `$name` reads the value.
- Double quotes protect spaces while still allowing variables to expand.
- Single quotes keep text literal.

Command substitution puts command output inside another command:

```bash
next_password=$(cat <file>)
echo "$next_password"
```

Useful environment commands:

```bash
echo "$HOME"
echo "$PATH"
env
export NAME=value
```

- `$HOME` is my home directory.
- `$PATH` is the list of directories searched for commands.
- `env` displays environment variables.
- `export` makes a variable available to programs started from the current shell.

## Decoding and Transforming Data

### Base64

Base64 is an encoding, not encryption. I decode it with:

```bash
base64 -d <file>
```

I can save the result with `base64 -d <file> > decoded.txt`.

### Character Translation

`tr` translates characters from one set to another:

```bash
tr 'a-z' 'A-Z' < notes.txt
tr 'A-Za-z' 'N-ZA-Mn-za-m' < message.txt
```

The first command changes lowercase letters to uppercase. The second applies ROT13. `< notes.txt` gives the file to `tr` as standard input.

### Hexdumps

A hexdump shows binary data as hexadecimal text. To turn it back into binary:

```bash
xxd -r dump.txt > data.bin
```

- `xxd` creates or reads hexadecimal dumps.
- `-r` reverses a dump back into binary.
- `>` saves the binary result to a new file.

## Compression and Archives

First I identify the file:

```bash
file <name>
```

Then I use the matching tool:

```bash
gzip -d <file>.gz
bzip2 -d <file>.bz2
tar xf <file>.tar
```

- `gzip -d` decompresses gzip data.
- `bzip2 -d` decompresses bzip2 data.
- `tar xf` extracts an archive. `x` means extract and `f` means use the named file.

A file extension is not always trustworthy. If a tool needs a matching extension, I can rename the file first:

```bash
mv <file> <file>.gz
gzip -d <file>.gz
```

For repeated compression, I run `file`, rename if needed, unpack, and run `file` again. I stop when the result is readable text such as `ASCII text`.

## SSH and Remote Access

Bandit uses SSH on port `2220`:

```bash
ssh bandit<level>@bandit.labs.overthewire.org -p 2220
```

General form:

```bash
ssh <user>@<host> -p <port>
```

- `ssh` starts a secure remote shell.
- `<user>@<host>` identifies the account and server.
- `-p` chooses the port. SSH uses lowercase `-p`.

To use a private key:

```bash
chmod 600 <keyfile>
ssh -i <keyfile> <user>@<host> -p <port>
```

- `-i` selects the identity file.
- The key should be private and have restrictive permissions.

To run one command remotely without opening an interactive shell:

```bash
ssh <user>@<host> -p <port> <command>
```

## Network Connections

Netcat connects to a TCP or UDP service:

```bash
nc <host> <port>
nc -vz <host> <port>
```

- `nc` sends and receives raw network data.
- `-v` gives more information.
- `-z` checks a port without sending normal data.
- Netcat is not a shell. What I type is sent to the service.

For TLS services:

```bash
openssl s_client -connect <host>:<port>
```

- `openssl` provides cryptography tools.
- `s_client` acts as a TLS client.
- `-connect` gives the host and port in `host:port` form.

For a port range:

```bash
nmap -p <start>-<end> <host>
```

- `nmap` scans hosts and ports.
- `-p` chooses which port or range to scan.

## Users, Processes, and Setuid

```bash
whoami
id
su <user>
```

- `whoami` prints my current username.
- `id` shows my user ID, group ID, and group memberships.
- `su <user>` switches to another user when I have the required password or permission.

A setuid program runs with the permissions of its owner. In `ls -l`, an `s` in the owner's execute position can show setuid:

```text
-rwsr-xr-x
```

I inspect setuid files with:

```bash
find <path> -type f -perm -4000 2>/dev/null
```

The `4000` permission bit represents setuid. I treat these programs carefully because they may read or change files my normal account cannot access.

## Cron and Scheduled Jobs

Cron runs commands on a schedule. I may inspect jobs with:

```bash
crontab -l
ls -la /etc/cron.d
cat /etc/cron.d/<job>
```

- `crontab -l` lists my own scheduled jobs.
- `/etc/cron.d` can contain system job definitions when I have permission to read it.
- A job may run as another user.

A cron schedule usually has five time fields followed by a command:

```text
minute hour day-of-month month day-of-week command
```

The important questions are which user runs the job, which script it runs, and whether I can modify that script or anything it calls.

## Shell Scripts and Loops

A shell script is a text file containing commands. A simple loop looks like this:

```bash
for value in one two three; do
    printf '%s\n' "$value"
done
```

- `for` begins the loop.
- `value` is the variable that changes.
- `in` supplies the values.
- `do` begins the repeated commands.
- `done` ends the loop.
- `printf` prints predictable output.

A numeric range can come from `seq`:

```bash
for number in $(seq 0 9); do
    printf '%s\n' "$number"
done
```

For zero-padded values:

```bash
for number in $(seq -w 0000 9999); do
    printf '%s\n' "$number"
done
```

`-w` pads the numbers to the same width. I only automate requests when the challenge requires it and I keep the loop limited to the stated task.

## Git Repositories

Some later levels hide information in Git history instead of the current working files:

```bash
git clone <repository-url>
git status
git log
git log --all --oneline
git show <commit>
git branch -a
git switch <branch>
git tag
git show <tag>
git diff <commit1> <commit2>
git push origin <branch>
```

- `git clone` copies a repository locally.
- `git status` shows changed and untracked files.
- `git log` shows commits.
- `--all` includes commits reachable from all references.
- `--oneline` makes the history compact.
- `git show` displays a commit or tag.
- `git branch -a` lists local and remote branches.
- `git switch` changes branches.
- `git tag` lists named points in history.
- `git diff` compares versions.
- `git push` sends a local branch to a remote repository.

When investigating a repository, I check commits, branches, tags, and older versions. A secret may have been removed from the current files but still exist in history.

## Interactive Programs and Shell Escapes

Some commands open an interactive screen:

- Press `q` to quit `man`, `less`, or `more`.
- In `vim`, press `Esc` to leave insert mode.
- In `vim`, type `:q!` and press Enter to quit without saving.
- In `vim`, type `:set shell=/bin/bash`, then use `:shell` when a challenge specifically requires a shell escape.

A shell escape starts another shell from inside a program. I use it only when the level's behavior makes it relevant and I read the instructions carefully.

## Terminal Shortcuts

| Action | Shortcut or command |
| --- | --- |
| Show command history | `history` |
| Run a command by history number | `!<number>` |
| Repeat the previous command | `!!` |
| Search command history | `Ctrl+R` |
| Edit a found history command | `Ctrl+J` |
| Complete a command or filename | `Tab` |
| Stop a running command | `Ctrl+C` |

## Getting Help

```bash
man <command>
<command> --help
type <command>
```

- `man` opens the full manual.
- `--help` usually gives a shorter option list.
- In a manual, `/` starts a search and `q` quits.
- `type` tells me how the shell interprets a command.

## Bandit Coverage Map

This is the kind of knowledge each part of the game uses. The exact challenge wording may change, so I still read every level carefully.

| Levels | Main ideas to review |
| --- | --- |
| 0 to 6 | SSH, `ls`, hidden files, unusual filenames, `cat`, `find`, permissions |
| 7 to 11 | `grep`, `sort`, `uniq`, `strings`, Base64, `tr` |
| 12 | `file`, `xxd`, gzip, bzip2, tar, repeated unpacking |
| 13 to 16 | SSH keys, `chmod`, `nc`, `openssl`, `nmap` |
| 17 to 19 | `diff`, SSH commands, setuid programs |
| 20 to 24 | `nc`, setuid behavior, cron, scripts, variables, loops |
| 25 to 26 | SSH behavior, `more`, terminal size, `vim`, shell escapes |
| 27 to 31 | Git repositories, commits, branches, tags, diffs, pushing |
| 32 to 33 | Shell parsing, uppercase commands, environment variables, escaping |

## When I Get Stuck

I return to the level's clues and ask:

1. What file, text, owner, size, permission, process, or port is the clue describing?
2. Have I checked the file type with `file`?
3. Am I searching the right directory?
4. Did I quote the filename or pattern correctly?
5. Did I use the correct option spelling and capitalization?
6. Is the output encoded, compressed, binary, or stored in Git history?
7. Do I need to inspect permissions, a scheduled job, a process, or a network service?
8. Have I read `man <command>` or `<command> --help`?

I test one small step at a time and keep passwords out of public notes.
