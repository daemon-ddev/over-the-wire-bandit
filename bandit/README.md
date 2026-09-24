# My OverTheWire Bandit Journey

This repository documents my journey through the [OverTheWire Bandit](https://overthewire.org/wargames/bandit/) command-line learning game.

I am using Bandit to build a stronger foundation in Linux, the command line, SSH, and practical problem solving. Each challenge gives me a chance to slow down, understand what is happening, and develop better habits instead of only looking for a quick answer.

These notes capture the commands I used, the ideas behind them, what I did, and what I learned. They are also a record of my progress and a reference I can return to when I need to reinforce a concept.

## What I Am Trying to Learn

- Become more confident working in a Linux terminal.
- Understand how SSH connects me to remote systems.
- Learn how files, directories, hidden files, permissions, and command options work.
- Get better at reading a problem, forming a plan, and testing it carefully.
- Build practical habits for investigating systems and handling information responsibly.
- Explain my solutions clearly enough that I can understand them again later.

## What I Am Trying to Accomplish

My goal is to complete the Bandit challenges while developing stronger technical and problem-solving skills. I want to become comfortable exploring unfamiliar systems, using documentation, solving problems methodically, and keeping useful technical notes along the way.

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

## How I Am Working Through It

1. I connect to the appropriate Bandit account using SSH.
2. I read the level goal carefully before running commands.
3. I inspect the current directory and identify the relevant file or directory.
4. I use the smallest appropriate command to solve the challenge.
5. I save the next password in a private local notes file.
6. I record the solution and what I learned in the matching level Markdown file.
7. I continue to the next SSH account and challenge.

## Keeping Credentials Private

Passwords are intentionally not included in the walkthroughs. Local credentials are kept in `passwords.md`, which is ignored by Git and must never be committed or published.

Before pushing changes, review the files that will be committed:

```bash
git status
git diff --cached
```

## Attribution

The challenges, game structure, and learning materials belong to the [OverTheWire community](https://overthewire.org/). This repository contains personal notes about working through those challenges.
