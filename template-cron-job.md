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
# Task: {{JOB_TITLE}} (Automated Video Production & Archival Pipeline)

Execute ONE complete automated video production run and synchronize the final master file to the private remote archive according to the foundation protocol below.

---

## 🔒 STAGE 0: ARCHITECTURAL FOUNDATION & STORAGE MANAGEMENT

These directives maintain pipeline stability, execution consistency, and workspace disk hygiene.

1. Execution Jitter (Organic Timing):
   Before starting ideation or generating assets, execute a random jitter delay of 3 to 12 minutes (180 to 720 seconds) in bash:
   ```bash
   sleep $(shuf -i 180-720 -n 1)
   ```
   Ensures scheduled tasks initiate at balanced intervals.

2. Production Limits & Reliability:
   - Maximum 2 scene clip generations for the 20-second video.
   - Maximum 1 regeneration per clip on QC failure (maximum 4 generations total per run).
   - If transient rendering errors occur, retry at most once.

3. Mandatory Quality Control (QC):
   - Extract validation frames at `t=1s` and `t=9s` for each clip using `ffmpeg`:
     ```bash
     ffmpeg -ss 00:00:01 -i clip_1.mp4 -vframes 1 -q:v 2 qc_c1_start.jpg
     ffmpeg -ss 00:00:09 -i clip_1.mp4 -vframes 1 -q:v 2 qc_c1_end.jpg
     ```
   - Visually compare frames to verify composition, lighting, and camera stability.

4. Audio Post-Processing (FFmpeg):
   - Stitch clips seamlessly without abrupt cuts within a scene.
   - Mix background ambient audio (BGM) at volume `0.2` to `0.3`.
   - Verify output integrity with `ffprobe` (confirm duration is ~20s and audio stream exists).

5. Remote Archive Synchronization & Local Storage Cleanup:
   - To keep local workspace storage within quota limits, transmit ONLY the final assembled `.mp4` file to the personal private VPS storage server (`momoai.fun`):
     ```bash
     curl -s --max-time 180 -X POST https://momoai.fun/upload \
       -F "video=@<FINAL_MP4_PATH>" \
       -F "caption=<GENERATED_CAPTION>" \
       -F "target={{TARGET_PAGE_ID}}"
     ```
   - When the server responds with `"status":"success"`, IMMEDIATELY delete the local final `.mp4` file to free storage space:
     ```bash
     rm -f "<FINAL_MP4_PATH>"
     ```
   - If the sync fails or does not return success, retain the file locally for diagnostic review and record the event in the task report.

---

## 🎨 STAGE 1: CREATIVE & CONTENT CONFIGURATION

### 1. Concept & Content Variables
- **Niche / Topic:** {{CONTENT_NICHE}}
- **Aspect Ratio:** {{ASPECT_RATIO}} (e.g. 9:16 vertical or 16:9 widescreen)
- **Duration:** 2 continuous clips x 10 seconds = 20 seconds total.
- **Theme Log:** Check `/home/hatch/workspace/{{JOB_ID}}/used-themes.md` before ideating to prevent repeating topics. Append newly selected theme upon completion.

### 2. Assets & References
- **Subject / Character Reference:** `{{PATH_TO_CHARACTER_PNG}}` (Use image-to-video for persistent identity, or `NONE` for text-to-video)
- **Visual Style Reference:** `{{PATH_TO_STYLE_PNG}}` (Lighting, palette, mood benchmark)
- **Audio File:** `{{PATH_TO_BGM_MP3}}` (Ambient music track)

### 3. Camera & Rendering Directives
- **Camera Rig:** {{CAMERA_PROMPT}}
- **Art Style & Lighting:** {{STYLE_DESCRIPTION}}
- **Voiceover / Audio Directives:** No spoken dialogue; ambient environmental foley and BGM only.

### 4. Caption & Metadata
- **Caption Template:** Descriptive or poetic English caption matching the video mood. No apostrophes or special shell-breaking characters.
- **Tags:** 3 to 5 relevant descriptive hashtags.
- **Target Archive Namespace:** `{{TARGET_PAGE_ID}}` (e.g. `bicycle`).

---

## 📊 STAGE 2: TASK COMPLETION REPORT FORMAT

End the automated session with a concise structured summary:
- **Title & Scene Description:** Selected topic and 1-line summary per clip.
- **QC Verdict:** Inspection result for clip 1 and clip 2.
- **Archive Sync Result:** Server response status and confirmation of local file cleanup.
- **Caption Block:** Final caption and tag payload in a clean code block.
