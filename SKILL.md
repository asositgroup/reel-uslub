---
name: reel-uslub
description: Foydalanuvchi namuna (referens) video yoki Instagram havolasini va o'z videosini yuborib "shunday video qilib ber" desa — referensni /reel-montaj usulida hamma jihatdan tahlil qiladi (ref_analyze + video-use, tuzilma, montaj ritmi, matn/grafika, rang, kamera, ovoz), uslub pasportini tuzadi va foydalanuvchi videosini AYNAN shu uslubda reel-montaj quvuri bilan montaj qiladi, oxirida natijani referens bilan raqamda solishtiradi. "shunday video qilib ber", "shu videodek montaj qil", "mana bu uslubda", "xuddi shunaqa qil", "bu reelga o'xshatib", "shu stilda", "Imona Ahmedova uslubida qil" so'rovlarida ishlat.
---

# Reel uslub nusxasi (referens → foydalanuvchi videosi)

Ish tili: **o'zbekcha**. Bu skill `reel-montaj` ustiga qurilgan: avval `~/.claude/skills/reel-montaj/SKILL.md` ni o'qi —
uning **9 ta qat'iy qoidasi va ish tartibi to'liq amal qiladi**. Bu yerda faqat qo'shimcha: referensni o'rganish → uslub pasporti →
xuddi shunday montaj → referens bilan solishtirish.

**Ustuvorlik:** foydalanuvchining qoidalari > referens. Referensda bo'lsa ham olinmaydi: overshoot/elastic/tepki (→ smooth-motion.md
ekvivalenti), ovozni to'sadigan musiqa (qoida 9), film/TV parchalari va boshqa mualliflik huquqi bor material (→ Pexels),
qoida 4 ga zid B-roll odamlari, ElevenLabs/kie.ai. Bunday har bir moslashni pasportda va rejada OCHIQ yoz.

