# Root README landing + handbook — design

Date: 2026-09-21  
Status: approved in brainstorming (user `ok` on sections 1–8; approach 1)  
Repo: Agent Kit (`examples/policies/` = canonical policies)  
Style: `examples/policies/readme-style.md` (README Markdown; not Hallmark; not Figma)

## Problem

Root `README.md` is a 28-line index: English one-liner, a path diagram, a table of links. It fails the kit’s own `readme-style` policy (landing in 3–5 seconds, working example in the first five lines, two-column feature table, copy-paste Quick Start).

The facts an app installer needs — hosts, MCP, policies, UI flow — already live in CHECKLIST, PROVIDER-MAPPING, MCP-SETUP, and RULES-CATALOG. A GitHub reader never sees them unless they open those files. Completing the README means putting a compressed handbook on that page, not adding more links.

## Goal

Rewrite **only** root `README.md` as an Indonesian **landing + below-the-fold handbook** for a person installing the kit into an **application** repo.

Success after implementation:

1. Above the fold: bold Indonesian one-liner, working `cp` / `Copy-Item` in the first five lines, five-row two-column feature table, Quick Start without `$`.
2. Below the fold: Host, MCP, policy catalog, UI flow, short “kerja di repo kit”, then the existing long-doc link table.
3. Plugin command strings match CHECKLIST §A2 / §A3 exactly (do not invent).
4. Long install scripts stay in CHECKLIST / MCP-SETUP / PROVIDER-MAPPING. README uses one key command or name per tool plus a link.
5. No copy into `docs/agent-policies/` or `.cursor/` inside this kit repo. README says that explicitly.
6. Indonesian prose; CLI strings stay as in CHECKLIST. H1 remains `# Agent Kit` (product name).
7. No screenshot, badge, license claim, or other README files edited.

## Non-goals

- Edit CHECKLIST, MCP-SETUP, PROVIDER-MAPPING, RULES-CATALOG, `examples/README.md`, Hallmark README, stack-cursorrules README, `vibe-mvp`, policies, or skills.
- Duplicate curl CBM, RTK allowlist, or CHECKLIST §D checkboxes into README.
- Create `docs/agent-policies/` or `.cursor/` in this kit repo.
- License file, badges, screenshots, contributor lists.
- Make README the source of truth for install commands. If README and a long doc disagree, **the long doc wins**; fix README to match.
- First-setup menu changes. Superpowers / Figma stay checklist items, not first-setup menu items.
- Apply the kit into some other application from this session.
- Automated tests (kit is documentation).
- CBM re-index unless `[REINDEX]`.
- Delete `examples/workflows/vibe-mvp/`.
- Fix encoding glitches in `readme-style.md` (`3?5`, `In today's?`).

## Decisions

| ID | Choice |
|----|--------|
| 1 | Root `README.md` only. |
| 2 | Approach 1: landing on top, handbook below. Not a full paste of CHECKLIST/MCP-SETUP (approach 2). Not a prose-free spec sheet (approach 3). |
| 3 | Indonesian for all prose, section headings, and table headers. Product title `# Agent Kit` stays. Command strings copied from CHECKLIST / MCP-SETUP. |
| 4 | Primary reader: human installing into an **app**. Quick Start is copy-policies-into-app, not “work in this git”. |
| 5 | Canonical install commands: CHECKLIST §A2 (Superpowers) and §A3 (Figma). README quotes those strings; does not paraphrase into new commands. |
| 6 | Canonical long MCP: MCP-SETUP.md. README: role table + one key command/name per MCP. |
| 7 | Stack cursorrules optional. Not a required Quick Start step. |
| 8 | No LICENSE claim (repo has no LICENSE). No screenshots (none on disk). |
| 9 | Target length 150–280 lines (hard cap 300). If a section needs more than one key command, link out. |
| 10 | AI agents reading README still follow `first-setup` (do not apply without confirmation). A human may copy files; the page must not read as “run this inside the kit repo”. |

## Architecture (page)

```text
# Agent Kit
bold one-liner (id)
copy commands (unix + windows, vendor path)
feature table (5 rows)
Quick Start (app repo)
kit vs app diagram + do-not-copy-in-kit
--- below the fold ---
Host table
MCP table + secret/stop-gate notes
Policy catalog table
UI flow
Kerja di repo kit (short)
Long-doc link table
```

Reader path: decide in 3–5 seconds at the top → copy in the app → skim handbook if they need hosts/MCP/UI → open CHECKLIST for checkboxes.

Sources README may compress, never replace:

