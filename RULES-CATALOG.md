# Katalog rule ? apa, kenapa, kapan

Setiap rule di bawah berasal dari pola Koala. Versi portable (tanpa frontmatter Cursor) ada di `examples/policies/`.

## 1. Tanya dulu (`ask-first`)

**Analogi:** Sebelum renovasi rumah, tukang tanya dulu warna dan ruangan mana.

**Tujuan:** Cegah kerja besar yang salah arah.

**Wajib sebelum ubah kode / DB / config / alur:**
1. Jangan langsung edit atau perintah besar.
2. Ajukan 3–7 pertanyaan + pilihan A/B/C (tandai rekomendasi).
3. Tunggu jawaban atau `defaults`.
4. Ringkas pemahaman 2–3 kalimat bahasa awam, baru kerja.

**Boleh langsung:** pertanyaan info saja; user bilang "langsung kerjakan" / "jangan tanya" / "pakai default"; typo 1 baris yang sangat jelas.

**File Koala:** `.cursor/rules/tanya-dulu-sebelum-kerja.mdc`  
**Portable:** `examples/policies/ask-first.md`

## 1b. Setup pertama (`first-setup`)

**Analogi:** Sebelum pasang listrik di rumah baru, tanya dulu kamar mana yang perlu stopkontak.

**Tujuan:** Setup multi-provider (dan opsi vibe MVP) **tidak** dijalankan otomatis. AI wajib konfirmasi: perlu / tidak, menu apa, provider mana; skip = pointer checklist saja tanpa ubah file. Setelah “ya”, tunjukkan rencana lalu apply (hybrid).

**Kapan:** “setup agent kit”, project baru tanpa policies, mapping Cursor/Claude/OpenCode/Codex, atau alur ide→PRD→MVP.

