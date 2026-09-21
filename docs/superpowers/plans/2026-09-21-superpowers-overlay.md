# Superpowers Overlay Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a pointer-only Superpowers overlay to Agent Kit, make it the feature/bug pipeline, and delete the vibe-MVP workflow.

**Architecture:** One new policy (`examples/policies/superpowers.md`) plus edits to existing kit docs. The plugin is not vendored; install commands live in CHECKLIST + PROVIDER-MAPPING. Consumer apps copy the overlay with other policies. This kit git still must not contain `docs/agent-policies/` or `.cursor/rules/`.

**Tech Stack:** Markdown policies, ripgrep (`rg`) for checks, git. No new runtime, npm package, or vendored skills.

**Spec:** `docs/superpowers/specs/2026-09-21-superpowers-overlay-design.md`

## Global Constraints

- Pointer overlay only — never create `examples/skills/superpowers/` or copy Superpowers `SKILL.md` bodies into the kit.
- Never create `docs/agent-policies/` or `.cursor/rules/` inside this kit repo.
- Do not edit `examples/CLAUDE.md.template` or `examples/skills/hallmark/**` (leave Hallmark `custom-theme.md` “Vibe answer” alone).
- Overlay and kit policies that agents read: Bahasa Indonesia, same register as `hallmark.md` / `first-setup.md`.
- Install commands must match [obra/superpowers README](https://github.com/obra/superpowers): Cursor `/add-plugin superpowers`; Claude Code `/plugin install superpowers@claude-plugins-official`; OpenCode `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]`; Codex App Plugins → Superpowers or CLI `/plugins` → `superpowers` → Install.
- Kit safety always wins: `security`, `database-readonly`, `first-setup`. Superpowers does not authorize secrets-in-git, MCP writes, or applying kit files into an app without first-setup confirmation.
- Seven required skill names (verbatim): `using-superpowers`, `brainstorming`, `systematic-debugging`, `writing-plans`, `executing-plans`, `test-driven-development`, `verification-before-completion`.
- Working tree may already contain unrelated uncommitted edits in some of the same files. Apply only the replacements in this plan. Do not revert unrelated hunks. `git add` only the paths listed in each commit step.
- This kit is documentation: “tests” are `rg` / `Test-Path` checks, not pytest.
- After the last task: MemPalace checkpoint; skip CBM re-index unless `[REINDEX]`.

---

## File structure

| Path | Responsibility |
|------|----------------|
| `examples/policies/superpowers.md` | Overlay: when, priority, seven skills, missing-plugin stop, build mode, pointers. |
| `examples/policies/first-setup.md` | Setup door: no vibe menu; Superpowers not a menu (checklist). |
| `examples/policies/ask-first.md` | Leftover: first-setup + non-feature; build mode = spec+plan. |
| `examples/policies/ponytail.md` | TDD order vs diff size (2 sentences). |
| `examples/policies/hallmark.md` | Spec first, then Hallmark for visuals (2 sentences). |
| `AGENTS.md` | Kit-root agent bridge: Superpowers for features; no vibe section. |
| `examples/AGENTS.md` | App template bridge (`docs/agent-policies/` paths). |
| `CLAUDE.md` | Kit architecture notes: overlay, no vibe pointer. |
| `CHECKLIST-NEW-PROJECT.md` | Required plugin install + behavior tests. |
| `PROVIDER-MAPPING.md` | Host table + install commands + Cursor `.mdc` overlay. |
| `RULES-CATALOG.md` | Catalog entry; ask-first/first-setup text match overlay. |
| `README.md`, `examples/README.md` | Index: overlay, no vibe row. |
| `examples/workflows/stack-cursorrules/README.md` | Detect stack from Superpowers spec/plan, not vibe. |
| `examples/workflows/vibe-mvp/README.md` | Delete. |

Do not add `examples/workflows/superpowers/`.

---

### Task 1: Overlay policy file

**Files:**
- Create: `examples/policies/superpowers.md`
- Test: `rg` on that path (file missing → fail; after write → required strings present)

**Interfaces:**
- Consumes: spec decisions 1a, 2b, 3b, 4a, 7a, 9a, 10a, A.
- Produces: canonical overlay at `examples/policies/superpowers.md` (Indonesian). Later tasks only link this path; they do not duplicate install command tables.

- [ ] **Step 1: Prove the overlay file does not exist**

Run from repo root (PowerShell):

```powershell
Test-Path -LiteralPath examples/policies/superpowers.md
```

Expected: `False`

- [ ] **Step 2: Write `examples/policies/superpowers.md`**

Write this exact file (no extra sections):

```markdown
# Superpowers — proses fitur dan bug (overlay kit)

Plugin: [obra/superpowers](https://github.com/obra/superpowers) (MIT). Kit **tidak** men-vendor skill. Pasang plugin di host yang dipakai — perintah: [CHECKLIST-NEW-PROJECT.md](../../CHECKLIST-NEW-PROJECT.md) dan [PROVIDER-MAPPING.md](../../PROVIDER-MAPPING.md).

Salin file ini ke `docs/agent-policies/superpowers.md` di project app bersama policy lain.

## Kapan wajib

Fitur baru, perbaikan bug, atau kerja multi-langkah yang mengubah perilaku produk. Bukan typo 1 baris. Bukan first-setup (itu `first-setup.md`).

## Plugin wajib

Skill Superpowers harus terpasang di host sesi ini. Jika tidak: **berhenti**. Suruh user pasang per [CHECKLIST-NEW-PROJECT.md](../../CHECKLIST-NEW-PROJECT.md) / [PROVIDER-MAPPING.md](../../PROVIDER-MAPPING.md). Jangan implementasi fitur. Jangan putaran 3–7 A/B/C `ask-first` untuk fitur.

Jangan skip overlay ini kecuali user mengubah policy kit.

## Skill wajib (nama persis)

1. `using-superpowers`
2. `brainstorming` — fitur baru / kerja berbentuk produk
3. `systematic-debugging` — bug
4. `writing-plans` — setelah spec disetujui
5. `executing-plans` — setelah plan disetujui
6. `test-driven-development` — saat eksekusi
7. `verification-before-completion` — sebelum klaim selesai

Skill Superpowers lain (worktree, review, subagent, writing-skills, …) opsional, tidak dilarang.

## Prioritas (atas menang)

1. `security`, `database-readonly`, `first-setup` — selalu. Superpowers tidak boleh skip konfirmasi setup, secret di git, atau tulis DB lewat MCP.
2. Fitur / bug / multi-langkah — tujuh skill di atas.
3. `ask-first` — hanya first-setup dan kerja **non-fitur** (config, rename). Typo 1 baris tetap langsung. Bukan 3–7 A/B/C untuk fitur baru.
4. Ponytail — ukuran diff. Tidak skip TDD.
5. Hallmark — setelah spec, untuk halaman visual user-facing + `hallmark audit` sebelum ship.
6. Caveman — lepas saat brainstorm / spec / plan review. Balik setelah eksekusi mulai. Security / aksi irreversible / user bingung: selalu lepas.

## Spec dan plan

Ikuti path plugin yang terpasang (default app: `docs/superpowers/specs/`, plans). Jangan invent root spec kit untuk app.

**Build mode:** user sudah approve **spec dan plan** Superpowers → eksekusi sampai DoD (`executing-plans` + TDD). Jangan tanya ulang per modul. Bukan PRD KhazP / vibe-MVP.

## Refresh plugin

Update = pasang ulang plugin di host. Bukan vendor bump di repo kit.
```

- [ ] **Step 3: Prove required strings exist**

```powershell
rg -n "using-superpowers|brainstorming|systematic-debugging|writing-plans|executing-plans|test-driven-development|verification-before-completion" examples/policies/superpowers.md
rg -n "berhenti" examples/policies/superpowers.md
rg -n "KhazP|vibe-MVP" examples/policies/superpowers.md
```

Expected: first two commands print hits; third prints at least the “Bukan PRD KhazP / vibe-MVP” line (allowed mention). File must also contain `ask-first`, `first-setup`, `database-readonly`, `security`, `Ponytail`, `Hallmark`.

```powershell
rg -n "ask-first|first-setup|database-readonly|security|Ponytail|Hallmark" examples/policies/superpowers.md
```

Expected: all of those words present.

- [ ] **Step 4: Commit**

```powershell
git add -- examples/policies/superpowers.md
git commit -m "Add Superpowers overlay policy for feature and bug work."
```

---

### Task 2: first-setup without vibe

**Files:**
- Modify: `examples/policies/first-setup.md`
- Test: `rg vibe-mvp examples/policies/first-setup.md` must be empty

**Interfaces:**
- Consumes: overlay path `examples/policies/superpowers.md` (exists after Task 1). Menu is not Superpowers.
- Produces: first-setup menu (1) kit policies (2) stack cursorrules (3) skip; combo 1+2; skip pointers without vibe.

- [ ] **Step 1: Show current vibe hits (must exist before edit)**

```powershell
rg -n "vibe|Vibe|PRD" examples/policies/first-setup.md
```

Expected: hits (menu items 2–3, “alur vibe”, “file vibe”, skip pointer).

- [ ] **Step 2: Replace trigger, larangan, and “belum yakin” lines**

In `examples/policies/first-setup.md`:

Replace:

```markdown
- Minta alur **ide → PRD → MVP** (vibe workflow)
```

with nothing — delete that bullet.

Replace:

```markdown
**JANGAN** langsung menyalin policy, membuat `.cursor/rules/`, `CLAUDE.md`, `opencode.json`, file vibe, atau rule dari awesome-cursorrules **sebelum** user mengonfirmasi.
```

with:

```markdown
**JANGAN** langsung menyalin policy, membuat `.cursor/rules/`, `CLAUDE.md`, `opencode.json`, atau rule dari awesome-cursorrules **sebelum** user mengonfirmasi.
```

Replace:

```markdown
- **Belum yakin** — jelaskan beda “policy harian” vs “alur vibe”, lalu tanya lagi  
```

with:

```markdown
- **Belum yakin** — jelaskan beda “policy harian” vs “rule stack”, lalu tanya lagi  
```

- [ ] **Step 3: Replace menu B and the gabung example**

Replace the whole `### B. Mau yang mana?` section through the gabung sentence with:

```markdown
### B. Mau yang mana? (boleh kombinasi)

1. **Policy agent-kit saja** — aturan kerja AI (keamanan, memory, Superpowers overlay, dll.)  
2. **Stack cursorrules** — rule tech dari awesome-cursorrules, dipilih sesuai stack (lihat `examples/workflows/stack-cursorrules/` di Agent Kit)  
3. **Skip** — tidak pasang apa-apa sekarang  

Boleh gabung: 1 + 2 (policy + rule stack).

Superpowers **bukan** menu. Plugin wajib dicentang di checklist untuk setiap host yang dipilih.
```

- [ ] **Step 4: Fix section D numbering, skip pointer, after-apply tests**

Replace:

```markdown
### D. Stack cursorrules? (jika menu B menyertakan opsi 4, atau user minta rule stack)
```

with:

```markdown
### D. Stack cursorrules? (jika menu B menyertakan opsi 2, atau user minta rule stack)
```

Replace:

```markdown
1. Deteksi stack: tanya user + baca PRD/Tech Design (jika ada) + scan project lama.  
```

with:

```markdown
1. Deteksi stack: tanya user + baca spec/plan Superpowers di app (jika ada) + scan project lama.  
```

Replace:

```markdown
- Beri 2–4 baris pointer ke checklist, mapping provider, vibe workflow, dan (opsional) stack-cursorrules.  
```

with:

```markdown
- Beri 2–4 baris pointer ke checklist dan mapping provider (opsional: stack-cursorrules).  
```

Replace the “Setelah apply — uji cepat” OpenCode line:

```markdown
- OpenCode: uji ask-first pada permintaan refactor
```

with:

```markdown
- Semua host yang dipilih: plugin Superpowers terpasang; overlay `superpowers.md` ikut tersalin
- OpenCode: fitur baru → Superpowers (bukan diff langsung); config/rename boleh `ask-first`
```

Keep Cursor / Claude / Codex bullets. Add one bullet if missing:

```markdown
- Cursor: `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) di **app** mengarah ke overlay, bukan isi skill
```

- [ ] **Step 5: Verify vibe-mvp path is gone**

```powershell
rg -n "vibe-mvp|KhazP|alur vibe" examples/policies/first-setup.md
```

Expected: no matches.

```powershell
rg -n "Superpowers \*\*bukan\*\* menu|opsi 2" examples/policies/first-setup.md
```

Expected: hits.

- [ ] **Step 6: Commit**

```powershell
git add -- examples/policies/first-setup.md
git commit -m "Remove vibe-MVP from first-setup; keep Superpowers off the menu."
```

---

### Task 3: ask-first leftover + build mode

**Files:**
- Modify: `examples/policies/ask-first.md`

**Interfaces:**
- Consumes: overlay `examples/policies/superpowers.md`; first-setup without vibe.
- Produces: ask-first applies to first-setup + non-feature only; feature work defers to Superpowers; build mode = approved spec and plan.

- [ ] **Step 1: Show the old feature gate**

```powershell
rg -n "3–7 pertanyaan|PRD \+ Tech Design|vibe MVP" examples/policies/ask-first.md
```

Expected: hits on the current wajib-sebelum-eksekusi block, build mode, and first-setup sentence.

- [ ] **Step 2: Replace “Wajib sebelum eksekusi”**

Replace:

```markdown
## Wajib sebelum eksekusi

