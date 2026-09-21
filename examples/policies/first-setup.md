# Setup pertama — tanya dulu, baru pasang

**Analogi:** Sebelum memasang listrik di rumah baru, tukang tanya dulu: kamar mana yang perlu stopkontak? Kalau pemilik bilang “nanti saja”, tukang tidak pasang apa-apa — hanya kasih daftar langkah singkat.

## Kapan aturan ini berlaku

User (atau situasi project) menyentuh **setup pertama**, misalnya:

- “Setup agent kit”, “pasang rules”, “siapkan Cursor / Claude / OpenCode / Codex”
- Project baru belum punya `docs/agent-policies/` atau `AGENTS.md`
- Minta mapping multi-provider sekaligus
- Minta **rule stack** dari awesome-cursorrules (project baru atau lama)

## Larangan keras

**JANGAN** langsung menyalin policy, membuat `.cursor/rules/`, `CLAUDE.md`, `opencode.json`, atau rule dari awesome-cursorrules **sebelum** user mengonfirmasi.

Informasi saja (“apa itu first-setup?”) boleh dijawab tanpa pasang file.

## Pengecualian: working directory = repo Agent Kit

Kalau repo ini **adalah** Agent Kit (ada `examples/policies/`, `CHECKLIST-NEW-PROJECT.md`, `PROVIDER-MAPPING.md` di root):

- Policy aktif sudah di `examples/policies/`. Ikuti `AGENTS.md` root.
- **Jangan** menyalin ke `docs/agent-policies/` atau membuat `.cursor/rules/` di repo kit — keduanya di-gitignore dan menduplikasi sumber.
- first-setup **apply** hanya untuk memasang kit ke **project aplikasi** (folder lain). Trigger “belum punya `docs/agent-policies/`” **tidak** berarti harus generate folder itu di dalam kit.

## Alur wajib (hybrid)

1. **Jelaskan singkat** apa yang bisa dipasang (bahasa awam).
2. **Tanya konfirmasi** — perlu sekarang atau tidak.
3. Jika **tidak / skip:** beri ringkasan + tautan checklist; **jangan ubah file**.
4. Jika **ya:** tanya detail (menu + provider), ringkas pemahaman 2–3 kalimat, **baru** pasang file.
5. Setelah pasang: tunjukkan apa yang berubah + cara uji singkat.

Ask-first tetap berlaku untuk detail teknis; aturan ini khusus **pintu setup pertama / multi-provider**.

## Pertanyaan wajib (setelah user bilang tertarik / “setup”)

### A. Perlu setup sekarang?

- **Ya** — lanjut tanya menu + provider  
- **Tidak / nanti** — skip (lihat bawah)  
- **Belum yakin** — jelaskan beda “policy harian” vs “rule stack”, lalu tanya lagi  

### B. Mau yang mana? (boleh kombinasi)

1. **Policy agent-kit saja** — aturan kerja AI (keamanan, memory, Superpowers overlay, dll.)  
2. **Stack cursorrules** — rule tech dari awesome-cursorrules, dipilih sesuai stack (lihat `examples/workflows/stack-cursorrules/` di Agent Kit)  
3. **Skip** — tidak pasang apa-apa sekarang  

Boleh gabung: 1 + 2 (policy + rule stack).

Superpowers **bukan** menu. Plugin wajib dicentang di checklist untuk setiap host yang dipilih.

### C. Provider mana yang dipakai? (boleh lebih dari satu)

Tanya eksplisit; **jangan pasang adapter untuk tool yang tidak dipilih**.

| Provider | Contoh jawaban user |
|----------|---------------------|
| Cursor | “Ya, pakai Cursor” |
| Claude Code | “Ya / tidak” |
| OpenCode | “Ya / tidak” |
| Codex | “Ya / tidak” |
| Generic saja (`AGENTS.md`) | “Cukup file umum” |

### D. Stack cursorrules? (jika menu B menyertakan opsi 2, atau user minta rule stack)

Ikuti workflow `examples/workflows/stack-cursorrules/` di Agent Kit:

1. Deteksi stack: tanya user + baca spec/plan Superpowers di app (jika ada) + scan project lama.  
2. Usulkan **maks 5–7** rule dari semua kategori upstream yang relevan.  
3. Tunjukkan rencana (path `.mdc` + `.md`) — tunggu “lanjut”.  
4. Policy Agent Kit **tetap menang** atas rule luar.

Kalau user skip opsi ini: lanjut setup tanpa rule stack.

### E. Setelah “Ya” + pilihan jelas

1. Tunjukkan **rencana singkat** (file apa yang akan dibuat/disalin) — belum eksekusi.  
2. Tunggu “lanjut” / “apply” / “defaults”.  
3. Baru salin/edit sesuai checklist project baru dan mapping provider di Agent Kit.  
4. Secret / password MCP: **jangan** ditulis ke git; arahkan ke env / config lokal.

## Jika user skip / “tidak perlu”

- Ucapkan singkat: setup ditunda.  
- Beri 2–4 baris pointer ke checklist dan mapping provider (opsional: stack-cursorrules).  
- **Tidak** membuat atau mengubah file project.  
- Jangan mengulang tanya setup di setiap pesan; tanya lagi hanya jika user meminta setup.

## Setelah apply — uji cepat

Sesuaikan provider yang dipilih:

- Cursor: Agent chat baru; “List active project rules”
- Cursor: `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) di **app** mengarah ke overlay, bukan isi skill
- Claude Code: `/context` menampilkan instruksi
- Semua host yang dipilih: plugin Superpowers terpasang; overlay `superpowers.md` ikut tersalin
- OpenCode: fitur baru → Superpowers (bukan diff langsung); config/rename boleh `ask-first`
- Codex / generic: “ikuti AGENTS.md”

## Bahasa

Tulis ke user dalam **Bahasa Indonesia** yang mudah, seperti ke orang non-teknis. Istilah teknis boleh, beri analogi singkat.
