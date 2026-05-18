# Module 01 — The Tools

## VS Code (Visual Studio Code)

**What it is:** A text editor built for code. Think of it like Word, but for code files. It colours the code so you can read it, shows errors in real time, and has a built-in terminal so you can run commands without switching windows.

**What you use it for:** Reading and understanding code files. Occasionally pasting in new file content that Claude generates. Opening the integrated terminal to run git commands.

**The terminal inside VS Code:** Press `` Ctrl + ` `` (backtick, top-left of keyboard). This opens a terminal window at the bottom of VS Code. It starts in whatever folder you have open. This is the same as Git Bash.

---

## Git Bash

**What it is:** A terminal that understands Unix-style commands on Windows. Windows normally uses backslashes (`C:\Users\phill`) but git and most developer tools use forward slashes (`/c/Users/phill`). Git Bash translates between them.

**The confusion we hit:** When you ran git commands in regular Windows Command Prompt or PowerShell, paths didn't work. Git Bash fixes this.

**The critical rule for paths in Git Bash:**

| Windows path | Git Bash equivalent |
|---|---|
| `C:\Users\phill\Documents\GitHub\velocity-architecture` | `/c/Users/phill/Documents/GitHub/velocity-architecture` |

Drive letter becomes lowercase. Backslashes become forward slashes. `C:` becomes `/c/`.

---

## The Terminal — Commands You Actually Use

These are the exact commands from our sessions, explained:

### Navigation

```bash
pwd
```
"Print working directory." Shows you where you currently are. Run this when you're confused about your location.

```bash
ls
```
Lists files and folders in the current directory. Use this to confirm the repo files are there before running git commands.

```bash
cd /c/Users/phill/Documents/GitHub/velocity-architecture
```
"Change directory." Moves you into a folder. Always do this before running any git commands — you must be inside the repo folder.

```bash
cd ..
```
Goes up one level. If you're in `velocity-architecture`, this takes you to `GitHub`.

---

### Git Commands

```bash
git init
```
Initialises a new git repository in the current folder. Only run once, when setting up a new repo.

```bash
git status
```
Shows you what files have changed and what's staged for commit. Run this constantly — it tells you the current state.

```bash
git add .
```
Stages all changed files for commit. The `.` means "everything in the current folder and subfolders."

```bash
git add deploy.yml
```
Stages one specific file. Useful when you only want to commit one change.

```bash
git commit -m "Fix start.sh — use npm start instead of wrong entry point"
```
Saves the staged changes with a message describing what changed. The message goes in quotes.

```bash
git push origin main
```
Sends the commit to GitHub. `origin` is the name of your GitHub remote. `main` is the branch name.

```bash
git push -u origin master
```
The `-u` sets the upstream — you run this once on a new repo to link your local branch to the remote. After that you just use `git push`.

```bash
git remote add origin https://github.com/ZenCloudAU/velocity-architecture.git
```
Connects your local repo to the GitHub repo. Run once when setting up.

```bash
git remote -v
```
Shows what remote(s) your repo is connected to. Use this to check the URL is correct.

---

## The Specific Error We Hit (And What It Meant)

### "bash: cd: No such file or directory"

```bash
# This failed because Windows backslashes don't work in Git Bash:
cd C:\Users\phill\Documents\GitHub\velocity-architecture

# This works because it uses forward slashes:
cd /c/Users/phill/Documents/GitHub/velocity-architecture
```

### "fatal: not a git repository"

You ran a git command in a folder that doesn't have git initialised. Two possible causes:
1. You're in the wrong folder — run `pwd` to check, then `cd` to the right one.
2. You haven't initialised git yet — run `git init`.

### "src refspec main does not match any"

Your branch is called `master`, not `main`. GitHub created a repo expecting `main`, your local git has `master`. Fix:
```bash
git push -u origin master
```

### "remote: Repository not found"

The GitHub repo doesn't exist yet. Go to github.com/ZenCloudAU, create the repo manually, then come back and push.

---

## Next

[Module 02 → Git and GitHub: The Full Picture](../02-git-and-github/README.md)
