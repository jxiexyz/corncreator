# CornCreator 🌽

Universal foundation framework and cron automation templates for autonomous AI video agents (Muse / Meta Hatch).

---

## 🏛️ Architecture: Decoupled & Air-Gapped

To generate AI videos and distribute them to social platforms (Facebook Reels, TikTok, YouTube Shorts) without triggering region blocks or account bans, the generation engine and the social uploader **must be completely decoupled**:

```
┌──────────────────────────────────────────────┐
│            MUSE AI AGENT (Sandbox)           │
│  1. Cron Trigger + Organic Jitter (3-12 min) │
│  2. Generate 2 continuous clips (20s total)  │
│  3. Frame QC Inspection (t=1s & t=9s)        │
│  4. FFmpeg Audio Mix & ffprobe verify        │
│  5. Push binary to VPS Bridge via cURL       │
│  6. Auto-delete local MP4 on status 200      │
└──────────────────────┬───────────────────────┘
                       │ HTTP POST (Multipart)
                       ▼
┌──────────────────────────────────────────────┐
│           VPS BRIDGE (momoai.fun)            │
│  FastAPI receiver (`/upload?target=...`)     │
│  - Receives video & caption                  │
│  - Selects Facebook Page credentials         │
│  - Uploads to Facebook Reels via Graph API   │
│  - Sends rich Telegram notification + link   │
│  - Cleans up VPS temp storage immediately    │
└──────────────────────────────────────────────┘
```

### Why This Prevents Bans
- **Zero Social Metadata:** Muse never authenticates to Facebook, never handles tokens, and never hits `graph.facebook.com`.
- **Evasion of Geofencing:** Prevents Meta age verification and Asian/Indonesian region flagging from locking the video generation module.
- **Organic Execution Jitter:** Injects random delays (`sleep 180-720s`) so tasks never fire on machine-exact second marks.
- **Disk Hygiene:** Video files are wiped locally as soon as the bridge acknowledges receipt, preventing VM disk exhaustion.

---

## 📁 Repository Structure

| File | Purpose |
| :--- | :--- |
| **`template-cron-job.md`** | **The universal foundation template.** Contains locked architectural rules (Stage 0) and customizable creative fields (Stage 1). |
| **`anime-shorts-daily.md`** | **Production reference.** Live 3x daily vertical anime short automation targeting the *Wintermute Cozy Style* page. |

---

## 🚀 How to Create a New Cron Automation

### 1. In Muse Workspace
Clone or pull this repository inside your Muse environment:
```bash
git clone https://github.com/jxiexyz/corncreator.git ~/corncreator
```

### 2. Prompt Muse to Create the New Job
Prompt your Muse AI agent with the instructions below:

> *"Buatkan file cron job baru di `/home/hatch/workspace/cron/<nama-tugas>.md` berdasarkan template `/home/hatch/workspace/corncreator/template-cron-job.md`.*
> 
> *Kunci dan pertahankan seluruh **STAGE 0 (Locked Foundation)** tanpa ada yang diubah (Jitter sleep, QC sampling t=1s & t=9s, FFmpeg mixing, cURL ke momoai.fun, dan auto-cleanup).*
> 
> *Lalu sesuaikan **STAGE 1 (Creative Specifications)** untuk konsep baru ini:*
> - *Topik / Niche: [Tentukan topik, misal: Cyberpunk Drone Shots / Lo-Fi Coffee / Horror Ambience]*
> - *Jadwal Cron: [Misal: 0 8,20 * * *]*
> - *Target FB Page: [Misal: target=page2 atau default]*
> - *Style & Camera: [Deskripsikan visual dan pergerakan kamera]*
> - *Audio BGM: [Path ke mp3 atau ambient]*"

---

## ⚙️ Multi-Target Routing (VPS Bridge)

The VPS bridge (`fbbridge`) supports routing uploads to multiple distinct Facebook Pages or targets.

In `/home/ubuntu/fbbridge/fb_config.json`:
```json
{
  "default_target": "wintermute",
  "accounts": {
    "wintermute": {
      "page_id": "1423146027540788",
      "page_name": "Wintermute Cozy Style",
      "page_token": "EAA..."
    },
    "page2": {
      "page_id": "987654321098765",
      "page_name": "Second Page Name",
      "page_token": "EAA..."
    }
  },
  "telegram_bot_token": "8871940520:***",
  "telegram_chat_id": "-1003818905785",
  "telegram_thread_id": 8580
}
```

In your cron task cURL call:
```bash
curl -s --max-time 180 -X POST https://momoai.fun/upload \
  -F "video=@$FINAL_MP4" \
  -F "caption=$CAPTION" \
  -F "target=page2"
```

If `target` is omitted, the bridge automatically falls back to `default_target`.