Untuk permintaan yang mengubah kode, DB, config, atau alur kerja:

1. JANGAN langsung edit / jalankan perintah besar.
2. Ajukan 3–7 pertanyaan klarifikasi dulu (lihat juga **putaran desain** di bawah bila ada UI).
3. Tunggu jawaban user (atau `defaults` / `lanjut dengan default`).
4. Ringkas ulang pemahaman dalam 2–3 kalimat bahasa awam, baru kerja.
```

with:

```markdown
## Wajib sebelum eksekusi

**Fitur baru / bug / kerja multi-langkah:** jangan pakai putaran 3–7 A/B/C di file ini. Ikuti `superpowers.md` (plugin + tujuh skill). File ini bukan gerbang fitur.

**Non-fitur** (config, rename, setup) yang mengubah kode, DB, config, atau alur kerja:

1. JANGAN langsung edit / jalankan perintah besar.
2. Ajukan 3–7 pertanyaan klarifikasi dulu (lihat juga **putaran desain** di bawah bila ada UI **setelah spec Superpowers ada**).
3. Tunggu jawaban user (atau `defaults` / `lanjut dengan default`).
4. Ringkas ulang pemahaman dalam 2–3 kalimat bahasa awam, baru kerja.
```

- [ ] **Step 3: Replace putaran desain item 3, build mode, and first-setup trigger**

Replace:

```markdown
3. Jika user balas **`defaults`**: jangan tanya desain lagi — pakai **Hallmark + design tokens merek** (warna/tipe dari brand atau fallback token yang sudah disepakati di PRD/Tech Design), tetap responsive.
```

with:

```markdown
3. Jika user balas **`defaults`**: jangan tanya desain lagi — pakai **Hallmark + design tokens merek** (warna/tipe dari brand atau fallback token yang sudah disepakati di spec Superpowers), tetap responsive.
```

Insert **before** `## Putaran desain` this sentence as its own paragraph:

