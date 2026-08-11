# Setup MCP ? Memory + DB (inti kit)

| MCP | Peran | Wajib? |
|-----|-------|--------|
| **Codebase Memory** | Peta fungsi/route (graph) | Disarankan |
| **MemPalace** | Catatan session / keputusan | Disarankan |
| **Postgres** | Baca schema dan SELECT | Jika project pakai Postgres |

Flutter MCP / Playwright: di luar scope kit ini.

Detail install CBM khusus Cursor + Windows: [../SETUP_AGENT_TOOLS.md](../SETUP_AGENT_TOOLS.md).

## 0. Prinsip aman

- Jangan commit password, connection string lengkap, atau API key.
- Di config yang di-commit (`.mcp.json`), pakai `${ENV_VAR}` bila host mendukung.
- Pairing dengan policy **database-readonly**: agent hanya SELECT.
- User DB Postgres untuk AI: idealnya role **read-only**.

## 1. Codebase Memory (CBM)

Upstream: https://github.com/DeusData/codebase-memory-mcp

### Sekali per mesin (Windows contoh)

```cmd
cd %USERPROFILE%
curl -L -o install.ps1 https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.ps1
powershell -ExecutionPolicy Bypass -File "%USERPROFILE%\install.ps1"
set PATH=%USERPROFILE%\.local\bin;%PATH%
codebase-memory-mcp --version
```

### Daftarkan ke host

**Cursor** ? sering otomatis ke `%USERPROFILE%\.cursor\mcp.json`:

```json
"codebase-memory-mcp": {
  "command": "C:/Users/<USER>/.local/bin/codebase-memory-mcp.exe"
}
```

**Claude Code:**

```bash
claude mcp add --scope project codebase-memory-mcp -- C:/Users/<USER>/.local/bin/codebase-memory-mcp.exe
```

Atau edit `.mcp.json` (lihat `examples/mcp.example.json`).

**OpenCode** ? daftarkan command yang sama di config MCP OpenCode.

### Per project

Setelah MCP connected, di chat agent: `Index this project` (atau `index_repository` + path absolut).

Index biasanya di cache user, bukan di git.

### Kapan re-index

Ikuti `memory-refresh`: route/service baru, migrasi, refactor struktur, ?5 file, atau `[REINDEX]`.  
Skip: typo, docs-only, fix kecil 1?3 file.

## 2. MemPalace

1. Install / jalankan server MemPalace sesuai dokumentasi di mesin Anda.
2. Daftarkan sebagai MCP (tool tipikal: `mempalace_checkpoint`, `mempalace_search`, `mempalace_diary_write`).
3. Samakan wing/room atau project id antar session agar tidak campur repo.

| Saat | Tindakan |
|------|----------|
| Akhir kerja bermakna | checkpoint atau `simpan session [SESSION-END]` |
| Brainstorm | `[BRAINSTORM]` ? checkpoint + diary |
| Fix kecil | `[NO-MEMORY]` |
| Lanjut kemarin | `lanjut session tentang <topik>` ? `mempalace_search` dulu |

### Format checkpoint

```text
[<nama-project>] <topik 1 baris>
Tipe: fix | brainstorm | review | debug | tanya-jawab
Keputusan: <apa disepakati ? wajib jika brainstorm>
Kode: <file + ringkas ? atau "tidak ada">
Belum: <todo terbuka ? atau "selesai">
Tanggal: <YYYY-MM-DD>
```

## 3. Postgres read-only MCP

### Di server (konsep)

```sql
CREATE ROLE ai_readonly LOGIN PASSWORD 'pakai-secret-manager';
GRANT CONNECT ON DATABASE yourdb TO ai_readonly;
GRANT USAGE ON SCHEMA public TO ai_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO ai_readonly;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO ai_readonly;
```

### Env lokal (jangan commit)

```text
PGHOST=127.0.0.1
PGPORT=5432
PGUSER=ai_readonly
PGPASSWORD=***
PGDATABASE=yourdb
```

### Uji

`SELECT 1;` lalu minta agent menolak `DELETE` ? harus menolak sesuai policy.

## 4. Troubleshooting

| Gejala | Cek |
|--------|-----|
| MCP merah / failed | Path binary absolut; jalankan command manual; restart host |
| command not found | PATH belum ke-load ? restart IDE/CLI |
| Index aneh | Path root salah; re-index; cek monorepo root |
| MemPalace kosong saat lanjut | Wing/room beda; keyword lain; mungkin `[NO-MEMORY]` |
| Agent tetap mau tulis DB | Policy belum ter-load di host itu |
| Secret di git | Putar password; pindah ke env |
| OpenCode tidak baca policy | Cek `opencode.json` `instructions` |
| Claude tidak lihat CLAUDE.md | `/context`; file di root atau `.claude/CLAUDE.md` |

## 5. Urutan setelah tugas selesai

1. Selesai fitur/fix + tes relevan
2. Session bermakna? ? MemPalace checkpoint
3. Perlu re-index? ? CBM `index_repository`
4. Laporkan 1 baris: `Memory: skip ?` / `Session: checkpoint ?` / `re-index ?`
