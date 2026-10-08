# reel-uslub — Claude Code skill

Namuna (referens) reel + o'z videongiz → videongizni AYNAN referens uslubida montaj qilish.
Referens hamma jihatdan tahlil qilinadi (tuzilma, montaj ritmi, matn/grafika, rang, kamera, ovoz), uslub pasporti tuziladi,
montaj [reel-montaj](https://github.com/asositgroup/reel-montaj) quvuri bilan qilinadi va natija referens bilan raqamda solishtiriladi.
O'rganilgan muallif nomi bilan ham ishlaydi ("Imona Ahmedova uslubida qil") — o'sha muallifning hamma videolari birga referens bo'ladi.

## O'rnatish
**macOS / Linux:**
```bash
git clone https://github.com/asositgroup/reel-montaj.git ~/.claude/skills/reel-montaj   # majburiy bog'liqlik
git clone https://github.com/asositgroup/reel-uslub.git ~/.claude/skills/reel-uslub
```
**Windows (PowerShell):**
```powershell
git clone https://github.com/asositgroup/reel-montaj.git "$env:USERPROFILE\.claude\skills\reel-montaj"
git clone https://github.com/asositgroup/reel-uslub.git "$env:USERPROFILE\.claude\skills\reel-uslub"
```
Claude Code'ni qayta oching. Ishlatish: referens havolasi + videongizni yuborib "shunday video qilib ber" deng.

## Kerakli narsalar
- reel-montaj skilli va uning bog'liqliklari (Python 3.9+, ffmpeg, Node.js / HyperFrames, yt-dlp, faster-whisper)
- [video-use](https://github.com/browser-use/video-use) → `~/video-use` (referens tahlili uchun helpers; Scribe/ElevenLabs ishlatilmaydi)
- `AbuDev/design-library/` — o'rganilgan uslublar kutubxonasi (bu repoda yo'q)

## Skriptlar
- `scripts/grade_match.py <ref1.mp4[,ref2.mp4,...]> <manba.mp4> [out_dir]` — referens(lar) rangiga moslash filtri (edit.json "grade") + solishtirish varag'i.
- `scripts/style_compare.py <ref1/metrics.json> [ref2 ...] <out/metrics.json> [--md out.md]` — tayyor montajni referens(lar) median/oraliq bilan solishtirish.
