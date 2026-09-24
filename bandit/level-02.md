# Bandit Level 1 to Level 2

## Goal

Find the password for level 2 in a file named `-` in the home directory, then use it to log in to `bandit2` over SSH.

## Concept

The hyphen is a valid filename, but a bare `-` can have a special meaning in Unix commands. For example, `cat -` usually means that `cat` should read from standard input. Prefixing the filename with `./` tells `cat` to open the file named `-` in the current directory.

The password is stored separately in [passwords.md](passwords.md) and is not included in this write-up.

## Commands Used

```bash
ls
cat ./-
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

## What I Did

1. Started from the `bandit1` SSH session.
2. Ran `ls` to list the files in the home directory.
3. Saw that the password was stored in a file named `-`.
4. Ran `cat ./-` so the hyphen was treated as a filename instead of a special argument.
5. Saved the password in my private local notes.
6. Used the password to log in as `bandit2` on port `2220`.

## What I Learned

- A hyphen can be used as a filename, but it may also have a special meaning in Unix commands.
- The `./` prefix explicitly refers to a file in the current directory.
- `cat ./-` reads the contents of a file named `-`.
- Special filenames require careful command syntax.