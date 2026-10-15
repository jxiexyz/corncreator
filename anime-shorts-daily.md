---
id: anime-shorts-daily
title: Daily Cozy Anime Animation
enabled: true
owner: personal-creative-vault
mode: task
schedule:
  kind: daily
  timezone: America/New_York
  time: 02:00:00
timeout_secs: 3600
metadata:
  presentation_locale: en-US
---
# Task: Generate 1 Daily Cozy Anime Short (Pipeline v4.3)

Execute ONE full animation following the pipeline specification: `/home/hatch/workspace/user/files/anime-shorts-pipeline-full_0_1heh.md` — PART 1 only (PART 2 addendum is proposed, do not use).

## Context
- User: Creator of vertical 9:16 aesthetic anime shorts. Casual, friendly tone.
- Daily scheduled run. Check `/home/hatch/workspace/anime-shorts/used-themes.md` first for historical themes and the most recent DAY/NIGHT selections.

## Assets (Existing — Do Not Regenerate)
- Character Reference: `/home/hatch/workspace/anime-shorts/main-character.png` — use as image-to-video reference across ALL clips.
- Aesthetic References: `/home/hatch/workspace/anime-shorts/ref-cozy-night.png`, `ref-cozy-day.png`, `ref-cozy-stream.png` — use as reference for start-frame artistic style.
- Audio: `/home/hatch/workspace/anime-shorts/bgm.mp3`.
- Tools: `ffmpeg` + `ffprobe` available at `/usr/bin`.

## Core Directives
1. DAY/NIGHT: Alternate randomly per run, but never pick the same lighting 3 times in a row (check `used-themes.md`).
2. Ideation: Follow the "Ideation engine — LOCKED as permanent Stage 1" rules + "LIVED-IN VARIETY" principles: anti-generic, specific location, micro-activities, environmental interaction, and camera framing distinct from past logged entries. Append new theme to `used-themes.md`.
3. Format: 2 clips x 10 seconds = 20 seconds total. NO cuts within a clip — each clip is ONE continuous static shot, full cozy view. The only transition cut occurs between clip 1 and clip 2 during final stitching.
4. Camera: Use the verbatim prompt: "CAMERA LOCKED ON A TRIPOD — completely static, absolutely zero camera movement: no pan, no tilt, no zoom, no drift, no tracking, no push-in, no pull-back. The viewpoint is FROZEN — every background element stays pixel-locked in the exact same position from the first frame to the last; only the living world moves."
5. Mandatory QC per clip: Extract frames at t=1s and t=9s, visually inspect and compare composition. If camera drifts, zooms, or morphs, regenerate that clip (maximum 1 regeneration per clip).
6. Style Matching (Stage 2): Brief facial features as "narrow expressive brown eyes (not big round moe eyes), gentle closed-lip smile, vivid and lively anime face" — preserve character identity from `main-character.png` (messy bun hairstyle, face-framing strands, oversized black sweatshirt).
7. Character placement: Small in frame; NO fireflies in any prompt; no spoken dialogue (ambient environmental sound only); vertical 9:16 aspect ratio; style bible, lighting, and cozy mood strictly matched to DAY or NIGHT pick.
8. Generation limits: Maximum 2 video generations for the film + maximum 1 regeneration per clip if QC fails (maximum 4 total). If a transient error occurs, retry once; if it persists, stop gracefully and report the outcome.
9. Stage 4B: Mix `bgm.mp3` with volume 0.2 into the FINAL film only; verify duration is ~20 seconds with `ffprobe` and ensure the audio stream exists.
10. Stage 4C (Cloud Archive Sync): AFTER the final film is assembled and verified, upload ONLY the final mp4 to the archive endpoint:
   ```bash
   curl -s --max-time 180 -X POST https://momoai.fun/upload -F "video=@<final-mp4>" -F "caption=<English poetic caption + hashtags>"
   ```
   Do NOT upload raw clips or start frames. If the server response contains `"status":"success"`, DELETE the local final mp4 file (only that file — raw clips, start frames, and run directories remain saved). If the upload fails or does not return success, keep the local file, record the event in the report, and retry at most once.

## Report Format
- Film Title, 1-line description per clip, QC verdict per clip, archive sync result (success/failure + server response), and pipeline deviations if any.
- Poetic ENGLISH caption + 3-5 relevant hashtags in a plain code block (no apostrophes).
- Note: If Stage 4C archive sync succeeds, state that the local final file was cleaned up per guidelines — no need to attach the video file in chat.
- Language: Casual, friendly tone.
