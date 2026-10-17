# Gorengan Panas!

**Game arcade pixel-art: kamu jadi tukang gorengan kaki lima.** Empat slot di
wajan, tiap gorengan punya meter kematangan — ketuk tepat saat meter masuk
**zona emas** untuk menyajikan ke pelanggan. Terlambat sedikit saja =
**GOSONG**, bintang berkurang. Tiga kali gosong → "WARUNG TUTUP".

> "Minyak udah panas, pelanggan udah lapar. Jangan sampai gosong."

## Kenapa ide ini dipilih

Jam 7 pagi, Junan buka link dan ternyata disuruh jadi tukang gorengan — profesi
paling Indonesia yang belum pernah dijadikan game timing arcade. Riset sebelum
build (17 Okt 2026): game masak Indonesia yang ada (KuloNiku: Bowl Up! di Steam,
Tahu Bulat, Nasi Goreng Frenzy) semuanya simulasi/tycoon. Tidak ada yang
mengambil sudut **refleks + manajemen api kompor** dalam format arcade 2–4
menit. Efek kejutnya: "kok kepikiran sih" (tukang gorengan sebagai protagonis
game) + "gila, ini nagih" (mekanik risk/reward api: makin besar api makin cepat
matang, tapi zona emas makin sempit).

Program "1 Hari 1 Project", edisi kedua — 2026-10-17. (Edisi pertama kemarin:
web app, jadi hari ini slotnya game sesuai rotasi.)

## Fitur

- **4 slot wajan** — ketuk slot kosong untuk menaruh gorengan; tipe otomatis
  mengikuti pesanan pelanggan terdepan (tanpa menu ribet, tempo tetap cepat)
- **Zona emas** — meter kematangan 0–100 per gorengan; sajikan di zona emas
  untuk dapat koin, tepat di tengah zona (±4) = "SEMPURNA!" bonus ×1,5
- **3 level api kompor** — Kecil (16 dtk, zona lebar) / Sedang (10 dtk) /
  Besar (6,5 dtk, zona sempit): risiko vs kecepatan beneran terasa
- **Mentah vs gosong** — ketuk kecepetan = "MENTAH!" (dibuang, combo reset);
  kelamaan = "GOSONG!" (bintang −1, asap + screen shake)
- **Antrean pelanggan pixel** — maks 4, bawa pesanan + bar kesabaran;
  layani biar dapat tip 50%, biarkan kabur = combo reset
- **Shift 45 detik, endless** — tiap shift unlock menu baru: tahu+tempe →
  bakwan → pisang → cireng; spawn makin cepat
- **Koin + kombo** — harga per tipe (Rp1.500–3.000), kombo +10%/level (cap
  +100%), format `Rp 24.500`, high score tersimpan di `localStorage`
  (`gorenganPanas.high.v1`)
- **Game feel** — teks melayang ("MANTAP!", "SEMPURNA!", "GOSONG!"),
  gelembung minyak, percikan emas, screen shake, kombo menyala di ×5+
- **Audio prosedural WebAudio** — sizzle (gain ikut jumlah gorengan di wajan),
  ding, arpeggio sempurna, buzz gosong, kring koin; tombol mute
  (`gorenganPanas.mute.v1`)
- **Kontrol sentuh + keyboard** — tap/klik; `1`–`4` slot, `K` putar api,
  `Spasi` mulai/restart, `P` jeda; auto-pause saat tab disembunyikan
- **100% client-side** — tanpa login, tanpa database, tanpa backend, tanpa
  fetch eksternal saat main (font diambil sekali dari Google Fonts)
- Mobile-friendly (portrait 420×760, canvas tajam via DPR), hormat
  `prefers-reduced-motion`

## Cara main

Buka link hasil → "MULAI JUALAN" → ketuk wajan kosong untuk menaruh gorengan →
ketuk lagi saat meter masuk zona emas (batang hijau) → kumpulkan koin, jaga
3 bintangmu. Mainkan di HP (tap) atau laptop (keyboard).

## Cara menjalankan dari source

Tidak perlu build tool — satu file HTML statis:

```bash
cd src
python3 -m http.server 8080
# buka http://localhost:8080/index.html
```

(atau buka `src/index.html` langsung di browser; audio butuh satu kali
interaksi klik/keyboard karena kebijakan autoplay browser.)

## Tech stack

- Satu file HTML statis (HTML + CSS + JS inline, tanpa framework)
- Canvas 2D — seluruh pixel-art digambar prosedural via kode (original,
  tanpa aset berlisensi)
- WebAudio API — semua efek suara disintesis, tanpa file audio
- Font "Press Start 2P" via Google Fonts (fallback monospace)
- Penyimpanan: `localStorage` untuk high score + preferensi mute

## Struktur

```
2026-10-17-gorengan-panas/
├── README.md                     # file ini
├── src/
│   └── index.html                # GAME LENGKAP (satu file, siap main)
└── openspec/changes/archive/2026-10-17-gorengan-panas/
    ├── proposal.md               # apa & kenapa
    ├── specs/gorengan-panas/spec.md  # 20 requirement SHALL
    ├── design.md                 # arsitektur single-file, audio, pixel-art
    └── tasks.md                  # checklist build + verifikasi
```

Catatan: gamenya sendiri di-host sebagai web artifact Muse (slug
`gorengan-panas`), bukan di repo ini — repo ini adalah source of truth untuk
source lengkap, spec, desain, dan dokumentasi.

## Link hasil

- Game: (link artifact publik — lihat pesan pengantar)
- Rilis: 2026-10-17

## Catatan/temuan saat build

- Verifikasi mandiri dua lapis: (1) builder memainkan gamenya langsung via
  sesi probe + screenshot — menu → taruh → mentah → gosong → sajian emas →
  kombo → ganti api → jeda → shift 2 (unlock bakwan terbukti) → mute →
  game over "REKOR BARU!" → restart, semua lolos tanpa error konsol;
  (2) harness Node dengan stub DOM/canvas/WebAudio mensimulasikan alur penuh
  (menu → main → pause/resume → 14 siklus gosong → game over → restart) —
  PASS, 0 exception.
- Selama banner shift (4 dtk) cooking ikut pause — tidak ada gosong curang.
- Yang belum terverifikasi: output audio aktual (lingkungan audit senyap;
  semua jalur WebAudio jalan tanpa error) dan presisi jendela SEMPURNA ±4
  (berbagi cabang kode dengan sajian normal).
- Copy Bahasa Indonesia natural; tanpa emoji di label UI (preferensi Junan).
- Yang disengaja TIDAK dibuat: pilihan tipe gorengan manual (memperlambat
  tempo), mode cerita (fokus arcade murni), leaderboard online (butuh
  backend/login — high score lokal cukup).
