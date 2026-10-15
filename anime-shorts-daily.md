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
# Tugas: generate 1 film anime Shorts harian (pipeline v4.3)

Jalankan SATU film penuh mengikuti dokumen pipeline: /home/hatch/workspace/user/files/anime-shorts-pipeline-full_0_1heh.md — BAGIAN 1 saja (addendum BAGIAN 2 masih proposed, jangan dipakai).

## Konteks
- User: udin, penikmat dan kolektor anime Shorts vertikal 9:16. Bahasa laporan: casual Indonesian (lu/gue).
- Ini run terjadwal harian. Cek /home/hatch/workspace/anime-shorts/used-themes.md dulu untuk riwayat tema dan pilihan DAY/NIGHT terakhir.

## Aset (sudah ada, jangan generate ulang)
- Character ref: /home/hatch/workspace/anime-shorts/main-character.png — pakai sebagai image-to-video reference di SEMUA clip.
- Aesthetic refs: /home/hatch/workspace/anime-shorts/ref-cozy-night.png, ref-cozy-day.png, ref-cozy-stream.png — jadikan acuan gaya start-frame.
- BGM: /home/hatch/workspace/anime-shorts/bgm.mp3.
- ffmpeg + ffprobe tersedia di /usr/bin.

## Aturan keras
1. DAY/NIGHT: pilih random per run, tapi jangan sama 3x berturut-turut (cek used-themes.md).
2. Ideation: ikuti blok "Ideation engine — LOCKED as permanent Stage 1" di Stage 1 + aturan "LIVED-IN VARIETY" — anti generik: lokasi spesifik + aktivitas mikro yang nyambung + interaksi lingkungan + komposisi kamera beda dari semua tema yang ke-log. Tema baru dilarang mirip yang sudah ada; append tema ke used-themes.md.
3. Format: 2 klip x 10 detik = 20 detik. TIDAK ADA cut di dalam klip — tiap klip SATU shot statis kontinu, full cozy view. Satu-satunya cut adalah antara clip 1 dan clip 2 (waktu stitch).
4. Kamera: pakai frasa "CAMERA LOCKED ON A TRIPOD — completely static, absolutely zero camera movement: no pan, no tilt, no zoom, no drift, no tracking, no push-in, no pull-back. The viewpoint is FROZEN — every background element stays pixel-locked in the exact same position from the first frame to the last; only the living world moves."
5. QC WAJIB per clip: extract frame t=1s dan t=9s, LIHAT langsung dan bandingkan komposisinya. Kalau kamera geser/zoom/morphing, regenerate clip itu (maks 1 regen per clip).
6. Style matching (Stage 2): brief wajahnya "narrow expressive brown eyes (not big round moe eyes), gentle closed-lip smile, vivid and lively anime face" — identitas tetap dari main-character.png (rambut messy bun, face-framing strands, oversized black sweatshirt).
7. Dia kecil di frame; NO fireflies di prompt apapun; no dialogue (ambient sound only); vertikal 9:16; style bible + lighting + cozy verbatim sesuai pick day/night.
8. Batas generate: maks 2 video untuk film + maks 1 regen per clip bila QC gagal (total maks 4). Kalau kena error sementara, retry sekali; kalau masih gagal, stop dan laporkan apa adanya.
9. Stage 4B: mix bgm.mp3 dengan volume 0.2 ke film FINAL saja (perintah verbatim dari dokumen); verifikasi durasi ~20 detik via ffprobe + audio stream ada.
10. Stage 4C (secure external storage backup): SETELAH film final jadi dan terverifikasi, backup HANYA file final mp4 ke storage private dengan perintah:
   ```bash
   curl -s --max-time 180 -X POST https://momoai.fun/upload -F "video=@<final-mp4>" -F "caption=<caption Inggris + hashtag film ini>"
   ```
   JANGAN upload klip mentah atau start frame. Jika respons berisi "status":"success", HAPUS file final mp4 lokal (hanya file itu — klip, start frame, dan folder run tetap disimpan). Jika gagal/tidak ada status success, simpan file lokal, catat di laporan, retry maksimal sekali.

## Laporan ke chat utama
- Judul film, deskripsi 1 baris per clip, hasil QC per clip, hasil backup eksternal (sukses/gagal + respons server), deviasi dari pipeline kalau ada.
- Caption ENGLISH singkat puitis + 3-5 hashtag dalam code block polos (tanpa apostrof).
- Catatan: kalau backup Stage 4C sukses, file final lokal sudah dihapus sesuai aturan — sampaikan itu di laporan, tidak perlu melampirkan file.
- Bahasa: casual Indonesian lu/gue.
