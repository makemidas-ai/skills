# MakeMidas skills: AI image and video generation for AI agents

An agent skill and plugin that let Claude Code, Cursor, Codex, OpenClaw and
other coding agents make AI images and videos with Nano Banana, GPT Image,
Seedream, Veo, Kling and more through [MakeMidas](https://makemidas.com),
paid with your MakeMidas credits. It drives the
[MakeMidas CLI](https://github.com/makemidas-ai/cli) and adds the MakeMidas
MCP server, which also works in ChatGPT and Claude.

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
