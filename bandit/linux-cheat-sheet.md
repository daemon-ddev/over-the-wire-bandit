# My Linux Cheat Sheet

I use this cheat sheet when I get stuck in the Linux terminal. I replace anything in `<angle brackets>` with my own value. Anything in `[square brackets]` is optional.

## My Method

When I get a new problem, I:

1. Read the whole problem and write down every clue.
2. Choose the command that matches the problem.
3. Fill in the command with my own values.
4. Check the output.
5. Adjust the command and try again if needed.

## Which Command Should I Use?

| I want to... | I use... |
| --- | --- |
| See hidden files | `ls -a` |
| Find a file by name, size, or owner | `find` |
| Find text inside a file | `grep` |
| Find a line that appears only once | `sort` and then `uniq -u` |
| Pull readable text from a binary file | `strings` |
| Check what kind of file I have | `file` |
| Decode Base64 text | `base64 -d` |
| Replace letters or characters | `tr` |
| Unpack a compressed file | `gzip -d`, `bzip2 -d`, or `tar xf` |
| Log in with an SSH key | `ssh -i` |
| Connect to a network port | `nc` |

## Moving Around and Reading Files

| I want to... | Pattern | Example |
| --- | --- | --- |
| Enter a folder | `cd <folder>` | `cd projects` |
| Go up one folder | `cd ..` | `cd ..` |
| Go to my home folder | `cd ~` | `cd ~` |
| See my current location | `pwd` | `pwd` |
| List files | `ls [options] [folder]` | `ls -la projects` |
| Show hidden files | `ls -a` | `ls -a` |
| Read a file | `cat <file>` | `cat notes.txt` |
| Read a file with a strange name | `cat "./<file>"` | `cat "./-notes"` |
| Read the first lines of a file | `head -n <number> <file>` | `head -n 5 notes.txt` |
| Check a file type | `file <file>` | `file notes.txt` |

## Creating and Managing Files

| I want to... | Pattern | Example |
| --- | --- | --- |
| Create a folder | `mkdir <name>` | `mkdir backup` |
| Create a private temporary folder | `mktemp -d` | `mktemp -d` |
| Copy a file | `cp <source> <destination>` | `cp notes.txt backup/` |
| Rename or move a file | `mv <old> <new>` | `mv notes.txt old-notes.txt` |
| Change file permissions | `chmod <mode> <file>` | `chmod 600 mykey` |

## Searching with `find`

I use this pattern when I need to search through a directory:

```bash
find <where to start> [filters] 2>/dev/null
```

`2>/dev/null` hides error messages, such as permission errors, so I can see the useful results more easily.

### Starting Location

| Location | Meaning |
| --- | --- |
| `.` | Start in the current folder |
| `/` | Search the whole server |
| `<path>` | Start in a specific folder |

### Common Filters

| I want to find... | Pattern | Example |
| --- | --- | --- |
| A name | `-name "<name>"` | `-name "*.log"` |
| Files only | `-type f` | `-type f` |
| Folders only | `-type d` | `-type d` |
| An exact size | `-size <number><unit>` | `-size 1000c` |
| Something bigger than a size | `-size +<number><unit>` | `-size +5M` |
| Something smaller than a size | `-size -<number><unit>` | `-size -5M` |
| A specific owner | `-user <user>` | `-user alex` |
| A specific group | `-group <group>` | `-group staff` |
| A file that is not executable | `! -executable` | `! -executable` |

### Size Units

- `c` means bytes.
- `k` means kilobytes.
- `M` means megabytes.
- `G` means gigabytes.

There is no space between the number and the unit.

For example:

```bash
find / -type f -user alex -size 1000c 2>/dev/null
```

## Searching with `grep`

| I want to... | Pattern | Example |
| --- | --- | --- |
| Show lines containing text | `grep "<pattern>" <file>` | `grep "error" app.log` |
| Ignore uppercase and lowercase differences | `grep -i "<pattern>" <file>` | `grep -i "error" app.log` |
| Show line numbers | `grep -n "<pattern>" <file>` | `grep -n "error" app.log` |
| Show lines that do not match | `grep -v "<pattern>" <file>` | `grep -v "error" app.log` |
| Count matching lines | `grep -c "<pattern>" <file>` | `grep -c "error" app.log` |
| Show lines after a match | `grep -A <number> "<pattern>" <file>` | `grep -A 2 "error" app.log` |
| Search through a whole folder | `grep -r "<pattern>" <folder>` | `grep -r "error" logs/` |

