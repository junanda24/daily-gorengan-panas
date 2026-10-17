# Proposal — Gorengan Panas!

**Tanggal:** 2026-10-17
**Jenis output:** Game (HTML5 canvas, web artifact static)
**Backend:** Tidak — 100% client-side cukup (high score di `localStorage`)

## Apa ini

Game arcade pixel-art: kamu jadi **tukang gorengan kaki lima**. Minyak panas,
pelanggan lapar, kompor bisa diatur. Empat slot di wajan, tiap gorengan punya
meter kematangan — ketuk tepat saat meternya di **zona emas** untuk menyajikan.
Kecepatan = risiko: api besar bikin cepat matang tapi zona emas menyempit.
Tiga kali gosong → "WARUNG TUTUP".

## Kenapa ide ini

1. **Efek kejut:** jam 7 pagi Junan buka link dan ternyata dia disuruh jadi
   tukang gorengan. Bukan kalkulator, bukan to-do list — profesi paling
   Indonesia yang belum pernah dijadikan game timing arcade. "Kok kepikiran sih"
   dijamin keluar.
2. **Bukan klon:** riset 17 Okt 2026 — game masak Indonesia yang ada (KuloNiku:
   Bowl Up! di Steam, Tahu Bulat, Nasi Goreng Frenzy) semuanya simulasi/tycoon.
   Tidak ada yang mengambil sudut **refleks + manajemen api** dalam format
   arcade 2–3 menit. Mekanik inti (zona emas menyusut vs kecepatan api) adalah
   risk/reward asli, bukan reskin.
3. **Nagih, bukan demo teknis:** loop inti 5 detik (taruh → tunggu → ketuk),
   combo multiplier, shift dengan unlock menu baru, high score lokal. Satu sesi
   2–4 menit, "sekali lagi" hampir otomatis.
4. **Rotasi bentuk:** kemarin (16 Okt) web app (Berapa Sih?). Hari ini game —
   sesuai rotasi sehat program.

## Kenapa game, kenapa tanpa backend

- Game = slot rotasi hari ini; format yang belum dipakai minggu ini.
- Backend tidak dibutuhkan: tidak ada login, tidak ada multiplayer, tidak ada
  data bersama. High score cukup di `localStorage`. Menambah server hanya
  menambah biaya tanpa menambah seru — melanggar prinsip "pakai backend kalau
  memang butuh".

## Tambahan wajib

- **Anti-AI-slop:** copy Bahasa Indonesia natural & jenaka ("GOSONG! Pelanggan
  kabur."), tanpa emoji di label UI (aturan Junan) — ikon digambar pixel.
- **IP check:** SEMUA aset visual original, digambar prosedural via kode canvas
  (tidak ada sprite/karakter berlisensi, tidak ada foto pihak ketiga). Tidak
  perlu disclaimer fan-made.
- **CODE_STANDARDS.md:** single quotes, semicolons, `const`, `===`, tanpa
  `console.log` di build, tanpa kode mati. Struktur mengikuti ARCHITECTURE.md
  bagian Game (satu game loop, state machine eksplisit, object pooling,
  keyboard + touch).
- **Aksesibilitas:** tombol punya label, kontras cukup, hormat
  `prefers-reduced-motion`, bisa dimainkan penuh via keyboard.

## Keputusan sadar (yang TIDAK dibuat)

- Tanpa mode cerita / visual novel — fokus arcade murni, selesai dalam 1 sesi.
- Tanpa pilihan tipe gorengan manual per slot — tipe otomatis mengikuti pesanan
  pelanggan terdepan (mengurangi tap, menjaga tempo).
- Tanpa leaderboard online — butuh backend & login; high score lokal cukup.

## Persetujuan

Junan memberi otoritas penuh lewat scheduler (tanpa gate approval). Proposal ini
lolos standar TERCENGANG — disetujui sendiri, lanjut ke Apply.
