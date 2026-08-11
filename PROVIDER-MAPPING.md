# Mapping provider ? file ditaruh di mana

Satu kebijakan, banyak pintu. Salin isi dari `examples/policies/`, lalu mapping ke lokasi di bawah.

## Tabel cepat

| Kebijakan | Cursor | Claude Code | OpenCode | Generic / lain |
|-----------|--------|-------------|----------|----------------|
| Inti + Ponytail | `.cursor/rules/ponytail.mdc` + `AGENTS.md` | `AGENTS.md` (dan/atau import ke `CLAUDE.md`) | `AGENTS.md` root (+ global `~/.config/opencode/AGENTS.md`) | `AGENTS.md` |
| Tanya dulu, caveman, security, memory, DB RO | `.cursor/rules/<nama>.mdc` (`alwaysApply: true`) | Sertakan lewat `CLAUDE.md` "Also follow ?" atau file di `.claude/` | `opencode.json` ? `instructions: ["docs/agent-policies/*.md"]` | Gabungkan ringkas ke `AGENTS.md` atau folder `docs/agent-policies/` |
| Perintah build dan arsitektur | Opsional di rule terpisah | **`CLAUDE.md`** | Boleh di `AGENTS.md` atau `instructions` | `AGENTS.md` / `CLAUDE.md` |
| MCP MemPalace / CBM / Postgres | User: `~/.cursor/mcp.json` | Project: `.mcp.json` atau `claude mcp add` | Config MCP OpenCode | Sesuai host |

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

## C. Claude Code

| Kebutuhan | Lokasi |
|-----------|--------|
| Instruksi project | `./CLAUDE.md` atau `./.claude/CLAUDE.md` |
| Preferensi pribadi | `~/.claude/CLAUDE.md` |
| MCP team-shared | `.mcp.json` di root (commit) |
| MCP pribadi | `claude mcp add --scope user` |

Praktik: `CLAUDE.md` = build + arsitektur + "baca AGENTS.md + docs/agent-policies/*". Uji dengan `/context`.

Docs: https://code.claude.com/docs/en/claude-md ? https://code.claude.com/docs/en/mcp

## D. OpenCode

| Kebutuhan | Lokasi |
|-----------|--------|
| Rules project | `AGENTS.md` (prioritas); fallback `CLAUDE.md` |
| Rules global | `~/.config/opencode/AGENTS.md` |
| Banyak file policy | `opencode.json` ? `instructions` |

Contoh: lihat `examples/opencode.json.example`. Init: `/init`.

Docs: https://opencode.ai/docs/rules

## E. Konflik dan prioritas

1. Satu sumber di `docs/agent-policies/`; file lain hanya merujuk.
2. OpenCode: jika `AGENTS.md` dan `CLAUDE.md` ada, biasanya AGENTS yang dipakai ? pastikan lengkap.
3. Saat putaran klarifikasi (ask-first), prioritaskan kejelasan (normal mode); caveman boleh kembali setelah arah jelas.
4. Database read-only = MCP/ad-hoc. Migrasi SQL = jalur manusia/CI.

## F. Di luar kit ini

Flutter/Dart MCP, Playwright, skills Cursor di `.cursor/skills/` — boleh ditambah nanti; bukan checklist inti.

## G. Rule dari awesome-cursorrules (Cursor ke platform lain)

Koleksi contoh rule Cursor ada di repositori komunitas:

https://github.com/PatrickJS/awesome-cursorrules

Isinya banyak file `.mdc` (kadang `.cursorrules`). Format itu dibuat untuk Cursor. Claude Code, OpenCode, dan agent lain biasanya cukup baca **teks Markdown biasa** (`.md`) — tanpa “kepala” khusus Cursor.

Analogi: resep di buku masak Cursor punya stiker di pojok (“pakai di oven model X”). Stikernya dibuang dulu; isi resepnya yang dibawa ke dapur lain.

### Prinsip (jangan dilewati)

1. **Pilih yang relevan saja** — jangan salin ratusan rule. Ambil 1–3 yang cocok stack project Anda.
2. **Kit inti tetap menang** — tanya-dulu, ponytail, security, memory, DB read-only dari Agent Kit jangan diganti habis oleh rule stack dari luar.
3. **Sesuaikan stack** — rule Next.js tidak otomatis cocok untuk Flutter/Postgres; edit sebelum dipakai.
4. **Sumber resmi = link GitHub di atas** — clone di mana saja. Kit ini tidak bergantung folder lokal tertentu.

### Langkah manual (5 menit per rule)

1. Buka repo PatrickJS; di folder `rules/` pilih satu file `.mdc` yang cocok.
2. Salin isinya ke editor.
3. **Hapus kepala Cursor** di awal file (blok di antara `---` … `---`), misalnya:

```text
---
description: "..."
globs: **/*
alwaysApply: false
---
```

Semua baris itu hanya untuk Cursor. Hapus sampai baris setelah `---` kedua.

4. Simpan sisa teks sebagai file `.md`, contoh: `docs/agent-policies/stack-nextjs.md`.
5. **Mapping ke tool** (sama seperti tabel di atas):

| Tool | Apa yang dilakukan |
|------|--------------------|
| Generic / Claude / OpenCode | File `.md` di `docs/agent-policies/`; rujuk dari `AGENTS.md` atau `opencode.json` → `instructions` |
| Cursor | Boleh tetap `.mdc` + frontmatter di `.cursor/rules/`, **atau** pakai `.md` yang sama lewat `AGENTS.md` |

6. Uji singkat: minta agent “ringkas rule aktif untuk stack X” — pastikan rule baru muncul dan tidak bentrok dengan ask-first / security.

### Jangan lakukan

- Commit seluruh isi awesome-cursorrules ke project Anda.
- Menimpa `ask-first` / `ponytail` / `security` hanya karena rule luar lebih panjang.
- Menyimpan secret atau path mesin pribadi di dalam file rule.
