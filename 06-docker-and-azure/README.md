# Module 06 — Docker and Azure: What Happens Behind the Scenes

## Docker — The Container Concept

When you write code on your Windows machine and it runs differently on a server, that's an environment problem. Different operating systems, different installed software, different versions. Docker solves this by packaging everything — code, runtime, dependencies — into a sealed unit called a **container**.

A container is like a self-contained room that has exactly what it needs and nothing else. It runs the same way everywhere.

---

## The Dockerfile

Your Dockerfile lives at the root of the repo. It's a set of instructions for building the container:

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

EXPOSE 80
CMD ["sh", "start.sh"]
```

In plain English:
1. Start from a base image that has Node.js 20 on lightweight Alpine Linux
2. Set the working directory inside the container to `/app`
3. Copy package files first → install dependencies (efficient caching)
4. Copy the rest of the code
5. Compile TypeScript
6. Tell Docker this app uses port 80
7. When the container starts, run `start.sh`

---

## start.sh — The Startup Script

```bash
#!/bin/sh
set -e

nginx -g "daemon off;" &
NGINX_PID=$!

npm start &
NODE_PID=$!

trap "kill $NGINX_PID $NODE_PID 2>/dev/null; exit 0" TERM INT

wait -n $NGINX_PID $NODE_PID
```

In plain English:
1. Start nginx in the background (handles incoming HTTP traffic on port 80)
2. Start the Node.js app in the background (runs on port 3000 internally)
3. If either process dies, shut everything down cleanly

Nginx acts as a **reverse proxy** — it receives requests on port 80 (the standard HTTP port) and forwards them to your Node app on port 3000. Users see port 80. Your code listens on 3000. nginx bridges the gap.

---

## The GitHub Actions Workflow

When you push to main, this pipeline runs:

```yaml
jobs:
  build-and-push:
    steps:
      - uses: actions/checkout@v4          # Download your code
      - uses: actions/setup-node@v4        # Install Node.js 20
      - run: npm ci                         # Install dependencies
      - run: npm run build                  # Compile TypeScript
      - uses: docker/login-action@v3       # Log into GHCR
      - uses: docker/build-push-action@v5  # Build + push Docker image

  deploy:
    steps:
      - uses: azure/login@v1               # Log into Azure
      - run: az container delete ...       # Delete old container
      - run: az container create ...       # Create new container from new image
      - run: echo "IP:" $(az container show...)  # Print new IP
```

---

## The Environment Variable Error We Fixed

The Azure CLI command in the workflow had environment variables formatted incorrectly:

```yaml
# Broken — line breaks in the wrong place:
--secure-environment-variables
  ANTHROPIC_API_KEY=${{ secrets.ANTHROPIC_API_KEY }}
  GH_TOKEN=${{ secrets.GH_TOKEN }}
```

```yaml
# Fixed — all on one line with correct spacing:
--secure-environment-variables ANTHROPIC_API_KEY=${{ secrets.ANTHROPIC_API_KEY }} GH_TOKEN=${{ secrets.GH_TOKEN }}
```

The Azure CLI expects environment variables as space-separated `key=value` pairs on a single line.

---

## Reading the GitHub Actions Log

When a deploy fails, go to:
`github.com/ZenCloudAU/velocity-architecture/actions`

Click the failed run. Expand the failed step. The error message will be specific. Every error we hit was fixable by reading that message carefully.

Common log patterns:
- `Error: Process completed with exit code 1` → Something inside the step failed. Look above this line for the actual error.
- `ERROR: Incorrectly formatted...` → Syntax problem in the CLI command.
- `ERROR: (ResourceNotFound)` → Azure resource doesn't exist — wrong name or hasn't been created yet.

---

## What Cloudflare Adds (Phase 5)

Cloudflare sits between your domain and Azure:

```
User visits velocityarchitectureframework.com
        ↓
Cloudflare (handles HTTPS/SSL termination)
        ↓
Azure Container (your app, port 80)
```

Benefits:
- Free SSL certificate → HTTPS (the padlock)
- Hides your Azure IP (security)
- CDN caching (faster globally)
- DDoS protection

No code changes required — just DNS configuration.

---

## Next

[Module 07 → Troubleshooting: Every Error, Decoded](../07-troubleshooting/README.md)
