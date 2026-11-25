# MakeMidas skills: AI image, video and audio generation for AI agents

Agent skills and a plugin that let Claude Code, Cursor, Codex, OpenClaw and
other coding agents make AI images, videos, music, sound effects,
voiceovers and transcripts with Nano Banana, GPT Image, Seedream, Veo,
Kling, Seedance, Suno, Lyria, Gemini TTS, Whisper and more through
[MakeMidas](https://makemidas.com), paid with your MakeMidas credits. The
skills drive the [MakeMidas CLI](https://github.com/makemidas-ai/cli), and
the plugin adds the MakeMidas MCP server, which also works in ChatGPT and
Claude.

| Skill | For |
|---|---|
| [`makemidas`](plugins/makemidas/skills/makemidas) | Setup, sign-in, credits, prices, past generations; routes to the others |
| [`makemidas-image`](plugins/makemidas/skills/makemidas-image) | Images from a prompt or pictures; compare image models |
| [`makemidas-video`](plugins/makemidas/skills/makemidas-video) | Video clips from text, a first frame or first and last frames |
| [`makemidas-film`](plugins/makemidas/skills/makemidas-film) | Short films and ads: script, storyboard, approved frames and clips, one assembled MP4 |
| [`makemidas-creative-director`](plugins/makemidas/skills/makemidas-creative-director) | On-brand ideas from your brand, characters and asset library, then made |
| [`makemidas-music`](plugins/makemidas/skills/makemidas-music) | Songs, instrumentals and jingles |
| [`makemidas-sound-effects`](plugins/makemidas/skills/makemidas-sound-effects) | Sound effects and seamless loops |
| [`makemidas-voiceover`](plugins/makemidas/skills/makemidas-voiceover) | Text to speech |
| [`makemidas-transcribe`](plugins/makemidas/skills/makemidas-transcribe) | Transcripts and subtitles |
| [`makemidas-edit`](plugins/makemidas/skills/makemidas-edit) | Background removal, image and video upscaling |

The [`makemidas`](plugins/makemidas) plugin bundles all ten skills and the
MakeMidas MCP server.

## Install

Claude Code:

```bash
/plugin marketplace add makemidas-ai/skills
/plugin install makemidas@makemidas-skills
```

Any agent that reads skills (the first command lists the skills to pick from; the second adds one):

```bash
npx skills add makemidas-ai/skills
npx skills add makemidas-ai/skills --skill makemidas-video
```

Or add only the MCP server:

```bash
claude mcp add --transport http makemidas https://makemidas.com/api/mcp
```

Docs: https://makemidas.com/skills · https://makemidas.com/developers · License: MIT
