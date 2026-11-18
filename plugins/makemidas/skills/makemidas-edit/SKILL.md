---
name: makemidas-edit
description: Remove the background from a photo, upscale an image (2x, 4x, 8x or sharpen), or upscale a short video clip to 1080P, 2K or 4K with the MakeMidas CLI, and save the result. Use when the user wants a transparent PNG/WebP or cutout, a product photo on no background, a bigger or sharper image, or a higher resolution video; also for "抠图", "去背景", "高清放大", "视频放大". Spends the user's MakeMidas credits.
license: MIT
---

# Edits with MakeMidas

## Before you start

1. `makemidas --version`. If missing, use `npx -y @makemidas/cli` in its
   place (Node.js 20+); ask before `npm i -g @makemidas/cli`.
2. `makemidas auth status --json`. Exit code 3 means signed out: run
   `makemidas auth login` (the user approves in the browser), or use
   `MAKEMIDAS_API_KEY` from the user. Never print or store the key.
3. Edits spend credits: `makemidas models edit --json` lists the prices, and
   `makemidas estimate remove-bg|upscale|upscale-video <file>` prices one.

## Remove a background

```bash
makemidas remove-bg product.jpg --json          # saves a transparent WebP
```

## Upscale an image

```bash
makemidas upscale photo.png --scale 4 --json    # 1 sharpens at the same size; default 2
```

The price depends on the output size; check with
`makemidas estimate upscale photo.png --scale 4`.

## Upscale a video

```bash
makemidas upscale-video clip.mp4 --resolution 4K --json   # 1080P, 2K (default) or 4K
```

MP4 or MOV, up to 60 seconds; priced per second of the clip. It runs in the
background on the server: the CLI waits and saves the result, or use
`--no-wait` and `makemidas get <id> --wait`.

Inputs can be files or links. `--out <dir>`, `--force`, `--no-download` and
`--max-credits <n>` work as on every generating command.

## Errors

Exit 2: bad input (unsupported or too large file, clip too long). 3: signed
out. 4: not enough credits (nothing charged). 5: failed (refunded). 6:
network; retry once. 7: still running.