## Kirish ma'lumotlari
- Referens: Instagram/TikTok/YouTube havolasi yoki fayl (1–3 ta bo'lishi mumkin — bir xil muallif bo'lsa umumiy uslub).
- Foydalanuvchi videosi: fayl(lar) (`1_manba/` ga ko'chiriladi). Yo'q bo'lsa — so'ra.
- Noaniq bo'lsa so'ra: referensning QAYSI jihati kerak (hammasi / faqat matn uslubi / faqat montaj ritmi / faqat rang)?
  Standart — hammasi.

## Kutubxonadagi uslub nomi bilan ("Imona Ahmedova uslubida qil")
Referens video berilmasa, lekin o'rganilgan muallif/uslub nomi aytilsa — 1-qadamni o'tkazib yubor va o'sha muallifning
**HAMMA o'rganilgan videolarini birga** referens qil (bittasini emas):
- `INDEX.md` dan oilani top. Oila pasporti bo'lsa (`oila-*/PASSPORT.md`) — u asosiy manba: elementlar qaysi videodan, qaysi qolip
  (masalan monolog / split-podkast) foydalanuvchi videosiga qarab qanday tanlanishi, raqamli maqsadlar (median + oraliq), OLINMAYDI ro'yxati.
- **Imona Ahmedova** → `design-library/oila-O-iman/PASSPORT.md` (4 video: R1 split-podkast + R2–R4 monolog; tafsilot uslub-31/32).
- Rang: `grade_match.py` ga hamma referens videolar vergul bilan (pasport 4-bo'lim buyrug'i).
- Tekshiruv: `style_compare.py` ga hamma referens `metrics.json` lari (pasport 5-bo'lim) — median va oraliq bilan solishtiradi.
- Uslub pasporti (2-qadam) jadvalida har element yonida manba videoni yoz (R1…R4).
Qolgan qadamlar (foydalanuvchi videosi → reja → tasdiq → montaj → style_compare) bir xil.

## Ish tartibi

### 1. Referens tahlili (har referens uchun)
```bash
cd ~/Desktop/AI-Videos/AbuDev/design-library/_src          # Windows: ~/OneDrive/Ishchi stol/AbuDev/design-library/_src
yt-dlp -o "<id>.%(ext)s" --write-info-json "<havola, ?stkn/utm parametrlarisiz>"   # fayl berilgan bo'lsa nusxa ol
~/Desktop/AI-Videos/.venv/bin/python ~/.claude/skills/reel-montaj/scripts/ref_analyze.py <id>.mp4
```
- Avval `_src/<id>_an/` bor-yo'qligini va `INDEX.md` da shu muallif/uslub borligini tekshir — bo'lsa qayta tahlil qilma, `uslub-NN/STYLE.md` dan foydalan.
- `report.md` + `metrics.json` + `tv_*.png` (video-use) + `contact_*.jpg` ni KO'R; har grafik element kirishi uchun 18 kadrlik strip
  (`ffmpeg -ss T -t 0.62 -i v.mp4 -vf scale=216:-1,tile=9x2:padding=2 -frames:v 1 s.jpg`) — kirish/chiqish vaqti, ease, blur.
- Yangi uslub bo'lsa: `uslub-NN/STYLE.md` (reel-montaj 1b: 15 band + 13-bo'lim raqamlari), `INDEX.md`, reel-montaj jadvaliga qator.
  Raqamlash: oxirgi raqamdan keyingisi (xotira: design-library-mac).

### 2. Uslub pasporti (loyihada `analysis/style_passport.md`)
Referensdan ANIQ qiymatlar bilan (taxmin emas — report/metrics/strip'dan):
| Jihat | Referens | Bizda (moslash) |
|---|---|---|
| Tuzilma (beat'lar) | HUK → … → CTA → yakun, vaqtlar | foydalanuvchi transkriptiga qanday tushadi |
| Temp | so'z/daq, pauzalar olinganmi | pauza kesish kerakmi (foydalanuvchidan so'ra — sinxron!) |
| Montaj ritmi | kesim har N s, kadr median, gap o'rtasida kesim ulushi | virtual kameralar (1.0/1.3/1.75) yoki ko'p kamera, kesim joylari |
| Matn/subtitr | shrift, o'lcham, rang (hex), bo'laklash, joylashuv, urg'u usuli | shrift nomzodlari mock bilan, yuzga tushmaydi |
| Grafik elementlar | har biri: qachon, qanday kiradi/chiqadi | smooth-motion qiymatlari bilan |
| O'tishlar | turi, davomiyligi, musiqa zarbasiga bog'liqmi | |
| Rang | luma/kontrast/to'yinganlik/iliqlik | `grade_match.py` filtri |
| Kamera | statik / push / qo'l | `#cam` push, kesimda sakramasin |
| B-roll | turi, ulushi, manbasi | Pexels so'rovlari, qoida 4 |
| Audio | musiqa turi, nutq−pauza dB, SFX/daq, nimaga bog'langan | qoida 9 bo'yicha daraja, SFX lug'atidan |
| CTA / yakun | | foydalanuvchining haqiqiy handle/avatar (so'ra) |
| OLINMAYDI | (ro'yxat) | sababi bilan |

### 3. Foydalanuvchi videosi (reel-montaj 0–1)
`new_project.py <slug> "<Mavzu>"` → manba `1_manba/` ga → ffprobe, kontakt varaq, transkript (`tr.py`), matnni adabiy o'zbekchaga tikla.
Rang: `python ~/.claude/skills/reel-uslub/scripts/grade_match.py <referens.mp4> <manba.mp4> analysis --ref-times <spiker kadrlari>`
→ `analysis/grade_match.jpg` ni KO'R (teri tabiiymi) → filtrni `edit.json` `"grade"` ga. Referens oq-qora kadrlar ishlatsa — o'sha
vazifadagi kadrlarga (huk/salbiy urg'u) `hue=s=0` alohida.

### 4. Reja → TASDIQ (qoida 1)
Foydalanuvchiga: transkript + uslub pasporti qisqa ko'rinishi + beat-ma-beat jadval:
| Vaqt (bizda) | Gap | Referensdagi mos element (vaqti) | Bizdagi element | Uslub # |
Yonma-yon ko'rsatish uchun referensdan 3–4 kalit kadr (`contact`/strip) bilan. ❓ ochiq savollar: shrift, handle, musiqa varianti,
pauza kesish. **"boshla" demaguncha qurma.**

### 5. Montaj (reel-montaj 3–5)
Andoza tanlash: referens oilasiga eng yaqin (`templates/*storis_smooth*` A, `*ucanvisa_v2/v3*` B–E) — keyin pasport bo'yicha qayta yoz.
Referens elementlari aniq qiymatlari STYLE.md 12-bo'limidan (HyperFrames retsepti). `smooth_check.py hf/index.html` = 0.

### 6. Render + uslub tekshiruvi
reel-montaj 6 (render, loudnorm, av_check, smooth_check, video-use self-eval) va QO'SHIMCHA:
```bash
python ~/.claude/skills/reel-montaj/scripts/ref_analyze.py hf/renders/<nom>_final.mp4 analysis/out_an
python ~/.claude/skills/reel-uslub/scripts/style_compare.py <ref_an>/metrics.json analysis/out_an/metrics.json --md analysis/style_compare.md
```
`FARQ` qatorlarini tuzat (yoki pasportdagi ataylab farq bo'lsa izohla). `QOIDA` — tuzatilmaydi. Ko'pi bilan 3 aylanish.
Yonma-yon varaq: referens va bizning video bir xil beat'lardan 4 kadr (`hstack`) → foydalanuvchiga ko'rsatish uchun.

### 7. Topshirish
reel-montaj 6 dagi kabi (`3_tayyor/`, telefon nusxasi). Hisobotga qo'sh: qaysi referens, uslub #, `style_compare.md` jadvali,
OLINMAGAN narsalar va sababi. Xotiraga (MEMORY.md) qisqa yozuv.

## Skriptlar
- `scripts/grade_match.py` — referens rangiga moslash filtri (qora/oq nuqta, to'yinganlik to'liq; ekspozitsiya/iliqlik `--strength 0.4`),
  natija varag'i bilan. Sahna mazmuni boshqa bo'lgani uchun to'liq ko'chirish terini buzadi — strength'ni ko'z bilan tanla.
- `scripts/style_compare.py` — ikki `metrics.json` (ref_analyze) ni solishtirish: OK / YAQIN / FARQ / QOIDA.
- reel-montaj: `ref_analyze.py` (hamma jihat tahlili + `metrics.json`), qolgan hamma montaj skriptlari.
