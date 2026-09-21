# Katalog rule ? apa, kenapa, kapan

Setiap rule di bawah berasal dari pola Koala.

| Tempat | Path |
|--------|------|
| Repo Agent Kit (sumber, di git) | `examples/policies/` |
| Project aplikasi (setelah first-setup) | `docs/agent-policies/` |
| Adapter Cursor (setelah first-setup **di app**) | `.cursor/rules/*.mdc` |

Di repo kit ini, **jangan** menganggap `docs/agent-policies/` atau `.cursor/rules/` ada di git.

## 1. Tanya dulu (`ask-first`)

**Analogi:** Sebelum renovasi rumah, tukang tanya dulu warna dan ruangan mana. Setelah denah disetujui, jangan tanya ulang tiap bata.

**Tujuan:** Cegah kerja besar yang salah arah — tanpa menghambat eksekusi setelah rencana jelas.

**Fitur baru / bug:** `superpowers.md`, bukan 3–7 A/B/C di sini.

**Non-fitur / first-setup:** 3–7 pertanyaan + A/B/C (tandai rekomendasi), tunggu `defaults`, ringkas 2–3 kalimat.

**Putaran desain (ada UI):** setelah spec Superpowers — brand, tone, referensi opsional, responsive — atau `defaults` = Hallmark + tokens.

**Build mode:** spec + plan Superpowers disetujui → kerjakan sampai DoD; jangan tanya ulang per modul (kecuali blocker keamanan/destruktif/domain besar).

**Boleh langsung:** pertanyaan info saja; "langsung kerjakan" / "jangan tanya" / `defaults`; typo 1 baris; build mode aktif.

**Portable (kit):** `examples/policies/ask-first.md`  
**App setelah setup:** `docs/agent-policies/ask-first.md` · Cursor: `.cursor/rules/ask-first.mdc`

## 1b. Setup pertama (`first-setup`)

**Analogi:** Sebelum pasang listrik di rumah baru, tanya dulu kamar mana yang perlu stopkontak.

**Tujuan:** Setup multi-provider (dan opsi stack cursorrules) **tidak** dijalankan otomatis. AI wajib konfirmasi: perlu / tidak, menu apa, provider mana; skip = pointer checklist saja tanpa ubah file. Setelah “ya”, tunjukkan rencana lalu apply (hybrid). Superpowers plugin wajib di checklist, bukan menu.

**Kapan:** “setup agent kit”, project baru tanpa policies, mapping Cursor/Claude/OpenCode/Codex.

**Portable (kit):** `examples/policies/first-setup.md`  
**App setelah setup:** `docs/agent-policies/first-setup.md` · Cursor: `.cursor/rules/first-setup.mdc` (`alwaysApply: true`)  
**Stack cursorrules (opsional):** `examples/workflows/stack-cursorrules/` → upstream [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)

## 1c. Stack cursorrules (opsional)

**Analogi:** Bawa buku tips yang cocok pekerjaanmu, bukan seluruh perpustakaan.

**Tujuan:** Pasang 5–7 rule tech dari komunitas sesuai stack (spec/plan Superpowers, jawaban user, atau scan project lama). Dual format: `.mdc` (Cursor) + `.md` (portable). Policy Agent Kit **selalu menang** atas rule luar.

**Kapan:** Setup pertama atau project lama yang butuh tips framework spesifik.

**Jangan:** vendor seluruh repo PatrickJS; `alwaysApply: true` pada rule luar tanpa minta user.

**Workflow:** `examples/workflows/stack-cursorrules/` · detail mapping: [PROVIDER-MAPPING.md](PROVIDER-MAPPING.md) §H

## 1d. Superpowers (proses fitur / bug)

**Analogi:** Tukang bikin denah dan daftar langkah dulu, baru pasang bata — dan tes setiap sambungan.

**Tujuan:** Fitur/bug lewat plugin Superpowers (brainstorm → spec → plan → TDD), bukan `ask-first` 3–7. Kit tidak men-vendor skill.

**Wajib:** tujuh skill `using-superpowers`, `brainstorming`, `systematic-debugging`, `writing-plans`, `executing-plans`, `test-driven-development`, `verification-before-completion`. Plugin absen → berhenti + pasang.

**Kit tetap menang:** `security`, `database-readonly`, `first-setup`. Ponytail = ukuran, bukan skip tes. Hallmark setelah spec untuk UI.

