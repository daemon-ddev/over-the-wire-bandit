# Bandit Level 0 to Level 1

## Goal

Find the password for level 1 in a file named `readme` in the home directory, then use it to log in to `bandit1` over SSH.

## Concept

The `readme` file is in the home directory of the `bandit0` user. The `cat` command displays the contents of a file in the terminal. The displayed password is then used to authenticate as the next Bandit user.

The password is stored separately in [passwords.md](passwords.md) and is not included in this write-up.

## Commands Used

```bash
ls
cat readme
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

## What I Did

1. Started from the `bandit0` SSH session.
2. Ran `ls` to list the files in the home directory.
3. Confirmed that the file named `readme` was present.
4. Ran `cat readme` to display the password for level 1.
5. Saved the password in my private local notes.
6. Used the password to log in as `bandit1` on port `2220`.

## What I Learned

- `ls` lists files and directories in the current directory.
- `cat` prints the contents of a file.
- Passwords can be stored in a file and used for the next SSH login.
- Each Bandit level uses a different username and password.