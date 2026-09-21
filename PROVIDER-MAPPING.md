# Mapping provider ? file ditaruh di mana

Satu kebijakan, banyak pintu.

**Repo Agent Kit:** sumber = `examples/policies/`. Jangan mapping ke `docs/` atau `.cursor/` di dalam kit.

**Repo aplikasi:** salin isi dari `examples/policies/`, lalu mapping ke lokasi di bawah.

## Tabel cepat

| Kebijakan | Cursor | Claude Code | OpenCode | Generic / lain |
|-----------|--------|-------------|----------|----------------|
| Inti + Ponytail | `.cursor/rules/ponytail.mdc` + `AGENTS.md` | `AGENTS.md` (dan/atau import ke `CLAUDE.md`) | `AGENTS.md` root (+ global `~/.config/opencode/AGENTS.md`) | `AGENTS.md` |
| Setup pertama (`first-setup`) — konfirmasi sebelum pasang | `.cursor/rules/first-setup.mdc` (`alwaysApply: true`) | Lewat `CLAUDE.md` / `docs/agent-policies/first-setup.md` | `instructions` + `AGENTS.md` pointer | `docs/agent-policies/first-setup.md` |
| Tanya dulu, caveman, security, memory, DB RO | `.cursor/rules/<nama>.mdc` (`alwaysApply: true`) | Sertakan lewat `CLAUDE.md` "Also follow …" atau file di `.claude/` | `opencode.json` → `instructions: ["docs/agent-policies/*.md"]` | Gabungkan ringkas ke `AGENTS.md` atau folder `docs/agent-policies/` |
| Perintah build dan arsitektur | Opsional di rule terpisah | **`CLAUDE.md`** | Boleh di `AGENTS.md` atau `instructions` | `AGENTS.md` / `CLAUDE.md` |
| Hallmark (UI anti-slop) | `.cursor/rules/hallmark.mdc` (`alwaysApply: true`) + skill di `examples/skills/hallmark/` (atau path salinan project) | `~/.claude/skills/hallmark/` atau skills project; pointer di `CLAUDE.md` | Pointer di `AGENTS.md` / `instructions` ke `skills/hallmark/SKILL.md` | Salin `SKILL.md` + `references/`; atau `npx skills add nutlope/hallmark` |
| Superpowers (proses fitur/bug) | `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) = overlay `docs/agent-policies/superpowers.md` (bukan skill). Plugin: `/add-plugin superpowers` | Plugin: `/plugin install superpowers@claude-plugins-official`. Overlay lewat `CLAUDE.md` / `docs/agent-policies/superpowers.md` | `opencode.json` `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` + overlay di `instructions` / `AGENTS.md` | Overlay `docs/agent-policies/superpowers.md`; pasang plugin di host yang benar-benar dipakai |
| Figma (sumber visual UI) | Overlay `.cursor/rules/figma.mdc` (`alwaysApply: true`) → `docs/agent-policies/figma.md` (bukan skill). Plugin: `/add-plugin figma` | Plugin: `claude plugin install figma@claude-plugins-official`. Overlay `docs/agent-policies/figma.md` | Overlay di `instructions` / `AGENTS.md`. Remote MCP **bukan** katalog — UI berhenti | Overlay `docs/agent-policies/figma.md`; Codex: plugin atau `codex mcp add figma --url https://mcp.figma.com/mcp` |
| MCP MemPalace / CBM / Claude Mem / RTK / Postgres | User: `~/.cursor/mcp.json` (Claude Mem: `npx claude-mem install` + `claude-mem cursor install user`) | Project: `.mcp.json` atau `claude mcp add`; Claude Mem: `npx` atau `/plugin marketplace add thedotmack/claude-mem` + `/plugin install claude-mem` | `npx claude-mem install --ide opencode` | Codex CLI: pilih di installer; host lain: README [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) |

## A. Generic (paling portable)