**Portable:** `examples/policies/first-setup.md`  
**Cursor (kit ini):** `.cursor/rules/first-setup.mdc` (`alwaysApply: true`)  
**Vibe (opsional):** `examples/workflows/vibe-mvp/` → upstream [vibe-coding-prompt-template](https://github.com/KhazP/vibe-coding-prompt-template)  
**Stack cursorrules (opsional):** `examples/workflows/stack-cursorrules/` → upstream [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)

## 1c. Stack cursorrules (opsional)

**Analogi:** Bawa buku tips yang cocok pekerjaanmu, bukan seluruh perpustakaan.

**Tujuan:** Pasang 5–7 rule tech dari komunitas sesuai stack (PRD, jawaban user, atau scan project lama). Dual format: `.mdc` (Cursor) + `.md` (portable). Policy Agent Kit **selalu menang** atas rule luar.

**Kapan:** Setup pertama atau project lama yang butuh tips framework spesifik.

**Jangan:** vendor seluruh repo PatrickJS; `alwaysApply: true` pada rule luar tanpa minta user.

**Workflow:** `examples/workflows/stack-cursorrules/` · detail mapping: [PROVIDER-MAPPING.md](PROVIDER-MAPPING.md) §H

## 2. Ponytail (kode minimal)

**Analogi:** Jangan beli mesin besar kalau gunting sudah cukup.

**Tangga:** YAGNI ? sudah ada di repo? ? stdlib? ? fitur native? ? dependency terpasang? ? satu baris? ? tulis minimum.

**Tidak boleh malas di:** pemahaman masalah, validasi input, error yang cegah data hilang, security, a11y, hal yang diminta eksplisit.

**File Koala:** `.cursor/rules/ponytail.mdc` + `AGENTS.md`  
**Portable:** `examples/policies/ponytail.md`  
Upstream: https://github.com/DietrichGebert/ponytail

## 3. Caveman (jawaban singkat)

**Tujuan:** Hemat token. Substansi teknis tetap. Kode/commit/PR tetap normal.

Kontrol: `/caveman lite|full|ultra` ? stop: `stop caveman` / `normal mode`  
Auto-Clarity: lepas gaya caveman untuk security warning, aksi irreversible, user bingung.

**File Koala:** `.cursor/rules/caveman.mdc` ? **Portable:** `examples/policies/caveman.md`  
Upstream: https://github.com/JuliusBrussee/caveman

## 4. Security (DevSecOps / AppSec)

Secret di env/vault; validasi input; query parameterized; auth framework + RBAC; SCA/SAST bila memungkinkan.

**File Koala:** `.cursor/rules/security-devsecops-ssdls-appsec.mdc` ? **Portable:** `examples/policies/security.md`

## 5. Memory refresh (Codebase Memory + MemPalace)

| Lapisan | Tool | Fresh? |
|---------|------|--------|
| File di disk | Read, Grep | Ya |
| Graph CBM | index_repository, search_graph | Tidak ? re-index |
| MemPalace | checkpoint, search, diary | Tidak ? simpan manual |

| Tag | Arti |
|-----|------|
| `[REINDEX]` | Wajib index graph setelah tugas |
| `[MEMPALACE]` | Wajib update MemPalace |
| `[SESSION-END]` | Wajib checkpoint session |
| `[BRAINSTORM]` | Checkpoint + diary |
| `[NO-MEMORY]` | Skip simpan/refresh |

Session bermakna (wajib checkpoint): ubah kode, brainstorm, debug root cause, review pola bisnis, atau tag di atas.

**File Koala:** `.cursor/rules/memory-refresh-policy.mdc` ? **Portable:** `examples/policies/memory-refresh.md`  
Setup: [MCP-SETUP.md](MCP-SETUP.md)

## 6. Database read-only (via MCP)

AI boleh lihat data, tidak boleh ubah lewat MCP. Hanya SELECT + inspeksi schema. Tolak INSERT/UPDATE/DELETE/DROP/ALTER/TRUNCATE/CREATE.

**File Koala:** `.cursor/rules/database-readonly.mdc` ? **Portable:** `examples/policies/database-readonly.md`

## 7. Stack backend (contoh ? sesuaikan!)

**Peringatan:** File Koala `.cursor/rules/nodejs-mongodb-jwt-express-react-cursorrules-promp.mdc` masih menyebut **MongoDB + Mongoose**, sementara aplikasi Koala nyata memakai **PostgreSQL**. Anggap ini **contoh bentuk rule stack**, bukan kebenaran DB Koala.

Yang tetap berguna: ringkas flow sebelum coding; pseudocode endpoint; validasi + error REST; state machine jelas.

Untuk project mirip Koala, ganti stack jadi Node + Express + **PostgreSQL** + JWT (+ Flutter client bila ada).

**Portable:**
- Mongo contoh: `examples/policies/stack-backend.mongo.example.md`
- Postgres adaptasi: `examples/policies/stack-backend.postgres.example.md`

## 8. Flutter expert (opsional)

Kapan: project punya Flutter. Ikuti arsitektur repo yang ada; jangan paksakan struktur contoh.

**File Koala:** `.cursor/rules/flutter-app-expert-cursorrules-prompt-file.mdc` ? tidak disalin default di kit inti.

## 9. README style (opsional)

README seperti landing page; contoh kerja di 5 baris pertama; tabel fitur; Quick Start copy-paste.

**File Koala:** `.cursor/rules/rules/readme-best-practices-cursorrules-prompt-file.mdc` ? **Portable:** `examples/policies/readme-style.md`

## 10. Hallmark (UI anti-AI-slop)

**Analogi:** Bukan cuma ganti cat; bedakan kerangka rumah supaya tidak semua halaman AI keliatan sama.

**Tujuan:** UI/landing/komponen visual tidak jatuh ke default AI slop (template hero + 3 kartu + CTA yang sama terus).

**Verb:** default build · `hallmark audit` (skor, tanpa edit) · `hallmark redesign` · `hallmark study` (screenshot/URL).

**Bukan untuk:** isi README Markdown (tetap pakai `readme-style`). Tetap tunduk `ask-first` / `security` / `ponytail`.

**Portable:** `examples/skills/hallmark/` (`SKILL.md` + `references/` + README setup)
**Cursor (kit ini):** `.cursor/rules/hallmark.mdc` (`alwaysApply: true` = bridge; baca skill penuh dari `examples/skills/hallmark/`)
Upstream: https://github.com/Nutlope/hallmark · demo: https://www.usehallmark.com/

## File root terkait

| File | Peran |
|------|--------|
| `AGENTS.md` | Perilaku kode (Ponytail) ? dibaca banyak tool |
| `CLAUDE.md` | Perintah build, arsitektur project |

Jangan tempel seluruh katalog ke `CLAUDE.md`. Cukup: ikuti `AGENTS.md` + policies.
