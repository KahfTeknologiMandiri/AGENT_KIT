# Tanya dulu, kerja belakangan

## Wajib sebelum eksekusi

**Fitur baru / bug / kerja multi-langkah:** jangan pakai putaran 3–7 A/B/C di file ini. Ikuti `superpowers.md` (plugin + tujuh skill). File ini bukan gerbang fitur.

**Non-fitur** (config, rename, setup) yang mengubah kode, DB, config, atau alur kerja:

1. JANGAN langsung edit / jalankan perintah besar.
2. Ajukan 3–7 pertanyaan klarifikasi dulu. UI setelah spec Superpowers: putaran desain = **approve frame Figma** (`figma.md`), bukan pilih tema Hallmark.
3. Tunggu jawaban user (atau `defaults` / `lanjut dengan default`).
4. Ringkas ulang pemahaman dalam 2–3 kalimat bahasa awam, baru kerja.

## Bahasa

- Tulis seperti ke orang non-teknis.
- Hindari jargon; kalau perlu istilah teknis, beri analogi singkat.
- Setiap pertanyaan: beri contoh jawaban konkret.

## Format setiap putaran tanya

1. Ringkas singkat apa yang dipahami dari permintaan.
2. Pertanyaan bernomor + pilihan A/B/C (tandai **rekomendasi**).
3. Saran: apa yang sebaiknya dilakukan / dihindari, dan kenapa (1 kalimat).
4. Cara jawab cepat: balas `defaults` atau `1b 2a 3c`.

Putaran desain jalan **setelah** spec Superpowers ada (atau `defaults` pada spec). Itu = **approve frame Figma** (`figma.md`), bukan putaran vibe / tema catalog Hallmark. Brand / tone / referensi tinggal di spec sebagai brief generate Figma.

## Putaran desain (wajib bila ada UI)

Kalau pekerjaan menyentuh halaman, landing, shell, atau komponen visual di **app**:

1. Ikuti `figma.md`: MCP Figma; generate frame jika belum ada; **tunggu approve** URL + node.
2. `defaults` pada spec **tidak** melewati Figma — tetap generate + tunggu approve frame.
3. Setelah frame di-approve: baca Hallmark (`hallmark.md` + skill) untuk slop / a11y / responsive **tanpa** ganti makrostruktur.
4. Responsive: desktop + mobile wajib untuk permukaan yang di-ship. `hallmark audit` sebelum selesai.

Jangan anggap “shell minimal / placeholder AI” selesai kalau permukaan itu user-facing.

## Build mode (setelah spec dan plan Superpowers disetujui)

Jika user sudah **approve spec dan plan Superpowers** yang relevan (atau bilang lanjut/approve pada dokumen itu):

1. **Jangan** membuka putaran ask-first baru untuk setiap modul/fitur P0.
2. Kerjakan sesuai urutan plan / DoD sampai selesai atau sampai blocker nyata (`executing-plans` + TDD). Setiap permukaan UI **baru** tetap butuh node Figma yang sudah di-approve (`figma.md`); jangan skip Figma karena build mode.
3. Tanya lagi **hanya** jika: keamanan/secret, perubahan destruktif, domain bisnis belum dipilih padahal mengubah schema besar, dokumen bertentangan, atau perbaikan Hallmark akan ganti makrostruktur Figma.

Analogi: setelah denah rumah disetujui, tukang tidak tanya ulang tiap pasang bata — hanya tanya jika tembok nabrak pipa.

Keluar build mode: user minta ubah arah besar, atau DoD tercapai.

## Pengecualian (boleh langsung)

- Pertanyaan informasi saja
- User bilang: "langsung kerjakan", "jangan tanya", "pakai default" / `defaults` — **kecuali** UI visual: `figma.md` tetap gerbang (generate + approve)
- Ejaan / 1 baris non-visual pada string yang sudah ada
- **Build mode** aktif (lihat atas) — lanjut eksekusi tanpa tanya rutin

## Setup pertama / multi-provider

Untuk pasang policy, adapter Cursor/Claude/OpenCode/Codex, atau stack cursorrules: ikuti juga `first-setup.md` (konfirmasi perlu/tidak + provider mana) sebelum menyentuh file.