```text
project/
  AGENTS.md
  CLAUDE.md
  docs/agent-policies/     # salinan dari examples/policies/
  .mcp.json                # opsional, tanpa secret
```

Di `AGENTS.md`, arahkan ke `docs/agent-policies/*.md`.

## B. Cursor

1. Salin tiap policy ke `.cursor/rules/<nama>.mdc` dengan frontmatter `alwaysApply: true`.
2. Simpan juga `AGENTS.md` di root.
3. MCP: biasanya `%USERPROFILE%\.cursor\mcp.json`.
4. Buka **Agent chat baru** setelah menambah rule.
5. Edit memory-refresh: ganti path/scope agar cocok project baru.
6. **Hallmark:** salin `examples/skills/hallmark/` ke project (atau pakai path kit), tambah `.cursor/rules/hallmark.mdc` yang mengarah ke `SKILL.md` itu. Detail: [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md).
7. **Superpowers:** pasang plugin (`/add-plugin superpowers`). Tambah `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) yang berisi/mengarah ke overlay `docs/agent-policies/superpowers.md`. Jangan tempel tubuh skill Superpowers ke `.mdc`.
8. **Claude Mem:** `npx claude-mem install`; pilih Cursor; `claude-mem cursor install user`; skip CMEM Pro dengan `--provider claude`. MCP di `%USERPROFILE%\.cursor\mcp.json`. Jangan tulis `.cursor/hooks` ke repo Agent Kit.
9. **Figma:** `/add-plugin figma`. Tambah `.cursor/rules/figma.mdc` (`alwaysApply: true`) yang mengarah ke overlay `docs/agent-policies/figma.md`. Jangan tempel tubuh skill Figma ke `.mdc`. Jangan tulis `.cursor/` ke repo Agent Kit.

## C. Claude Code

| Kebutuhan | Lokasi |
|-----------|--------|
| Instruksi project | `./CLAUDE.md` atau `./.claude/CLAUDE.md` |
| Preferensi pribadi | `~/.claude/CLAUDE.md` |
| Hallmark skill | `~/.claude/skills/hallmark/` (salin dari `examples/skills/hallmark/`) atau `npx skills add nutlope/hallmark` |
| Superpowers plugin | `/plugin install superpowers@claude-plugins-official` |
| Superpowers overlay | `docs/agent-policies/superpowers.md` + pointer di `AGENTS.md` / `CLAUDE.md` |
| Figma plugin | `claude plugin install figma@claude-plugins-official` |
| Figma overlay | `docs/agent-policies/figma.md` + pointer di `AGENTS.md` / `CLAUDE.md` |
| Figma MCP manual | `claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp` |
| Claude Mem | `npx claude-mem install` atau `/plugin marketplace add thedotmack/claude-mem` lalu `/plugin install claude-mem` |
| MCP team-shared | `.mcp.json` di root (commit) |
| MCP pribadi | `claude mcp add --scope user` |

Praktik: `CLAUDE.md` = build + arsitektur + "baca AGENTS.md + docs/agent-policies/*". Uji dengan `/context`.

Docs: https://code.claude.com/docs/en/claude-md · https://code.claude.com/docs/en/mcp

## D. OpenCode

| Kebutuhan | Lokasi |
|-----------|--------|
| Rules project | `AGENTS.md` (prioritas); fallback `CLAUDE.md` |
| Rules global | `~/.config/opencode/AGENTS.md` |
| Banyak file policy | `opencode.json` → `instructions` |
| Hallmark | Pointer ke `skills/hallmark/SKILL.md` (jangan wajib-load tiap chat jika file besar — load saat kerja UI) |
| Superpowers plugin | `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` di `opencode.json`; restart |
| Superpowers overlay | Pointer ke `docs/agent-policies/superpowers.md` |
| Figma overlay | Pointer ke `docs/agent-policies/figma.md` |
| Figma MCP | Remote **bukan** katalog Figma. Kerja UI: berhenti. Jangan PAT. Desktop MCP bukan generate |
| Claude Mem | `npx claude-mem install --ide opencode` |

Contoh: lihat `examples/opencode.json.example`. Init: `/init`.

Docs: https://opencode.ai/docs/rules

## E. Codex

| Scope | Lokasi |
|-------|--------|
| Personal | `~/.codex/skills/hallmark/` |
| Project | `.codex/skills/hallmark/` |

Salin `SKILL.md` + `references/` dari `examples/skills/hallmark/`, atau `npx skills add nutlope/hallmark`.

### Superpowers

- App: Plugins → Superpowers (marketplace).
- CLI: `/plugins` → search `superpowers` → Install Plugin.
- Overlay: `docs/agent-policies/superpowers.md` (salinan dari kit).

### Claude Mem

- CLI: `npx claude-mem install` dan pilih Codex CLI di installer. Jangan invent `--ide`.
- Skip CMEM Pro: `--provider claude`.

### Figma

- App: plugin Figma (OAuth).
- CLI: `codex mcp add figma --url https://mcp.figma.com/mcp`.
- Overlay: `docs/agent-policies/figma.md`.

