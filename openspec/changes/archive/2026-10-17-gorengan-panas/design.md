# Design — Gorengan Panas!

## Stack

Satu file HTML statis (HTML + CSS + JS inline, tanpa framework, tanpa build
step) — sesuai ARCHITECTURE.md §5 Game (HTML5 canvas) dan pola "single-file
section discipline". Font pixel "Press Start 2P" via Google Fonts CDN
(fallback monospace). 100% client-side.

## Arsitektur kode (seksi berurutan dalam satu file)

```
1. CONFIG & DATA   — konstanta api, tabel gorengan, unlock shift, copy teks
2. AUDIO           — synth WebAudio (sizzle/ding/buzz/koin), mute
3. STATE           — satu objek state + state machine MENU/PLAYING/PAUSED/GAME_OVER
4. ENTITIES        — Gorengan, Pelanggan, Partikel (object pool)
5. INPUT           — pointer (tap/klik) + keyboard, hit-test slot & tombol
6. UPDATE          — delta-time (dt detik, clamp 0,05), satu rAF loop
7. RENDER          — pixel helper (px rect), scene warung, wajan, HUD di canvas
8. UI OVERLAY (DOM) — menu / pause / game over / banner shift (font pixel)
9. BOOT            — resize/DPR, reduced-motion, localStorage load
```

Aturan: entity punya `update(dt)`/`render(ctx)`; DOM overlay tidak tahu logika
game (hanya baca state & kirim aksi); tidak ada variabel global berserakan
selain `state` & `S` (services: audio, storage).

## State buckets (lima bucket ARCHITECTURE.md)

1. **Server state** — tidak ada (client-only, sesuai proposal).
2. **Global client state** — `state = { phase, coins, combo, stars, heat,
   shift, shiftT, time }`.
3. **Local/entity state** — `fryers[4]` (slot: null | {type, progress}),
   `customers[]`, `particles` (pool 120), `floaters[]` (teks melayang).
4. **Input state** — transient (pointer pos, key map); tidak disimpan.
5. **Persistent** — `localStorage`: `gorenganPanas.high.v1`,
   `gorenganPanas.mute.v1`.

## Layout (portrait, logical 420×760, scale-to-fit)

Dari atas ke bawah: (1) langit malam + lampu gantung + papan "GORENGAN PANAS";
(2) antrean pelanggan (maks 4, pixel people + bubble pesanan + bar sabar);
(3) wajan besar: 4 slot minyak bergelembung + meter kematangan & zona emas
per slot; (4) kompor: 3 tombol api (Kecil/Sedang/Besar) + indikator; (5) HUD:
koin, combo, bintang, shift & timer. Di desktop canvas max-width 480px,
tengah, background vignette.

## Pixel art prosedural

Helper `px(x, y, w, h, color)` menggambar kotak "pixel" (skala PIX=3).
Sprite didefinisikan sebagai array string per tipe:
- tahu: kubus kuning keemasan; tempe: balok coklat; bakwan: blob oranye;
  pisang: bentuk melengkung kuning; cireng: lingkaran putih.
- pelanggan: badan + kepala + variasi warna baju/rambut/kulit (seeded).
- wajan: elips hitam + minyak oranye + gelembung (partikel naik).
- api kompor: segitiga pixel biru/oranye beranimasi (3 frame).
Warna gorengan bergradasi mentah (pucat) → emas → gosong (hitam) mengikuti
progress — feedback visual tanpa membaca meter.

## Audio (WebAudio, tanpa aset)

- `sizzle`: buffer noise putih → lowpass → gain mengikuti jumlah gorengan
  di wajan (0 bila kosong/pause/mute).
- `ding`: dua sine (880→1320 Hz) 0,15 dtk saat sajian emas.
- `perfect`: arpeggio 3 nada saat SEMPURNA.
- `buzz`: sawtooth 110 Hz 0,4 dtk saat gosong.
- `coin`: square 1568 Hz pendek + 2093 Hz (efek "kring").
- `pop`: saat menaruh gorengan; `leave`: nada turun saat pelanggan kabur.
AudioContext dibuat saat interaksi pertama (kebijakan autoplay).

## Game feel

- Teks melayang (pool): "MANTAP! +Rp3.000", "SEMPURNA! ×1,5", "MENTAH!",
  "GOSONG! −1 ★" (bintang digambar pixel, bukan emoji).
- Screen shake 6px/0,3 dtk saat gosong (nonaktif bila reduced-motion).
- Partikel: gelembung minyak (loop), asap abu-abu saat gosong, percikan
  emas saat sempurna, hati/koin kecil saat tip.
- Kombo ≥5: meter kombo menyala + label "KOMBO ×N".

## Edge cases

- Tap saat PAUSED/GAME_OVER diabaikan (kecuali tombol overlay).
- Semua slot penuh + pelanggan baru: spawn tetap (pelanggan menunggu).
- Pelanggan memesan tipe yang belum pernah ditaruh: logika auto-taruh
  (SHALL-04) menutup celah ini.
- Tab hidden → auto-pause (visibilitychange).
- Resize/orientasi: canvas re-fit, hit-test pakai koordinat logical.

## Verifikasi (mandiri)

1. Trace tiap SHALL → kode (checklist di tasks.md).
2. Export HTML → review penuh: CODE_STANDARDS (quotes, semicolons, `===`,
   tanpa `console.log`, tanpa dead code).
3. Harness Node: stub `document`/canvas 2D/AudioContext/localStorage/rAF;
   simulasikan menu→mulai→taruh 4→maju waktu→ketuk emas/mentah/gosong→
   3 gosong→game over→restart. Assert tanpa exception & transisi benar.
4. Cek visual: tidak ada teks Inggris di UI, target sentuh ≥44px,
   360px tanpa scroll.
