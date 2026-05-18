# Module 05 — The VAF Agentic Architect: How It All Fits Together

## What You Built

A live, cloud-hosted web application that:
1. Accepts a topic from a user via a browser
2. Reads the VAF knowledge base from GitHub
3. Sends the topic + KB context to Claude
4. Claude generates a VAF artefact (Guardrail Canvas, TOM, or ADR)
5. The artefact is committed to your GitHub repo
6. The artefact is displayed in the browser with a link to GitHub

This is not a prototype. It runs on Azure at velocityarchitectureframework.com. It is a real deployed application.

---

## The Four Phases We Built

### Phase 1 — Core Agent ✅
The orchestrator, four agents, KB loader, and action engine. This is the brain.

### Phase 2 — Azure Deployment via GitHub Actions ✅
The infrastructure. Every push to `main` auto-builds and deploys.

### Phase 3 — GitHub Commits ✅
Generated artefacts are written to `/artefacts/{type}/` in the repo as `.md` files.

### Phase 4 — Browser Portal ✅
A web interface at the root URL. User enters a topic → clicks Generate → sees the artefact → link to GitHub.

### Phase 5 — Next (In Progress)
- Port 80 + HTTPS via Cloudflare (remove `:3000` from the URL)
- Portal authentication
- Multi-tenant client deployment
- Valor integration

---

## The Architecture Diagram

```
Browser (user)
      │
      ▼
Express Web Server (port 80)
      │
      ▼
Orchestrator
      │
      ├──→ Guardrail Canvas Agent  ──→ Claude API (claude-sonnet-4-6)
      ├──→ Trade-off Matrix Agent  ──→ Claude API
      ├──→ ADR Agent               ──→ Claude API
      └──→ KB Loader               ──→ GitHub API
                                         │
                                         ▼
                                   VAF Knowledge Base
                                         │
                                         ▼
                                   Action Engine
                                         │
                                         ▼
                              Commits artefact to GitHub
                              + Returns to browser
```

---

## The Files and What They Do

```
app/
├── index.ts          ← Entry point. Starts Express, sets up routes.
├── orchestrator.ts   ← Receives topic + type, routes to correct agent.
├── kb-loader.ts      ← Reads VAF KB files from GitHub via API.
├── action-engine.ts  ← Takes agent output, commits to GitHub.
└── agents/
    ├── gc-agent.ts   ← Guardrail Canvas prompt + generation
    ├── tom-agent.ts  ← Trade-off Matrix prompt + generation
    └── adr-agent.ts  ← Architecture Decision Record prompt + generation

public/
└── index.html        ← The browser portal UI

Dockerfile            ← Container build instructions
start.sh              ← Starts nginx (reverse proxy) + Node app
.github/
└── workflows/
    └── deploy.yml    ← GitHub Actions CI/CD pipeline
```

---

## The Bug We Fixed (start.sh)

**The problem:** `start.sh` was running `node dist/index.js` but `package.json` compiled to `dist/app/app.js`. The entry point was wrong.

**The fix:** Changed `start.sh` to run `npm start` instead. Let `package.json` own the path.

```bash
# Before (broken):
node dist/index.js

# After (working):
npm start
```

**Why this matters for your learning:** This kind of error — one file referencing a path that another file defines differently — is extremely common. The fix is always: find the single source of truth and make everything else defer to it.

---

## The Azure Setup

| Resource | Value |
|---|---|
| Resource group | `vaf-rg` |
| Container name | `vaf-agentic-architect` |
| Region | `australiasoutheast` |
| IP | `20.190.118.105` |
| Domain | velocityarchitectureframework.com |
| Registry | `ghcr.io/zencloudau/velocity-architecture:latest` |

GitHub Secrets required:
- `AZURE_CREDENTIALS` — service principal JSON
- `ANTHROPIC_API_KEY` — your Claude API key
- `GH_TOKEN` — GitHub PAT with repo access

---

## Next

[Module 06 → Docker and Azure: What Happens Behind the Scenes](../06-docker-and-azure/README.md)