## F. Konflik dan prioritas

1. Satu sumber di app: `docs/agent-policies/`; file lain hanya merujuk. Di repo kit: `examples/policies/`.
2. OpenCode: jika `AGENTS.md` dan `CLAUDE.md` ada, biasanya AGENTS yang dipakai — pastikan lengkap.
3. Saat putaran klarifikasi (ask-first / first-setup), prioritaskan kejelasan (normal mode); caveman boleh kembali setelah arah jelas.
4. Database read-only = MCP/ad-hoc. Migrasi SQL = jalur manusia/CI.
5. **Figma vs Hallmark vs readme-style:** UI/visual di app → overlay `figma.md` (sumber) lalu Hallmark (kualitas + audit) **setelah** spec Superpowers. README Markdown → readme-style. MCP Figma absen → berhenti, bukan placeholder AI / catalog Hallmark.
6. **Build mode:** setelah spec **dan** plan Superpowers disetujui, eksekusi P0 sampai DoD tanpa tanya ulang per modul (kecuali blocker).
7. **first-setup:** sebelum apply multi-provider atau stack cursorrules, wajib konfirmasi user; jangan pasang adapter untuk tool yang tidak dipilih. Superpowers plugin wajib di checklist, bukan opsi vibe. Figma plugin/MCP wajib di checklist untuk UI, bukan opsi vibe.
8. **Stack cursorrules vs kit inti:** tips tech dari awesome-cursorrules **tidak** boleh menimpa Superpowers overlay / ask-first / security / ponytail / database-readonly / first-setup / memory-refresh / figma. Prefer `alwaysApply: false` + globs sempit. Alur: [examples/workflows/stack-cursorrules/](examples/workflows/stack-cursorrules/).
9. **Superpowers vs kit aman:** overlay menang untuk proses fitur/bug. `security` / `database-readonly` / `first-setup` tetap menang di batas itu. Skill tidak ketemu → berhenti + pasang plugin, bukan `ask-first` fitur.
10. **Figma vs kit aman:** overlay Figma menang untuk sumber visual. `security` tetap menang (tidak ada token di git). Superpowers tetap menang untuk spec fitur. Skill Figma tidak ketemu / MCP absen → berhenti + pasang, bukan generate UI Hallmark.

## G. Di luar kit ini