```markdown
Putaran desain Hallmark jalan **setelah** spec Superpowers ada (atau `defaults` pada spec). Jangan ganti spec dengan putaran desain terpisah gaya vibe.
```

Replace the whole `## Build mode` section with:

```markdown
## Build mode (setelah spec dan plan Superpowers disetujui)

Jika user sudah **approve spec dan plan Superpowers** yang relevan (atau bilang lanjut/approve pada dokumen itu):

1. **Jangan** membuka putaran ask-first baru untuk setiap modul/fitur P0.
2. Kerjakan sesuai urutan plan / DoD sampai selesai atau sampai blocker nyata (`executing-plans` + TDD).
3. Tanya lagi **hanya** jika: keamanan/secret, perubahan destruktif, domain bisnis belum dipilih padahal mengubah schema besar, atau dokumen bertentangan.

Analogi: setelah denah rumah disetujui, tukang tidak tanya ulang tiap pasang bata — hanya tanya jika tembok nabrak pipa.

Keluar build mode: user minta ubah arah besar, atau DoD tercapai.
```

Replace:

```markdown
Untuk pasang policy, adapter Cursor/Claude/OpenCode/Codex, alur vibe MVP, atau stack cursorrules: ikuti juga `first-setup.md` (konfirmasi perlu/tidak + provider mana) sebelum menyentuh file.
```

with:

```markdown
Untuk pasang policy, adapter Cursor/Claude/OpenCode/Codex, atau stack cursorrules: ikuti juga `first-setup.md` (konfirmasi perlu/tidak + provider mana) sebelum menyentuh file.
```

