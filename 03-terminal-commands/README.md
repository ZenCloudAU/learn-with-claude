# Module 03 — Terminal Commands Reference

A plain-English reference for every command that appeared in our sessions.

---

## Navigation

| Command | What it does | Example |
|---|---|---|
| `pwd` | Show current location | `/c/Users/phill/Documents/GitHub/velocity-architecture` |
| `ls` | List files in current folder | Shows all files and subfolders |
| `ls -la` | List all files including hidden | Shows `.github`, `.env`, etc |
| `cd [path]` | Move to a folder | `cd /c/Users/phill/Documents/GitHub/velocity-architecture` |
| `cd ..` | Go up one folder level | From `velocity-architecture` to `GitHub` |
| `mkdir [name]` | Create a folder | `mkdir my-new-repo` |

---

## Git Commands

| Command | What it does |
|---|---|
| `git init` | Start tracking a folder with git (run once) |
| `git status` | Show what's changed, what's staged |
| `git add .` | Stage all changes |
| `git add [file]` | Stage one specific file |
| `git commit -m "message"` | Save staged changes with a description |
| `git push origin main` | Send commits to GitHub (main branch) |
| `git push origin master` | Send commits to GitHub (master branch) |
| `git push -u origin master` | First push — sets the upstream |
| `git pull` | Download latest changes from GitHub |
| `git branch` | Show all branches, current one marked with * |
| `git log --oneline` | Show recent commits as a list |
| `git remote -v` | Show what GitHub URL this repo connects to |
| `git remote add origin [url]` | Connect a local repo to GitHub |
| `git stash` | Temporarily save uncommitted changes |

---

## npm Commands (Node Package Manager)

| Command | What it does |
|---|---|
| `npm install` | Install all packages listed in package.json |
| `npm ci` | Clean install — faster, stricter (used in CI/CD) |
| `npm run build` | Compile TypeScript → JavaScript |
| `npm start` | Start the application |
| `npm run dev` | Start in development mode (usually with hot reload) |

---

## Docker Commands (Reference Only — You Don't Run These)

GitHub Actions runs these automatically. But knowing what they mean helps you read the logs.

| Command | What it does |
|---|---|
| `docker build -t image-name .` | Build a Docker image from the Dockerfile |
| `docker push image-name` | Upload the image to a registry |
| `docker run image-name` | Start a container from an image |
| `docker ps` | List running containers |
| `docker logs container-name` | Show output/errors from a container |

---

## Azure CLI Commands (Reference Only)

These run inside GitHub Actions. You don't run them manually.

| Command | What it does |
|---|---|
| `az login` | Log in to Azure |
| `az container create` | Create a new Azure Container Instance |
| `az container delete` | Delete an existing container |
| `az container show` | Show the status of a container |

---

## Windows-Specific

| Situation | Command |
|---|---|
| Open Git Bash | Right-click in folder → "Git Bash Here" OR search "Git Bash" in Start |
| Open VS Code terminal | `Ctrl + `` (backtick) |
| Copy in terminal | Select text → right-click → Copy |
| Paste in terminal | Right-click → Paste |

---

## The Errors Reference

| Error | What it means | Fix |
|---|---|---|
| `No such file or directory` | Wrong path | Check with `pwd` and `ls`, fix the path |
| `fatal: not a git repository` | Not inside a git repo | `cd` to the repo folder, or run `git init` |
| `remote: Repository not found` | GitHub repo doesn't exist | Create the repo on github.com first |
| `src refspec main does not match` | Branch is `master` not `main` | Use `git push -u origin master` |
| `Permission denied` | Auth issue | Check GitHub credentials are set up |
| `npm: command not found` | Node.js not installed | Install Node.js from nodejs.org |

---

## Next

[Module 04 → TypeScript Basics: Reading the Code We Write](../04-typescript-basics/README.md)
