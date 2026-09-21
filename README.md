# Agent Kit

**Agent Kit memberi agen di Cursor, Claude Code, OpenCode, dan Codex aturan yang sama di repo aplikasimu: kebijakan, mapping host, dan MCP untuk memori, database, serta shell.**

```text
cp -R path/ke/agent-kit/examples/policies docs/agent-policies
```

Kalau kit sudah kamu taruh di dalam project sebagai `docs/agent-kit/`:

```text
cp -R docs/agent-kit/examples/policies docs/agent-policies
```

Windows:

```text
Copy-Item -Recurse path\ke\agent-kit\examples\policies docs\agent-policies
Copy-Item -Recurse docs\agent-kit\examples\policies docs\agent-policies
```

Salin itu di **repo aplikasi**. Repo git Agent Kit ini bukan tempatnya.

| Yang kamu dapat | Kegunaannya |
|-----------------|-------------|
| Policy di `examples/policies/` | Agen tahu kapan harus tanya, bagaimana jaga secret, kapan Superpowers, dan kapan database hanya boleh dibaca. |
| Mapping host | File yang sama hidup di Cursor, Claude Code, OpenCode, dan Codex. |
| MCP | CBM, MemPalace, Claude Mem, RTK, Postgres read-only, dan Figma kalau ada UI. |
| Superpowers | Fitur dan bug lewat spec, plan, lalu TDD. Plugin wajib di setiap host yang kamu pakai. |
| Figma + Hallmark | UI dari frame yang sudah kamu approve, lalu diaudit. README tetap mengikuti `readme-style`. |

## Mulai cepat

Kerjakan di **repo aplikasi**, bukan di git kit. Checkbox lengkap: [CHECKLIST-NEW-PROJECT.md](CHECKLIST-NEW-PROJECT.md).

Sama di semua OS, langkah 0: jangan minta AI menginstal sendiri. Ia harus tanya dulu lewat `first-setup`: Ya, Tidak, atau Nanti.

Kalau kit sudah di-vendor sebagai `docs/agent-kit/`, ganti `path/ke/agent-kit` (atau `path\ke\agent-kit`) jadi `docs/agent-kit`. WSL ikut Linux.

Pilih **satu** stack: postgres **atau** mongo. Jangan rename keduanya.

### Windows (PowerShell)

`cd` ke folder repo aplikasi, lalu:

```text
Copy-Item -Recurse path\ke\agent-kit\examples\policies docs\agent-policies
Rename-Item docs\agent-policies\stack-backend.postgres.example.md stack-backend.md
Copy-Item path\ke\agent-kit\examples\AGENTS.md AGENTS.md
Copy-Item path\ke\agent-kit\examples\CLAUDE.md.template CLAUDE.md
```

Kalau projectmu Mongo, ganti `postgres` jadi `mongo` di baris `Rename-Item`. Lanjut ke Plugin di bawah.

### Linux

`cd` ke folder repo aplikasi, lalu:

```text
cp -R path/ke/agent-kit/examples/policies docs/agent-policies
mv docs/agent-policies/stack-backend.postgres.example.md docs/agent-policies/stack-backend.md
cp path/ke/agent-kit/examples/AGENTS.md AGENTS.md
cp path/ke/agent-kit/examples/CLAUDE.md.template CLAUDE.md
```

Kalau projectmu Mongo, ganti `postgres` jadi `mongo` di baris `mv`. Lanjut ke Plugin di bawah.

### macOS

Buka Terminal.app, `cd` ke folder repo aplikasi, lalu:

```text
cp -R path/ke/agent-kit/examples/policies docs/agent-policies
mv docs/agent-policies/stack-backend.postgres.example.md docs/agent-policies/stack-backend.md
cp path/ke/agent-kit/examples/AGENTS.md AGENTS.md
cp path/ke/agent-kit/examples/CLAUDE.md.template CLAUDE.md
```

