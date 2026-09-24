# Bandit Level 00

## Goal

Log in to the Bandit server using SSH and the credentials provided for level 00.

## Concept

SSH, or Secure Shell, lets you connect securely to a remote computer from the terminal. The login requires three pieces of information:

- The remote username: `bandit0`
- The remote host: `bandit.labs.overthewire.org`
- The SSH port: `2220`

After connecting, SSH prompts for the level password. The password is stored separately in [passwords.md](passwords.md).

## Commands Used

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

## What I Did

1. Opened the terminal.
2. Ran the SSH command with the `bandit0` username, Bandit host, and port `2220`.
3. Entered the password for level 00 when SSH prompted me.
4. Successfully logged in to the Bandit server.

## What I Learned

- SSH is used to securely access a remote machine.
- The `-p` option specifies a non-default SSH port.
- The username, hostname, and port must all be correct to connect.
- Passwords should be kept separate from public walkthrough notes when possible.
