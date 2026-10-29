---
name: find-agent-tools
description: Find an MCP server or agent skill for a job from the agentbuffs.com catalogue, ranked by quality, with the install config for your client. Use when the user asks for a tool, integration, MCP server or skill, asks "is there an MCP for X", or when a task needs a capability you do not have.
---

# Find agent tools

agentbuffs.com lists MCP servers and agent skills, ranked by quality. Its API is
public and needs no key. Every answer is JSON.

Base URL: `https://agentbuffs.com/api/discover/v1`

## Three steps

1. **Pick the job.** `GET /use-cases` lists 32 jobs. Each has a `slug`, a
   `question` such as "I want to turn designs into code", and item counts.
2. **Get the best tools for it.** `GET /use-cases/<slug>?kind=mcp&client=claude-code&limit=5`
   returns ranked items. `kind` is `mcp` or `skill`; leave it out for both.
   `client` keeps only items that client can run.
3. **Install one.** `GET /items/<slug>?client=claude-code` returns the item and
   its install snippet for that client.

When no use case fits, search instead:
`GET /search?q=<words>&kind=mcp&client=claude-code`. The answer also names the
use cases that match, in `matchedUseCases`.

## Client slugs

`claude-code`, `claude-desktop`, `codex`, `cursor`, `vscode`, `copilot-cli`,
`chatgpt`, `gemini-cli`, `opencode`, `windsurf`, `zed`, `cline`, `goose`,
`jetbrains`, `warp`, `continue`, `kilo-code`, `antigravity`. `GET /` lists them all.

## How to use the answers

- Show the user the top two or three items: name, `summary`, `official`, and
  `page` (the agentbuffs.com page). Prefer `official: true` when scores are close.
- Install only after the user agrees. Never invent an install command: take it
  from `install` in the item answer.
- A `<YOUR_VALUE>` placeholder in a snippet is a value the user must provide,
  such as an API key. Ask for it. Never guess it.
- On `429`, wait for the `Retry-After` seconds, then retry once.

## Example

```bash
curl -s 'https://agentbuffs.com/api/discover/v1/use-cases/design-to-code?kind=mcp&client=claude-code&limit=3'
```

## The MCP server

The same catalogue is also a remote MCP server with two tools, `find_tools` and
`get_install`. If the user wants it permanently:

```bash
claude mcp add --transport http agentbuffs https://agentbuffs.com/api/mcp
```
