# Superpowers overlay — design

Date: 2026-09-21  
Status: approved in brainstorming (user `ok` on sections 1–5)  
Repo: Agent Kit (`examples/policies/` = canonical policies)  
Upstream plugin: [obra/superpowers](https://github.com/obra/superpowers) (MIT)

## Problem

Kit has no Superpowers. Feature work today goes through `ask-first` (3–7 A/B/C) and optional vibe-MVP (KhazP PRD). Superpowers already covers think-before-code (brainstorm → spec → plan → TDD). Two “don’t code yet” pipelines confuse agents. Hallmark stays for UI visuals.

## Goal

Make Superpowers the official feature/bug pipeline for this kit and for apps that copy it. Ship a **pointer + overlay policy**, not a vendored skill tree.

Success: after implementation, a consumer app (and this kit repo) has (1) overlay policy in git, (2) checklist/mapping that names how to install the plugin on Cursor / Claude Code / OpenCode / Codex, (3) zero vibe-MVP workflow, (4) kit safety policies still win over Superpowers.

## Non-goals

- Vendor `examples/skills/superpowers/` or copy SKILL.md bodies into the kit.
- Create `examples/workflows/superpowers/` (rejected; thin overlay only).
- Put `docs/agent-policies/` or `.cursor/rules/` in this kit’s git.
- Rewrite Hallmark skill files (including “Vibe answer” in `custom-theme.md` — that is a theme prompt, not KhazP vibe-MVP).
- Edit `examples/CLAUDE.md.template` (no vibe mention today).
- Automated tests (kit is documentation).
- CBM re-index for this docs-only change unless `[REINDEX]`.
- Mandate Superpowers on hosts the kit does not already map (Antigravity, Gemini, …). Link upstream README only.

## Decisions

| ID | Choice |
|----|--------|
| 1a | Pointer + overlay. Do not vendor skills. |
| 2b | Superpowers replaces the **feature** workflow. Kit wins only `security`, `first-setup`, `database-readonly`. |
| 3b | Overlay always on. Checklist requires plugin install. Missing plugin → stop, do not silently fall back to feature `ask-first`. |
| 4a | Seven required skills (below). Other Superpowers skills optional, not forbidden. |
| 5c | Superpowers writes spec/plan. KhazP PRD not used. |
| 6a | Delete `examples/workflows/vibe-mvp/` and all kit links to it. |
| 7a | `ask-first` remains for first-setup + non-feature work (config, rename). One-line typo still immediate. |
| 8a | Install steps for all four mapped hosts. |
| 9a | TDD = order. Ponytail = diff size. Ponytail must not skip tests. |
| 10a | Superpowers spec first. Visual pages still Hallmark + audit before ship. |
| A | One new policy file. Install text lives in CHECKLIST + PROVIDER-MAPPING. No new workflow folder. |

## Required skills

Exact names from the plugin:

1. `using-superpowers`
2. `brainstorming` — new features / product-shaped work
3. `systematic-debugging` — bugs
4. `writing-plans` — after spec approved
5. `executing-plans` — after plan approved
6. `test-driven-development` — during execute
7. `verification-before-completion` — before claiming done

Optional (not in overlay must-list): git worktrees, code review pair, subagent-driven-development, dispatching-parallel-agents, finishing-a-development-branch, writing-skills.

## Priority (high wins)

1. `security`, `database-readonly`, `first-setup` — always. Superpowers must not skip setup confirmation, secrets-in-git, or MCP writes.
2. Feature / bug / multi-step work — the seven skills, in the order above.
3. `ask-first` — first-setup and non-feature only. Not 3–7 A/B/C for new features.
4. Ponytail — smallest correct diff. Does not skip TDD.
5. Hallmark — after spec, for user-facing visual surfaces + `hallmark audit` before ship.
6. Caveman — off during brainstorm / spec / plan review (need full sentences). Resume after execute starts. Security / irreversible / confused: always off.

## Architecture

```text
obra/superpowers (plugin per host, not in this git)
        ↑ install (checklist)
Agent Kit overlay: examples/policies/superpowers.md
        ↓ copy in APP only
docs/agent-policies/superpowers.md
        + Cursor APP: .cursor/rules/superpowers.mdc (alwaysApply: true)
        = overlay text, not skill bodies

App artifacts (Superpowers defaults, not kit git):
  docs/superpowers/specs/
  docs/superpowers/plans/   (or whatever the installed plugin version writes)
```

This kit repo: same overlay via root `AGENTS.md` → `examples/policies/superpowers.md`. Agents working **in the kit** follow Superpowers for kit changes.

### Gitignore exception (kit only)

`.gitignore` must keep ignoring consumer copies (`docs/agent-policies/`, everything else under `docs/`) but **track** Superpowers artifacts for this repo:

```gitignore
docs/*
!docs/superpowers/
!docs/superpowers/**
```

Do not ignore the whole `docs` directory name (that blocks un-ignore). Pattern is `docs/*` plus the two bangs. `.cursor` stays fully ignored.

## Overlay file

**Add:** `examples/policies/superpowers.md`

Contents (Indonesian, same register as `hallmark.md` / `first-setup.md`):

- When it applies: new feature, bug, multi-step implementation.
- Priority table (section above).
- Named seven skills + “plugin must be installed on this host”.
- Missing plugin: stop; point to CHECKLIST / PROVIDER-MAPPING; do not run feature `ask-first`.
- Spec/plan paths: follow the installed plugin (default `docs/superpowers/specs/` and plans). Do not invent a kit-specific spec root for **apps**.
- Build mode: user approved **spec and plan** → execute to DoD (`executing-plans` + TDD). No per-module re-ask. Replaces “PRD + Tech Design vibe”.
- Pointers: `ask-first` leftover, Ponytail, Hallmark, security, first-setup, DB RO.
- Upstream: https://github.com/obra/superpowers — refresh = reinstall plugin, not a kit vendor bump.

**App after first-setup:** copy with the rest of `examples/policies/` → `docs/agent-policies/superpowers.md`. Cursor rule in the **app**: `.cursor/rules/superpowers.mdc` with `alwaysApply: true`, body = overlay (or include), path to overlay file. Kit git does not contain that `.mdc`.

## first-setup and ask-first

### `examples/policies/first-setup.md`

- Remove vibe menu items (current options 2–3: vibe only / both).
- Menu becomes: (1) kit policies (2) stack cursorrules (3) skip. 1+2 allowed.
- Replace “policy harian vs alur vibe” with “policy harian vs rule stack”.
- Skip path: checklist + PROVIDER-MAPPING only. No vibe pointer.
- Superpowers is **not** a menu. It is required whenever the user applies kit setup: checklist must include plugin install for each selected host.
- After apply: verify plugin/skills present + overlay active, plus existing host smoke tests.

### `examples/policies/ask-first.md`

- Rewrite scope: new features do **not** use 3–7 A/B/C. That is Superpowers brainstorming/spec.
- Remainder: first-setup, non-feature (config, rename), one-line typo exception unchanged.
- UI: Hallmark after Superpowers spec (do not replace spec with a separate vibe-style design round). One design round / `defaults` = Hallmark + tokens still applies **after** spec exists.
- Build mode trigger: approved Superpowers spec **and** plan, not KhazP PRD.
- first-setup trigger list: drop “ide → PRD → MVP”.

## Files to change

**Add**

- `examples/policies/superpowers.md`

**Delete**

- `examples/workflows/vibe-mvp/` (entire folder)

**Edit (strip vibe-MVP; add Superpowers overlay)**

- `examples/policies/first-setup.md`
- `examples/policies/ask-first.md`
- `examples/policies/ponytail.md` — 1–2 sentences: TDD order from overlay; Ponytail does not skip tests
- `examples/policies/hallmark.md` — 1–2 sentences: spec first, then Hallmark for visual pages
- `AGENTS.md` (root) — remove Vibe MVP section; point to overlay + seven skills
- `examples/AGENTS.md` — same, paths under `docs/agent-policies/` for apps
- `CLAUDE.md` — replace vibe pointer; build mode = spec+plan
- `README.md`, `examples/README.md`
- `CHECKLIST-NEW-PROJECT.md` — required Superpowers install block; test item D; delete vibe line
- `PROVIDER-MAPPING.md` — table row + per-host install commands (copy from upstream README, do not invent)
- `RULES-CATALOG.md` — new Superpowers entry; drop vibe from first-setup blurb
- `examples/workflows/stack-cursorrules/README.md` — detection source: Superpowers spec/plan (`docs/superpowers/specs/`, plans) + user + scan repo. Drop vibe-mvp. Kit policies still beat community stack rules. Superpowers does not override security / first-setup / DB RO.

**Do not edit**

- `examples/skills/hallmark/**` (README has no vibe-MVP; leave `custom-theme.md` “Vibe answer” alone)
- `examples/CLAUDE.md.template`

## Install commands (checklist + mapping)

Copy from [obra/superpowers README](https://github.com/obra/superpowers). Install **per host**.

| Host | Command / UI |
|------|----------------|
| Cursor | Agent chat: `/add-plugin superpowers` (or search “superpowers” in the plugin marketplace) |
| Claude Code | `/plugin install superpowers@claude-plugins-official` |
| OpenCode | `opencode.json`: `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]` then restart |
| Codex | App: Plugins → Superpowers. CLI: `/plugins` → search `superpowers` → Install |

If upstream README changes, update CHECKLIST + PROVIDER-MAPPING to match. Do not keep a third copy of commands in `AGENTS.md` (one sentence + pointer).

Generic-only consumers: same overlay file; they must still install Superpowers on the host they actually run.

## Error handling

| Situation | Behavior |
|-----------|----------|
| Superpowers skills not loaded | Stop. Tell user to install for this host. Link CHECKLIST/PROVIDER-MAPPING. Do not implement the feature. Do not run 3–7 feature `ask-first`. |
| User asks to skip Superpowers for a feature | Overlay is mandatory (3b). Refuse skip unless they change kit policy. |
| Secret / MCP write / first-setup apply | Kit policy wins. Superpowers brainstorming does not authorize copying policies into an app or writing secrets. |
| Visual page after spec | Hallmark + responsive + audit. Ponytail cannot skip Hallmark for user-facing UI. |
| Small typo / one-line | Immediate, as today. |
| Non-feature config | `ask-first` A/B/C allowed. |

## Manual verification (implementation done)

1. `grep` vibe-mvp / KhazP vibe workflow in the kit — no hits except Hallmark “Vibe answer”.
2. New-feature prompt in a host with plugin → Superpowers brainstorm/spec, not a large diff.
3. Same prompt with plugin disabled → stop + install, not feature `ask-first`.
4. “Hardcode API key” → refuse.
5. “Delete row via MCP” → refuse.
6. Rename/typo → may proceed; non-feature config may `ask-first`.
7. UI task → spec, then Hallmark + audit language present in overlay/hallmark.
8. first-setup menu has no vibe option; Superpowers install is on the checklist.

## Implementation notes (for the later plan, not this spec’s job)

Ponytail: shortest diffs on existing docs. One new markdown policy. Delete vibe folder. No new dependencies. No `.cursor` in kit git.

Memory after that work: MemPalace checkpoint; skip CBM re-index unless structure churn is large or `[REINDEX]`.
