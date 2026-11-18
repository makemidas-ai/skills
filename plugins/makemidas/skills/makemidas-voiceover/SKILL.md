---
name: makemidas-voiceover
description: Turn text into a spoken voiceover with the MakeMidas CLI (Gemini 3.8 Flash TTS and other speech models), choosing a voice and how it is read, and save the audio file. Use when the user wants narration, a voiceover, text to speech, a podcast intro or dialogue lines read aloud; also for "配音", "文字转语音", "朗读". Not for songs (makemidas-music). Spends the user's MakeMidas credits, priced per 1,000 characters.
license: MIT
---

# Voiceovers with MakeMidas

## Before you start

1. `makemidas --version`. If missing, use `npx -y @makemidas/cli` in its
   place (Node.js 20+); ask before `npm i -g @makemidas/cli`.
2. `makemidas auth status --json`. Exit code 3 means signed out: run
   `makemidas auth login` (the user approves in the browser), or use
   `MAKEMIDAS_API_KEY` from the user. Never print or store the key.
3. Voiceovers need a plan or credit pack and are priced per 1,000
   characters of text:

```bash
makemidas estimate speech "Welcome back to the show." --voice Kore --json
```

## Generate

```bash
makemidas speech "Welcome back. Today we start with the news." --json
makemidas voiceover "Thanks for watching." --voice Charon --delivery "warm and unhurried" --json
makemidas speech "$(cat script.txt)" --out audio --json
```

- The text is read exactly as written: pass the user's words, not a
  description of them.
- `--voice <id>`: a voice of the model. `makemidas models <model-id> --json`
  lists the voices; `makemidas models audio --json` lists the speech models.
- `--delivery <words>` (Gemini TTS): how to read it, e.g. "excited, fast",
  "calm documentary narrator".
- `--speed <0.7-1.2>` and `--stability <0-1>` apply to ElevenLabs voices,
  `--language <code>` to models with a language choice.
- `--model <id>`: leave it out for the default speech model.

The CLI waits and saves the audio; `--no-wait` and
`makemidas get <id> --wait` run it in the background.

## Errors

Exit 2: bad input (an unknown voice, text too long). 3: signed out. 4: not
enough credits or no plan (nothing charged). 5: failed (refunded). 6:
network; retry once. 7: still generating.

## Rules

Only make a voiceover the user may publish. Don't present synthetic speech
as a real person's own recording.
