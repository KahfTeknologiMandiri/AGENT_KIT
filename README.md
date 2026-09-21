# Agent Kit

**Agent Kit memasang kebijakan agen + mapping host + MCP memori/DB/shell ke repo aplikasi, supaya Cursor / Claude Code / OpenCode / Codex mengikuti aturan yang sama.**

```text
cp -R path/ke/agent-kit/examples/policies docs/agent-policies
```

Jika kit di-vendor sebagai `docs/agent-kit/`:

```text
cp -R docs/agent-kit/examples/policies docs/agent-policies
```

Windows:

```text
Copy-Item -Recurse path\ke\agent-kit\examples\policies docs\agent-policies
Copy-Item -Recurse docs\agent-kit\examples\policies docs\agent-policies
```

Jalankan perintah itu di **repo aplikasi**, bukan di git kit ini.

| Yang didapat | Untuk apa |
|--------------|-----------|
| Policy di `examples/policies/` | Perilaku agen (tanya dulu, security, DB RO, Superpowers, …) |
| Mapping host | File yang sama hidup di Cursor / Claude / OpenCode / Codex |
| MCP | CBM, MemPalace, Claude Mem, RTK, Postgres RO, Figma (UI) |
| Superpowers | Fitur/bug: spec → plan → TDD; plugin wajib per host |
| Figma + Hallmark | UI dari frame yang di-approve, lalu audit; README tetap `readme-style` |

## Mulai cepat

Repo **aplikasi**. Checkbox lengkap: [CHECKLIST-NEW-PROJECT.md](CHECKLIST-NEW-PROJECT.md).

1. Jangan pasang otomatis — AI ikut `first-setup` (Ya / Tidak / Nanti).
2. Salin `examples/policies/` → `docs/agent-policies/` (perintah di atas).
3. Pilih `stack-backend.postgres.example.md` **atau** `stack-backend.mongo.example.md` → rename `stack-backend.md`.
4. Salin `examples/AGENTS.md` → `AGENTS.md`; isi `CLAUDE.md` dari `examples/CLAUDE.md.template`.
5. Pasang plugin Superpowers di **setiap** host yang dipakai:

```text
Cursor: di Agent chat `/add-plugin superpowers` (atau cari “superpowers” di marketplace plugin)
Claude Code: `/plugin install superpowers@claude-plugins-official`
OpenCode: `opencode.json` → `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` lalu restart
Codex App: Plugins → Superpowers. Codex CLI: `/plugins` → search `superpowers` → Install
```

6. Jika ada UI: pasang Figma plugin/MCP. Tanpa MCP Figma → UI baru berhenti.

```text
Cursor: `/add-plugin figma` lalu OAuth
Claude Code: `claude plugin install figma@claude-plugins-official` (atau `claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp` + `/mcp` Authenticate)
Codex App: plugin Figma. CLI: `codex mcp add figma --url https://mcp.figma.com/mcp`
OpenCode: remote OAuth **belum** di katalog Figma — kerja UI di host ini **berhenti** sampai Figma allowlist; jangan PAT; Desktop MCP bukan generate
```

7. MCP disarankan: CBM, MemPalace, Claude Mem, RTK; Postgres jika project pakai Postgres. Perintah penuh: [MCP-SETUP.md](MCP-SETUP.md).
8. Buka chat agen baru; uji perilaku di CHECKLIST §D.

Stack cursorrules (maks 5–7) **opsional**, setelah konfirmasi. Bukan langkah wajib.

## Kit vs aplikasi

```text
kit ini:      examples/policies/          ← kanonik
app konsumen: examples/policies → docs/agent-policies
              lalu mapping lewat PROVIDER-MAPPING.md / CHECKLIST-NEW-PROJECT.md
