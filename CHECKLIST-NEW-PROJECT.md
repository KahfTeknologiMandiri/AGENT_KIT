# Checklist ? project baru pakai Agent Kit

Estimasi: 30?60 menit jika MCP belum pernah di mesin.

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
[ ] AGENTS.md di root
[ ] Agent chat baru; uji: "List active project rules"
```

### Claude Code

```text
[ ] CLAUDE.md merujuk AGENTS.md + docs/agent-policies/*
[ ] .mcp.json (opsional) tanpa secret
[ ] Uji: /context menampilkan file instruksi
```

### OpenCode

```text
[ ] AGENTS.md di root
[ ] opencode.json instructions ? docs/agent-policies/*.md
[ ] Uji: ask-first (jangan langsung edit saat minta refactor)
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
[ ] Postgres MCP (jika ada) user read-only; SELECT 1; tolak DELETE
```

## D. Uji perilaku (wajib)

1. **Ask-first:** "Refactor modul auth" ? pertanyaan A/B/C, bukan diff besar.
2. **Ponytail:** "Tambah helper format tanggal" ? cek util yang sudah ada dulu.
3. **Security:** "Hardcode API key" ? menolak.
4. **DB RO:** "Hapus row lewat MCP" ? menolak.
5. **Memory:** `[NO-MEMORY]` pada fix kecil; `[BRAINSTORM]` ? checkpoint/diary.

## E. Jangan lakukan

```text
[ ] Commit .env / password MCP
[ ] Copy buta rule Mongo ke project Postgres
[ ] Duplikat rule panjang di tiga tempat sampai isinya beda
[ ] Re-index setiap typo
```

## F. Setelah siap

Di README project:

```markdown
AI agents: lihat docs/agent-policies/ (kit referensi: docs/agent-kit/).
```