- [ ] **Step 4: Verify**

```powershell
rg -n "vibe MVP|PRD \+ Tech Design|ide → PRD" examples/policies/ask-first.md
rg -n "superpowers.md" examples/policies/ask-first.md
rg -n "approve spec dan plan Superpowers" examples/policies/ask-first.md
```

Expected: first command no matches; second and third have hits.

- [ ] **Step 5: Commit**

```powershell
git add -- examples/policies/ask-first.md
git commit -m "Limit ask-first to setup and non-feature work."
```

---

### Task 4: Ponytail, Hallmark, AGENTS, CLAUDE

**Files:**
- Modify: `examples/policies/ponytail.md`
- Modify: `examples/policies/hallmark.md`
- Modify: `AGENTS.md`
- Modify: `examples/AGENTS.md`
- Modify: `CLAUDE.md`

**Interfaces:**
- Consumes: `examples/policies/superpowers.md`; ask-first leftover from Task 3.
- Produces: agent-facing pointers that name Superpowers for features and drop vibe-MVP.

- [ ] **Step 1: Baseline vibe/PRD hits**

```powershell
rg -n "vibe-mvp|Vibe MVP|PRD\+Tech Design|PRD \+ Tech Design" AGENTS.md examples/AGENTS.md CLAUDE.md examples/policies/ponytail.md examples/policies/hallmark.md
```

Expected: hits in `AGENTS.md` (vibe section + item 1–2), `examples/AGENTS.md` (items 1–2), `CLAUDE.md` (vibe pointer + build mode). Ponytail/Hallmark may have zero vibe hits.

- [ ] **Step 2: Append to `examples/policies/ponytail.md` after the last paragraph**

```markdown

Fitur baru / bug: urutan kerja = TDD Superpowers (`test-driven-development` di overlay `superpowers.md`). Tangga Ponytail = ukuran diff. Jangan skip tes karena “kode minimal”.
```

- [ ] **Step 3: Edit `examples/policies/hallmark.md` gerbang + build mode**

Replace:

```markdown
1. Selesaikan **putaran desain** di `ask-first` — atau terima `defaults` (Hallmark + tokens merek).
```

with:

```markdown
1. Spec Superpowers sudah ada (atau `defaults` pada spec). Baru **putaran desain** Hallmark di `ask-first` — atau `defaults` (Hallmark + tokens merek).
```

Replace:

```markdown
Saat `ask-first` **build mode** aktif: tetap wajib Hallmark + responsive; **jangan** tanya ulang desain tiap halaman jika sudah `defaults` / putaran desain selesai.
```

with:

```markdown
Saat **build mode** aktif (spec + plan Superpowers disetujui): tetap wajib Hallmark + responsive; **jangan** tanya ulang desain tiap halaman jika sudah `defaults` / putaran desain selesai.
```

In “Yang dilarang”, replace:

```markdown
- Menimpa `security`, `database-readonly`, atau first-setup.
```

with:

```markdown
- Menimpa `security`, `database-readonly`, first-setup, atau overlay Superpowers.
```

- [ ] **Step 4: Replace kit `AGENTS.md` “Before changing…” items 1–2, vibe section, stack-cursorrules intro, and the docs-gitignore sentence**

Replace:

```markdown
This repo **is** Agent Kit. Canonical policies: `examples/policies/`. Do **not** copy them to `docs/agent-policies/` here (`docs/` and `.cursor/` are gitignored; that would duplicate the source).
```

with:

```markdown
This repo **is** Agent Kit. Canonical policies: `examples/policies/`. Do **not** copy them to `docs/agent-policies/` here (`docs/agent-policies/` and `.cursor/` are gitignored; that would duplicate the source). Superpowers specs/plans under `docs/superpowers/` are tracked.
```

Replace items 1–2 under “Before changing code…” with:

```markdown
1. Follow `examples/policies/superpowers.md` for new features, bugs, and multi-step product work (plugin + seven skills). `ask-first.md` is only for first-setup and non-feature work (config, rename) unless proceed/`defaults`. After approved Superpowers spec **and** plan: **build mode** (ship to DoD; no per-feature re-ask). UI: spec first, then one Hallmark **design round** (or `defaults` → Hallmark + tokens).
2. For **first-time / multi-provider setup** (or stack cursorrules): follow `examples/policies/first-setup.md` — confirm with the user before creating or copying setup files; skip = checklist links only, no edits. Apply only into an **application** repo, not into this kit. Stack rules: `examples/workflows/stack-cursorrules/`. Superpowers plugin install is required on each selected host (checklist), not a first-setup menu item.
```

Delete the entire `## Vibe MVP (opsional)` section (heading through “Jangan pasang overlay vibe/PRD ke repo kit ini.”).

Replace:

```markdown
Setelah PRD/tech design di **app**: ikuti `examples/workflows/stack-cursorrules/`. File hasil: `docs/agent-policies/stack-*.md` + `.cursor/rules/stack-*.mdc` (`alwaysApply: false`) **di repo aplikasi**. **Jangan** menimpa ask-first / security / database-readonly / first-setup.
```

with:

```markdown
Setelah spec/plan Superpowers (atau stack sudah jelas) di **app**: ikuti `examples/workflows/stack-cursorrules/`. File hasil: `docs/agent-policies/stack-*.md` + `.cursor/rules/stack-*.mdc` (`alwaysApply: false`) **di repo aplikasi**. **Jangan** menimpa Superpowers overlay / ask-first / security / database-readonly / first-setup.
```

Insert after the UI / visual design section (before stack cursorrules) a short Superpowers section:

```markdown
## Superpowers (fitur / bug)

Overlay: `examples/policies/superpowers.md`. Pasang plugin per host (CHECKLIST / PROVIDER-MAPPING). Jangan vendor skill ke repo ini.
```

