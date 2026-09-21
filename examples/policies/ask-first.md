# Tanya dulu, kerja belakangan

## Wajib sebelum eksekusi

**Fitur baru / bug / kerja multi-langkah:** jangan pakai putaran 3–7 A/B/C di file ini. Ikuti `superpowers.md` (plugin + tujuh skill). File ini bukan gerbang fitur.

**Non-fitur** (config, rename, setup) yang mengubah kode, DB, config, atau alur kerja:

1. JANGAN langsung edit / jalankan perintah besar.
2. Ajukan 3–7 pertanyaan klarifikasi dulu (lihat juga **putaran desain** di bawah bila ada UI **setelah spec Superpowers ada**).
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

Putaran desain Hallmark jalan **setelah** spec Superpowers ada (atau `defaults` pada spec). Jangan ganti spec dengan putaran desain terpisah gaya vibe.

## Putaran desain (wajib bila ada UI)

Kalau pekerjaan menyentuh halaman, landing, shell, atau komponen visual:

1. **Satu putaran khusus desain** (boleh digabung putaran teknis yang sama), minimal:
   - Brand / nama produk di UI
   - Tone visual (contoh: tenang korporat / berani editorial / minimal)
   - Referensi visual opsional (1–2 link/screenshot) — atau “tanpa referensi”
   - Responsive: desktop + mobile wajib untuk permukaan yang di-ship
2. Setelah itu: **baca dan ikuti Hallmark** (`hallmark` policy + skill) sebelum menulis UI.
3. Jika user balas **`defaults`**: jangan tanya desain lagi — pakai **Hallmark + design tokens merek** (warna/tipe dari brand atau fallback token yang sudah disepakati di spec Superpowers), tetap responsive.

Jangan anggap “shell minimal / placeholder AI” selesai kalau permukaan itu user-facing dan Hallmark berlaku.

## Build mode (setelah spec dan plan Superpowers disetujui)

Jika user sudah **approve spec dan plan Superpowers** yang relevan (atau bilang lanjut/approve pada dokumen itu):

1. **Jangan** membuka putaran ask-first baru untuk setiap modul/fitur P0.
2. Kerjakan sesuai urutan plan / DoD sampai selesai atau sampai blocker nyata (`executing-plans` + TDD).
3. Tanya lagi **hanya** jika: keamanan/secret, perubahan destruktif, domain bisnis belum dipilih padahal mengubah schema besar, atau dokumen bertentangan.

Analogi: setelah denah rumah disetujui, tukang tidak tanya ulang tiap pasang bata — hanya tanya jika tembok nabrak pipa.

Keluar build mode: user minta ubah arah besar, atau DoD tercapai.

## Pengecualian (boleh langsung)

- Pertanyaan informasi saja
- User bilang: "langsung kerjakan", "jangan tanya", "pakai default" / `defaults`
- Typo / 1 baris yang sudah sangat jelas
- **Build mode** aktif (lihat atas) — lanjut eksekusi tanpa tanya rutin

## Setup pertama / multi-provider

Untuk pasang policy, adapter Cursor/Claude/OpenCode/Codex, atau stack cursorrules: ikuti juga `first-setup.md` (konfirmasi perlu/tidak + provider mana) sebelum menyentuh file.
