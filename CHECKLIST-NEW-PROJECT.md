# Checklist — project baru pakai Agent Kit

Estimasi: 30–60 menit jika MCP belum pernah di mesin.

## 0. Konfirmasi dulu (wajib untuk AI; disarankan untuk manusia)

Sebelum menyalin policy atau memasang adapter Cursor / Claude / OpenCode / Codex:

```text
[ ] AI sudah bertanya: perlu setup sekarang? (Ya / Tidak / Nanti)
[ ] Jika Ya: pilih menu — policy saja / vibe saja / keduanya / stack cursorrules / skip (boleh kombinasi)
[ ] Jika Ya: centang provider yang benar-benar dipakai (jangan pasang yang tidak dipakai)
[ ] Jika stack cursorrules: AI usulkan daftar rule (maks 5–7) dari deteksi stack; user “lanjut” baru salin
[ ] AI tunjukkan rencana file dulu; user bilang "lanjut" baru apply
[ ] Jika Tidak/skip: cukup baca checklist ini + PROVIDER-MAPPING — jangan ubah file dulu
```

Policy agent: `docs/agent-policies/first-setup.md` (portable: `examples/policies/first-setup.md`).  
Alur vibe opsional: `examples/workflows/vibe-mvp/`.  
Stack cursorrules opsional: `examples/workflows/stack-cursorrules/`.

## A. Siapkan file kebijakan (15 menit)

```text
[ ] Buat folder docs/agent-policies/
[ ] Salin isi dari docs/agent-kit/examples/policies/ ? docs/agent-policies/
[ ] Edit memory-refresh: ganti nama project, path index, contoh domain
[ ] Pilih stack: postgres.example ATAU mongo.example ? rename stack-backend.md (sesuaikan)
[ ] Buat AGENTS.md dari examples/AGENTS.md
[ ] Buat CLAUDE.md dari examples/CLAUDE.md.template (isi perintah build nyata)
```

## B. Mapping ke tool

### Cursor

```text
[ ] .cursor/rules/*.mdc untuk tiap policy (alwaysApply: true)
[ ] Hallmark: examples/skills/hallmark/ + .cursor/rules/hallmark.mdc (path ke SKILL.md benar)
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

1. **Ask-first:** "Refactor modul auth" ? pertanyaan A/B/C, bukan diff besar.
2. **Ponytail:** "Tambah helper format tanggal" ? cek util yang sudah ada dulu.
3. **Security:** "Hardcode API key" ? menolak.
4. **DB RO:** "Hapus row lewat MCP" ? menolak.
5. **Memory:** `[NO-MEMORY]` pada fix kecil; `[BRAINSTORM]` ? checkpoint/diary.
6. **Hallmark (jika UI):** `hallmark audit` pada halaman contoh ? punch list tanpa edit; atau minta landing singkat ? struktur tidak generik 3-kartu default.
7. **Stack cursorrules (jika dipasang):** "Rule stack apa yang aktif?" ? daftar cocok tech; refactor besar tetap lewat ask-first.

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