Also extend Communication to mention Superpowers:

Replace:

```markdown
Default terse style: `examples/policies/caveman.md`. Drop that style for security warnings, irreversible actions, or when the user is confused. User may say `stop caveman` or `normal mode`.
```

with:

```markdown
Default terse style: `examples/policies/caveman.md`. Drop that style for security warnings, irreversible actions, when the user is confused, and during Superpowers brainstorm / spec / plan review. User may say `stop caveman` or `normal mode`.
```

- [ ] **Step 5: Mirror the same behavior in `examples/AGENTS.md` with app paths**

Replace items 1–2 with:

```markdown
1. Follow `docs/agent-policies/superpowers.md` for new features, bugs, and multi-step product work (plugin + seven skills). `ask-first.md` is only for first-setup and non-feature work (config, rename) unless proceed/`defaults`. After approved Superpowers spec **and** plan: **build mode** (ship to DoD; no per-feature re-ask). UI: spec first, then one Hallmark **design round** (or `defaults` → Hallmark + tokens).
2. For **first-time / multi-provider setup** (or stack cursorrules): follow `docs/agent-policies/first-setup.md` — confirm with the user before creating or copying setup files; skip = checklist links only, no edits. Stack rules: `examples/workflows/stack-cursorrules/`. Superpowers plugin install is required on each selected host (checklist), not a first-setup menu item.
```

Add after UI section:

```markdown
## Superpowers (fitur / bug)

Overlay: `docs/agent-policies/superpowers.md`. Pasang plugin per host (CHECKLIST / PROVIDER-MAPPING).
```

Replace the caveman Communication paragraph the same way as root `AGENTS.md`, but keep `docs/agent-policies/caveman.md`.

- [ ] **Step 6: Edit `CLAUDE.md` architecture + agent notes**

Replace:

```markdown
Do not create `docs/agent-policies/` or `.cursor/rules/` inside this kit repo.
```

(keep that — still true.)

Replace the architecture bullet:

```markdown
- Vibe pointer: `examples/workflows/vibe-mvp/`
```

with:

```markdown
- Superpowers overlay: `examples/policies/superpowers.md` (plugin wajib di host; jangan vendor skill)
```

Replace:

```markdown
- Ask-first: putaran desain untuk UI; **build mode** setelah PRD+Tech Design disetujui (kerja sampai DoD, jangan tanya per modul).
```

with:

```markdown
- Superpowers: fitur/bug → overlay + plugin. Ask-first: first-setup + non-fitur. UI: spec dulu, lalu Hallmark. **Build mode** setelah spec+plan Superpowers disetujui (kerja sampai DoD, jangan tanya per modul).
```

- [ ] **Step 7: Verify**

```powershell
rg -n "vibe-mvp|Vibe MVP" AGENTS.md examples/AGENTS.md CLAUDE.md examples/policies/ponytail.md examples/policies/hallmark.md
rg -n "superpowers.md" AGENTS.md examples/AGENTS.md CLAUDE.md examples/policies/ponytail.md examples/policies/hallmark.md
```

Expected: first command no matches; second has hits in all five files.

- [ ] **Step 8: Commit**

```powershell
git add -- examples/policies/ponytail.md examples/policies/hallmark.md AGENTS.md examples/AGENTS.md CLAUDE.md
git commit -m "Point AGENTS and UI policies at Superpowers overlay."
```

---

### Task 5: Checklist, mapping, catalog, READMEs, stack workflow

**Files:**
- Modify: `CHECKLIST-NEW-PROJECT.md`
- Modify: `PROVIDER-MAPPING.md`
- Modify: `RULES-CATALOG.md`
- Modify: `README.md`
- Modify: `examples/README.md`
- Modify: `examples/workflows/stack-cursorrules/README.md`

**Interfaces:**
- Consumes: overlay file; install commands from Global Constraints (verbatim).
- Produces: human setup path with required plugin install; catalog entry `1d`; no vibe-mvp links except Hallmark “Vibe answer” (untouched).

- [ ] **Step 1: Baseline vibe-mvp links**

```powershell
rg -n "vibe-mvp|vibe MVP|Vibe workflow|KhazP" CHECKLIST-NEW-PROJECT.md PROVIDER-MAPPING.md RULES-CATALOG.md README.md examples/README.md examples/workflows/stack-cursorrules/README.md
```

Expected: hits in all of these files.

- [ ] **Step 2: Edit `CHECKLIST-NEW-PROJECT.md`**

Replace:

```markdown
**Ini untuk repo aplikasi.** Jangan jalankan salinan ke `docs/agent-policies/` di dalam repo Agent Kit (`docs/` dan `.cursor/` di-gitignore; sumber = `examples/policies/`).
```

with:

```markdown
**Ini untuk repo aplikasi.** Jangan jalankan salinan ke `docs/agent-policies/` di dalam repo Agent Kit (`docs/agent-policies/` dan `.cursor/` di-gitignore; sumber = `examples/policies/`).
```

Replace the §0 checkbox line:

```markdown
[ ] Jika Ya: pilih menu — policy saja / vibe saja / keduanya / stack cursorrules / skip (boleh kombinasi)
```

with:

```markdown
[ ] Jika Ya: pilih menu — policy saja / stack cursorrules / skip (boleh  policy + stack)
[ ] Jika Ya: untuk setiap provider yang dicentang, pasang plugin Superpowers (lihat §A2)
```

Replace:

```markdown
Policy agent: `examples/policies/first-setup.md` (setelah copy ke app: `docs/agent-policies/first-setup.md`).  
Alur vibe opsional: `examples/workflows/vibe-mvp/`.  
Stack cursorrules opsional: `examples/workflows/stack-cursorrules/`.
```

