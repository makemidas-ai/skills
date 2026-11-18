---
name: makemidas-music
description: Generate songs, instrumentals, jingles and background music with the MakeMidas CLI (Suno V6, Lyria 3.5 and other music models), with the user's own lyrics or lyrics the model writes, and save the audio files. Use when the user asks for a song, track, beat, jingle, theme, score or music bed, or to sing lyrics they wrote; also for "写首歌", "生成音乐", "配乐". Not for speech (makemidas-voiceover) or sound effects (makemidas-sound-effects). Spends the user's MakeMidas credits.
license: MIT
---

# Music with MakeMidas

## Before you start

1. `makemidas --version`. If missing, use `npx -y @makemidas/cli` in its
   place (Node.js 20+); ask before `npm i -g @makemidas/cli`.
2. `makemidas auth status --json`. Exit code 3 means signed out: run
   `makemidas auth login` (the user approves in the browser), or use
   `MAKEMIDAS_API_KEY` from the user. Never print or store the key.
3. Music needs a plan or credit pack. Each request is one price whatever
   the length: `makemidas models audio --json` lists them.

## Generate

```bash
makemidas music "upbeat indie pop about the first warm day of spring" --json
makemidas music "slow acoustic ballad, soft female vocal" --lyrics "$(cat lyrics.txt)" --title "Porch Light" --json
makemidas music "lo-fi hip hop, dusty piano, vinyl crackle" --instrumental --json
```

- The prompt describes the style: genre, mood, instruments, voice, tempo.
- `--lyrics <text>`: words to sing. Without it, models that write lyrics
  write their own.
- `--instrumental`: no vocals (models that offer it).
- `--duration <s>`: track length, only on models that list durations.
- `--model <id>`: from `makemidas models audio --json` (music models);
  leave it out for the default. Some models (Suno) return two or more takes
  of one request, all saved.
- `--title`, `--out <dir>`, `--max-credits <n>` as usual.

Tracks take one to a few minutes; the CLI waits and saves them. Use
`--no-wait` and `makemidas get <id> --wait` to run it in the background.

## Errors

Exit 2: bad input (an option the model doesn't take). 3: signed out. 4: not
enough credits or no plan (nothing charged). 5: the model refused or failed
(refunded): change the prompt or lyrics. 6: network; retry once. 7: still
generating.

## Rules

Don't ask a model to imitate a named living artist's voice, or to sing
lyrics the user doesn't have the rights to.
