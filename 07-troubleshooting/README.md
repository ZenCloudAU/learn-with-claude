# Module 07 — Troubleshooting: Every Error We Hit, Decoded

These are real errors from real sessions. The exact messages. The exact fixes.

---

## Git Errors

### "bash: cd: No such file or directory"

**Session:** VAF Agentic Architect v3 — Phase 5
**What happened:** Pasted a Windows path into Git Bash.

```bash
# This fails in Git Bash:
cd C:\Users\phill\Documents\GitHub\velocity-architecture

# This works:
cd /c/Users/phill/Documents/GitHub/velocity-architecture
```

**Rule:** Git Bash uses forward slashes. Drive letter `C:` becomes `/c/`.

---

### "fatal: not a git repository (or any of the parent directories): .git"

**Session:** Agentic Cert repo setup
**What happened:** Ran `git add .` in a folder before running `git init`.

```bash
# Fix:
git init
git add .
git commit -m "Initial build"
```

**Rule:** A folder is not a git repo until you run `git init`. Always check with `pwd` that you're in the right folder.

---

### "error: src refspec main does not match any"

**Session:** Agentic Cert repo setup
**What happened:** Tried `git push -u origin main` but the branch was called `master`.

```bash
# Check your branch:
git branch

# Push to the correct branch name:
git push -u origin master
```

**Rule:** Newer GitHub defaults to `main`. Older local git defaults to `master`. Check first.

---

### "remote: Repository not found. fatal: repository '...' not found"

**Session:** Agentic Cert repo setup
**What happened:** Tried to push before creating the repo on GitHub.

**Fix:**
1. Go to github.com/ZenCloudAU
2. Click **New repository**
3. Name it exactly as your remote URL expects
4. **Do not** add README, .gitignore, or license (keep it empty)
5. Come back and push

---

## GitHub Actions / Azure Errors

### "ERROR: Incorrectly formatted secure environment settings. Argument values should be in the format a=b c=d"

**Session:** VAF Agentic Architect v2 → v3
**What happened:** Environment variables in the `az container create` command were split across lines incorrectly.

```yaml
# Broken:
--secure-environment-variables
  ANTHROPIC_API_KEY=${{ secrets.ANTHROPIC_API_KEY }}
  GH_TOKEN=${{ secrets.GH_TOKEN }}

# Fixed:
--secure-environment-variables ANTHROPIC_API_KEY=${{ secrets.ANTHROPIC_API_KEY }} GH_TOKEN=${{ secrets.GH_TOKEN }}
```

---

### App starts but immediately exits / Port not responding

**Session:** start.sh debugging
**What happened:** `start.sh` was running `node dist/index.js` but the compiled output path was `dist/app/app.js`.

```bash
# start.sh before (broken):
node dist/index.js

# start.sh after (fixed):
npm start
```

**Rule:** `npm start` reads the `start` script from `package.json`, which owns the correct entry point. Let one file define the truth; all other references should defer to it.

---

### GitHub Actions fails with "Build TypeScript" step error

**Possible causes:**
1. A TypeScript syntax error in one of the `.ts` files
2. `tsconfig.json` include path doesn't match where the files actually are
3. A dependency missing from `package.json`

**How to diagnose:** Expand the failing step in the Actions log. The TypeScript compiler (`tsc`) will print the file name, line number, and the exact error.

---

## Node.js / npm Errors

### "npm: command not found"

Node.js is not installed or not on the PATH.
Fix: Download and install Node.js 20 from nodejs.org. Restart Git Bash after installing.

### "Cannot find module 'express'"

Dependencies aren't installed.
Fix: `npm install` or `npm ci`.

---

## Azure Errors

### Container starts but app is unreachable at the IP

Possible causes:
1. The container is listening on port 3000 but ACI exposes port 80 — check the `az container create` port mapping
2. nginx isn't running — check `start.sh`
3. The app crashed on startup — check `az container logs --name vaf-agentic-architect --resource-group vaf-rg`

### "ResourceNotFound" in Azure CLI

The resource group, container name, or location doesn't match what's in Azure. Check:
```
Resource group: vaf-rg
Container name: vaf-agentic-architect
Location: australiasoutheast
```

---

## The Debugging Pattern

When something breaks, this is the sequence:

1. **Read the error message exactly.** Don't skim. The specific words matter.
2. **Identify which layer failed:** git? npm? TypeScript? Docker? Azure?
3. **Find the specific location:** which file, which line, which step in Actions.
4. **Fix one thing at a time.** Don't make multiple changes then push — you won't know which fix worked.
5. **Push and watch the Actions log.** Confirm the fix before declaring it done.

---

## The Meta-Lesson

Every single error we hit in every session had a specific, findable, fixable cause. Not one of them was "something is fundamentally broken and unfixable." They were all:

- Wrong path
- Wrong branch name
- Missing repo
- Syntax error in one command
- Wrong entry point in one file

The difference between a confused beginner and a confident builder is not knowledge of every possible error. It's the habit of reading error messages precisely and fixing the specific thing they name.

That's it. That's the whole skill.