| Topic | Source of truth |
|-------|-----------------|
| Checkbox setup, plugin strings | CHECKLIST-NEW-PROJECT.md |
| Per-host file paths | PROVIDER-MAPPING.md |
| Full MCP install | MCP-SETUP.md |
| Policy meaning | RULES-CATALOG.md + `examples/policies/` |

## Copy (locked)

**One-liner** (bold, immediately under H1):

> Agent Kit memasang kebijakan agen + mapping host + MCP memori/DB/shell ke repo aplikasi, supaya Cursor / Claude Code / OpenCode / Codex mengikuti aturan yang sama.

**Working example** (`readme-style` “first five lines” = H1 + one-liner + this fence; no `$`):

```text
cp -R path/ke/agent-kit/examples/policies docs/agent-policies
```

Vendor path and Windows sit **immediately after** that fence, still above the feature table. They do not count against the five-line rule.

```text
cp -R docs/agent-kit/examples/policies docs/agent-policies
Copy-Item -Recurse path\ke\agent-kit\examples\policies docs\agent-policies
Copy-Item -Recurse docs\agent-kit\examples\policies docs\agent-policies
```

Do not add `xcopy` as a third dialect. Two Unix lines + two Windows lines, no extra commentary between them except a one-line label if the fence split needs it (e.g. “Windows:”).

## Feature table (locked)

Headers: `Yang didapat` | `Untuk apa`

| Yang didapat | Untuk apa |
|--------------|-----------|
| Policy di `examples/policies/` | Perilaku agen (tanya dulu, security, DB RO, Superpowers, …) |
| Mapping host | File yang sama hidup di Cursor / Claude / OpenCode / Codex |
| MCP | CBM, MemPalace, Claude Mem, RTK, Postgres RO, Figma (UI) |
| Superpowers | Fitur/bug: spec → plan → TDD; plugin wajib per host |
| Figma + Hallmark | UI dari frame yang di-approve, lalu audit; README tetap `readme-style` |

Five rows. No extra marketing rows. No “seamless / robust / comprehensive”.

## Quick Start (app repo)

Numbered list. One key action per step. Link CHECKLIST for full checkboxes.

1. Jangan pasang otomatis — AI ikut `first-setup` (Ya / Tidak / Nanti).
2. Salin `examples/policies/` → `docs/agent-policies/` (perintah di atas).
3. Pilih `stack-backend.postgres.example.md` **atau** `stack-backend.mongo.example.md` → rename `stack-backend.md`.
4. Salin `examples/AGENTS.md` → `AGENTS.md`; isi `CLAUDE.md` dari `examples/CLAUDE.md.template`.
5. Pasang plugin Superpowers di **setiap** host yang dipakai. README lists the four CHECKLIST §A2 lines only:

```text
Cursor: di Agent chat `/add-plugin superpowers` (atau cari “superpowers” di marketplace plugin)
Claude Code: `/plugin install superpowers@claude-plugins-official`
OpenCode: `opencode.json` → `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` lalu restart
Codex App: Plugins → Superpowers. Codex CLI: `/plugins` → search `superpowers` → Install
```

6. Jika ada UI: Figma plugin/MCP. README lists the four CHECKLIST §A3 lines only:

```text
Cursor: `/add-plugin figma` lalu OAuth
Claude Code: `claude plugin install figma@claude-plugins-official` (atau `claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp` + `/mcp` Authenticate)
Codex App: plugin Figma. CLI: `codex mcp add figma --url https://mcp.figma.com/mcp`
OpenCode: remote OAuth **belum** di katalog Figma — kerja UI di host ini **berhenti** sampai Figma allowlist; jangan PAT; Desktop MCP bukan generate
```

   Tanpa MCP Figma → UI baru berhenti.

7. MCP disarankan: CBM, MemPalace, Claude Mem, RTK; Postgres jika project pakai Postgres. Perintah penuh: [MCP-SETUP.md](MCP-SETUP.md).
8. Buka chat agen baru; uji perilaku di CHECKLIST §D.

Stack cursorrules (maks 5–7) mentioned as **opsional** after confirmation, not a numbered Quick Start step.

## Kit vs app (keep, Indonesian)

Keep the existing path diagram, translated:

```text
kit ini:      examples/policies/          ← kanonik
app konsumen: examples/policies → docs/agent-policies
              lalu mapping lewat PROVIDER-MAPPING.md / CHECKLIST-NEW-PROJECT.md