**Portable (kit):** `examples/policies/superpowers.md`  
**App setelah setup:** `docs/agent-policies/superpowers.md` · Cursor: `.cursor/rules/superpowers.mdc` (`alwaysApply: true` = overlay)  
**Plugin:** [obra/superpowers](https://github.com/obra/superpowers) — pasang per host (CHECKLIST / PROVIDER-MAPPING)

## 2. Ponytail (kode minimal)

**Analogi:** Jangan beli mesin besar kalau gunting sudah cukup.

**Tangga:** YAGNI ? sudah ada di repo? ? stdlib? ? fitur native? ? dependency terpasang? ? satu baris? ? tulis minimum.

**Tidak boleh malas di:** pemahaman masalah, validasi input, error yang cegah data hilang, security, a11y, hal yang diminta eksplisit.

**Portable (kit):** `examples/policies/ponytail.md` + root `AGENTS.md`  
**App setelah setup:** `.cursor/rules/ponytail.mdc` + `AGENTS.md`  
Upstream: https://github.com/DietrichGebert/ponytail

## 3. Caveman (jawaban singkat)

**Tujuan:** Hemat token. Substansi teknis tetap. Kode/commit/PR tetap normal.

Kontrol: `/caveman lite|full|ultra` ? stop: `stop caveman` / `normal mode`  
Auto-Clarity: lepas gaya caveman untuk security warning, aksi irreversible, user bingung.

**Portable (kit):** `examples/policies/caveman.md`  
**App setelah setup:** `.cursor/rules/caveman.mdc`  
Upstream: https://github.com/JuliusBrussee/caveman

## 4. Security (DevSecOps / AppSec)

Secret di env/vault; validasi input; query parameterized; auth framework + RBAC; SCA/SAST bila memungkinkan.

**Portable (kit):** `examples/policies/security.md`  
**App setelah setup:** `.cursor/rules/security-devsecops-ssdls-appsec.mdc`

## 5. Memory refresh (Codebase Memory + MemPalace + Claude Mem)

| Lapisan | Tool | Fresh? |
|---------|------|--------|
| File di disk | Read, Grep | Ya |
| Graph CBM | index_repository, search_graph | Tidak — re-index |
| MemPalace | checkpoint, search, diary | Tidak — simpan manual |
| Claude Mem | search, timeline, get_observations | Tidak — hook + worker; search saat lanjut topik |

| Tag | Arti |
|-----|------|
| `[REINDEX]` | Wajib index graph setelah tugas |
| `[MEMPALACE]` | Wajib update MemPalace |
| `[SESSION-END]` | Wajib checkpoint session |
| `[BRAINSTORM]` | Checkpoint + diary |
| `[NO-MEMORY]` | Skip MemPalace **dan** skip search/corpus Claude Mem |

Lanjut topik: search Claude Mem dulu, lalu MemPalace. Keputusan tetap di MemPalace. `smart_search` Claude Mem bukan pengganti CBM.

Session bermakna (wajib checkpoint): ubah kode, brainstorm, debug root cause, review pola bisnis, atau tag di atas.

**Portable (kit):** `examples/policies/memory-refresh.md`  
**App setelah setup:** `.cursor/rules/memory-refresh-policy.mdc`  
Setup: [MCP-SETUP.md](MCP-SETUP.md)

## 6. Database read-only (via MCP)

AI boleh lihat data, tidak boleh ubah lewat MCP. Hanya SELECT + inspeksi schema. Tolak INSERT/UPDATE/DELETE/DROP/ALTER/TRUNCATE/CREATE.

**Portable (kit):** `examples/policies/database-readonly.md`  
**App setelah setup:** `.cursor/rules/database-readonly.mdc`

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

**Tujuan:** UI/landing/shell/komponen visual tidak jatuh ke default AI slop; **responsive** desktop+mobile; audit sebelum ship.

**Gerbang:** setelah spec Superpowers, baca skill penuh sebelum tulis UI; larang ship placeholder AI sebagai “selesai”; Ponytail tidak mengizinkan skip Hallmark pada UI user-facing.

**Verb:** default build · `hallmark audit` (skor, tanpa edit) · `hallmark redesign` · `hallmark study` (screenshot/URL).

**Bukan untuk:** isi README Markdown (tetap pakai `readme-style`). Security / first-setup / DB RO tetap menang. Overlay Superpowers wajib untuk fitur. Hormati build mode + `defaults` desain di `ask-first`.

**Portable (kit):** `examples/policies/hallmark.md`  
**App setelah setup:** `docs/agent-policies/hallmark.md` · Cursor: `.cursor/rules/hallmark.mdc` (`alwaysApply: true` = bridge)  
**Skill:** `examples/skills/hallmark/` (`SKILL.md` + `references/` + README setup)  
Upstream: https://github.com/Nutlope/hallmark · demo: https://www.usehallmark.com/

## File root terkait

| File | Peran |
|------|--------|
| `AGENTS.md` | Perilaku kode (Ponytail) ? dibaca banyak tool |
| `CLAUDE.md` | Perintah build, arsitektur project |

Jangan tempel seluruh katalog ke `CLAUDE.md`. Cukup: ikuti `AGENTS.md` + policies (`examples/policies/` di kit; `docs/agent-policies/` di app).