with:

```markdown
Policy agent: `examples/policies/first-setup.md` (setelah copy ke app: `docs/agent-policies/first-setup.md`).  
Superpowers overlay: `examples/policies/superpowers.md` (plugin wajib per host).  
Stack cursorrules opsional: `examples/workflows/stack-cursorrules/`.
```

Insert **after section A** (before `## B. Mapping ke tool`) this full section:

```markdown
## A2. Superpowers plugin (wajib)

Pasang **per host** yang dipilih di §0. Perintah dari [obra/superpowers](https://github.com/obra/superpowers) — jangan invent.

```text
[ ] Cursor: di Agent chat `/add-plugin superpowers` (atau cari “superpowers” di marketplace plugin)
[ ] Claude Code: `/plugin install superpowers@claude-plugins-official`
[ ] OpenCode: `opencode.json` → `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` lalu restart
[ ] Codex App: Plugins → Superpowers. Codex CLI: `/plugins` → search `superpowers` → Install
```

Tanpa centang untuk setiap provider yang dipakai = setup belum selesai. Overlay kit (`docs/agent-policies/superpowers.md`) bukan pengganti plugin.
```

Under Cursor mapping, after Hallmark checkbox, add:

```markdown
[ ] Superpowers overlay: `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) mengarah ke `docs/agent-policies/superpowers.md` (bukan isi skill)
```

Replace section D item 1 and 7 with this full D list:

```markdown
## D. Uji perilaku (wajib)

1. **Superpowers:** "Tambah fitur X" → brainstorm/spec, bukan diff besar. Plugin absen → berhenti + suruh pasang, bukan 3–7 A/B/C fitur.
2. **Ask-first sisa:** config/rename boleh A/B/C; typo 1 baris langsung. Bukan gerbang fitur baru.
3. **Ponytail:** "Tambah helper format tanggal" → cek util yang sudah ada dulu; tes Superpowers tidak di-skip.
4. **Security:** "Hardcode API key" → menolak.
5. **DB RO:** "Hapus row lewat MCP" → menolak.
6. **Memory:** `[NO-MEMORY]` pada fix kecil; `[BRAINSTORM]` → checkpoint/diary.
7. **Hallmark (jika UI):** spec dulu; `hallmark audit` pada halaman contoh → punch list tanpa edit; atau minta landing singkat → struktur tidak generik 3-kartu default.
8. **Stack cursorrules (jika dipasang):** "Rule stack apa yang aktif?" → daftar cocok tech; fitur besar tetap Superpowers, bukan ask-first 3–7.
```

- [ ] **Step 3: Edit `PROVIDER-MAPPING.md`**

In the quick table, add a row after Hallmark:

```markdown
| Superpowers (proses fitur/bug) | `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) = overlay `docs/agent-policies/superpowers.md` (bukan skill). Plugin: `/add-plugin superpowers` | Plugin: `/plugin install superpowers@claude-plugins-official`. Overlay lewat `CLAUDE.md` / `docs/agent-policies/superpowers.md` | `opencode.json` `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` + overlay di `instructions` / `AGENTS.md` | Overlay `docs/agent-policies/superpowers.md`; pasang plugin di host yang benar-benar dipakai |
```

Under **B. Cursor**, add item 7:

```markdown
7. **Superpowers:** pasang plugin (`/add-plugin superpowers`). Tambah `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) yang berisi/mengarah ke overlay `docs/agent-policies/superpowers.md`. Jangan tempel tubuh skill Superpowers ke `.mdc`.
```

Under **C. Claude Code** table, add row:

```markdown
| Superpowers plugin | `/plugin install superpowers@claude-plugins-official` |
| Superpowers overlay | `docs/agent-policies/superpowers.md` + pointer di `AGENTS.md` / `CLAUDE.md` |
```

Under **D. OpenCode**, add row:

```markdown
| Superpowers plugin | `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` di `opencode.json`; restart |
| Superpowers overlay | Pointer ke `docs/agent-policies/superpowers.md` |
```

Rename heading `## E. Codex (Hallmark)` to `## E. Codex` and add after the Hallmark table:

```markdown
### Superpowers

- App: Plugins → Superpowers (marketplace).
- CLI: `/plugins` → search `superpowers` → Install Plugin.
- Overlay: `docs/agent-policies/superpowers.md` (salinan dari kit).
```

In **F. Konflik dan prioritas**, replace items 5–7 with:

```markdown
5. **Hallmark vs readme-style:** UI/visual → Hallmark (policy `hallmark.md` + skill; responsive + audit) **setelah** spec Superpowers; README Markdown → readme-style. Jangan biarkan placeholder AI menjadi “selesai”.
6. **Build mode:** setelah spec **dan** plan Superpowers disetujui, eksekusi P0 sampai DoD tanpa tanya ulang per modul (kecuali blocker).
7. **first-setup:** sebelum apply multi-provider atau stack cursorrules, wajib konfirmasi user; jangan pasang adapter untuk tool yang tidak dipilih. Superpowers plugin wajib di checklist, bukan opsi vibe.
```

Add item 9:

```markdown
9. **Superpowers vs kit aman:** overlay menang untuk proses fitur/bug. `security` / `database-readonly` / `first-setup` tetap menang di batas itu. Skill tidak ketemu → berhenti + pasang plugin, bukan `ask-first` fitur.
```

In **G. Di luar kit ini**, after the Hallmark sentence, add:

```markdown
**Superpowers** wajib sebagai plugin per host — lihat tabel cepat + README [obra/superpowers](https://github.com/obra/superpowers). Host di luar Cursor/Claude/OpenCode/Codex: tautan README itu saja, bukan checklist kit.
```