```

**Jangan** salin kebijakan ke `docs/agent-policies/` di dalam repo kit ini (`docs/agent-policies/` dan `.cursor/` di-gitignore). Spec/plan Superpowers di `docs/superpowers/` dilacak git.

## Host

| Yang dipasang | Cursor | Claude Code | OpenCode | Codex / generic |
|---------------|--------|-------------|----------|-----------------|
| Overlay policy | `.cursor/rules/*.mdc` (`alwaysApply: true`) mengarah ke `docs/agent-policies/` | `CLAUDE.md` + `docs/agent-policies/` | `AGENTS.md` + `opencode.json` `instructions` | `AGENTS.md` + `docs/agent-policies/` |
| Superpowers | `/add-plugin superpowers` + overlay `superpowers.mdc` | `/plugin install superpowers@claude-plugins-official` | `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` | App: Plugins → Superpowers. CLI: `/plugins` → `superpowers` |
| Figma (jika UI) | `/add-plugin figma` + overlay `figma.mdc` | `claude plugin install figma@claude-plugins-official` | Overlay saja; remote MCP **bukan** katalog → UI **berhenti** | Plugin app atau `codex mcp add figma --url https://mcp.figma.com/mcp` |
| Hallmark | rule `.mdc` + skill `examples/skills/hallmark/` (disalin ke app) | `~/.claude/skills/hallmark/` atau project skills | pointer `SKILL.md` di `AGENTS.md` / `instructions` | `~/.codex/skills/hallmark/` atau `.codex/skills/` |

Overlay Superpowers/Figma **bukan** isi skill. Jangan vendor skill Superpowers atau Figma ke git kit. Host di luar empat itu: tautan README upstream, bukan checklist kit. Detail: [PROVIDER-MAPPING.md](PROVIDER-MAPPING.md).

## MCP

| MCP | Peran | Wajib? | Kunci di README |
|-----|--------|--------|-----------------|
| Codebase Memory | Graph kode | Disarankan | `codebase-memory-mcp --version` + index project |
| MemPalace | Keputusan / checkpoint | Disarankan | uji `checkpoint` 1× |
| Claude Mem | Observasi sesi (hook + worker) | Disarankan | `npx claude-mem install` + `--provider claude`; **bukan CMEM Pro** |
| RTK | Shell ter-allowlist | Disarankan | uji `rtk --version` + `git status` |
| Postgres | SELECT + schema | Jika project Postgres | user **read-only**; tolak DELETE |
| Figma | Frame sumber UI | Wajib untuk UI di app | remote `https://mcp.figma.com/mcp`; OAuth, bukan PAT |

Jangan commit secret, `.env`, `FIGMA_ACCESS_TOKEN`, atau `~/.claude-mem/settings.json`. Claude Mem hilang → kerja lanjut tanpa search; **bukan** gerbang stop. Figma MCP hilang → UI baru **berhenti**; jangan fallback catalog Hallmark. Flutter MCP / Playwright-as-MCP di luar kit. Playwright sebagai perintah boleh lewat allowlist RTK.

## Katalog kebijakan

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

Path di kit = `examples/policies/`; di app = `docs/agent-policies/`. Isi lengkap: [RULES-CATALOG.md](RULES-CATALOG.md).

## Alur UI

```text
spec Superpowers disetujui
  → Figma (MCP wajib; generate boleh, implement nol file sampai node di-approve)
    → Hallmark (implement dari frame + audit sebelum ship)
```

- Overlay `figma.md` = sumber layout/IA/token. Hallmark = kualitas + `hallmark audit`, bukan pengganti frame.
- Tanpa MCP Figma → **berhenti**. Jangan invent tema catalog Hallmark.
- OpenCode: remote MCP Figma bukan katalog → UI baru **berhenti**; jangan PAT.
- Pengecualian Figma: ejaan/tanda baca di string yang **sudah ada**; README Markdown (`readme-style`); kerja non-UI; **repo kit ini**.
- Responsive desktop+mobile. Placeholder AI bukan “selesai”.
- Build mode (spec+plan Superpowers disetujui) tidak menghapus gerbang node Figma untuk permukaan UI baru.
- Skill Hallmark di-vendor di `examples/skills/hallmark/`; skill Figma/Superpowers **tidak**.

Overlay: [examples/policies/figma.md](examples/policies/figma.md), [examples/policies/hallmark.md](examples/policies/hallmark.md). Setup skill: [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md).

Overlay Figma **idle** di repo kit (docs/examples, bukan produk UI).

## Kerja di repo kit

Bukan Mulai cepat.

- Sumber kebijakan: `examples/policies/` (git). `AGENTS.md` / `CLAUDE.md` root memakai folder itu.
- **Jangan** membuat `docs/agent-policies/` atau `.cursor/rules/` di repo ini.
- Spec/plan Superpowers kit: `docs/superpowers/` (dilacak git).
- Edit policy di `examples/policies/`. Jangan vendor skill Superpowers atau Figma.
- Hallmark: vendor di `examples/skills/hallmark/`; update upstream per README skill itu.
- Tidak ada server app / test suite. Tugas docs selesai jika tautan dan fakta selaras CHECKLIST / MCP-SETUP / PROVIDER-MAPPING / RULES-CATALOG.

## Dokumen panjang

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

Agen **di repo ini:** `examples/policies/` (lewat `AGENTS.md` root).  
Agen **di app:** `docs/agent-policies/` (kit referensi: repo ini / `docs/agent-kit/` di project lain).
