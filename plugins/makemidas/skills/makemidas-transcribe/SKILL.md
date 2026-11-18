---
name: makemidas-transcribe
description: Transcribe audio or video to text with timestamps and speakers using the MakeMidas CLI (Whisper and Nemotron), from a local file or a link, and save it as TXT, SRT, VTT or JSON. Use when the user wants a transcript, subtitles or captions file, meeting or interview notes from a recording, or the text of a podcast or video; also for "转写", "语音转文字", "字幕". Spends the user's MakeMidas credits, priced by the recording's length.
license: MIT
---

# Transcripts with MakeMidas

## Before you start

1. `makemidas --version`. If missing, use `npx -y @makemidas/cli` in its
   place (Node.js 20+); ask before `npm i -g @makemidas/cli`.
2. `makemidas auth status --json`. Exit code 3 means signed out: run
   `makemidas auth login` (the user approves in the browser), or use
   `MAKEMIDAS_API_KEY` from the user. Never print or store the key.
3. Transcription is priced by the length of the recording:

```bash
makemidas estimate transcribe interview.m4a --json
```

## Transcribe

```bash
makemidas transcribe interview.m4a --json                  # saves a .txt
makemidas transcribe talk.mp4 --format srt --out subs --json
makemidas transcribe https://example.com/episode.mp3 -o -  # print the text
makemidas transcribe meeting.wav -m nvidia/nemotron-3.5-transcribe --json
```

- Input: a local file (MP3, WAV, M4A, AAC, FLAC, OGG, Opus, WEBM, MP4 or
  MOV, up to 100 MB and 60 minutes), a link the server downloads, or a
  `fileId` an earlier upload printed in `--json`.
- `--format txt|srt|vtt|json` (default `txt`); `-o -` prints the
  transcript on stdout; `--title` names it.
- `-m, --model`: the default is `openai/whisper-large-v2`;
  `nvidia/nemotron-3.5-transcribe` is faster and cheaper.
  `makemidas models transcription --json` lists both with prices.

Uploaded files are deleted after 7 days. The CLI waits for the transcript;
`--no-wait` and `makemidas get <id> --wait` run it in the background.

## Errors

Exit 2: bad input (an unsupported or too long file). 3: signed out. 4: not
enough credits (nothing charged). 5: failed (refunded). 6: network; retry
once. 7: still running.

## Rules

Only transcribe recordings the user has the right to process, and treat
transcripts of other people's conversations as private.
