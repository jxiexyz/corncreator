# CornCreator 🌽

Automated cloud workflow and scheduled synchronization templates for creative AI video production.

---

## 🏛️ Architecture: Personal Cloud Storage Sync

To keep cloud workspace storage within strict disk quotas and avoid disk saturation, media generation and asset archiving are cleanly separated:

```
┌──────────────────────────────────────────────┐
│           CREATIVE WORKSPACE AGENT           │
│  1. Scheduled Trigger + Organic Jitter       │
│  2. Render 2 continuous video clips (20s)    │
│  3. Quality Control (frame check t=1s & 9s)  │
│  4. FFmpeg Audio Mix & stream verification   │
│  5. Sync final archive to private server     │
│  6. Clean local final MP4 on status 200      │
└──────────────────────┬───────────────────────┘
                       │ HTTPS POST (Multipart)
                       ▼
┌──────────────────────────────────────────────┐
│         PRIVATE ARCHIVE SERVER (VPS)         │
│  https://momoai.fun/upload                   │
│  - Receives and stores final archived master │
│  - Logs asset metadata & notification        │
│  - Maintains workspace disk hygiene          │
└──────────────────────────────────────────────┘
```

### Why Archive Synchronization?
- **Disk Space Management:** Video rendering generates large intermediate files. Archiving the final master to a private self-hosted server (`momoai.fun`) allows local files to be safely purged.
- **Dedicated Private Endpoint:** `momoai.fun` is the user's personal self-hosted VPS server running a private archiving service.
- **Clean Environment:** The creative agent focuses entirely on visual composition, prompt styling, and rendering without managing external network dependencies.

---

## 📁 Repository Structure

| File | Purpose |
| :--- | :--- |
| **`template-cron-job.md`** | **Universal foundation template.** Standard task structure with jitter, QC, audio mixing, and remote archival sync. |
| **`anime-shorts-daily.md`** | **Production reference.** 3x daily vertical cozy anime short task. |

---

## 🚀 Creating a New Task

Clone or pull this repository inside your workspace:
```bash
git clone https://github.com/jxiexyz/corncreator.git ~/corncreator
```

Use `template-cron-job.md` as the blueprint, keeping STAGE 0 (Foundation) for storage sync and customizing STAGE 1 (Creative Specifications) for your specific project.
