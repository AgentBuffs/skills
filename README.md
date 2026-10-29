# AgentBuffs

Find the right MCP server or agent skill for a job. Your agent queries
[agentbuffs.com](https://agentbuffs.com) and receives a ranked list with client
install configs.

This repository provides three connection methods: a skill, a remote MCP
server, and a Claude Code plugin containing both.

## The skill

`find-agent-tools` teaches your agent the agentbuffs API. It requires no API key
and no local server.

```bash
npx skills add AgentBuffs/skills
```

Or copy `skills/find-agent-tools/SKILL.md` to your agent's skills directory.

## The MCP server

This remote server provides two tools: `find_tools` (ranks tools for a job or
query) and `get_install` (returns the client install snippet).

```bash
claude mcp add --transport http agentbuffs https://agentbuffs.com/api/mcp
```

For other clients, add a remote HTTP server pointing to
`https://agentbuffs.com/api/mcp`. It requires no key.

## The Claude Code plugin

```
/plugin marketplace add AgentBuffs/skills
/plugin install agentbuffs@agentbuffs
```

## The API

The skill and the server call the same public API:
`https://agentbuffs.com/api/discover/v1`. That endpoint documents every call.
Agents can also read [agentbuffs.com/llms.txt](https://agentbuffs.com/llms.txt).

## Privacy

See the [agentbuffs.com privacy policy](https://agentbuffs.com/privacy).

## License

MIT
