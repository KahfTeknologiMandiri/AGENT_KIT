# CLAUDE.md

Guidance for Claude Code and other agents working in this repository.

## Project

**agent-kit** — portable Agent Kit for Cursor, Claude Code, OpenCode, and generic agents. Ships policy catalog, provider mapping, MCP memory/DB setup, new-project checklist, and copy-ready examples. Consumers copy `examples/policies/` into their app as `docs/agent-policies/`.

Also follow root `AGENTS.md` and everything under `docs/agent-policies/`.

## Commands

This repo is documentation + examples (no app server). Typical checks:

```bash
# Review kit structure
# Edit policies under docs/agent-policies/ and mirror into .cursor/rules/*.mdc for Cursor
```

When adapting into an **application** repo, replace this section with real install / dev / test / migrate commands.

## Architecture (short)

```text
examples/policies  →  docs/agent-policies (project copy)
                   →  .cursor/rules/*.mdc (Cursor)
                   →  AGENTS.md / CLAUDE.md / opencode.json (other hosts)
examples/skills/hallmark → Cursor hallmark.mdc bridge / Claude·Codex skills dirs
examples/workflows/vibe-mvp → optional idea→MVP (confirm via first-setup first)
examples/workflows/stack-cursorrules → optional PatrickJS rules matched to tech stack
```

- Entry docs: `CHECKLIST-NEW-PROJECT.md`, `PROVIDER-MAPPING.md`, `MCP-SETUP.md`, `RULES-CATALOG.md`
- Portable examples: `examples/` (policies + `skills/hallmark/` + `workflows/`)
- Active project policies: `docs/agent-policies/` (includes `first-setup.md`)
- UI skill: `examples/skills/hallmark/` (see its README for Cursor / Claude / Codex / OpenCode)

## Agent notes

- Do not launch mobile/web apps unless asked.
- Never put secrets in git. Use env vars.
- First-time / multi-provider setup: confirm via `docs/agent-policies/first-setup.md` before copying files.
- Stack cursorrules: `examples/workflows/stack-cursorrules/` — max 5–7; kit policies win over external rules.
- DB via MCP: read-only (see `docs/agent-policies/database-readonly.md`).
- UI/landing: Hallmark (`examples/skills/hallmark/SKILL.md`); README prose: `readme-style`.
- After meaningful work: MemPalace checkpoint; re-index CBM only when kit structure changes a lot or `[REINDEX]`.
