# Bandit Level 4 to Level 5

## Goal

Find the password for level 5 in the only human-readable file inside the `inhere` directory, then use it to log in to `bandit5` over SSH.

## Concept

The `inhere` directory contains several files with different file types. The `file` command identifies the type and contents of a file, which makes it useful for finding the one that contains readable text. Because some filenames begin with a hyphen, the paths should be prefixed with `./` so they are treated as filenames instead of command options.

The password is stored separately in [passwords.md](passwords.md) and is not included in this write-up.

## Commands Used

```bash
cd inhere
file ./-file*
cat ./-file07
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

## What I Did

1. Started from the `bandit4` SSH session.
2. Changed into the `inhere` directory.
3. Used `file ./-file*` to check the type of every file.
4. Identified the only file containing human-readable text.
5. Used `cat` to read the password from that file.
6. Saved the password in my private local notes.
7. Used the password to log in as `bandit5` on port `2220`.

## What I Learned

- The `file` command identifies the type of a file.
- Wildcards such as `*` can match multiple filenames.
- The `./` prefix prevents filenames beginning with `-` from being treated as options.
- Checking file types is useful when a directory contains many files and only one has the needed content.