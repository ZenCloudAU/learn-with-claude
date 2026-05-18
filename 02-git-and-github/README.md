# Module 02 — Git and GitHub

## The Difference

**Git** is the version control system that runs on your computer. It tracks changes to files.

**GitHub** is the website (github.com) that stores your git repositories online. It also runs GitHub Actions — the automation that deploys your code.

They're separate things. Git is the tool. GitHub is the host.

---

## The Workflow — Every Single Session

This is the pattern. Every time. No variation.

```bash
# 1. Navigate to the repo
cd /c/Users/phill/Documents/GitHub/velocity-architecture

# 2. Check what's changed
git status

# 3. Stage all changes
git add .

# 4. Commit with a description
git commit -m "Describe what you changed"

# 5. Push to GitHub → triggers the deploy
git push origin main
```

If you're on `master` branch (not `main`):
```bash
git push origin master
```

---

## Branches — What They Are

A branch is a parallel version of the code. When we set up your repos, sometimes git defaults to `master`, sometimes to `main`. GitHub expects `main` by default now.

To check which branch you're on:
```bash
git branch
```

The branch with `*` next to it is your current branch.

---

## The GitHub Actions File

Location: `.github/workflows/deploy.yml`

This is the automation script. It runs every time you push to `main`. Here's what it does in sequence:

1. **Checkout** — downloads your code onto the runner (GitHub's server)
2. **Setup Node.js** — installs Node.js 20
3. **npm ci** — installs all dependencies (from package.json)
4. **npm run build** — compiles TypeScript → JavaScript in the `dist/` folder
5. **Docker build** — packages everything into a container image
6. **Docker push** — uploads the image to GHCR (GitHub Container Registry)
7. **Azure deploy** — deletes the old Azure container, creates a new one with the new image

The error we hit:
```
ERROR: Incorrectly formatted secure environment settings.
Argument values should be in the format a=b c=d
```

This was Azure CLI syntax. Environment variables in the Azure CLI command need a specific format with no line breaks in the middle. Claude fixed it by keeping them on one line.

---

## GitHub Secrets

Your deploy file uses secrets — values that aren't stored in the code because they're sensitive (API keys, passwords, Azure credentials). They're stored in GitHub under:

`Repo → Settings → Secrets and variables → Actions`

Your secrets:
- `AZURE_CREDENTIALS` — the JSON credentials for Azure deployment
- `ANTHROPIC_API_KEY` — your Claude API key
- `GH_TOKEN` — a GitHub personal access token

When the Actions workflow runs, it substitutes `${{ secrets.ANTHROPIC_API_KEY }}` with the actual value. You never hardcode secrets in files.

---

## The GHCR — What It Is

**GHCR** = GitHub Container Registry. When Actions builds your Docker image, it pushes it to:

```
ghcr.io/zencloudau/velocity-architecture:latest
```

Azure then pulls this image when deploying. The image is like a snapshot of your complete app, ready to run anywhere.

---

## Setting Up a New Repo — The Complete Sequence

This is what we did for `agentic-cert`:

```bash
# 1. Create the folder and files locally (or copy from Claude output)
cd /c/Users/phill/Documents/GitHub
mkdir agentic-cert
cd agentic-cert

# 2. Initialise git
git init

# 3. Stage and commit everything
git add .
git commit -m "Initial build"

# 4. Go to github.com/ZenCloudAU → New repository → name it → Create (empty)

# 5. Connect local to remote
git remote add origin https://github.com/ZenCloudAU/agentic-cert.git

# 6. Push
git push -u origin master
```

---

## Next

[Module 03 → Terminal Commands Reference](../03-terminal-commands/README.md)
