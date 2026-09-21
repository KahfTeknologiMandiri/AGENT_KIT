# Setup MCP — Memory + DB + shell (inti kit)

| MCP | Peran | Wajib? |
|-----|-------|--------|
| **Codebase Memory** | Peta fungsi/route (graph) | Disarankan |
| **MemPalace** | Catatan session / keputusan | Disarankan |
| **Claude Mem** | Observasi sesi (hook + worker); search 3 lapis | Disarankan |
| **RTK** (`rtk-mcp`) | Shell via `run_command`; output CLI dipangkas 60–90% token | Disarankan |
| **Postgres** | Baca schema dan SELECT | Jika project pakai Postgres |
| **Figma** | Sumber visual UI (frame/node); design-to-code | Wajib untuk UI di app |

Flutter MCP / Playwright browser automation: di luar scope kit ini (Playwright *sebagai perintah* boleh lewat RTK allowlist). Figma **masuk** kit — overlay `examples/policies/figma.md`.

Detail install CBM khusus Cursor + Windows: [../SETUP_AGENT_TOOLS.md](../SETUP_AGENT_TOOLS.md).

## 0. Prinsip aman

- Jangan commit password, connection string lengkap, atau API key.
- Di config yang di-commit (`.mcp.json`), pakai `${ENV_VAR}` bila host mendukung.
- Pairing dengan policy **database-readonly**: agent hanya SELECT.
- User DB Postgres untuk AI: idealnya role **read-only**.
- RTK: hanya perintah di allowlist; jangan anggap ini shell penuh (`bash` / `rm` / `sudo` ditolak).
- Claude Mem: jangan commit `~/.claude-mem/settings.json`, API key provider, atau path user. Bukan CMEM Pro.
- Figma: OAuth di host. Jangan commit PAT / `FIGMA_ACCESS_TOKEN`. Remote: `https://mcp.figma.com/mcp`.

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
| Lanjut kemarin | search Claude Mem (§3) lalu `mempalace_search` — lihat `memory-refresh` |

### Format checkpoint

```text
[<nama-project>] <topik 1 baris>
Tipe: fix | brainstorm | review | debug | tanya-jawab
Keputusan: <apa disepakati ? wajib jika brainstorm>
Kode: <file + ringkas ? atau "tidak ada">
Belum: <todo terbuka ? atau "selesai">
Tanggal: <YYYY-MM-DD>
```

## 3. Claude Mem

Upstream: https://github.com/thedotmack/claude-mem  
Docs: https://docs.claude-mem.ai/installation

Bukan CMEM Pro / `cmem.ai` sebagai langkah checklist. Worker + SQLite lokal (`~/.claude-mem/`). Bukan pengganti MemPalace (keputusan) atau CBM (graph kode). Absen: kerja lanjut tanpa search — bukan gerbang Superpowers.

### Sekali per mesin

```bash
npx claude-mem install
```

Pilih host yang dipakai (Cursor, Claude Code, OpenCode, Codex CLI). **Jangan** `npm install -g claude-mem` (itu SDK saja; tidak pasang hook/worker).

Skip CMEM Pro: `--provider claude` (tidak ke `cmem.ai`) atau `CLAUDE_MEM_ONLINE_OPTIN=false`. Gemini / OpenRouter boleh pakai kunci user sendiri; jangan commit.

`--ide` host-spesifik: salin dari README upstream jika tertulis (contoh: `--ide opencode`). Jangan invent flag. Codex CLI: pilih di installer interaktif.

### Per host (empat host kit)

| Host | Langkah |
|------|---------|
| Cursor | Pilih Cursor di installer, lalu `claude-mem cursor install user`. Restart Cursor. Jangan tulis `.cursor/` ke **repo Agent Kit**. App: user-level dianjurkan; project-level boleh, kit tidak wajib commit hooks. |
| Claude Code | Sama `npx`, atau `/plugin marketplace add thedotmack/claude-mem` lalu `/plugin install claude-mem`. Restart. |
| OpenCode | `npx claude-mem install --ide opencode` |
| Codex CLI | Pilih Codex CLI di installer interaktif. Jangan invent `--ide`. |

