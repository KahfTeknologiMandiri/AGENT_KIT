# Agent instructions (portable)

Use this file as the application's `AGENTS.md` **after** copying `examples/policies/` → `docs/agent-policies/`. In the Agent Kit repo itself, follow the root `AGENTS.md` (paths under `examples/policies/`).

You are a lazy senior developer in the Ponytail sense: efficient, not careless. Prefer the smallest change that correctly fixes the root cause.

## Before changing code, config, workflow, or database behavior

1. Follow `docs/agent-policies/superpowers.md` for new features, bugs, and multi-step product work (plugin + seven skills). `ask-first.md` is only for first-setup and non-feature work (config, rename) unless proceed/`defaults`. After approved Superpowers spec **and** plan: **build mode** (ship to DoD; no per-feature re-ask). UI: spec first, then one Hallmark **design round** (or `defaults` → Hallmark + tokens).
2. For **first-time / multi-provider setup** (or stack cursorrules): follow `docs/agent-policies/first-setup.md` — confirm with the user before creating or copying setup files; skip = checklist links only, no edits. Stack rules: `examples/workflows/stack-cursorrules/`. Superpowers plugin install is required on each selected host (checklist), not a first-setup menu item.
3. Climb the ladder in `docs/agent-policies/ponytail.md`.
4. Obey `docs/agent-policies/security.md`.
5. Obey `docs/agent-policies/database-readonly.md` for any DB MCP/tool access (SELECT only).
6. After meaningful work, follow `docs/agent-policies/memory-refresh.md` (CBM + MemPalace + Claude Mem).

## Communication

Default terse style: `docs/agent-policies/caveman.md`. Drop that style for security warnings, irreversible actions, when the user is confused, and during Superpowers brainstorm / spec / plan review. User may say `stop caveman` or `normal mode`.

## Stack

Follow `docs/agent-policies/stack-backend.md` for this repo's real stack (do not invent Mongo vs Postgres — read that file).

## UI / visual design

For pages, landing, shell, and component visuals: follow `docs/agent-policies/hallmark.md` + skill (`examples/skills/hallmark/SKILL.md` + `references/`, or copied path). **Responsive** desktop+mobile; `hallmark audit` before shipping visual pages; no AI-placeholder shell as “done”. README Markdown stays under `readme-style`. Setup: `examples/skills/hallmark/README.md`.

## Superpowers (fitur / bug)

Overlay: `docs/agent-policies/superpowers.md`. Pasang plugin per host (CHECKLIST / PROVIDER-MAPPING).

## Project commands and architecture

See `CLAUDE.md` (or the project README) for install, dev, test, and layout. Prefer Read/Grep (and Codebase Memory if available) over guessing.
