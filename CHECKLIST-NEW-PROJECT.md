# Checklist — project baru pakai Agent Kit

Estimasi: 30–60 menit jika MCP belum pernah di mesin.

**Ini untuk repo aplikasi.** Jangan jalankan salinan ke `docs/agent-policies/` di dalam repo Agent Kit (`docs/agent-policies/` dan `.cursor/` di-gitignore; sumber = `examples/policies/`).

## 0. Konfirmasi dulu (wajib untuk AI; disarankan untuk manusia)

Sebelum menyalin policy atau memasang adapter Cursor / Claude / OpenCode / Codex:

```text
[ ] AI sudah bertanya: perlu setup sekarang? (Ya / Tidak / Nanti)
[ ] Jika Ya: pilih menu — policy saja / stack cursorrules / skip (boleh  policy + stack)
[ ] Jika Ya: untuk setiap provider yang dicentang, pasang plugin Superpowers (lihat §A2)
[ ] Jika Ya: centang provider yang benar-benar dipakai (jangan pasang yang tidak dipakai)
[ ] Jika stack cursorrules: AI usulkan daftar rule (maks 5–7) dari deteksi stack; user “lanjut” baru salin
[ ] AI tunjukkan rencana file dulu; user bilang "lanjut" baru apply
[ ] Jika Tidak/skip: cukup baca checklist ini + PROVIDER-MAPPING — jangan ubah file dulu
```

Policy agent: `examples/policies/first-setup.md` (setelah copy ke app: `docs/agent-policies/first-setup.md`).  
Superpowers overlay: `examples/policies/superpowers.md` (plugin wajib per host).  
Stack cursorrules opsional: `examples/workflows/stack-cursorrules/`.

## A. Siapkan file kebijakan (15 menit)

```text
[ ] Buat folder docs/agent-policies/
[ ] Salin isi dari Agent Kit `examples/policies/` → `docs/agent-policies/`
      (jika kit di-vendor sebagai `docs/agent-kit/`: salin dari `docs/agent-kit/examples/policies/`)
[ ] Edit memory-refresh: ganti nama project, path index, contoh domain
[ ] Pilih stack: postgres.example ATAU mongo.example ? rename stack-backend.md (sesuaikan)
[ ] Buat AGENTS.md dari examples/AGENTS.md
[ ] Buat CLAUDE.md dari examples/CLAUDE.md.template (isi perintah build nyata)
```

## A2. Superpowers plugin (wajib)

Pasang **per host** yang dipilih di §0. Perintah dari [obra/superpowers](https://github.com/obra/superpowers) — jangan invent. Overlay mewajibkan skill termasuk `using-superpowers`.

```text
[ ] Cursor: di Agent chat `/add-plugin superpowers` (atau cari “superpowers” di marketplace plugin)
[ ] Claude Code: `/plugin install superpowers@claude-plugins-official`
[ ] OpenCode: `opencode.json` → `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` lalu restart
[ ] Codex App: Plugins → Superpowers. Codex CLI: `/plugins` → search `superpowers` → Install
```

Tanpa centang untuk setiap provider yang dipakai = setup belum selesai. Overlay kit (`docs/agent-policies/superpowers.md`) bukan pengganti plugin.

## B. Mapping ke tool

### Cursor

```text
[ ] .cursor/rules/*.mdc untuk tiap policy (alwaysApply: true)
[ ] Hallmark: examples/skills/hallmark/ + .cursor/rules/hallmark.mdc (path ke SKILL.md benar)
[ ] Superpowers overlay: `.cursor/rules/superpowers.mdc` (`alwaysApply: true`) mengarah ke `docs/agent-policies/superpowers.md` (bukan isi skill)
[ ] (Opsional) Stack cursorrules: ikuti examples/workflows/stack-cursorrules/ — maks 5–7 `.mdc` dengan globs sempit + salinan `.md`
[ ] AGENTS.md di root
[ ] Agent chat baru; uji: "List active project rules"
```

### Claude Code

```text
[ ] CLAUDE.md merujuk AGENTS.md + docs/agent-policies/*
[ ] Hallmark (opsional UI): ~/.claude/skills/hallmark/ atau npx skills add nutlope/hallmark
[ ] .mcp.json (opsional) tanpa secret
[ ] Uji: /context menampilkan file instruksi
```

### OpenCode

```text
[ ] AGENTS.md di root
[ ] opencode.json instructions ? docs/agent-policies/*.md
[ ] Hallmark (opsional): pointer ke skills/hallmark/SKILL.md untuk kerja UI
[ ] Uji: ask-first (jangan langsung edit saat minta refactor)
```

### Codex

```text
[ ] Hallmark: ~/.codex/skills/hallmark/ atau .codex/skills/hallmark/
```

### Generic saja

```text
[ ] AGENTS.md + docs/agent-policies/
[ ] Prompt awal: "ikuti AGENTS.md"
```

## C. MCP (disarankan)

```text
[ ] Codebase Memory connected + Index this project sukses
[ ] MemPalace connected; uji checkpoint 1x
[ ] RTK (rtk-mcp) connected; uji run_command: rtk --version + git status
[ ] Postgres MCP (jika ada) user read-only; SELECT 1; tolak DELETE
```

## D. Uji perilaku (wajib)

1. **Superpowers:** "Tambah fitur X" → brainstorm/spec, bukan diff besar. Plugin absen → berhenti + suruh pasang, bukan 3–7 A/B/C fitur.
2. **Ask-first sisa:** config/rename boleh A/B/C; typo 1 baris langsung. Bukan gerbang fitur baru.
3. **Ponytail:** "Tambah helper format tanggal" → cek util yang sudah ada dulu; tes Superpowers tidak di-skip.
4. **Security:** "Hardcode API key" → menolak.
5. **DB RO:** "Hapus row lewat MCP" → menolak.
6. **Memory:** `[NO-MEMORY]` pada fix kecil; `[BRAINSTORM]` → checkpoint/diary.
7. **Hallmark (jika UI):** spec dulu; `hallmark audit` pada halaman contoh → punch list tanpa edit; atau minta landing singkat → struktur tidak generik 3-kartu default.
8. **Stack cursorrules (jika dipasang):** "Rule stack apa yang aktif?" → daftar cocok tech; fitur besar tetap Superpowers, bukan ask-first 3–7.

## E. Jangan lakukan

```text
[ ] Commit .env / password MCP
[ ] Copy buta rule Mongo ke project Postgres
[ ] Duplikat rule panjang di tiga tempat sampai isinya beda
[ ] Re-index setiap typo
[ ] Commit seluruh awesome-cursorrules ke project
[ ] Biarkan rule luar menimpa ask-first / security
```

## F. Setelah siap

Di README project:

```markdown
AI agents: lihat docs/agent-policies/ (kit referensi: docs/agent-kit/).
```
