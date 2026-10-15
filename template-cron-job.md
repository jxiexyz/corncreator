---
id: {{JOB_ID}}
title: {{JOB_TITLE}}
enabled: true
owner: personal-creative-vault
mode: task
schedule:
  kind: cron
  cron: "{{CRON_SCHEDULE}}"
  timezone: {{TIMEZONE}}
timeout_secs: 5400
metadata:
  presentation_locale: en-US
---
# Task: {{JOB_TITLE}} (Automated Video Production Pipeline)

Execute ONE complete automated video production run and push to the remote distribution bridge according to the locked foundation protocol below.

---

## 🔒 STAGE 0: LOCKED ARCHITECTURAL FOUNDATION (DO NOT ALTER)

These directives maintain pipeline stability, evasion of bot detection, cloud storage hygiene, and ban prevention.

1. Organic Execution Jitter:
   Before any ideation, API calls, or generation, inject random bash jitter between 3 and 12 minutes (180 to 720 seconds):
   ```bash
   sleep $(shuf -i 180-720 -n 1)
   ```
   Ensures runs trigger at natural, non-identical timestamps.

2. Production Limits & Circuit Breaker:
   - Maximum 2 scene clip generations for the 20-second video.
   - Maximum 1 regeneration per clip on QC failure (maximum 4 generations total per run).
   - If transient API errors occur, retry at most once. Never loop indefinitely.

3. Mandatory Quality Control (QC):
   - Extract validation frames at `t=1s` and `t=9s` for each clip using `ffmpeg`:
     ```bash
     ffmpeg -ss 00:00:01 -i clip_1.mp4 -vframes 1 -q:v 2 qc_c1_start.jpg
     ffmpeg -ss 00:00:09 -i clip_1.mp4 -vframes 1 -q:v 2 qc_c1_end.jpg
     ```
   - Visually compare frames to verify consistency, correct lighting, and adherence to camera movement rules.

4. Audio Post-Processing (FFmpeg):
   - Stitch clips seamlessly without abrupt cuts inside a clip.
   - Mix background ambient audio (BGM) at volume `0.2` to `0.3`.
   - Verify output integrity with `ffprobe` (confirm duration is ~20s and audio stream exists).

5. Air-Gapped Egress & Auto-Cleanup (Anti-Ban Guarantee):
   - Muse must NEVER connect directly to Facebook, Instagram, or social platform APIs.
   - Transmit ONLY the final assembled `.mp4` file to the remote VPS bridge via cURL:
     ```bash
     curl -s --max-time 180 -X POST https://momoai.fun/upload \
       -F "video=@<FINAL_MP4_PATH>" \
       -F "caption=<GENERATED_CAPTION>" \
       -F "target={{TARGET_PAGE_ID}}"
     ```
   - When the server responds with `"status":"success"`, IMMEDIATELY delete the local final `.mp4`:
     ```bash
     rm -f "<FINAL_MP4_PATH>"
     ```
   - If upload fails, retain the file locally for diagnostic review and record failure in the task report.

---

## 🎨 STAGE 1: CREATIVE & CONTENT CONFIGURATION (CUSTOMIZE HERE)

<!-- 
EDIT THE VALUES IN THIS SECTION FOR EACH SPECIFIC NICHE / THEME.
LEAVE STAGE 0 UNTOUCHED.
-->

### 1. Concept & Content Variables
- **Niche / Topic:** {{CONTENT_NICHE}} (e.g. Cozy Anime, Cyberpunk Street, Nature Meditation, Tech Explainer)
- **Aspect Ratio:** {{ASPECT_RATIO}} (e.g. 9:16 vertical for Reels/Shorts/TikTok or 16:9 for YouTube)
- **Duration:** 2 continuous clips x 10 seconds = 20 seconds total.
- **Theme Log:** Check `/home/hatch/workspace/{{JOB_ID}}/used-themes.md` before ideating to prevent repeating topics. Append newly selected theme upon completion.

### 2. Assets & References
- **Subject / Character Reference:** `{{PATH_TO_CHARACTER_PNG}}` (Use image-to-video for persistent identity, or `NONE` for pure text-to-video)
- **Visual Style Reference:** `{{PATH_TO_STYLE_PNG}}` (Lighting, palette, mood benchmark)
- **Audio File:** `{{PATH_TO_BGM_MP3}}` (Ambient music track)

### 3. Camera & Rendering Directives
- **Camera Rig:** {{CAMERA_PROMPT}}
  *(Example for static shots: "CAMERA LOCKED ON A TRIPOD — completely static, absolutely zero camera movement: no pan, no tilt, no zoom, no drift. Viewpoint frozen, only the living environment moves.")*
- **Art Style & Lighting:** {{STYLE_DESCRIPTION}}
- **Voiceover / Audio Directives:** No spoken dialogue; ambient environmental foley and BGM only.

### 4. Caption & Social Metadata
- **Caption Template:** Poetic or engaging English caption matching the video mood. No apostrophes or special shell-breaking characters.
- **Hashtags:** 3 to 5 targeted hashtags (e.g. `#cozyanime #chillvibes #lofiart`).
- **Bridge Target:** `{{TARGET_PAGE_ID}}` (Identifier configured in `fb_config.json`, e.g. `wintermute` or `default`).

---

## 📊 STAGE 2: TASK COMPLETION REPORT FORMAT

End the automated session with a concise structured summary:
- **Title & Scene Description:** Selected topic and 1-line summary per clip.
- **QC Verdict:** Inspection result for clip 1 and clip 2.
- **Remote Sync Result:** Bridge HTTP response, status code, and confirmation of local file cleanup.
- **Caption Block:** Final caption and hashtag payload in a clean code block.
