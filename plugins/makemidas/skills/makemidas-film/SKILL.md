---
name: makemidas-film
description: Make a short film, ad or multi-shot video with the MakeMidas CLI film projects - a brief becomes a script and storyboard, the user approves the first frames and then the film at the credits shown, and MakeMidas generates every shot (Seedance, Kling, Veo...), the voiceover and music and assembles one MP4; or download the pieces and a timeline to cut it yourself with ffmpeg. Use when the user wants a short film, a video ad, a trailer, a product story, an explainer with several scenes, or a storyboard turned into video; also for "做一部短片", "做个广告片", "分镜", "多镜头视频". For one clip use makemidas-video. Spends the user's MakeMidas credits, approved stage by stage.
license: MIT
---

# Short films with MakeMidas

A film project runs in stages, and each paid stage runs only at a price the
user approved:

1. **Storyboard**: the chosen text model writes a script and a list of shots
   (charged by the tokens it writes, usually a few credits).
2. **Frames**: an image model draws each shot's first frame.
3. **Film**: a video model animates each frame, a voiceover model reads the
   narration, a music model scores it, and MakeMidas assembles the film
   (assembly is free).

## Before you start

1. `makemidas --version` (0.3.0 or later has `film`). If missing, use
   `npx -y @makemidas/cli` in its place (Node.js 20+).
2. `makemidas auth status --json`; exit 3 means signed out: `makemidas auth
   login`, or `MAKEMIDAS_API_KEY` from the user. Never print the key.
3. Videos need a plan or credit pack. `makemidas credits --json` shows the
   balance.

## 1. Write the brief and the storyboard

A good brief says what the film is for, who watches it, the story beats, the
look (palette, light, lens, era), the shape (`9:16` for Reels/TikTok/Shorts,
`16:9` for YouTube, `1:1` for feeds) and any line of narration. If the user
gave only a topic, write the brief with them first; for a brand, read it
(`makemidas brands default`) and follow the makemidas-creative-director
skill.

```bash
makemidas film new "<brief>" --seconds 20 --aspect 9:16 --max-credits 40 --json
```

- `--seconds` 10-120 (about how long; the model picks clip lengths the video
  model makes).
- Cheapest useful settings for a first cut: `--video-model
  bytedance/seedance-2.0-mini -r 480p --frame-model
  black-forest-labs/flux-3-image-klein --music-model none`. Better looks
  cost more; check `makemidas models video --json` for per-second prices.
- Other settings: `--text-model`, `--speech-model`, `--voice <id|default>`,
  `--music-model`, `--music-prompt`, `--transition cut|fade|crossfade`,
  `--clip-audio`, `--brand <id|none>`.
- `--no-write` creates the project without writing; `makemidas film write
  <id> --max-credits 40` writes (or rewrites) it later.

Show the user the script and the shots (`makemidas film get <id>`), and
offer changes. To edit, write the whole storyboard to a JSON file (a list of
shots with `prompt` and `duration`; keep each shot's `id` to keep its frame)
and run `makemidas film update <id> --file shots.json`. Shot prompts work best
in English, self-contained, repeating the same character and style words in
every shot.

## 2. Approve the frames

`makemidas film get <id> --json` gives `next` and `pendingCredits`. **Tell
the user the price and wait for their yes**, then:

```bash
makemidas film approve <id> frames --quoted-credits <pendingCredits> --wait --json
```

Frames start within a minute. Show them (`shots[].frame.url`) and redo any
the user dislikes: `makemidas film redo <id> <shot-number> frame
--max-credits 20`.

## 3. Approve the film

Again show the price (`pendingCredits`) and wait for a yes:

```bash
makemidas film approve <id> film --quoted-credits <pendingCredits> --wait -o . --json
```

`--wait` follows the clips, the voiceover, the music and the assembly
(default 15 minutes, `--timeout <min>`), and saves `film-<id>.mp4` when it
is done. To not block your shell, leave out `--wait` and run `makemidas film
get <id> --wait -o .` later (in the background). While the film assembles,
poll every 5-10 seconds: each check moves the render along.

- Redo one piece: `makemidas film redo <id> <shot> clip|voice --max-credits
  <n>` (a frame redo after approval also remakes that clip). After approval
  only the voiceover model and voice can change: `makemidas film update <id>
  --speech-model <id> --voice <id>`, then redo the voice.
- Paused (out of credits, exit 4): the user tops up, then `makemidas film
  resume <id>`.

## Cut it yourself

```bash
makemidas film download <id> --pieces -o ./cut
```

saves every clip, voiceover and the music and `timeline.json` (each piece's
`start`, `duration` and local `file`, plus `transition` and a `howTo`). Trim
each clip to its duration, join them (`concat`, or `xfade` for fades), lay
each voiceover at its start (`adelay`), put the music under at about 0.3
volume (`amix`), and encode H.264 + AAC. Do this when the user wants
subtitles, a logo end card, their own music, or a different cut.

## Prices and rules

- Never pass `--quoted-credits` the user hasn't agreed to. Without
  `--quoted-credits` or `--max-credits`, paid steps print the price and exit 2;
  a changed price answers `price_changed` and charges nothing.
- Exit codes: 2 bad input or nothing to approve, 3 signed out, 4 not enough
  credits / paused / over `--max-credits`, 5 a piece or the assembly failed,
  6 network, 7 still working.
- Don't put real people in films without their consent.
- In ChatGPT, Claude or another MCP client, the MakeMidas MCP tools
  `create_film_project`, `get_film_project`, `update_storyboard`,
  `approve_film_project`, `redo_film_shot` and `assemble_film` do the same.
