# MakeMidas plugin

Make AI images, videos, music, sound effects, voiceovers and transcripts
with your MakeMidas credits from Claude Code, Cursor and other agents.

- **MCP server** (`mcp.json`): `https://makemidas.com/api/mcp`. Sign in once
  with OAuth; tools to generate images, videos, music, sound effects and
  voiceovers, transcribe, remove backgrounds and upscale, check results,
  list models and credits, read brands, characters and the asset library, run film projects, and save results to Notion.
- **Skills** (`skills/`): drive the [`makemidas` CLI](https://www.npmjs.com/package/@makemidas/cli)
  to save results as files on your computer.

| Skill | For |
|---|---|
| `makemidas` | Setup, sign-in, credits, prices, past generations; routes to the others |
| `makemidas-image` | Images from a prompt or pictures; compare image models |
| `makemidas-video` | Video clips from text, a first frame or first and last frames |
| `makemidas-film` | Short films and ads: script, storyboard, approved frames and clips, one assembled MP4 |
| `makemidas-creative-director` | On-brand ideas from your brand, characters and asset library, then made |
| `makemidas-music` | Songs, instrumentals and jingles, with your lyrics or written for you |
| `makemidas-sound-effects` | Sound effects and seamless loops |
| `makemidas-voiceover` | Text to speech in a choice of voices and delivery |
| `makemidas-transcribe` | Transcripts and subtitles (TXT, SRT, VTT, JSON) |
| `makemidas-edit` | Background removal, image upscaling, video upscaling |

Docs: https://makemidas.com/skills · https://makemidas.com/developers
