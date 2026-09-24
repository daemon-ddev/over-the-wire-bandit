# Bandit Level 5 to Level 6

## Goal

Find the password for level 6 in the `inhere` directory. The file is a regular file with the required size and is hidden among many directories and files.

## Concept

When a directory contains many nested folders, `find` can search through the whole directory tree using specific conditions. The `-type f` option limits the results to regular files, and `-size 1033c` finds files that are exactly 1033 bytes.

The password is stored separately in [passwords.md](passwords.md) and is not included in this write-up.

## Commands Used

```bash
ls
cd inhere
find . -type f -size 1033c
cd maybehere07
ls
cat .file2
```

## What I Did

1. Ran `ls` and found the `inhere` directory.
2. Tried to run `inhere` as a command and received a command-not-found error.
3. Used `cd inhere` to enter the directory correctly.
4. Ran `file ./*`, which showed that the top-level entries were directories.
5. Used `find . -type f -size 1033c` to search through the nested directories.
6. Found the target file at `./maybehere07/.file2`.
7. Changed into `maybehere07` and used `cat .file2` to read the password.
8. Saved the password in my private local notes.

## What I Learned

- A directory must be entered with `cd`; it cannot be run as a command.
- An absolute path starts with `/`, while `./` refers to the current directory.
- `find` can search recursively through nested directories.
- `-type f` finds regular files.
- `-size 1033c` finds files that are exactly 1033 bytes.
- Hidden files begin with a period and do not appear in a normal `ls` listing.