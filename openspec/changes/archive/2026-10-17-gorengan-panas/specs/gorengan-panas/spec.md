# Spec — Gorengan Panas!

## 1. Layar & alur

- **SHALL-01 — Layar menu.** WHEN halaman dibuka THEN tampil layar menu berisi:
  judul "GORENGAN PANAS!", subjudul "Jangan sampai gosong.", high score
  (atau "Belum ada rekor"), cara main 3 baris, tombol "MULAI JUALAN".
  WHEN tombol diketuk THEN game masuk fase PLAYING.
- **SHALL-02 — Layar game over.** WHEN bintang habis THEN tampil overlay
  "WARUNG TUTUP" + total koin + shift tercapai + high score (dengan penanda
  "REKOR BARU!" bila pecah) + tombol "JUALAN LAGI". WHEN tombol diketuk THEN
  state reset penuh dan fase PLAYING.
- **SHALL-03 — Pause.** WHEN tombol pause / tombol `P` ditekan THEN update
  berhenti dan overlay "ISTIRAHAT" tampil dengan tombol "LANJUTKAN".
  WHEN dilanjutkan THEN game berjalan dari titik yang sama (tidak ada lompatan
  waktu).

## 2. Wajan & memasak

- **SHALL-04 — Empat slot.** Selalu ada tepat 4 slot wajan. WHEN slot kosong
  diketuk THEN gorengan mentah (progress 0) masuk ke slot itu. Tipe gorengan
  = pesanan pelanggan terdepan yang tipenya belum ada di wajan; bila tidak ada,
  tipe acak dari daftar unlock.
- **SHALL-05 — Api kompor.** Ada 3 level: Kecil, Sedang, Besar. WHEN level
  diubah THEN berlaku untuk semua slot:
  | Api    | 0→100   | Zona emas | Lebar zona |
  |--------|---------|-----------|------------|
  | Kecil  | 16 dtk  | 55–90     | 5,6 dtk    |
  | Sedang | 10 dtk  | 60–88     | 2,8 dtk    |
  | Besar  | 6,5 dtk | 66–88     | ±1,4 dtk   |
- **SHALL-06 — Sajian pas.** WHEN gorengan diketuk saat progress di zona emas
  THEN gorengan terangkat: layani pelanggan terdepan yang memesan tipe itu
  (koin dasar + tip 50%); bila tidak ada yang memesan → "jualan lepas"
  (koin dasar, tanpa tip). Combo +1. WHEN progress di tengah zona (±4)
  THEN bonus "SEMPURNA!" ×1,5.
- **SHALL-07 — Mentah.** WHEN diketuk saat progress < batas bawah zona THEN
  gorengan dibuang, teks "MENTAH!" melayang, combo reset ke 0. Bintang TIDAK
  berkurang.
- **SHALL-08 — Gosong.** WHEN diketuk saat progress > batas atas zona ATAU
  progress mencapai 100 tanpa diketuk THEN gosong: bintang −1, teks "GOSONG!",
  semburan asap, screen shake, combo reset. Slot jadi kosong.
- **SHALL-09 — Tiga bintang.** Game mulai dengan 3 bintang. WHEN bintang = 0
  THEN fase GAME_OVER (SHALL-02).

## 3. Pelanggan & shift

- **SHALL-10 — Antrean.** Maks 4 pelanggan terlihat. Tiap pelanggan: tipe
  pesanan (dari tipe yang sudah unlock) + bar kesabaran (22 dtk, −1,5 dtk per
  shift, min 12 dtk). Spawn tiap 4–7 dtk (makin cepat per shift).
  WHEN kesabaran habis THEN pelanggan pergi marah, combo reset, bintang tetap.
- **SHALL-11 — Shift.** Tiap shift 45 dtk. WHEN shift berakhir THEN banner
  4 dtk ("SHIFT 2 — Bakwan dibuka!"), unlock tipe baru, spawn lebih cepat.
  Urutan unlock: S1 tahu+tempe, S2 +bakwan, S3 +pisang, S4 +cireng, S5+ hanya
  percepatan. Game endless sampai bintang habis.

## 4. Skor

- **SHALL-12 — Koin.** Harga dasar: tahu Rp2.000, tempe Rp2.000, bakwan
  Rp2.500, cireng Rp1.500, pisang Rp3.000. Tip pesanan +50%. Combo menambah
  +10% per level (cap +100%). Tampilan format `Rp 24.500`.
- **SHALL-13 — High score.** WHEN game over dan skor > high score tersimpan
  (`gorenganPanas.high.v1`) THEN simpan + tampilkan "REKOR BARU!".

## 5. Kontrol & input

- **SHALL-14 — Sentuh & keyboard.** Semua aksi bisa via tap/klik. Keyboard:
  `1`–`4` = ketuk slot 1–4, `K` = putar level api, `Spasi` = mulai/restart,
  `P` = pause. Target sentuh ≥ 44px.

## 6. Rasa (game feel) & audio

- **SHALL-15 — Juice.** Sajian emas → teks melayang ("MANTAP!"/"SEMPURNA!",
  "+Rp2.000") + bunyi ding; gosong → asap + shake + buzz; minyak → gelembung
  partikel + suara sizzle (gain mengikuti jumlah gorengan di wajan).
- **SHALL-16 — Audio prosedural.** Semua suara via WebAudio (tanpa file):
  sizzle (noise), ding, buzz, kring koin. WHEN interaksi pertama THEN
  AudioContext resume. Ada tombol mute (`gorenganPanas.mute.v1`).
- **SHALL-17 — Reduced motion.** WHEN `prefers-reduced-motion` THEN matikan
  screen shake dan kurangi partikel ≥50%.

## 7. Kualitas

- **SHALL-18 — Aset original.** Seluruh visual pixel-art digambar prosedural
  via kode; tidak ada sprite/foto/karakter berlisensi.
- **SHALL-19 — Tanpa cacat.** Tidak ada lorem ipsum, placeholder, tombol mati,
  atau teks Inggris di UI. Tidak ada error console pada alur
  menu → main 60 dtk → pause → resume → game over → restart.
- **SHALL-20 — Mobile.** Layout portrait nyaman di 360px; tidak ada scroll
  halaman saat main; canvas menyesuaikan DPR (tajam di layar retina).
