# Module 00 — Start Here: The Mental Model

## The Problem With How Coding Is Taught

Every coding tutorial assumes you want to become a developer. You don't. You want to build things that work and ship them. That's a different job.

The job you're doing is **architect-led development**. You hold the system design in your head (you're very good at this), you direct Claude to write the implementation, and you operate the tools that get it live. That's a legitimate and powerful mode of building software. It's closer to how senior engineers actually work than to how beginners are taught.

The gap is vocabulary. When you don't know what a word means in a terminal error message, the error becomes unreadable noise. This repo removes that gap, using only things we actually built.

---

## The Stack — What You're Working With

When you hear "the stack," it means the collection of technologies that make your project work. Here's yours, in plain language:

### **TypeScript**
The language the VAF Agentic Architect is written in. TypeScript is JavaScript with types — it's stricter, which means it catches errors before they cause problems at runtime. You don't write it. You read it and understand what it does.

### **Node.js**
The engine that runs your TypeScript code on a server. When you see `node dist/app.js` in a command, it means "run this compiled code using Node." Node.js 20 is the version we use. Think of it as the power socket your code plugs into.

### **Express**
A web framework that runs inside Node. It listens for someone visiting a URL (like http://velocityarchitectureframework.com) and decides what to send back. Your portal runs on Express.

### **Anthropic SDK (claude-sonnet-4-6)**
The connection between your app and Claude. When someone types a topic into the portal and hits Generate, the app sends that topic to Claude via the SDK and gets back a generated artefact. This is the AI layer.

### **Docker**
Docker packages your entire app — code, Node.js, dependencies — into a sealed container that runs identically everywhere. On your laptop. On Azure. Anywhere. The `Dockerfile` in your repo describes how to build that container.

### **GitHub Actions**
Automation that runs when you push code. When you type `git push origin main`, GitHub Actions automatically builds the Docker container and deploys it to Azure. You don't do the deployment manually — the push triggers it.

### **Azure Container Instances (ACI)**
Where your app lives on the internet. Azure is Microsoft's cloud. ACI is the specific service that runs Docker containers. Your container has an IP address (`20.190.118.105`) that points to your domain.

### **GitHub**
Where all your code is stored and versioned. Every change you push is tracked. If something breaks, you can roll back. It's also the source of truth for the GitHub Actions automation.

---

## The Flow — What Happens When You Push

This is the sequence every time you make a change:

```
You edit a file in VS Code
        ↓
git add → stages the change
        ↓
git commit → saves it with a message
        ↓
git push → sends it to GitHub
        ↓
GitHub Actions triggers automatically
        ↓
TypeScript compiles → Docker image builds → image pushed to GHCR
        ↓
Azure pulls the new image → old container deleted → new container starts
        ↓
Your change is live at velocityarchitectureframework.com
```

This is continuous deployment. You don't manually deploy anything. The push does it.

---

## The Files — What Lives Where

```
velocity-architecture/           ← the repo root
├── app/                         ← all your TypeScript source code
│   ├── index.ts                 ← the entry point — where it all starts
│   ├── agents/                  ← the four AI agents (one per artefact type)
│   ├── orchestrator.ts          ← decides which agent runs what
│   └── kb-loader.ts             ← loads the VAF knowledge base from GitHub
├── .github/
│   └── workflows/
│       └── deploy.yml           ← the GitHub Actions automation file
├── Dockerfile                   ← instructions for building the container
├── package.json                 ← lists all dependencies, defines build/start commands
├── tsconfig.json                ← TypeScript compiler settings
└── start.sh                     ← the startup script inside the container
```

---

## The Mindset Shift

When you're confused by an error, the error is not telling you you're wrong. It's telling you something specific went wrong in a specific place. Every error we hit in our sessions had a precise cause:

- `No such file or directory` → wrong path, or missing forward slash
- `Not a git repository` → git not initialised, or wrong folder
- `Repository not found` → GitHub repo doesn't exist yet, or remote URL is wrong
- `src refspec main does not match any` → branch is called `master` not `main`
- `Incorrectly formatted secure environment settings` → Azure environment variable syntax error in the workflow file

None of these were about you not being smart enough. They were all about one specific thing being slightly off. That's what debugging is — finding the one thing.

---

## Next

[Module 01 → The Tools: VS Code, Git Bash, Terminal](../01-the-tools/README.md)
