# Linux Notes I Keep Nearby

I use these notes when I am working in a Linux terminal, especially during the Bandit challenges. The commands are examples, so I replace anything in `<angle brackets>` with my own value. Text in `[square brackets]` means that part can be left out.

## How I Work Through a Problem

I try not to guess at commands. My usual process is:

1. Read the goal twice and write down every clue.
2. Decide what kind of information I need: a filename, text, a file type, permissions, or a network connection.
3. Choose the smallest command that can answer that question.
4. Replace the placeholders with the values from the problem.
5. Read the output before changing anything.
6. If the result is not useful, check the command help and try one small adjustment.

## Quick Command Finder

When I know what I am trying to do, this is the command I check first:

| What I am trying to do | Command I start with |
| --- | --- |
| See hidden files | `ls -a` |
| Find a file using clues such as its name, size, or owner | `find` |
| Look for text inside a file | `grep` |
| Find a line that appears only once | `sort` then `uniq -u` |
| Pull readable text from a binary file | `strings` |
| Identify a file | `file` |
| Decode Base64 | `base64 -d` |
| Replace characters or change their case | `tr` |
| Unpack compressed data | `gzip -d`, `bzip2 -d`, or `tar xf` |
| Log in with a private key | `ssh -i` |
| Send data to a network port | `nc` |

## First Things I Check

These are the commands I reach for when I first arrive in a directory:

| What I need to know | Command | Example |
| --- | --- | --- |
| Where I am | `pwd` | `pwd` |
| What is here | `ls [options] [folder]` | `ls -la projects` |
| Whether hidden files are present | `ls -a` | `ls -a` |
| The contents of a file | `cat <file>` | `cat notes.txt` |
| The first few lines | `head -n <number> <file>` | `head -n 5 notes.txt` |
| What kind of file it is | `file <file>` | `file notes.txt` |

If a filename starts with a dash, I make its path explicit so the command does not treat the name as an option:

```bash
cat "./-notes"
```

## Moving Around

```bash
cd <folder>     # enter a folder
cd ..           # go up one folder
cd ~            # go to my home folder
```

## Working with Files and Folders

| Task | Command | Example |
| --- | --- | --- |
| Create a folder | `mkdir <name>` | `mkdir backup` |
| Create a private temporary folder | `mktemp -d` | `mktemp -d` |
| Copy a file | `cp <source> <destination>` | `cp notes.txt backup/` |
| Move or rename a file | `mv <old> <new>` | `mv notes.txt old-notes.txt` |
| Change permissions | `chmod <mode> <file>` | `chmod 600 mykey` |

## Finding a File

I use `find` when the problem gives me clues about a file's name, size, owner, group, or permissions.

```bash
find <starting-place> [filters] 2>/dev/null
```

Useful starting places:

- `.` searches from my current directory.
- `/` searches the whole server.
- `<path>` searches one specific folder.

Useful filters:

```bash
-name "<name>"          # match a name, such as "*.log"
-type f                 # files only
-type d                 # folders only
-size <number><unit>    # exact size
-size +<number><unit>   # larger than a size
-size -<number><unit>   # smaller than a size
-user <user>            # owned by a user
-group <group>          # owned by a group
! -executable           # not executable
```

The size units are `c` for bytes, `k` for kilobytes, `M` for megabytes, and `G` for gigabytes. I do not put a space between the number and the unit.

For example:

```bash
find / -type f -user alex -size 1000c 2>/dev/null
```

## Finding Text

I use `grep` when I know what text I am looking for:

```bash
grep "<pattern>" <file>                    # matching lines
grep -i "<pattern>" <file>                 # ignore case
grep -n "<pattern>" <file>                 # include line numbers
grep -v "<pattern>" <file>                 # lines without a match
grep -c "<pattern>" <file>                 # count matching lines
grep -A <number> "<pattern>" <file>        # lines after a match
grep -r "<pattern>" <folder>               # search a folder
```

Other commands help when the clue is about the contents rather than an exact word:

```bash
strings <file> | grep "<pattern>"          # readable text from a binary
sort <file> | uniq -u                       # lines that appear once
sort <file> | uniq -d                       # lines that repeat
sort <file> | uniq -c                       # count each line
```

`uniq` only compares neighbouring lines, so I sort the input first. That puts matching lines beside each other.

## Passing Output Between Commands

The `|` symbol sends one command's output into the next command:

```bash
<command 1> | <command 2>
```

I use redirection when I want to save output or use a file as input:

```bash
<command> > <file>       # write output and replace the file
<command> >> <file>      # add output to the end of the file
<command> < <file>       # read input from the file
<command> 2>/dev/null    # hide error messages
```

For example, `history | grep "<pattern>"` searches my previous commands.

## Decoding or Unpacking Something

Before I choose an unpacking command, I run `file <name>`. The file type tells me what to try next.

| What I found | Command | Example |
| --- | --- | --- |
| Base64 text | `base64 -d <file>` | `base64 -d message.txt` |
| Characters that need replacing | `tr '<from>' '<to>' < <file>` | `tr 'a-z' 'A-Z' < notes.txt` |
| ROT13 text | `tr 'A-Za-z' 'N-ZA-Mn-za-m' < <file>` | `tr 'A-Za-z' 'N-ZA-Mn-za-m' < message.txt` |
| A hexdump | `xxd -r <file> > <output>` | `xxd -r dump.txt > data.bin` |
| Gzip data | `gzip -d <file>.gz` | `gzip -d data.gz` |
| Bzip2 data | `bzip2 -d <file>.bz2` | `bzip2 -d data.bz2` |
| A tar archive | `tar xf <file>.tar` | `tar xf data.tar` |

For a file compressed several times, I repeat this loop:

1. Run `file <name>`.
2. If necessary, rename the file so it ends in `.gz` or `.bz2`. Tar does not need that rename.
3. Use the command that matches the file type.
4. Run `file` on the new result and repeat.
5. Stop when the result is reported as `ASCII text`.

## Connecting to Another Machine

For Bandit, the basic SSH shape is:

```bash
ssh <user>@<host> -p <port>
```

Other connection commands I want to remember:

```bash
ssh -i <keyfile> <user>@<host> -p <port>       # use a key
scp -P <port> <user>@<host>:<remote file> .    # copy a remote file here
nc <host> <port>                                # send data to a port
nc -vz <host> <port>                            # check a port
ping -c <count> <host>                          # test a connection
dig +short <host>                               # look up an address
```

Things that are easy to mix up:

- SSH uses lowercase `-p` for its port.
- SCP uses uppercase `-P` for its port.
- A private key may need `chmod 600 <keyfile>` before SSH will accept it.
- `nc` is not a shell. What I type is sent as data to the port.

## Shortcuts I Use

| What I want to do | Shortcut or command |
| --- | --- |
| See previous commands | `history` |
| Run a command by its history number | `!<number>` |
| Repeat the previous command | `!!` |
| Search command history | Press `Ctrl+R`; press it again for older matches |
| Edit a found command before running it | `Ctrl+J` |
| Complete a filename or command | `Tab` |
| Stop a running command | `Ctrl+C` |

## When I Need Help

```bash
man <command>       # open the full manual
<command> --help    # see a shorter list of options
```

Inside a manual, I press `/`, type a word, and press Enter to search. I press `q` to quit.

When I get stuck, I go back to the original clues, check the file type, read the command help, and test one small step at a time.
