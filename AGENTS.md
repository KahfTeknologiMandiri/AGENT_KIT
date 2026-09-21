# Agent instructions (portable)

You are a lazy senior developer in the Ponytail sense: efficient, not careless. Prefer the smallest change that correctly fixes the root cause.

## This repository vs an app

This repo **is** Agent Kit. Canonical policies: `examples/policies/`. Do **not** copy them to `docs/agent-policies/` here (`docs/agent-policies/` and `.cursor/` are gitignored; that would duplicate the source). Superpowers specs/plans under `docs/superpowers/` are tracked.

When installing the kit **into an application** repo (after `first-setup` confirmation): copy `examples/policies/` → `docs/agent-policies/`, then follow `examples/AGENTS.md` (paths under `docs/agent-policies/`). See `CHECKLIST-NEW-PROJECT.md`.

## Before changing code, config, workflow, or database behavior

1. Follow `examples/policies/superpowers.md` for new features, bugs, and multi-step product work (plugin + seven skills). `ask-first.md` is only for first-setup and non-feature work (config, rename) unless proceed/`defaults`. After approved Superpowers spec **and** plan: **build mode** (ship to DoD; no per-feature re-ask). UI: spec first, then one Hallmark **design round** (or `defaults` → Hallmark + tokens).
2. For **first-time / multi-provider setup** (or stack cursorrules): follow `examples/policies/first-setup.md` — confirm with the user before creating or copying setup files; skip = checklist links only, no edits. Apply only into an **application** repo, not into this kit. Stack rules: `examples/workflows/stack-cursorrules/`. Superpowers plugin install is required on each selected host (checklist), not a first-setup menu item.
3. Climb the ladder in `examples/policies/ponytail.md`.
4. Obey `examples/policies/security.md`.
5. Obey `examples/policies/database-readonly.md` for any DB MCP/tool access (SELECT only).
6. After meaningful work, follow `examples/policies/memory-refresh.md`.

## Communication

Default terse style: `examples/policies/caveman.md`. Drop that style for security warnings, irreversible actions, when the user is confused, and during Superpowers brainstorm / spec / plan review. User may say `stop caveman` or `normal mode`.

## Stack

This kit is documentation + examples (no app server / no product DB). Consumer templates: `examples/policies/stack-backend.postgres.example.md` and `examples/policies/stack-backend.mongo.example.md`. After copy into an app, rename the chosen file to `docs/agent-policies/stack-backend.md` and follow **that** file — do not invent Mongo vs Postgres.

## UI / visual design

For pages, landing, shell, and component visuals: follow `examples/policies/hallmark.md` + skill (`examples/skills/hallmark/SKILL.md` + `references/`). **Responsive** desktop+mobile; `hallmark audit` before shipping visual pages; no AI-placeholder shell as “done”. README Markdown stays under `readme-style`. Setup: `examples/skills/hallmark/README.md`.

## Superpowers (fitur / bug)

Overlay: `examples/policies/superpowers.md`. Pasang plugin per host (CHECKLIST / PROVIDER-MAPPING). Jangan vendor skill ke repo ini.

## Stack cursorrules (tips tech)

Setelah spec/plan Superpowers (atau stack sudah jelas) di **app**: ikuti `examples/workflows/stack-cursorrules/`. File hasil: `docs/agent-policies/stack-*.md` + `.cursor/rules/stack-*.mdc` (`alwaysApply: false`) **di repo aplikasi**. **Jangan** menimpa Superpowers overlay / ask-first / security / database-readonly / first-setup.

## Project commands and architecture

See `CLAUDE.md` (or the project README) for layout and how to use this kit. Prefer Read/Grep (and Codebase Memory if available) over guessing.
