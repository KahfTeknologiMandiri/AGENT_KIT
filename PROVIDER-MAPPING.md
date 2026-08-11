# Mapping provider ? file ditaruh di mana

Satu kebijakan, banyak pintu. Salin isi dari `examples/policies/`, lalu mapping ke lokasi di bawah.

## Tabel cepat

| Kebijakan | Cursor | Claude Code | OpenCode | Generic / lain |
|-----------|--------|-------------|----------|----------------|
| Inti + Ponytail | `.cursor/rules/ponytail.mdc` + `AGENTS.md` | `AGENTS.md` (dan/atau import ke `CLAUDE.md`) | `AGENTS.md` root (+ global `~/.config/opencode/AGENTS.md`) | `AGENTS.md` |
| Setup pertama (`first-setup`) — konfirmasi sebelum pasang | `.cursor/rules/first-setup.mdc` (`alwaysApply: true`) | Lewat `CLAUDE.md` / `docs/agent-policies/first-setup.md` | `instructions` + `AGENTS.md` pointer | `docs/agent-policies/first-setup.md` |
| Tanya dulu, caveman, security, memory, DB RO | `.cursor/rules/<nama>.mdc` (`alwaysApply: true`) | Sertakan lewat `CLAUDE.md` "Also follow …" atau file di `.claude/` | `opencode.json` → `instructions: ["docs/agent-policies/*.md"]` | Gabungkan ringkas ke `AGENTS.md` atau folder `docs/agent-policies/` |
| Perintah build dan arsitektur | Opsional di rule terpisah | **`CLAUDE.md`** | Boleh di `AGENTS.md` atau `instructions` | `AGENTS.md` / `CLAUDE.md` |
| Hallmark (UI anti-slop) | `.cursor/rules/hallmark.mdc` (`alwaysApply: true`) + skill di `examples/skills/hallmark/` (atau path salinan project) | `~/.claude/skills/hallmark/` atau skills project; pointer di `CLAUDE.md` | Pointer di `AGENTS.md` / `instructions` ke `skills/hallmark/SKILL.md` | Salin `SKILL.md` + `references/`; atau `npx skills add nutlope/hallmark` |
| MCP MemPalace / CBM / RTK / Postgres | User: `~/.cursor/mcp.json` | Project: `.mcp.json` atau `claude mcp add` | Config MCP OpenCode | Sesuai host |

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

## C. Claude Code

| Kebutuhan | Lokasi |
|-----------|--------|
| Instruksi project | `./CLAUDE.md` atau `./.claude/CLAUDE.md` |
| Preferensi pribadi | `~/.claude/CLAUDE.md` |
| Hallmark skill | `~/.claude/skills/hallmark/` (salin dari `examples/skills/hallmark/`) atau `npx skills add nutlope/hallmark` |
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

Contoh: lihat `examples/opencode.json.example`. Init: `/init`.

Docs: https://opencode.ai/docs/rules

## E. Codex (Hallmark)

| Scope | Lokasi |
|-------|--------|
| Personal | `~/.codex/skills/hallmark/` |
| Project | `.codex/skills/hallmark/` |

Salin `SKILL.md` + `references/` dari `examples/skills/hallmark/`, atau `npx skills add nutlope/hallmark`.

## F. Konflik dan prioritas

1. Satu sumber di `docs/agent-policies/`; file lain hanya merujuk.
2. OpenCode: jika `AGENTS.md` dan `CLAUDE.md` ada, biasanya AGENTS yang dipakai — pastikan lengkap.
3. Saat putaran klarifikasi (ask-first / first-setup), prioritaskan kejelasan (normal mode); caveman boleh kembali setelah arah jelas.
4. Database read-only = MCP/ad-hoc. Migrasi SQL = jalur manusia/CI.
5. **Hallmark vs readme-style:** UI/visual → Hallmark; README Markdown → readme-style. Jangan biarkan Hallmark menimpa ask-first/security/first-setup.
6. **first-setup:** sebelum apply multi-provider, vibe, atau stack cursorrules, wajib konfirmasi user; jangan pasang adapter untuk tool yang tidak dipilih.
7. **Stack cursorrules vs kit inti:** tips tech dari awesome-cursorrules **tidak** boleh menimpa ask-first / security / ponytail / database-readonly / first-setup / memory-refresh. Prefer `alwaysApply: false` + globs sempit. Alur: [examples/workflows/stack-cursorrules/](examples/workflows/stack-cursorrules/).

## G. Di luar kit ini

Flutter/Dart MCP, Playwright *browser* MCP, skills Cursor di `.cursor/skills/` — boleh ditambah nanti; bukan checklist inti. **RTK** (`rtk-mcp`) termasuk inti disarankan — lihat [MCP-SETUP.md](MCP-SETUP.md) §4. **Hallmark** sudah di kit sebagai skill opsional-kuat untuk UI — lihat [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md).

## H. Rule dari awesome-cursorrules (otomatis + manual)

Koleksi contoh rule Cursor:

https://github.com/PatrickJS/awesome-cursorrules

**Alur otomatis (disarankan):** ikuti [examples/workflows/stack-cursorrules/](examples/workflows/stack-cursorrules/) — deteksi stack (tanya + PRD + scan repo) → usulkan maks **5–7** rule dari kategori yang relevan → konfirmasi user → pasang **dual** (`.mdc` Cursor + `.md` portable). Jangan vendor seluruh repo ke project.

Isinya banyak file `.mdc` (kadang `.cursorrules`). Format itu dibuat untuk Cursor. Claude Code, OpenCode, dan agent lain biasanya cukup baca **teks Markdown biasa** (`.md`) — tanpa “kepala” khusus Cursor.

Analogi: resep di buku masak Cursor punya stiker di pojok (“pakai di oven model X”). Stikernya dibuang dulu; isi resepnya yang dibawa ke dapur lain.

### Prinsip (jangan dilewati)

1. **Pilih yang relevan saja** — jangan salin ratusan rule. Ambil hingga **5–7** yang cocok stack (lebih sedikit lebih baik).
2. **Kit inti tetap menang** — tanya-dulu, ponytail, security, memory, DB read-only, first-setup dari Agent Kit jangan diganti habis oleh rule stack dari luar.
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