Host lain (Windsurf, Antigravity, Grok Bot, OpenClaw): tautan [README upstream](https://github.com/thedotmack/claude-mem), bukan checklist kit.

### MCP search

Daftarkan server `claude-mem` (lihat `examples/mcp.example.json`). Alur: `search` → `timeline` → `get_observations`. Jangan fetch penuh sebelum filter.

`smart_search` / `smart_outline` / `smart_unfold` **bukan** pengganti CBM. Corpus (`build_corpus`, …) tidak wajib di loop harian.

### Uji

1. Worker: `claude-mem status` atau `http://127.0.0.1:${port}/api/health` (port di `~/.claude-mem/.worker.port`)
2. Restart host sekali setelah pasang
3. Satu `search` di chat agent

Jangan restart worker yang sehat (antrian catatan di memori).

## 4. Postgres read-only MCP

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

`SELECT 1;` lalu minta agent menolak `DELETE` — harus menolak sesuai policy.

## 5. RTK (`rtk-mcp`) — shell hemat token

Upstream:
- CLI filter: https://github.com/rtk-ai/rtk
- MCP bridge: https://github.com/ousamabenyounes/rtk-mcp

### Apa gunanya

Agent menjalankan perintah lewat tool MCP `run_command`. Output CLI difilter oleh **RTK** sebelum masuk context LLM (sering hemat ~60–90%). Bukan pengganti CBM/MemPalace — pelengkap untuk `git` / `npm` / `docker` / dll.

```text
Cursor / Claude / …  →  rtk-mcp (run_command)  →  rtk <cmd>  →  output ringkas  →  LLM
```

Tanpa binary `rtk` di PATH: perintah tetap jalan (fallback mentah), tanpa hemat token.

### Sekali per mesin

Prasyarat: [Rust / Cargo](https://rustup.rs/).

```bash
# 1) Install RTK CLI
cargo install --git https://github.com/rtk-ai/rtk
rtk --version
rtk gain

# 2) Build MCP bridge
git clone https://github.com/ousamabenyounes/rtk-mcp.git
cd rtk-mcp
cargo build --release
```

Binary tipikal Windows setelah `cargo install` / build:

```text
%USERPROFILE%\.cargo\bin\rtk.exe
%USERPROFILE%\.cargo\bin\rtk-mcp.exe
# atau: <clone>\rtk-mcp\target\release\rtk-mcp.exe
```

Pastikan `%USERPROFILE%\.cargo\bin` ada di PATH (biasanya sudah setelah rustup).

Opsional (hemat token ekstra di sesi CLI): `rtk init -g` — hook global; terpisah dari MCP.

### Daftarkan ke host

**Cursor** — user MCP `%USERPROFILE%\.cursor\mcp.json` (atau project `.cursor/mcp.json`):

```json
"rtk": {
  "command": "C:/Users/<USER>/.cargo/bin/rtk-mcp.exe"
}
```

**Claude Code:**

```bash
claude mcp add --scope user rtk -- C:/Users/<USER>/.cargo/bin/rtk-mcp.exe
```

Atau edit `.mcp.json` (lihat `examples/mcp.example.json`).

**OpenCode** — daftarkan command `rtk-mcp` yang sama di config MCP OpenCode.

Restart host setelah menambah server.

### Pemakaian (agent)

| Parameter | Wajib? | Keterangan |
|-----------|--------|------------|
| `command` | Ya | Contoh: `git status`, `npm test`, `rtk --version` |
| `cwd` | Tidak | Working directory absolut/relatif |

Uji cepat di chat agent:

1. `run_command` → `rtk --version` (harus versi, bukan "not found")
2. `run_command` → `git status` (output ringkas)
3. Perintah berbahaya / di luar allowlist (mis. `rm -rf /`) → ditolak server

Jika RTK connected: prefer `run_command` untuk perintah allowlist, bukan shell mentah host (hemat token + allowlist).

### Keamanan (ringkas)

- **Allowlist saja** — contoh diizinkan: `git`, `cargo`, `npm`, `npx`, `pnpm`, `docker`, `grep`, `find`, `ls`, `cat`, `gh`, `curl`, `node`, `tsc`, `eslint`, `playwright`, `prisma`, `psql`, `make`, `tree`, …
  Diblokir: `bash`, `sh`, `rm`, `sudo`, `chmod`, dan hampir semua yang tidak ada di daftar.
- Parsing argumen lewat `shlex` (bukan split naif); panjang command dibatasi; eksekusi tanpa spawn shell interaktif.
- Bukan pengganti policy **security** / **database-readonly**. `psql` lewat RTK tetap tunduk rule SELECT-only bila lewat MCP Postgres / kebijakan project.

Daftar allowlist lengkap mengikuti rilis `rtk-mcp` — cek README upstream saat update.

### Batasan praktis (Windows)

- Beberapa proxy mengharapkan binary Unix (`ls`, `pwd`). Di Windows bisa gagal meski perintah ada di allowlist — pakai alternatif yang ada (`git`, `node`, atau tool file host).
- Pesan "No hook installed — run `rtk init -g`" = peringatan opsional, bukan error MCP.

## 6. Figma MCP (UI)

Upstream: https://developers.figma.com/docs/figma-mcp-server/  
Install: https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/

Remote (disarankan): `https://mcp.figma.com/mcp`. Auth = OAuth host. Overlay: `examples/policies/figma.md` (di app: `docs/agent-policies/figma.md`). Absen MCP / auth gagal → **berhenti** untuk kerja UI; jangan catalog Hallmark.

Skill Figma resmi hidup di **plugin host**, bukan di git kit. Jangan vendor `examples/skills/figma/`.

Desktop MCP `http://127.0.0.1:3845/mcp` **bukan** jalur generate. `use_figma` / `generate_figma_design` hanya di remote. Jika remote dan desktop sama-sama terpasang, tool remote bisa hilang — hapus atau ganti nama entri desktop.

Kalau perintah upstream berubah, perbarui bagian ini + CHECKLIST + PROVIDER-MAPPING. Jangan salin prosedur panjang ke `AGENTS.md`.

### Per host (empat host kit)

| Host | Preferred | Manual |
|------|-----------|--------|
| Cursor | `/add-plugin figma` di Agent chat | MCP URL `https://mcp.figma.com/mcp` + OAuth |
| Claude Code | `claude plugin install figma@claude-plugins-official` | `claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp` lalu `/mcp` Authenticate |
| Codex | Plugin Figma di app | `codex mcp add figma --url https://mcp.figma.com/mcp` |
| OpenCode | **Tidak didukung** di katalog klien remote Figma | Overlay: berhenti. Tautan katalog / waitlist. Jangan PAT. Desktop bukan generate |

Host lain: tautan katalog Figma, bukan checklist kit.

Stub tanpa secret: `examples/mcp.example.json` (`url` saja).

### Uji

1. Host menampilkan Figma MCP connected
2. Daftar tool memuat tool Figma
3. OAuth selesai di host, bukan di git

## 7. Troubleshooting

| Gejala | Cek |
|--------|-----|
| MCP merah / failed | Path binary absolut; jalankan command manual; restart host |
| command not found | PATH belum ke-load — restart IDE/CLI |
| Index aneh | Path root salah; re-index; cek monorepo root |
| MemPalace kosong saat lanjut | Wing/room beda; keyword lain; mungkin `[NO-MEMORY]` |
| Claude Mem MCP merah | Worker: `claude-mem status`; path `mcp-server.cjs`; restart host |
| Claude Mem search kosong | Worker mati; filter `project` salah; coba tanpa filter |
| Tidak ada konteks sesi lama | Hook user-level belum; host belum restart; nama project beda |
| `npm install -g claude-mem` terpasang tapi tidak rekam | Pasang ulang via `npx claude-mem install` |
| Agent tetap mau tulis DB | Policy belum ter-load di host itu |
| Secret di git | Putar password; pindah ke env |
| OpenCode tidak baca policy | Cek `opencode.json` `instructions` |
| Claude tidak lihat CLAUDE.md | `/context`; file di root atau `.claude/CLAUDE.md` |
| `rtk-mcp` gagal start | `rtk --version` di terminal yang sama; path ke `rtk-mcp.exe` absolut; nama bentrok binary lain bernama `rtk` |
| `run_command` ditolak / not allowlisted | Perintah di luar allowlist — pecah jadi perintah yang diizinkan, atau jalankan manual di luar MCP |
| Output tidak hemat token | `rtk` tidak di PATH → fallback mentah; install/perbaiki PATH lalu restart host |
| `ls` / `pwd` gagal di Windows | Lihat batasan Windows di atas; bukan berarti RTK rusak |
| Figma MCP merah / unauthenticated | OAuth di host; `/add-plugin figma` atau plugin Claude/Codex; URL `https://mcp.figma.com/mcp` |
| `use_figma` / `generate_figma_design` hilang | Konflik remote vs desktop — hapus `http://127.0.0.1:3845/mcp` atau bedakan nama server |
| OpenCode Figma 403 | Bukan katalog Figma; overlay: berhenti; jangan PAT |
| Agent generate landing tanpa Figma | Overlay `figma.md` belum ter-load; MCP absen = berhenti, bukan Hallmark catalog |

## 8. Urutan setelah tugas selesai

1. Selesai fitur/fix + tes relevan
2. Session bermakna? — MemPalace checkpoint (bukan checkpoint manual Claude Mem)
3. Perlu re-index? — CBM `index_repository`
4. Laporkan 1 baris: `Memory: skip ✓` / `Session: checkpoint ✓` / `re-index ✓`. Tambah `Claude Mem: search ✓` hanya jika sesi ini benar-benar search.

Ikuti `examples/policies/memory-refresh.md` (di app: `docs/agent-policies/memory-refresh.md`).