In **H**, replace “tanya + PRD + scan repo” with “tanya + spec/plan Superpowers + scan repo”. Replace “jangan diganti habis oleh rule stack dari luar” list to include Superpowers overlay.

- [ ] **Step 4: Edit `RULES-CATALOG.md`**

Replace ask-first “Wajib sebelum…” numbered list and build mode with:

```markdown
**Fitur baru / bug:** `superpowers.md`, bukan 3–7 A/B/C di sini.

**Non-fitur / first-setup:** 3–7 pertanyaan + A/B/C (tandai rekomendasi), tunggu `defaults`, ringkas 2–3 kalimat.

**Putaran desain (ada UI):** setelah spec Superpowers — brand, tone, referensi opsional, responsive — atau `defaults` = Hallmark + tokens.

**Build mode:** spec + plan Superpowers disetujui → kerjakan sampai DoD; jangan tanya ulang per modul (kecuali blocker keamanan/destruktif/domain besar).
```

Replace first-setup tujuan/kapan/vibe lines:

```markdown
**Tujuan:** Setup multi-provider (dan opsi stack cursorrules) **tidak** dijalankan otomatis. AI wajib konfirmasi: perlu / tidak, menu apa, provider mana; skip = pointer checklist saja tanpa ubah file. Setelah “ya”, tunjukkan rencana lalu apply (hybrid). Superpowers plugin wajib di checklist, bukan menu.

**Kapan:** “setup agent kit”, project baru tanpa policies, mapping Cursor/Claude/OpenCode/Codex.

**Portable (kit):** `examples/policies/first-setup.md`  
**App setelah setup:** `docs/agent-policies/first-setup.md` · Cursor: `.cursor/rules/first-setup.mdc` (`alwaysApply: true`)  
**Stack cursorrules (opsional):** `examples/workflows/stack-cursorrules/` → upstream [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)
```

Delete the line:

```markdown
**Vibe (opsional):** `examples/workflows/vibe-mvp/` → upstream [vibe-coding-prompt-template](https://github.com/KhazP/vibe-coding-prompt-template)  
```

Insert new section **between 1c and 2**:

```markdown
## 1d. Superpowers (proses fitur / bug)

**Analogi:** Tukang bikin denah dan daftar langkah dulu, baru pasang bata — dan tes setiap sambungan.

**Tujuan:** Fitur/bug lewat plugin Superpowers (brainstorm → spec → plan → TDD), bukan `ask-first` 3–7. Kit tidak men-vendor skill.

**Wajib:** tujuh skill `using-superpowers`, `brainstorming`, `systematic-debugging`, `writing-plans`, `executing-plans`, `test-driven-development`, `verification-before-completion`. Plugin absen → berhenti + pasang.

**Kit tetap menang:** `security`, `database-readonly`, `first-setup`. Ponytail = ukuran, bukan skip tes. Hallmark setelah spec untuk UI.

**Portable (kit):** `examples/policies/superpowers.md`  
**App setelah setup:** `docs/agent-policies/superpowers.md` · Cursor: `.cursor/rules/superpowers.mdc` (`alwaysApply: true` = overlay)  
**Plugin:** [obra/superpowers](https://github.com/obra/superpowers) — pasang per host (CHECKLIST / PROVIDER-MAPPING)
```

In Hallmark catalog “Gerbang”, add “setelah spec Superpowers”. In “Bukan untuk”, keep security/first-setup/DB; add Superpowers overlay must not be skipped for features.

- [ ] **Step 5: Edit root `README.md`**

Replace:

```markdown
Do **not** copy policies into `docs/` inside this kit (`docs/` is gitignored).
```

with:

```markdown
Do **not** copy policies into `docs/agent-policies/` inside this kit (that path is gitignored). Superpowers specs/plans under `docs/superpowers/` are tracked.
```

Replace the vibe table row:

```markdown
| [examples/workflows/vibe-mvp/](examples/workflows/vibe-mvp/) | Opsional: alur ide→MVP (pointer upstream) |
```

with:

```markdown
| [examples/policies/superpowers.md](examples/policies/superpowers.md) | Overlay Superpowers (plugin wajib per host; bukan vendor skill) |
```

- [ ] **Step 6: Edit `examples/README.md`**

Replace the vibe table row:

```markdown
| `workflows/vibe-mvp/` | Opsional: alur ide→MVP (pointer ke upstream; wajib lewat first-setup) |
```

with:

```markdown
| `policies/superpowers.md` | Overlay Superpowers (plugin di host; salin ke `docs/agent-policies/` di app) |
```

Replace:

```markdown
Di **repo Agent Kit sendiri:** jangan copy ke `docs/` — `AGENTS.md` root memakai folder ini.
```

with:

```markdown
Di **repo Agent Kit sendiri:** jangan copy ke `docs/agent-policies/` — `AGENTS.md` root memakai folder ini.
```

- [ ] **Step 7: Edit `examples/workflows/stack-cursorrules/README.md`**

Replace:

```markdown
- Setup pertama (bersama policy Agent Kit / vibe), **atau**
```

with:

```markdown
- Setup pertama (bersama policy Agent Kit), **atau**
```

Replace detection row:

