# Composio + Vercel AI SDK Agent

An AI agent built with **Vercel AI SDK** and **Composio**, powered by **Claude claude-sonnet-4-6**. The agent can perform real-world actions (like starring GitHub repos) using Composio's tool router.

## Stack

- [Vercel AI SDK](https://sdk.vercel.ai/) — streaming LLM interface
- [Composio](https://composio.dev) — tool/action routing for AI agents
- [Anthropic Claude claude-sonnet-4-6](https://anthropic.com) — the underlying model
- TypeScript + Node.js

## Quick Start

### 1. Install dependencies

```bash
npm install @composio/core @composio/vercel ai @ai-sdk/anthropic
```

### 2. Set environment variables

Copy `.env.example` to `.env` and fill in your keys:

```bash
cp .env.example .env
```

```env
COMPOSIO_API_KEY=<your-composio-api-key>
ANTHROPIC_API_KEY=<your-anthropic-api-key>
```

Get your Composio API key at [app.composio.dev](https://app.composio.dev).

### 3. Run the agent

```bash
npm run start
```

Or in watch mode for development:

```bash
npm run dev
```

## How It Works

```typescript
// agent.ts
const composio = new Composio({ provider: new VercelProvider() });
const session = await composio.create(userId);
const tools = await session.tools();

const stream = await streamText({
  model: anthropic("claude-sonnet-4-6"),
  prompt: "Star the composiohq/composio repo on GitHub",
  stopWhen: stepCountIs(10),
  tools,
});
```

1. **Composio** creates a tool router session scoped to a user
2. **Vercel AI SDK** streams the response from Claude
3. The model uses **Composio tools** to take real actions (e.g., GitHub API calls)
4. Streaming output is piped to stdout in real time

## Customization

- Change the `prompt` in `agent.ts` to give the agent a different task
- Replace `userId` with a dynamic value for multi-user setups
- Adjust `stepCountIs(10)` to limit/expand the agent's reasoning depth

## Documentation

- [Composio Docs](https://docs.composio.dev)
- [Tool Router Guide](https://docs.composio.dev/tool-router/overview)
- [Managing Multiple Accounts](https://docs.composio.dev/tool-router/managing-multiple-accounts)
- [Vercel AI SDK Docs](https://sdk.vercel.ai/docs)
