---
id: anime-shorts-daily
title: Daily Cozy Anime Animation (Reference Recipe)
base_template: template-cron-job.md
enabled: true
schedule:
  cron: "0 1,8,19 * * *"
  timezone: America/New_York
timeout_secs: 5400
---

# Recipe: Daily Cozy Anime Shorts

Implementation of `template-cron-job.md` configured for automated vertical cozy anime content.

---

## 🎨 PRESET VALUES & CONFIGURATION

Inherits architectural rules, jitter timing, QC logic, and disk hygiene from `template-cron-job.md`.

```yaml
JOB_ID: "anime-shorts-daily"
CONTENT_NICHE: "Cozy lofi anime daily life"
ASPECT_RATIO: "9:16"
TARGET_DURATION: "20s (2 clips x 10s)"

PATHS:
  theme_log: "/home/hatch/workspace/anime-shorts/used-themes.md"
  character_ref: "/home/hatch/workspace/anime-shorts/main-character.png"
  style_refs:
    - "/home/hatch/workspace/anime-shorts/ref-cozy-night.png"
    - "/home/hatch/workspace/anime-shorts/ref-cozy-day.png"
    - "/home/hatch/workspace/anime-shorts/ref-cozy-stream.png"
  audio_bgm: "/home/hatch/workspace/anime-shorts/bgm.mp3"

DIRECTIVES:
  camera_rig: >-
    CAMERA LOCKED ON A TRIPOD — completely static, absolutely zero camera movement:
    no pan, no tilt, no zoom, no drift, no tracking, no push-in, no pull-back.
    The viewpoint is FROZEN — every background element stays pixel-locked in the
    exact same position from the first frame to the last; only the living world moves.
  character_identity: >-
    narrow expressive brown eyes (not big round moe eyes), gentle closed-lip smile,
    vivid and lively anime face, messy bun hairstyle, face-framing strands,
    oversized black sweatshirt. Small in frame.
  audio: "Ambient environmental sound only + BGM volume 0.2. No spoken dialogue."

ARCHIVE:
  target_id: "wintermute"
```

---

## 📋 WORKFLOW SUMMARY

1. **Jitter:** Wait 3–12m randomly (`sleep $(shuf -i 180-720 -n 1)`).
2. **Context:** Read `used-themes.md`, alternate DAY/NIGHT (never 3x identical). Record new theme.
3. **Render:** Generate 2x 10s clips using `main-character.png` as visual anchor + camera rig directive.
4. **QC:** Extract frames at `t=1s` and `t=9s` via FFmpeg. Check for character morph or camera drift (max 1 regen/clip).
5. **Mux:** Stitch clips and mix `bgm.mp3` (vol 0.2). Verify stream with `ffprobe`.
6. **Archive & Clean:** POST master to archive endpoint. On `"status":"success"`, purge local MP4.
