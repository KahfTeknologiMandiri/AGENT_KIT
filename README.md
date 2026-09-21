# Agent Kit

**Portable rules + MCP memory/DB/shell setup for Cursor, Claude Code, OpenCode, and generic agents.**

```text
this kit:     examples/policies/          ← canonical
consumer app: examples/policies → docs/agent-policies
              then map via PROVIDER-MAPPING.md / CHECKLIST-NEW-PROJECT.md
```

Do **not** copy policies into `docs/agent-policies/` inside this kit (that path is gitignored). Superpowers specs/plans under `docs/superpowers/` are tracked.

| Doc | Role |
|-----|------|
| [CHECKLIST-NEW-PROJECT.md](CHECKLIST-NEW-PROJECT.md) | Setup checklist for an **app** (mulai dari konfirmasi §0) |
| [PROVIDER-MAPPING.md](PROVIDER-MAPPING.md) | Where files go per host (after copy into the app) |
| [MCP-SETUP.md](MCP-SETUP.md) | CBM + MemPalace + Claude Mem + RTK + Postgres RO + Figma (UI) |
| [RULES-CATALOG.md](RULES-CATALOG.md) | What each policy does |
| [examples/policies/first-setup.md](examples/policies/first-setup.md) | Setup pertama: tanya dulu, baru pasang (ke app) |
| [examples/](examples/) | Copy-ready templates |
| [examples/policies/superpowers.md](examples/policies/superpowers.md) | Overlay Superpowers (plugin wajib per host; bukan vendor skill) |
| [examples/policies/figma.md](examples/policies/figma.md) | Overlay Figma (sumber visual UI; plugin/MCP per host; bukan vendor skill) |
| [examples/workflows/stack-cursorrules/](examples/workflows/stack-cursorrules/) | Opsional: rule stack dari awesome-cursorrules (sesuai tech) |
| [examples/skills/hallmark/README.md](examples/skills/hallmark/README.md) | Hallmark UI skill setup (Cursor / Claude / Codex / OpenCode) |

AI agents **in this repo:** `examples/policies/` (via root `AGENTS.md`).  
AI agents **in an app:** `docs/agent-policies/` (kit referensi: repo ini / `docs/agent-kit/` di project lain).