Flutter/Dart MCP, Playwright *browser* MCP, skills Cursor di `.cursor/skills/` — boleh ditambah nanti; bukan checklist inti. **RTK** (`rtk-mcp`) termasuk inti disarankan — lihat [MCP-SETUP.md](MCP-SETUP.md) §5. **Claude Mem** termasuk inti disarankan — lihat [MCP-SETUP.md](MCP-SETUP.md) §3; upstream [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem). **Figma** termasuk overlay + MCP wajib untuk UI — lihat [MCP-SETUP.md](MCP-SETUP.md) §6 dan `examples/policies/figma.md`. **Hallmark** sudah di kit sebagai skill opsional-kuat untuk UI — lihat [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md). **Superpowers** wajib sebagai plugin per host — lihat tabel cepat + README [obra/superpowers](https://github.com/obra/superpowers). Host di luar Cursor/Claude/OpenCode/Codex: tautan README upstream saja, bukan checklist kit.

## H. Rule dari awesome-cursorrules (otomatis + manual)

Koleksi contoh rule Cursor:

https://github.com/PatrickJS/awesome-cursorrules

**Alur otomatis (disarankan):** ikuti [examples/workflows/stack-cursorrules/](examples/workflows/stack-cursorrules/) — deteksi stack (tanya + spec/plan Superpowers + scan repo) → usulkan maks **5–7** rule dari kategori yang relevan → konfirmasi user → pasang **dual** (`.mdc` Cursor + `.md` portable). Jangan vendor seluruh repo ke project.

Isinya banyak file `.mdc` (kadang `.cursorrules`). Format itu dibuat untuk Cursor. Claude Code, OpenCode, dan agent lain biasanya cukup baca **teks Markdown biasa** (`.md`) — tanpa “kepala” khusus Cursor.

Analogi: resep di buku masak Cursor punya stiker di pojok (“pakai di oven model X”). Stikernya dibuang dulu; isi resepnya yang dibawa ke dapur lain.

### Prinsip (jangan dilewati)

1. **Pilih yang relevan saja** — jangan salin ratusan rule. Ambil hingga **5–7** yang cocok stack (lebih sedikit lebih baik).
2. **Kit inti tetap menang** — Superpowers overlay, tanya-dulu, ponytail, security, memory, DB read-only, first-setup dari Agent Kit jangan diganti habis oleh rule stack dari luar.
3. **Sesuaikan stack** — rule Next.js tidak otomatis cocok untuk Flutter/Postgres; edit sebelum dipakai.
4. **Sumber resmi = link GitHub di atas** — clone/fetch saat perlu. Kit ini tidak bergantung folder lokal tertentu dan **tidak** menyimpan salinan penuh upstream.

### Langkah manual (5 menit per rule) — fallback

1. Buka repo PatrickJS; di folder `rules/` pilih satu file `.mdc` yang cocok.
2. Salin isinya ke editor.
3. **Untuk portable `.md`:** hapus kepala Cursor di awal file (blok di antara `---` … `---`), misalnya:

```text
---
description: "..."
globs: **/*
alwaysApply: false
---
```

Semua baris itu hanya untuk Cursor. Hapus sampai baris setelah `---` kedua.

4. Simpan sisa teks sebagai file `.md`, contoh: `docs/agent-policies/stack-nextjs.md`.
5. **Untuk Cursor:** simpan `.mdc` di `.cursor/rules/` dengan `alwaysApply: false` + `globs` sempit bila memungkinkan.
6. **Mapping ke tool:**

| Tool | Apa yang dilakukan |
|------|--------------------|
| Generic / Claude / OpenCode | File `.md` di `docs/agent-policies/`; rujuk dari `AGENTS.md` atau `opencode.json` → `instructions` |
| Cursor | `.mdc` + frontmatter di `.cursor/rules/`, **dan** salinan `.md` portable (dual format) |

7. Uji singkat: minta agent “ringkas rule aktif untuk stack X” — pastikan rule baru muncul dan tidak bentrok dengan ask-first / security.

### Jangan lakukan

- Commit seluruh isi awesome-cursorrules ke project Anda.
- Menimpa `ask-first` / `ponytail` / `security` / `first-setup` hanya karena rule luar lebih panjang.
- Menyimpan secret atau path mesin pribadi di dalam file rule.
- `alwaysApply: true` pada rule luar tanpa permintaan eksplisit user.