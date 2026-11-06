# MakeMidas skills

Agent plugins for [MakeMidas](https://makemidas.com): AI image and video
generation with your MakeMidas credits.

| Plugin | What it adds |
|---|---|
| [`makemidas`](plugins/makemidas) | The MakeMidas MCP server and a skill for the `makemidas` CLI |

## Install

Claude Code:

```bash
/plugin marketplace add makemidas-ai/skills
/plugin install makemidas@makemidas-skills
```

Any agent that reads skills:

```bash
npx skills add makemidas-ai/skills --skill makemidas
```

Or add only the MCP server:

```bash
claude mcp add --transport http makemidas https://makemidas.com/api/mcp
```

Docs: https://makemidas.com/developers · License: MIT
