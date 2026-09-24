# Bandit Level 3 to Level 4

## Goal

Find the password for level 4 in a hidden file inside the `inhere` directory, then use it to log in to `bandit4` over SSH.

## Concept

Files whose names begin with a period are hidden by default. The `-a` option tells `ls` to show hidden files, including `.` and `..`. The `-l` option displays the files in a detailed list, making it easier to identify the hidden password file.

The password is stored separately in [passwords.md](passwords.md) and is not included in this write-up.

## Commands Used

```bash
cd inhere
ls -la
cat ...Hiding-From-You
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

## What I Did

1. Started from the `bandit3` SSH session.
2. Changed into the `inhere` directory with `cd inhere`.
3. Ran `ls -la` to show all files, including hidden files.
4. Identified the hidden password file named `...Hiding-From-You`.
5. Used `cat ...Hiding-From-You` to read the password from the hidden file.
6. Saved the password in my private local notes.
7. Used the password to log in as `bandit4` on port `2220`.

## What I Learned

- Files beginning with `.` are hidden by default.
- `ls -a` shows hidden files.
- `ls -l` shows detailed file information.
- Combining options as `ls -la` shows hidden files in detailed format.