# OverTheWire Bandit Notes

Personal notes and walkthroughs for the [OverTheWire Bandit](https://overthewire.org/wargames/bandit/) technical learning game.

The notes focus on the commands used, the concepts behind each challenge, the steps taken, and the lessons learned. They are intended as a study reference while practicing basic Linux and command-line skills.

## Contents

- [Level 00](level-00.md)
- [Level 0 to Level 1](level-01.md)
- [Level 1 to Level 2](level-02.md)
- [Level 2 to Level 3](level-03.md)
- [Level 3 to Level 4](level-04.md)
- [Level 4 to Level 5](level-05.md)

Additional level notes can be added as the walkthrough progresses.

## Connecting to Bandit

Bandit uses SSH on port `2220`. The general connection format is:

```bash
ssh bandit<level>@bandit.labs.overthewire.org -p 2220
```

Replace `<level>` with the user for the challenge you are working on. SSH will prompt for the password after the connection is established.

## Suggested Workflow

1. Connect to the appropriate Bandit account using SSH.
2. Read the level goal carefully before running commands.
3. Inspect the current directory and identify the relevant file or directory.
4. Use the smallest appropriate command to solve the challenge.
5. Save the next password in a private local notes file.
6. Record the solution and what you learned in the matching level Markdown file.
7. Continue with the next SSH account.

## Keeping Credentials Private

Passwords are intentionally not included in the walkthroughs. Local credentials are kept in `passwords.md`, which is ignored by Git and must never be committed or published.

Before pushing changes, review the files that will be committed:

```bash
git status
git diff --cached
```

## Attribution

The challenges, game structure, and learning materials belong to the [OverTheWire community](https://overthewire.org/). This repository contains personal notes about working through those challenges.
