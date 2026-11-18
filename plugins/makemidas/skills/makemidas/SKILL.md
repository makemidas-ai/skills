---
name: makemidas
description: Set up and use the MakeMidas CLI, which makes AI images, videos, songs, sound effects, voiceovers and transcripts and edits pictures and clips with the user's MakeMidas credits (Nano Banana, GPT Image, Seedream, Flux, Seedance, Kling, Veo, Wan, Suno, Lyria, Gemini TTS, Whisper…). Use to install or sign in to the CLI, check credits, plans or model prices, list or delete past generations, or when a request mixes several kinds of media; for one clear task prefer the focused skill (makemidas-image, makemidas-video, makemidas-music, makemidas-sound-effects, makemidas-voiceover, makemidas-transcribe, makemidas-edit). Spends the user's MakeMidas credits.
license: MIT
---

# MakeMidas: setup, credits and routing

The `makemidas` CLI sends requests to the user's MakeMidas account, waits for
the result and saves the files where you run it. Results (file paths) go to
stdout, progress to stderr. Every command takes `--json`.

## Pick the skill for the job

| The user wants | Skill | Command |
|---|---|---|
| An image, an edit of a picture, or models side by side | `makemidas-image` | `image`, `compare` |
| A video clip, or a picture animated | `makemidas-video` | `video` |
| A song, jingle or instrumental | `makemidas-music` | `music` |
| A sound effect or loop | `makemidas-sound-effects` | `sfx` |
| A voiceover from text | `makemidas-voiceover` | `speech` |
| A transcript or subtitles from audio or video | `makemidas-transcribe` | `transcribe` |
| A background removed, an image or clip upscaled | `makemidas-edit` | `remove-bg`, `upscale`, `upscale-video` |

If only this skill is installed, the commands below and `makemidas --help`
cover everything; install the others with
`npx skills add makemidas-ai/skills`.

## 1. Make sure the CLI runs

```bash
makemidas --version
```

If it isn't installed, use `npx -y @makemidas/cli` in place of `makemidas`
everywhere (Node.js 20+). Ask the user before running
`npm i -g @makemidas/cli`.

## 2. Make sure the user is signed in

```bash
makemidas auth status --json
```

Exit code 3 means not signed in. Either:

- run `makemidas auth login`: it opens the browser, where the user signs in
  and allows "MakeMidas CLI", and waits up to 5 minutes. Tell the user to
  approve it in the browser; if your shell times out sooner, run it in the
  background and check `auth status` after; or
- ask the user for an API key (Settings → API keys on makemidas.com) and run
  with `MAKEMIDAS_API_KEY` set in the environment. Never write the key into
  files or the conversation.

## 3. Credits, models and prices

```bash
makemidas credits --json                 # balance, plan, expiring credits
makemidas credits buy                    # links and prices; opens nothing
makemidas models --json                  # or: models image|video|audio|transcription|edit
makemidas models <id> --json             # one model: options, defaults, prices
makemidas estimate video "the waves roll in" -d 5 --audio --json
```

Every generation costs credits, at the same prices as the website. There are
no free credits: they come from a monthly plan or a one-time credit pack.
`estimate <command>` prices any request with that command's options and
charges nothing; `--max-credits <n>` on a generating command prices it first
and stops (exit 4, nothing charged) when it costs more than `n`.

## 4. Past generations

```bash
makemidas list --json                    # recent generations and ids
makemidas get <id> --wait --json         # fetch one again, saving its files
makemidas delete <id> --yes              # deletes it and its files, no refund
```

Only delete what the user asked to delete.

## Exit codes

| Code | Meaning | What to do |
|---|---|---|
| 0 | done | |
| 2 | bad input (unknown model, unsupported option, bad file) | fix the command; check `makemidas models <id>` |
| 3 | not signed in or sign-in expired | `makemidas auth login`, or `MAKEMIDAS_API_KEY` |
| 4 | not enough credits, a plan limit, too many running, or over `--max-credits` | show `makemidas credits`; plans at https://makemidas.com/pricing |
| 5 | the model could not make it (credits refunded) | show the error; change the prompt or model before retrying |
| 6 | network or server error | retry once, then report |
| 7 | still running (`--no-wait`) | `makemidas get <id> --wait` |

## Rules

- Generations spend the user's credits. Don't run batches, 4K, long clips or
  several outputs the user didn't ask for; price it first when unsure.
- Don't generate images, videos or voices of real people without their
  consent, or content the user would not want stored in their account.
- Never print, log or copy tokens, `MAKEMIDAS_API_KEY` or the credentials
  file (`~/.config/makemidas/credentials.json`).
- Bugs or requests for the MakeMidas team: `makemidas feedback "<message>"
  --kind bug|feature|praise|other [--generation <id>]`, when the user agrees.
- In ChatGPT, Claude or another MCP client, the MakeMidas MCP server
  (`https://makemidas.com/api/mcp`) does the same without the CLI.
