# CLAUDE.md

Guidance for Claude Code and other agents working in this repository.

## Project

**agent-kit** — portable Agent Kit for Cursor, Claude Code, OpenCode, and generic agents. Ships policy catalog, provider mapping, MCP memory/DB setup, new-project checklist, and copy-ready examples.

**This repo:** follow root `AGENTS.md` and `examples/policies/`.  
**Application repos:** consumers copy `examples/policies/` → `docs/agent-policies/` (after first-setup confirmation), then follow that copy.

Do not create `docs/agent-policies/` or `.cursor/rules/` inside this kit repo.

## Commands

This repo is documentation + examples (no app server). Typical checks:

```bash
# Review kit structure
# Edit policies under examples/policies/ (canonical). Consumer apps copy to docs/agent-policies/
# and mirror into .cursor/rules/*.mdc for Cursor — do that in the app, not here.
```

When adapting into an **application** repo, replace this section with real install / dev / test / migrate commands.

## Architecture (short)

```text
this kit:     examples/policies/     ← canonical (git)
              examples/skills/hallmark/
              examples/workflows/

consumer app (after first-setup):
              examples/policies  →  docs/agent-policies (project copy)
                                 →  .cursor/rules/*.mdc (Cursor)
                                 →  AGENTS.md / CLAUDE.md / opencode.json (other hosts)
```

- Entry docs: `CHECKLIST-NEW-PROJECT.md`, `PROVIDER-MAPPING.md`, `MCP-SETUP.md`, `RULES-CATALOG.md`
- Portable examples: `examples/` (policies + `skills/hallmark/` + `workflows/`)
- Active policies **in this kit:** `examples/policies/` (includes `first-setup.md`)
- Active policies **in an app:** `docs/agent-policies/`
- Superpowers overlay: `examples/policies/superpowers.md` (plugin wajib di host; jangan vendor skill)
- UI skill: `examples/skills/hallmark/` (see its README for Cursor / Claude / Codex / OpenCode)

## Agent notes

- Do not launch mobile/web apps unless asked.
- Never put secrets in git. Use env vars.
- First-time / multi-provider setup: confirm via `examples/policies/first-setup.md` before copying files. Apply into an **app**, not into this kit.
- Stack cursorrules: `examples/workflows/stack-cursorrules/` — max 5–7; kit policies win over external rules.
- DB via MCP: read-only (see `examples/policies/database-readonly.md`).
- UI/landing/shell: Hallmark (`examples/policies/hallmark.md` + `examples/skills/hallmark/SKILL.md`) — responsive + audit; README prose: `readme-style`.
- Superpowers: fitur/bug → overlay + plugin. Ask-first: first-setup + non-fitur. UI: spec dulu, lalu Hallmark. **Build mode** setelah spec+plan Superpowers disetujui (kerja sampai DoD, jangan tanya per modul).
- After meaningful work: MemPalace checkpoint; re-index CBM only when kit structure changes a lot or `[REINDEX]`.