```

One sentence after it: **jangan** salin kebijakan ke `docs/agent-policies/` di dalam repo kit ini (`docs/agent-policies/` dan `.cursor/` di-gitignore). Spec/plan Superpowers di `docs/superpowers/` dilacak git.

Do not present that sentence as the Quick Start.

## Host table (locked)

Columns: `Yang dipasang` | `Cursor` | `Claude Code` | `OpenCode` | `Codex / generic`

| Yang dipasang | Cursor | Claude Code | OpenCode | Codex / generic |
|---------------|--------|-------------|----------|-----------------|
| Overlay policy | `.cursor/rules/*.mdc` (`alwaysApply: true`) mengarah ke `docs/agent-policies/` | `CLAUDE.md` + `docs/agent-policies/` | `AGENTS.md` + `opencode.json` `instructions` | `AGENTS.md` + `docs/agent-policies/` |
| Superpowers | `/add-plugin superpowers` + overlay `superpowers.mdc` | `/plugin install superpowers@claude-plugins-official` | `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` | App: Plugins → Superpowers. CLI: `/plugins` → `superpowers` |
| Figma (jika UI) | `/add-plugin figma` + overlay `figma.mdc` | `claude plugin install figma@claude-plugins-official` | Overlay saja; remote MCP **bukan** katalog → UI **berhenti** | Plugin app atau `codex mcp add figma --url https://mcp.figma.com/mcp` |
| Hallmark | rule `.mdc` + skill `examples/skills/hallmark/` (disalin ke app) | `~/.claude/skills/hallmark/` atau project skills | pointer `SKILL.md` di `AGENTS.md` / `instructions` | `~/.codex/skills/hallmark/` atau `.codex/skills/` |

Notes under the table (short):

- Overlay Superpowers/Figma **bukan** isi skill. Jangan vendor skill Superpowers atau Figma ke git kit.
- Host di luar empat itu: tautan README upstream, bukan checklist kit.
- Detail: [PROVIDER-MAPPING.md](PROVIDER-MAPPING.md).

Host Superpowers/Figma cells must not invent a different command than CHECKLIST. Overlay filenames (`superpowers.mdc`, `figma.mdc`) come from PROVIDER-MAPPING, not from a new invention.

## MCP table (locked)

Columns: `MCP` | `Peran` | `Wajib?` | `Kunci di README`

| MCP | Peran | Wajib? | Kunci di README |
|-----|--------|--------|-----------------|
| Codebase Memory | Graph kode | Disarankan | `codebase-memory-mcp --version` + index project |
| MemPalace | Keputusan / checkpoint | Disarankan | uji `checkpoint` 1× |
| Claude Mem | Observasi sesi (hook + worker) | Disarankan | `npx claude-mem install` + `--provider claude`; **bukan** CMEM Pro |
| RTK | Shell ter-allowlist | Disarankan | uji `rtk --version` + `git status` |
| Postgres | SELECT + schema | Jika project Postgres | user **read-only**; tolak DELETE |
| Figma | Frame sumber UI | Wajib untuk UI di app | remote `https://mcp.figma.com/mcp`; OAuth, bukan PAT |

Notes under the table:

- Jangan commit secret, `.env`, `FIGMA_ACCESS_TOKEN`, `~/.claude-mem/settings.json`.
- Claude Mem hilang → kerja lanjut tanpa search; **bukan** gerbang stop.
- Figma MCP hilang → UI baru **berhenti**; jangan fallback catalog Hallmark.
- Flutter MCP / Playwright-as-MCP: di luar kit. Playwright sebagai perintah boleh lewat allowlist RTK.

Do not paste CBM curl installer, Claude Mem hook paths, or RTK allowlist JSON.

## Policy catalog table (locked)

Columns: `Policy` | `Kapan` | `Bukan untuk`

Rows, in this order:

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

One sentence under the table: path di kit = `examples/policies/`; di app = `docs/agent-policies/`. Isi lengkap: [RULES-CATALOG.md](RULES-CATALOG.md).

Do not paste Koala analogies from the catalog.

## UI flow (locked)

```text
spec Superpowers disetujui
  → Figma (MCP wajib; generate boleh, implement nol file sampai node di-approve)
    → Hallmark (implement dari frame + audit sebelum ship)
```

Bullets:

- Overlay `figma.md` = sumber layout/IA/token. Hallmark = kualitas + `hallmark audit`, bukan pengganti frame.
- Tanpa MCP Figma → **berhenti**. Jangan invent tema catalog Hallmark.
- OpenCode: remote MCP Figma bukan katalog → UI baru **berhenti**; jangan PAT.
- Pengecualian Figma: ejaan/tanda baca di string yang **sudah ada**; README Markdown (`readme-style`); kerja non-UI; **repo kit ini**.
- Responsive desktop+mobile. Placeholder AI bukan “selesai”.
- Build mode (spec+plan Superpowers disetujui) tidak menghapus gerbang node Figma untuk permukaan UI baru.
- Skill Hallmark di-vendor di `examples/skills/hallmark/`; skill Figma/Superpowers **tidak**.

Links: `examples/policies/figma.md`, `examples/policies/hallmark.md`, [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md).

One line: overlay Figma **idle** di repo kit (docs/examples, bukan produk UI).

## Kerja di repo kit (locked)

Not Quick Start. Short:

- Sumber kebijakan: `examples/policies/` (git). `AGENTS.md` / `CLAUDE.md` root memakai folder itu.
- **Jangan** membuat `docs/agent-policies/` atau `.cursor/rules/` di repo ini.
- Spec/plan Superpowers kit: `docs/superpowers/` (dilacak git).
- Edit policy di `examples/policies/`. Jangan vendor skill Superpowers atau Figma.
- Hallmark: vendor di `examples/skills/hallmark/`; update upstream per README skill itu.
- Tidak ada server app / test suite. Tugas docs selesai jika tautan dan fakta selaras CHECKLIST / MCP-SETUP / PROVIDER-MAPPING / RULES-CATALOG.

## Long-doc table (keep, Indonesian role column)

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

Closing two lines (Indonesian, same meaning as today):

- Agen **di repo ini:** `examples/policies/` (lewat `AGENTS.md` root).
- Agen **di app:** `docs/agent-policies/` (kit referensi: repo ini / `docs/agent-kit/` di project lain).

Do not close with “Happy coding!” or equivalent.

## Style rules (`readme-style`)

- Bold one-liner first.
- Working copy in the first five lines.
- Feature table, not a long Feature bullet list.
- Quick Start copy-paste ready. No `$` on bash.
- Vary sentence length. No empty marketing words.
- Do not open with “In today’s…” / “Di era…”.
- Link only files that exist in this git.

Hallmark and Figma overlays do **not** apply to this README rewrite (README Markdown + this kit repo).

## Error handling

| Situation | Behavior |
|-----------|----------|
| CHECKLIST plugin string differs from a “nicer” paraphrase | Use CHECKLIST verbatim. |
| README would need curl/allowlist JSON to be “complete” | Link MCP-SETUP. Do not paste. |
| Reader might copy into **this** kit | Kit-vs-app sentence + Quick Start labeled repo aplikasi. |
| Claude Mem missing | Not a stop gate. Do not say otherwise. |
| Figma MCP missing / OpenCode | UI stop. No Hallmark catalog fallback. No PAT. |
| CMEM Pro | Named only as **bukan** the install path. |
| Mongo example vs Postgres app | `stack-backend` row + Quick Start choose-one. |
| No LICENSE in repo | Do not invent a license section. |
| Screenshot temptation | Skip. Nothing on disk. |

## Files to change

**Edit**

- `README.md` — full rewrite per this spec.

**Do not edit**

- Any other file in this git for this task.
- `examples/policies/readme-style.md` (encoding nits out of scope).

## Manual verification (implementation done)

1. `readme-style`: bold one-liner; `cp -R` (and Windows `Copy-Item`) appear before the feature table; five-row feature table; Quick Start has no `$`.
2. Body language is Indonesian except H1 `Agent Kit` and CLI strings.
3. `rg` Superpowers/Figma plugin lines in README equal CHECKLIST §A2/§A3 strings.
4. `rg` does not introduce `vibe-mvp`, `KhazP`, or CMEM Pro as an install path.
5. README tells the reader not to copy into `docs/agent-policies/` **inside this kit**, and Quick Start copies into an **app**.
6. MCP table names CBM, MemPalace, Claude Mem, RTK, Postgres, Figma with the wajib column above.
7. UI section: spec → Figma → Hallmark; OpenCode stops; no PAT.
8. Line count 150–280 (cap 300). No placeholder words, no screenshot links, no license section.
9. `git status` shows only `README.md` for this task (plus this spec, already committed).
10. Kit still has no tracked `.cursor/` or `docs/agent-policies/`.

## Implementation notes (for the later plan, not this spec’s job)

Ponytail: one file. Replace the current README; do not append a second README. Zero new dependencies. Zero `.cursor` in kit git.

Memory after that work: MemPalace checkpoint; skip CBM `index_repository` unless `[REINDEX]`.
