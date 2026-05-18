# Module 04 — TypeScript Basics: Reading the Code We Write

You don't need to write TypeScript. You need to read it well enough to understand what's happening in your own codebase. That's the goal of this module.

---

## The Basics

### Variables

```typescript
const apiKey = process.env.ANTHROPIC_API_KEY;
```

- `const` — a value that doesn't change
- `apiKey` — the name we're giving this value
- `process.env.ANTHROPIC_API_KEY` — reading an environment variable (a secret stored outside the code)

```typescript
let port = 3000;
```

- `let` — a value that can change later

---

### Functions

```typescript
async function generateArtefact(topic: string): Promise<string> {
  // do something
  return result;
}
```

- `async` — this function does something that takes time (like calling an API)
- `topic: string` — the function takes one input called `topic`, which must be text
- `Promise<string>` — it will eventually return a text value
- `return result` — what it sends back

---

### The Anthropic SDK Call (The Core of Your App)

```typescript
const message = await anthropic.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 1000,
  messages: [
    { role: "user", content: prompt }
  ],
});
```

This is the actual call to Claude. In plain language:
- Send a message to Claude
- Use the claude-sonnet-4-6 model
- The message is whatever is in `prompt`
- Wait for the response (`await`)
- Store the response in `message`

---

### Express Routes (How the Portal Works)

```typescript
app.get('/', (req, res) => {
  res.sendFile('public/index.html');
});

app.post('/generate', async (req, res) => {
  const { topic } = req.body;
  const artefact = await generateArtefact(topic);
  res.json({ artefact });
});
```

- `app.get('/')` — when someone visits the homepage, send them the HTML file
- `app.post('/generate')` — when the form is submitted, generate an artefact and send it back
- `req.body` — the data sent from the form (the topic the user typed)
- `res.json(...)` — send back a JSON response

---

### tsconfig.json — What It Controls

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "outDir": "./dist",
    "rootDir": "./"
  },
  "include": ["app/**/*"]
}
```

- `outDir: ./dist` — compiled JavaScript goes into the `dist/` folder
- `rootDir: ./` — source code is at the root
- `include: ["app/**/*"]` — only compile files inside the `app/` folder

The error we hit: `package.json` said `node dist/app.js` but the compiled output was actually at `dist/app/app.js` due to the `rootDir` setting. Fixed by using `npm start` which let `package.json` control the path.

---

### package.json — The Control File

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/app/app.js"
  },
  "dependencies": {
    "@anthropic-ai/sdk": "^0.27.0",
    "express": "^4.18.2"
  }
}
```

- `"build": "tsc"` — `npm run build` compiles TypeScript
- `"start": "node dist/app/app.js"` — `npm start` runs the compiled app
- `dependencies` — the external packages the app needs to function

---

## The Four Agents in Your App

Your orchestrator routes requests to one of four agents based on what type of artefact is needed:

| Agent | Artefact it generates | VAF Layer |
|---|---|---|
| Guardrail Canvas Agent | VP1 — Guardrail Canvas (GC) | Strategic |
| Trade-off Matrix Agent | VP2 — Trade-off Matrix (TOM) | Decision |
| ADR Agent | VP3 — Architecture Decision Record | Execution |
| KB Loader | Reads the VAF knowledge base from GitHub | Foundation |

Each agent sends a specific prompt to Claude with the topic and relevant VAF context, receives the generated artefact, and commits it to your GitHub repo under `/artefacts/{type}/`.

---

## Next

[Module 05 → The VAF Project: How It All Fits Together](../05-the-vaf-project/README.md)
