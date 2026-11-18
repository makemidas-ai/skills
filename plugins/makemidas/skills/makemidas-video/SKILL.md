---
name: makemidas-video
description: Generate AI video clips with the MakeMidas CLI (Seedance, Kling, Veo, Hailuo, Wan, Gemini Omni, Grok Imagine, HappyHorse) from a prompt, a first frame, a first and last frame or reference images, with or without sound, and save the MP4. Use when the user asks to make a video, clip, animation or b-roll, to animate a picture (image to video), or to make a video between two frames; also for "做个视频", "图生视频", "文生视频". Spends the user's MakeMidas credits, priced per second.
license: MIT
---

# Videos with MakeMidas

## Before you start

1. `makemidas --version`. If missing, use `npx -y @makemidas/cli` in its
   place (Node.js 20+); ask before `npm i -g @makemidas/cli`.
2. `makemidas auth status --json`. Exit code 3 means signed out: run
   `makemidas auth login` (the user approves in the browser), or use
   `MAKEMIDAS_API_KEY` from the user. Never print or store the key.
3. Video is priced per second by resolution, and needs a plan or credit
   pack. Price it before running anything the user didn't size:

```bash
makemidas estimate video "the waves roll in" -d 5 --audio --json
```

## Generate

```bash
makemidas video "waves roll onto a beach at sunset" -d 5 --aspect 16:9 --json
makemidas video "the fox looks up and snow starts falling" --ref fox.png -d 5 --json
makemidas video "day turns to night" --ref day.png --ref night.png --json
makemidas video "the two friends wave" --ref a.png --ref b.png --frame-mode reference --model <id> --json
```

- `--model <id>`: from `makemidas models video --json`, which lists each
  model's resolutions, lengths, frame modes, sound and per-second price.
  Leave it out for the cheapest. `makemidas models <id>` shows one model.
- `--duration <s>` (`-d`): a length the model lists. `--resolution
  480p|720p|1080p` (as the model offers). `--aspect` `16:9`, `9:16`, `1:1`, …
- `--audio`: add sound on models that support it; some add sound to every
  clip.
- `--ref`: one image is the first frame, two are the first and last frame.
  `--frame-mode reference` uses them as references instead (models that
  list it, such as Seedance).
- `--max-credits <n>` prices the clip first and stops (exit 4, nothing
  charged) if it costs more.

## Waiting

Clips take one to several minutes; the CLI waits (up to `--timeout`
minutes, default 15) and saves the MP4 to `--out` (default: current
directory). To avoid blocking your shell:

```bash
makemidas video "…" --no-wait --json        # prints the id, exit code 7
makemidas get <id> --wait --json            # run in the background
```

## Errors

Exit 2: bad input, such as a length or resolution the model doesn't offer
(check `makemidas models <id>`). 3: signed out. 4: not enough credits, no
plan, or too many running (nothing charged). 5: the model refused or failed
(refunded): change the prompt or model. 6: network; retry once. 7: still
rendering: `makemidas get <id> --wait`.

## Show the result

Show the user the saved file path, and play or embed it if your environment
can.