## Pipes and Redirection

A pipe sends the output from one command into another command:

```bash
<command 1> | <command 2>
```

I use redirection to save output or provide input:

```bash
<command> > <file>              # save output and overwrite the file
<command> >> <file>             # add output to the end of the file
<command> < <file>              # use a file as the command's input
<command> 2>/dev/null           # hide error messages
```

### Useful Command Chains

```bash
sort <file> | uniq -u
sort <file> | uniq -d
sort <file> | uniq -c
strings <file> | grep "<pattern>"
history | grep "<pattern>"
```

`uniq` only compares lines next to each other, so I use `sort` first when I want reliable results.

- `uniq -u` shows lines that appear once.
- `uniq -d` shows lines that are repeated.
- `uniq -c` counts how often each line appears.

## Decoding and Unpacking Files

| I want to... | Pattern | Example |
| --- | --- | --- |
| Decode Base64 | `base64 -d <file>` | `base64 -d message.txt` |
| Replace characters | `tr '<from>' '<to>' < <file>` | `tr 'a-z' 'A-Z' < notes.txt` |
| Apply ROT13 | `tr 'A-Za-z' 'N-ZA-Mn-za-m' < <file>` | `tr 'A-Za-z' 'N-ZA-Mn-za-m' < message.txt` |
| Turn a hexdump back into binary | `xxd -r <file> > <output>` | `xxd -r dump.txt > data.bin` |
| Unpack gzip | `gzip -d <file>.gz` | `gzip -d data.gz` |
| Unpack bzip2 | `bzip2 -d <file>.bz2` | `bzip2 -d data.bz2` |
| Unpack tar | `tar xf <file>.tar` | `tar xf data.tar` |

### Repeated Compression

When a file has been compressed several times, I repeat these steps:

1. Run `file <name>` to see what type of file it is.
2. Rename it to end in `.gz` or `.bz2` if that tool needs the extension. `tar` does not need a renamed extension.
3. Unpack it with the matching command.
4. Run `file` on the result and repeat until it says `ASCII text`.

## Remote Connections and Networking

| I want to... | Pattern | Example |
| --- | --- | --- |
| Log in to a server | `ssh <user>@<host> -p <port>` | `ssh alex@server.example.com -p 22` |
| Log in with a key | `ssh -i <keyfile> <user>@<host> -p <port>` | `ssh -i mykey alex@server.example.com -p 22` |
| Copy a file from a server | `scp -P <port> <user>@<host>:<remote file> <destination>` | `scp -P 22 alex@server.example.com:notes.txt .` |
| Connect to a port | `nc <host> <port>` | `nc localhost 8080` |
| Check whether a port is open | `nc -vz <host> <port>` | `nc -vz server.example.com 22` |
| Test a connection | `ping -c <count> <host>` | `ping -c 3 server.example.com` |
| Look up a DNS address | `dig +short <host>` | `dig +short server.example.com` |

### Things I Need to Remember

- `ssh` uses lowercase `-p` for the port.
- `scp` uses uppercase `-P` for the port.
- SSH keys usually need `chmod 600 <keyfile>` or SSH may refuse to use them.
- `nc` is not a shell. Anything I type is sent as data to the port.

## Terminal Shortcuts

| I want to... | I use... |
| --- | --- |
| See past commands | `history` |
| Run a past command by number | `!<number>` |
| Repeat the last command | `!!` |
| Search through past commands | Press `Ctrl+R`, then press it again for older matches |
| Edit a command I found before running it | `Ctrl+J` |
| Complete a name automatically | `Tab` |
| Stop a command that is running | `Ctrl+C` |

## When I Get Stuck

| I want to... | I use... | What it does |
| --- | --- | --- |
| Read the full manual | `man <command>` | Opens the manual for a command. Press `/` to search and `q` to quit. |
| Get a quick summary | `<command> --help` | Shows the command's available options. |

When I am stuck, I go back to the clues in the problem, check the file type, read the command help, and test one small step at a time.
