---
name: makemidas-image
description: Generate or edit AI images with the MakeMidas CLI (Nano Banana, GPT Image, Seedream, Flux, Grok Imagine, Qwen Image) from a prompt or reference pictures, save them to disk, and compare models side by side. Use when the user asks to generate, draw or render an image, illustration, photo, product shot, icon, cover, poster, mockup or thumbnail, to edit, restyle or combine pictures, or to compare image models; also for "生成图片", "画一张", "出图". Spends the user's MakeMidas credits.
license: MIT
---

# Images with MakeMidas

## Before you start

1. `makemidas --version`. If missing, use `npx -y @makemidas/cli` in its
   place (Node.js 20+); ask before `npm i -g @makemidas/cli`.
2. `makemidas auth status --json`. Exit code 3 means signed out: run
   `makemidas auth login` (the user approves in the browser), or use
   `MAKEMIDAS_API_KEY` from the user. Never print or store the key.
3. Images spend the user's credits. Unless asked otherwise: one image, the
   default model, `1:1`. Say what a run costs when it is more than a few
   credits (4K, several images, an expensive model).

## Generate

```bash
makemidas image "a red fox in fresh snow, golden hour, 35mm film" --aspect 3:2 --json
makemidas image "same fox as a watercolor" --ref fox.png --model <id> --json
makemidas image "a poster for a jazz night" -r 4K --max-credits 60 --json
```

- `--model <id>`: from `makemidas models image --json`, which lists ids,
  sizes, reference limits and prices. Leave it out for the cheapest model.
  To find a family (Nano Banana, GPT Image…), match the label in that list.
- `--aspect` (`1:1`, `16:9`, `9:16`, `3:2`, …), `--resolution 1K|2K|4K`
  (models that list sizes), `--count 1-4`.
- `--ref <file|url>` adds a reference image; repeat for more (up to the
  model's limit). With a reference, the prompt says what to change or keep.
- `--style <id>` (photo, cinematic, film, anime, …); `--enhance` lets
  MakeMidas expand a short prompt.
- `--out <dir>` picks the folder (default: current directory). Files are
  named `makemidas-<id>-<n>.<ext>`; existing files are kept unless `--force`.
  `--no-download` prints signed links (valid one hour) instead.

Images come back within a minute or two. With `--json` the command prints
the generation (`{ id, kind, model, status, credits, outputs, files }`),
where `files` are the saved paths.

## Compare models

```bash
makemidas compare "a tiny bakery on a rainy street at dusk" --models <id-a>,<id-b> --json
```

Two to four different image model ids, one image each, each at its own
price; `--max-credits` caps the total. Models that could not start are
listed as warnings and cost nothing.

## Prices

```bash
makemidas estimate image "a poster" -r 4K --model <id> --json
```

## Errors

Exit 2: bad input (check `makemidas models <id>`). 3: signed out. 4: not
enough credits or a plan limit (nothing charged). 5: the model refused or
failed (refunded): change the prompt or model. 6: network; retry once.

## Show the result

Show the user the saved files (open or embed them if your environment can).