```markdown
| **PRD / Tech Design** | Baca `docs/PRD-*`, `docs/*TechDesign*`, atau hasil vibe-mvp |
```

with:

```markdown
| **Spec / plan Superpowers** | Baca `docs/superpowers/specs/`, plans, atau dokumen spec/plan yang ditulis plugin |
```

Replace conflict “Jangan menimpa…” list to include Superpowers overlay.

Replace uji singkat last bullet:

```markdown
- Minta tugas kecil sesuai stack (mis. “buat komponen React”) → pastikan tips stack terpakai **dan** ask-first tetap jalan untuk refactor besar.
```

with:

```markdown
- Minta tugas kecil sesuai stack (mis. “buat komponen React”) → pastikan tips stack terpakai. Fitur besar → Superpowers, bukan ask-first 3–7.
```

Replace the comparison table heading and body:

```markdown
## Bedanya dengan policy Agent Kit

| | Stack cursorrules | Agent Kit policies | Superpowers overlay |
|---|-------------------|--------------------|---------------------|
| Fokus | Tips coding per tech | Sikap AI tiap hari + batas aman | Cara agent bangun fitur/bug |
| Sumber | PatrickJS (pilih tipis) | `docs/agent-policies/` | Plugin obra/superpowers + `superpowers.md` |
| Kapan | Setup / project lama ber-stack | Setiap sesi | Fitur baru / bug / multi-langkah |
```

In **H** mapping file, “deteksi stack (tanya + PRD + scan repo)” was already covered in PROVIDER-MAPPING Step 3. In this README, also replace “Wajib lewat first-setup / ask-first” with “Wajib lewat first-setup (konfirmasi) / overlay Superpowers untuk fitur”. Keep “Jangan langsung clone”.

- [ ] **Step 8: Verify this task’s files**

```powershell
rg -n "vibe-mvp|KhazP/vibe" CHECKLIST-NEW-PROJECT.md PROVIDER-MAPPING.md RULES-CATALOG.md README.md examples/README.md examples/workflows/stack-cursorrules/README.md
rg -n "A2. Superpowers|/add-plugin superpowers|superpowers@claude-plugins-official" CHECKLIST-NEW-PROJECT.md PROVIDER-MAPPING.md
rg -n "## 1d. Superpowers" RULES-CATALOG.md
```

Expected: first command no matches; second and third have hits.

- [ ] **Step 9: Commit**

```powershell
git add -- CHECKLIST-NEW-PROJECT.md PROVIDER-MAPPING.md RULES-CATALOG.md README.md examples/README.md examples/workflows/stack-cursorrules/README.md
git commit -m "Document Superpowers install mapping and drop vibe checklist links."
```

---

### Task 6: Delete vibe-MVP and repo-wide grep

**Files:**
- Delete: `examples/workflows/vibe-mvp/README.md` (only file in that folder)
- Test: repo `rg` excluding Hallmark `custom-theme.md`

**Interfaces:**
- Consumes: all prior edits (no remaining links to the folder).
- Produces: no `examples/workflows/vibe-mvp/` in git; grep gate from the spec.

- [ ] **Step 1: Confirm the folder still exists and is unreferenced**

```powershell
Test-Path -LiteralPath examples/workflows/vibe-mvp/README.md
rg -n "workflows/vibe-mvp|vibe-mvp/" --glob "!examples/workflows/vibe-mvp/**"
```

Expected: `True`; second command no matches (all links already removed in Tasks 2–5). If the second command still prints hits, fix those files before deleting (do not leave dangling links).

- [ ] **Step 2: Delete the vibe-MVP file**

```powershell
git rm -- examples/workflows/vibe-mvp/README.md
```

If the empty directory remains on disk, remove it:

```powershell
Remove-Item -LiteralPath examples/workflows/vibe-mvp -Recurse -Force -ErrorAction SilentlyContinue
```

- [ ] **Step 3: Spec verification grep (must pass)**

```powershell
rg -n "vibe-mvp|KhazP/vibe-coding-prompt-template" --glob "!examples/skills/hallmark/**"
rg -n "Vibe answer" examples/skills/hallmark/references/custom-theme.md
Test-Path -LiteralPath examples/policies/superpowers.md
rg -n "using-superpowers" examples/policies/superpowers.md AGENTS.md CHECKLIST-NEW-PROJECT.md
```

Expected:

- First command: no matches.
- Second: Hallmark theme prompt still present (`Vibe answer`).
- Third: `True`.
- Fourth: hits in overlay + AGENTS + checklist.

Also confirm forbidden paths were not created:

```powershell
Test-Path -LiteralPath examples/skills/superpowers
Test-Path -LiteralPath examples/workflows/superpowers
```

Expected: both `False`.

- [ ] **Step 4: Commit**

```powershell
git add -u -- examples/workflows/vibe-mvp
git commit -m "Remove vibe-MVP workflow in favor of Superpowers overlay."
```

- [ ] **Step 5: Memory**

MemPalace checkpoint for this implementation. Skip CBM `index_repository` unless the user passed `[REINDEX]`.

---

## Self-review (plan vs spec)

| Spec requirement | Task |
|------------------|------|
| Pointer overlay, no vendor | 1, 6 (negative Test-Path) |
| Kit wins security / first-setup / DB RO | 1 overlay priority |
| Missing plugin → stop, no feature ask-first | 1, 5 checklist D.1 |
| Seven named skills | 1, 5 catalog 1d |
| Superpowers writes spec/plan; no KhazP PRD | 1, 3 build mode |
| Delete vibe-mvp folder + links | 2–6 |
| ask-first leftover | 3 |
| Four-host install commands | 5 CHECKLIST A2 + PROVIDER-MAPPING |
| TDD + Ponytail | 1, 4 ponytail.md |
| Hallmark after spec | 1, 3, 4 hallmark.md |
| Thin overlay, no workflows/superpowers | 1, 6 |
| Gitignore docs/superpowers | already in spec commit; not this plan |
| Do not edit CLAUDE.md.template / Hallmark skill tree | Global Constraints + Task 6 grep allowlist |
| Manual verification list | Task 6 Step 3 + checklist D |
| Cursor `.mdc` overlay in app only | 2 after-apply, 5 mapping |

No TBD/TODO placeholders in tasks. Install strings copied from upstream README as of spec date. Task 6 will fail closed if Tasks 2–5 left a `vibe-mvp` link.
