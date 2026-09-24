# Bandit Level 2 to Level 3

## Goal

Find the password for level 3 in a file named `--spaces in this filename--` in the home directory, then use it to log in to `bandit3` over SSH.

## Concept

Filenames can contain spaces and characters that normally have special meanings in the terminal. Quoting the entire filename tells the shell to treat it as one path and preserves the spaces and hyphens exactly as they appear.

The password is stored separately in [passwords.md](passwords.md) and is not included in this write-up.

## Commands Used

```bash
cat "./--spaces in this filename--"
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

## What I Did

1. Started from the `bandit2` SSH session.
2. Used `cat` with the filename surrounded by double quotes.
3. Added `./` to identify the file in the current directory.
4. Read the password from `--spaces in this filename--`.
5. Saved the password in my private local notes.
6. Used the password to log in as `bandit3` on port `2220`.

## What I Learned

- Quotation marks keep a filename with spaces together as one argument.
- The `./` prefix identifies a file in the current directory.
- Quoting also prevents leading hyphens in the filename from being interpreted as command options.
- Shell commands must account for spaces and special characters in filenames.