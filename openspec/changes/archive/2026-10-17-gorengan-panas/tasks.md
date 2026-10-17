# Tasks — Gorengan Panas!

## Propose (selesai)
- [x] proposal.md — disetujui sendiri (otoritas scheduler)
- [x] specs/gorengan-panas/spec.md — 20 SHALL
- [x] design.md — arsitektur single-file + state buckets + audio + pixel art
- [x] tasks.md — file ini

## Apply — build artifact `gorengan-panas`
- [ ] Buat web artifact static `gorengan-panas` (HTML5 canvas, satu file)
- [ ] Seksi 1–2: CONFIG/DATA + AUDIO (synth WebAudio, mute)
- [ ] Seksi 3–4: STATE (state machine) + ENTITIES (gorengan, pelanggan, pool partikel)
- [ ] Seksi 5–6: INPUT (tap + keyboard) + UPDATE (delta-time loop)
- [ ] Seksi 7: RENDER pixel-art (warung malam, wajan 4 slot, pelanggan, api)
- [ ] Seksi 8: overlay DOM (menu, pause, game over, banner shift) — copy ID natural
- [ ] Seksi 9: BOOT (resize/DPR, reduced-motion, localStorage)
- [ ] Juice: floaters, shake, asap, gelembung, sizzle gain

## Verify (mandiri)
- [ ] Trace SHALL-01…SHALL-20 → kode
- [ ] Export HTML → review CODE_STANDARDS (quotes/semicolon/===/tanpa console.log/dead code)
- [ ] Harness Node (stub canvas/audio/storage): simulasi penuh tanpa exception
- [ ] Copy check: tanpa lorem ipsum/placeholder/emoji-label/teks Inggris di UI
- [ ] Perbaiki temuan via artifact.edit sampai lolos

## Archive + ship
- [ ] Archive openspec → openspec/changes/archive/2026-10-17-gorengan-panas/
- [ ] Export artifact → salin ke src/ (source lengkap, bukan cuma spec!)
- [ ] README.md lengkap → git init + commit
- [ ] Buat repo junanda24/daily-gorengan-panas → push via push_repo.py
- [ ] Verifikasi push via API (file source ADA di listing)
- [ ] Catat di BUILT.md + log ~/memory/2026-10-17.md + tracking entry
