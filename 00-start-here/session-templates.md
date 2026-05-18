# Session Prompt Templates

Copy-paste these at the start of any new Claude session to restore full context immediately.

---

## Template 1 — VAF Agentic Architect (Phase 5 Onwards)

```
PROJECT CONTEXT:
- What I'm building: VAF Agentic Architect v2 — Claude-powered agentic EA that reads Velocity Architecture Framework KB from GitHub and autonomously generates EA artefacts (VP1 Guardrail Canvas, VP2 Trade-off Matrix, VP3 ADR), commits them to GitHub, and serves a browser portal
- Tech stack: TypeScript 5.3, Node.js 20, Express 4.18, Anthropic SDK (claude-sonnet-4-6), Axios, Pino, Docker, GitHub Actions, Azure Container Instances
- Repo: github.com/ZenCloudAU/velocity-architecture
- Live portal: http://velocityarchitectureframework.com:3000
- Azure: Resource group vaf-rg, container vaf-agentic-architect, region australiasoutheast, IP 20.190.118.105
- GitHub secrets: AZURE_CREDENTIALS, ANTHROPIC_API_KEY, GH_TOKEN
- Domain: velocityarchitectureframework.com (GoDaddy, A record pointing to Azure IP)
- CI/CD: GitHub Actions — push to main auto-builds Docker image, pushes to GHCR (ghcr.io/zencloudau/velocity-architecture:latest), deploys to Azure

CURRENT STATE — ALL COMPLETE:
- ✅ Phase 1: Core agent (orchestrator, 4 agents, KB loader, action engine)
- ✅ Phase 2: Azure deployment via GitHub Actions
- ✅ Phase 3: GitHub commits — artefacts land in /artefacts/{type}/ on generation
- ✅ Phase 4: Browser portal at / — enter topic, generate, view artefacts, link to GitHub

OPERATOR INSTRUCTIONS:
- Execute only. No filler, no commentary, no suggestions outside the task.
- You are the lead software engineer. Write all code. Phil does not edit code manually.
- Before writing any file: read every relevant file, identify all issues, fix everything in one pass.

NEXT TASK: [describe what you want to do next]
```

---

## Template 2 — New Coding Problem

```
I am Phil Myint. Principal Architect. Brisbane. Building on the VAF Agentic Architect project (TypeScript, Node.js 20, Express, Docker, Azure). I have no coding background — I read and direct, Claude writes.

Problem: [describe the error or what you want to build]

If you need to see a file, ask me to paste it. I will paste terminal output exactly as it appears.

Execute only. No filler.
```

---

## Template 3 — Learning Check

```
I am working through my learn-with-claude repo. I want to understand [concept].

Explain it using only examples from the VAF Agentic Architect project (github.com/ZenCloudAU/velocity-architecture). No generic examples. Use the actual files and commands from that project.
```

---

## The One Habit That Changes Everything

Before every session: `cd` to the repo, run `pwd`, run `ls`. Confirm you're in the right place. This prevents 80% of the errors that occurred in past sessions.

```bash
cd /c/Users/phill/Documents/GitHub/velocity-architecture
pwd
ls
```

If `ls` shows the repo files (Dockerfile, package.json, app/ folder), you're in the right place. If not, navigate until you are.
