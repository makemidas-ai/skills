---
name: makemidas-sound-effects
description: Generate sound effects and seamless loops with the MakeMidas CLI (Suno Sound Effects and other sound models) from a description, and save the audio files. Use when the user needs a sound effect, Foley, UI sound, impact, whoosh, ambience or a loop at a tempo for a game, app or video; also for "音效", "生成音效". Not for music (makemidas-music) or speech (makemidas-voiceover). Spends the user's MakeMidas credits.
license: MIT
---

# Sound effects with MakeMidas

## Before you start

1. `makemidas --version`. If missing, use `npx -y @makemidas/cli` in its
   place (Node.js 20+); ask before `npm i -g @makemidas/cli`.
2. `makemidas auth status --json`. Exit code 3 means signed out: run
   `makemidas auth login` (the user approves in the browser), or use
   `MAKEMIDAS_API_KEY` from the user. Never print or store the key.
3. Sound effects need a plan or credit pack; each request is one price
   (`makemidas models audio --json`).

## Generate

```bash
makemidas sfx "a wooden door creaks open slowly" --json
makemidas sfx "a treasure chest opens with a magical shimmer" --out sounds --json
makemidas sfx "soft rain on a tin roof" --loop --bpm 90 --json
```

- Describe the sound itself: the source, the material, the space, how it
  starts and ends.
- `--loop` makes a seamless loop, and `--bpm <40-240>` sets its tempo
  (models that loop).
- The model sets the length; there is no `--duration` for `sfx`.
- `--model <id>` from `makemidas models audio --json`; `--out <dir>`,
  `--max-credits <n>` as usual.

The CLI waits and saves the files; `--no-wait` and
`makemidas get <id> --wait` run it in the background.

## Errors

Exit 2: bad input. 3: signed out. 4: not enough credits or no plan (nothing
charged). 5: failed (refunded). 6: network; retry once. 7: still
generating.
