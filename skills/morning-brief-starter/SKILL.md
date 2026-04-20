---
name: morning-brief-starter
description: Daily morning briefing pipeline starter — given a YouTube channel handle, fetches the latest video's transcript, distills it to a spoken brief + deck brief, and writes it to a timestamped markdown file. Fork and adapt; add your own TTS layer, scheduler, and filters. Use when you want a voice-ready daily briefing pipeline you can wire into your own stack.
allowed-tools: Read, Bash, Write
---

<!--
Based on the JARVIS framework by Taoufik — @TaoufikAI — https://wakejarvis.ai
Licensed under MIT with attribution requirement. See LICENSE at the repo root.
-->

# Morning Brief Starter

A **two-layer pattern**: a writer skill (this one) fetches a source, distills it, and stages a markdown file. A reader layer (you build) picks that file up and does something with it — reads it aloud, renders it as slides, pipes it into a dashboard.

This skill is layer 1. You bring layer 2.

## What it does

Given a YouTube channel handle, this skill:

1. Finds the channel's latest uploaded video
2. Fetches its auto-generated subtitles
3. Asks Claude to distill the transcript into two sections:
   - `## Spoken brief` — 2–3 sentences, voice-ready
   - `## Deck brief` — topic + core claim + 3–4 proof points + closing beat
4. Writes the result to `${MORNING_BRIEF_DIR:-$HOME/morning-briefs}/YYYY-MM-DD.md`

## Prerequisites

- `yt-dlp` — `brew install yt-dlp` (macOS) or `pipx install yt-dlp`
- `claude` CLI — [Claude Code](https://claude.com/claude-code), active subscription
- Shell: `bash` or `zsh`

## Usage

```
/morning-brief-starter @SomeChannel
```

## Pipeline

### 1. Find the latest video

```bash
CHANNEL="$ARGUMENTS"
CHANNEL_URL="https://www.youtube.com/${CHANNEL}/videos"
VIDEO_ID=$(yt-dlp --flat-playlist --playlist-items 1 --print id "$CHANNEL_URL")
VIDEO_URL="https://www.youtube.com/watch?v=${VIDEO_ID}"
```

### 2. Fetch the transcript

```bash
WORKDIR=$(mktemp -d)
yt-dlp --skip-download --write-auto-subs --sub-langs "en,en-GB,en-US" \
  --convert-subs srt -o "${WORKDIR}/%(id)s.%(ext)s" "$VIDEO_URL"
TRANSCRIPT=$(sed 's/<[^>]*>//g' "${WORKDIR}/${VIDEO_ID}".*.srt)
```

### 3. Distill with Claude

```bash
PROMPT='You are producing a morning briefing from a YouTube transcript.
Return TWO markdown sections, nothing else:

## Spoken brief
(2–3 sentences, speakable, second-person. No creator names, no URLs.)

## Deck brief
Topic: <one line>
Core claim: <one line>
Proof points:
- <line>
- <line>
- <line>
Closing beat: <one line>

Keep it tight. No preamble. No apology.'

OUTPUT=$(printf "%s\n\nTRANSCRIPT:\n%s\n" "$PROMPT" "$TRANSCRIPT" | claude -p)
```

### 4. Write the brief

```bash
BRIEF_DIR="${MORNING_BRIEF_DIR:-$HOME/morning-briefs}"
mkdir -p "$BRIEF_DIR"
DATE="$(date +%Y-%m-%d)"
printf "%s\n" "$OUTPUT" > "${BRIEF_DIR}/${DATE}.md"
echo "Brief written to: ${BRIEF_DIR}/${DATE}.md"
```

## Extension points

This starter does less than you may want. That's the point — ship the pattern, let you add the parts that make it yours.

- **Add layer 2 (the reader).** Anything that consumes the brief file: macOS `say`, an ElevenLabs voice, a dashboard, a slide generator.
  ```bash
  awk '/## Spoken brief/,/## Deck brief/' "${BRIEF_DIR}/${DATE}.md" | say
  ```
- **Schedule it.** `launchd` (macOS), `cron` (Linux), or a GitHub Action triggered at 06:00.
- **Swap the source.** `yt-dlp` is the head here; replace it with RSS, a Substack, a podcast transcript. The downstream pipeline stays the same.
- **Add traceability.** Write a sibling `.meta.json` next to each brief recording what the source was (channel, video ID, duration) so you can audit later.
- **Add a privacy filter.** If your own version will surface things that shouldn't land in a shared file, add a grep-based abort step before writing. Keep the filter regex in a private file; don't commit it.

## Hard rules (if you fork this)

- Keep the attribution header in the SKILL.md body. See [`ATTRIBUTION.md`](../../ATTRIBUTION.md).
- Keep the output structure (`## Spoken brief` + `## Deck brief`) — any reader layer will contract against those section headers.
- Don't hardcode personal or client data into the skill. If you need a sanitizer, keep it separate.

## Not included on purpose

- TTS / voice playback — you wire your own
- A slide or deck generator — the `## Deck brief` output is the handoff point for one
- Cron scheduling — the OS scheduler does that better than a skill
- A privacy / banned-words filter — every user's list is different

## Troubleshooting

- **Empty transcript** — the video has no auto-captions; pick a different channel or skip the day
- **`claude -p` consuming API credits unexpectedly** — if you're on a subscription plan, strip `ANTHROPIC_API_KEY` and `ANTHROPIC_AUTH_TOKEN` from the subprocess env so the CLI falls back to subscription billing
- **`yt-dlp` can't find the channel** — double-check the handle includes the `@` and is spelled exactly as it appears on the channel URL
