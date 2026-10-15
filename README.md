# CornCreator

Daily workflow and scheduling templates for cozy 9:16 vertical anime short animations.

## Overview

This repository contains the standardized pipeline definition for generating high-quality, aesthetic 20-second vertical animations. It manages:
- Two-clip continuous scene composition with locked perspective.
- Visual consistency checks (frame sampling at 1s and 9s).
- Background ambient audio integration via ffmpeg.
- Automated remote archive synchronization.

---

## Files

- `anime-shorts-daily.md`: Daily scheduled task specification with prompt guidelines and styling references.

---

## Getting Started

To add this task to your workspace:

```bash
git clone https://github.com/jxiexyz/corncreator.git ~/corncreator
```

Copy the task definition into your scheduled tasks folder:
```bash
cp ~/corncreator/anime-shorts-daily.md /home/hatch/workspace/cron/anime-shorts-daily.md
```