Kalau projectmu Mongo, ganti `postgres` jadi `mongo` di baris `mv`. Path pakai `/`, bukan `\`. Lanjut ke Plugin di bawah.

### Plugin dan MCP (semua OS)

1. Pasang plugin Superpowers di **setiap** host yang kamu pakai:

```text
Cursor: di Agent chat `/add-plugin superpowers` (atau cari “superpowers” di marketplace plugin)
Claude Code: `/plugin install superpowers@claude-plugins-official`
OpenCode: `opencode.json` → `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` lalu restart
Codex App: Plugins → Superpowers. Codex CLI: `/plugins` → search `superpowers` → Install
```

2. Kalau project punya UI, pasang Figma juga. Tanpa MCP Figma, UI baru harus berhenti.

```text
Cursor: `/add-plugin figma` lalu OAuth
Claude Code: `claude plugin install figma@claude-plugins-official` (atau `claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp` + `/mcp` Authenticate)
Codex App: plugin Figma. CLI: `codex mcp add figma --url https://mcp.figma.com/mcp`
OpenCode: remote OAuth **belum** di katalog Figma — kerja UI di host ini **berhenti** sampai Figma allowlist; jangan PAT; Desktop MCP bukan generate
```

3. MCP yang disarankan: CBM, MemPalace, Claude Mem, dan RTK. Tambah Postgres kalau projectmu memang Postgres. Per OS: [MCP-SETUP.md](MCP-SETUP.md).
4. Buka chat agen baru, lalu uji perilaku di CHECKLIST §D.

Stack cursorrules (maksimal 5–7 rule) boleh, setelah kamu konfirmasi. Bukan langkah wajib.

## Kit vs aplikasi

```text
kit ini:      examples/policies/          ← kanonik
app konsumen: examples/policies → docs/agent-policies
              lalu mapping lewat PROVIDER-MAPPING.md / CHECKLIST-NEW-PROJECT.md
