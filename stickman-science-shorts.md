---
id: stickman-science-shorts
title: Stickman Science Thought Experiments
enabled: true
owner: personal-creative-vault
mode: task
schedule:
  kind: cron
  cron: "0 6,12,18 * * *"
  timezone: Asia/Jakarta
timeout_secs: 5400
metadata:
  presentation_locale: en-US
---
# Task: Generate Daily Stickman Science Short (What-If Animation)

Execute ONE full automated episode following the production pipeline and synchronize the final master file to the remote Facebook Reels archive bridge.

---

## 🔒 STAGE 0: ARCHITECTURAL FOUNDATION & STORAGE MANAGEMENT

1. Execution Jitter (Organic Timing):
   Before starting ideation or generating assets, execute a random jitter delay of 3 to 12 minutes (180 to 720 seconds) in bash:
   ```bash
   sleep $(shuf -i 180-720 -n 1)
   ```
   Ensures scheduled tasks initiate at balanced intervals.

2. Production Limits & Reliability:
   - Exactly 2 scene clips generated for the 20-second video (10s Clip A + 10s Clip B).
   - Maximum 1 regeneration per clip on QC failure (maximum 4 generations total per run).
   - If transient rendering errors occur, retry at most once.

3. Mandatory Quality Control (QC):
   - Extract validation frames at `t=1s` and `t=9s` for each clip using `ffmpeg`:
     ```bash
     ffmpeg -ss 00:00:01 -i clip_a.mp4 -vframes 1 -q:v 2 qc_ca_start.jpg
     ffmpeg -ss 00:00:09 -i clip_a.mp4 -vframes 1 -q:v 2 qc_ca_end.jpg
     ```
   - Visually compare frames to verify character consistency, line weight, and camera stability.

4. Video Assembly & Audio Processing (FFmpeg):
   - Stitch Clip A and Clip B seamlessly into a continuous 20-second vertical video (9:16 aspect ratio).
   - Mix subtle ambient background music or sound effects if available.
   - Verify output integrity with `ffprobe` (confirm duration is ~20s and resolution is vertical 9:16).

5. Remote Archive Synchronization & Local Storage Cleanup:
   - Transmit ONLY the final assembled `.mp4` file to the personal Facebook Reels bridge endpoint (`https://momoai.fun/upload`):
     ```bash
     curl -s --max-time 180 -X POST https://momoai.fun/upload \
       -F "video=@<FINAL_MP4_PATH>" \
       -F "caption=<INDONESIAN_CAPTION_AND_TAGS>" \
       -F "target=sitikman"
     ```
   - Parameter `target=sitikman` is mandatory so the video routes directly to the Facebook Page "Sitikman jos".
   - When the server responds with `"status":"success"`, IMMEDIATELY delete the local final `.mp4` file to free storage space:
     ```bash
     rm -f "<FINAL_MP4_PATH>"
     ```
   - If the sync fails or does not return success, retain the file locally for diagnostic review and record the event in the task report.

---

## 🎨 STAGE 1: CREATIVE & CONTENT SPECIFICATIONS

### 1. Concept & Content Variables
- **Niche / Theme:**
  Hypothetical scientific "What If?" educational thought experiment formatted in Indonesian:
  `"Apa yang terjadi jika kamu [skenario absurd / ekstrem / aneh]?"`
- **Topic Reference Log:**
  Check `/home/hatch/workspace/stickman/used-themes.md` before ideating to prevent repeating topics. Append newly selected topic upon completion.
- **Example Topics:**
  - *"Apa yang terjadi jika kamu jatuh ke lubang menembus inti bumi?"*
  - *"Apa yang terjadi jika kamu tidak tidur sama sekali selama 7 hari?"*
  - *"Apa yang terjadi jika bumi berhenti berputar selama 1 detik?"*
  - *"Apa yang terjadi jika kamu menelan biji semangka atau permen karet?"*
  - *"Apa yang terjadi jika semua oksigen di bumi hilang selama 5 detik?"*
  - *"Apa yang terjadi jika kamu menjilat tiang besi yang membeku?"*
  - *"Apa yang terjadi jika kamu bersin dengan mata terbuka?"*

### 2. Assets & References
- **Character Reference:** `/home/hatch/workspace/corncreator/assets/stickman_ref.jpg` (2D White stickman caveman with round white head, clean black outline, thin limbs, brown animal-hide vest with diagonal chest strap, brown headband, jagged brown loincloth).
- **Format:** Vertical 9:16 (1080x1920 or 720x1280).
- **Duration:** 2 continuous clips x 10 seconds = 20 seconds total.
  - **Clip A (0-10s):** Hook & Setup. The stickman enthusiastically tries or encounters the absurd scenario.
  - **Clip B (10-20s):** Scientific Mechanism & Comedic Resolution. Visual representation of the biological/physical reaction and comedic conclusion.

### 3. Visual Style & Rendering Directives (English Prompts for Video Generator)
- **Prompt Structure:** Always begin with `"Generate video: 10-second 2D minimalist cartoon animation in 9:16 vertical format."`
- **Aesthetic:** Minimalist 2D flat cartoon vector style, clean black line art, bright solid colors, clean geometric background (e.g., stylized grassy savanna, ice plain, cartoon laboratory).
- **Motion:** Expressive 2D character animation, clear body language, smooth cartoon physics (squash and stretch).
- **Strict Negative Constraints:** No realistic 3D rendering, no human face textures, no extra limbs, no morphing, no text overlays on video, no subtitles inside video frames.

### 4. Caption & Metadata (Bahasa Indonesia)
Format the upload caption in clean Indonesian for Facebook Reels:
```text
[Judul / Hook Pertanyaan: "Apa yang terjadi jika kamu ...?"]

[Penjelasan ilmiah santai 2-3 kalimat yang menjelaskan reaksi tubuh atau fenomena sains yang terjadi secara logis dan menghibur.]

💡 [1 kalimat kesimpulan atau fakta menarik ringkas.]

#edukasi #sains #animasistickman #sitikmanjos #reels #faktamenarik
```
*Note: Strict rule: DO NOT mention "Momo AI" or "MOMO AI" in any text, caption, or prompt.*

---

## 📊 STAGE 2: TASK COMPLETION REPORT FORMAT

End the automated session with a concise structured summary:
- **Episode Topic:** The chosen scientific question.
- **Clip Breakdown:** 1-line summary of Clip A (Hook) and Clip B (Resolution).
- **QC Verdict:** Inspection result for Clip A and Clip B.
- **Archive Sync Result:** Server response from `https://momoai.fun/upload` and confirmation of local file cleanup.
- **Caption Block:** Final Indonesian caption and hashtag payload.