```

Di repo kit ini, **jangan** menyalin kebijakan ke `docs/agent-policies/`. Folder itu dan `.cursor/` di-gitignore. Spec dan plan Superpowers untuk kit sendiri hidup di `docs/superpowers/` dan dilacak git.

## Host

Tabel ini menjawab file apa yang hidup di host mana. Perintah plugin harus sama dengan CHECKLIST. Overlay seperti `superpowers.mdc` dan `figma.mdc` hanya pointer, bukan isi skill.

| Yang dipasang | Cursor | Claude Code | OpenCode | Codex / generic |
|---------------|--------|-------------|----------|-----------------|
| Overlay policy | `.cursor/rules/*.mdc` (`alwaysApply: true`) mengarah ke `docs/agent-policies/` | `CLAUDE.md` + `docs/agent-policies/` | `AGENTS.md` + `opencode.json` `instructions` | `AGENTS.md` + `docs/agent-policies/` |
| Superpowers | `/add-plugin superpowers` + overlay `superpowers.mdc` | `/plugin install superpowers@claude-plugins-official` | `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` | App: Plugins → Superpowers. CLI: `/plugins` → `superpowers` |
| Figma (jika UI) | `/add-plugin figma` + overlay `figma.mdc` | `claude plugin install figma@claude-plugins-official` | Overlay saja; remote MCP **bukan** katalog → UI **berhenti** | Plugin app atau `codex mcp add figma --url https://mcp.figma.com/mcp` |
| Hallmark | rule `.mdc` + skill `examples/skills/hallmark/` (disalin ke app) | `~/.claude/skills/hallmark/` atau project skills | pointer `SKILL.md` di `AGENTS.md` / `instructions` | `~/.codex/skills/hallmark/` atau `.codex/skills/` |

Jangan masukkan skill Superpowers atau Figma ke git kit.

Kalau host-mu di luar empat itu, ikuti README upstream, bukan checklist kit. Path lengkap: [PROVIDER-MAPPING.md](PROVIDER-MAPPING.md).

## MCP

Setiap MCP cukup satu cek di bawah. Perintah install lengkap ada di MCP-SETUP.

| MCP | Peran | Wajib? | Cek cepat |
|-----|--------|--------|-----------|
| Codebase Memory | Graph kode | Disarankan | `codebase-memory-mcp --version` + index project |
| MemPalace | Keputusan / checkpoint | Disarankan | uji `checkpoint` 1× |
| Claude Mem | Observasi sesi (hook + worker) | Disarankan | `npx claude-mem install` + `--provider claude`; **bukan CMEM Pro** |
| RTK | Shell ter-allowlist | Disarankan | uji `rtk --version` + `git status` |
| Postgres | SELECT + schema | Jika project Postgres | user **read-only**; tolak DELETE |
| Figma | Frame sumber UI | Wajib untuk UI di app | remote `https://mcp.figma.com/mcp`; OAuth, bukan PAT |

Jangan commit secret, file `.env`, `FIGMA_ACCESS_TOKEN`, atau `~/.claude-mem/settings.json`.

Kalau Claude Mem hilang, kerja tetap jalan tanpa search. Itu **bukan** alasan berhenti. Kalau Figma MCP hilang, UI baru **berhenti**. Jangan ganti dengan tema catalog Hallmark.

Flutter MCP dan Playwright-as-MCP di luar kit. Playwright sebagai perintah biasa boleh lewat allowlist RTK.

## Katalog kebijakan

Pakai tabel ini untuk memilih policy. Penjelasan lengkap ada di RULES-CATALOG.

| Policy | Kapan | Bukan untuk |
|--------|--------|-------------|
| `first-setup` | Pasang kit ke app; konfirmasi dulu | Menu Superpowers / Figma (itu checklist) |
| `superpowers` | Fitur baru / bug / kerja multi-langkah | Typo 1 baris; first-setup |
| `ask-first` | First-setup + non-fitur (config, rename) | Fitur baru (itu Superpowers) |
| `ponytail` | Ukuran diff | Skip TDD / skip Figma+Hallmark pada UI |
| `security` | Secret, input, query, auth | — |
| `database-readonly` | Akses DB lewat MCP | Tulis lewat MCP |
| `memory-refresh` | Setelah kerja bermakna; lanjut topik | Re-index CBM tiap typo |
| `figma` lalu `hallmark` | UI app setelah spec | README Markdown; repo kit; ejaan di string lama |
| `readme-style` | README Markdown | Halaman visual app |
| `caveman` | Jawaban agen | Security / irreversible / user bingung / review spec |
| `stack-backend` | Satu file setelah pilih Postgres **atau** Mongo | Menyalin contoh Mongo ke project Postgres |
| stack cursorrules | Opsional, maks 5–7, konfirmasi dulu | `alwaysApply: true` pada rule luar; menimpa policy kit |

Di kit, path-nya `examples/policies/`. Di app, `docs/agent-policies/`. Penjelasan tiap file: [RULES-CATALOG.md](RULES-CATALOG.md).

## Alur UI

Kalau kamu membangun halaman di **aplikasi**:

```text
spec Superpowers disetujui
  → Figma (MCP wajib; generate boleh, implement nol file sampai node di-approve)
    → Hallmark (implement dari frame + audit sebelum ship)
```

Overlay `figma.md` adalah sumber layout, IA, dan token. Hallmark mengurus kualitas plus `hallmark audit`. Hallmark tidak mengganti frame.

Tanpa MCP Figma, **berhenti**. Jangan mengarang tema dari catalog Hallmark.

Di OpenCode, remote MCP Figma belum masuk katalog, jadi UI baru **berhenti**. Jangan pakai PAT.

Figma tidak wajib untuk ejaan di string yang sudah ada, README Markdown (`readme-style`), kerja non-UI, dan **repo kit ini**.

Halaman harus responsive di desktop dan mobile. Placeholder AI bukan “selesai”.

Build mode (spec dan plan Superpowers sudah disetujui) tidak menghapus gerbang node Figma untuk permukaan UI baru.

Skill Hallmark di-vendor di `examples/skills/hallmark/`. Skill Figma dan Superpowers **tidak**.

Overlay: [examples/policies/figma.md](examples/policies/figma.md), [examples/policies/hallmark.md](examples/policies/hallmark.md). Setup skill: [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md).

Di repo kit ini overlay Figma **idle**. Kit ini docs dan examples, bukan produk UI.

## Kerja di repo kit

Kalau kamu mengedit Agent Kit sendiri, bukan memasangnya ke app:

Sumber kebijakan: `examples/policies/` (git). `AGENTS.md` dan `CLAUDE.md` di root memakai folder itu.

**Jangan** membuat `docs/agent-policies/` atau `.cursor/rules/` di repo ini.

Spec dan plan Superpowers kit: `docs/superpowers/` (dilacak git).

Edit policy di `examples/policies/`. Jangan vendor skill Superpowers atau Figma.

Hallmark: vendor di `examples/skills/hallmark/`. Update upstream mengikuti README skill itu.

Tidak ada server app dan tidak ada test suite. Tugas docs selesai jika tautan dan fakta selaras dengan CHECKLIST, MCP-SETUP, PROVIDER-MAPPING, dan RULES-CATALOG.

## Dokumen panjang

Butuh checkbox, path host, atau perintah MCP yang lebih panjang? Buka file di bawah.

| Doc | Isi panjang |
|-----|-------------|
| [CHECKLIST-NEW-PROJECT.md](CHECKLIST-NEW-PROJECT.md) | Checkbox pasang ke app, termasuk plugin |
| [PROVIDER-MAPPING.md](PROVIDER-MAPPING.md) | Path per host |
| [MCP-SETUP.md](MCP-SETUP.md) | Perintah MCP |
| [RULES-CATALOG.md](RULES-CATALOG.md) | Isi tiap policy |
| [examples/](examples/) | Template salinan |
| [examples/policies/first-setup.md](examples/policies/first-setup.md) | Konfirmasi sebelum pasang |
| [examples/policies/superpowers.md](examples/policies/superpowers.md) | Overlay Superpowers |
| [examples/policies/figma.md](examples/policies/figma.md) | Overlay Figma |
| [examples/workflows/stack-cursorrules/](examples/workflows/stack-cursorrules/) | Rule stack opsional |
| [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md) | Setup Hallmark |

Agen **di repo ini** mengikuti `examples/policies/` lewat `AGENTS.md` root.

Agen **di app** mengikuti `docs/agent-policies/`. Kit-nya tetap repo ini, atau salinan `docs/agent-kit/` di project lain.
