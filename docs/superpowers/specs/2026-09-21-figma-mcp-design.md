# Figma MCP — design

Date: 2026-09-21  
Status: approved in brainstorming (user `ok` on sections 1–3; approach 1)  
Repo: Agent Kit (`examples/policies/` = canonical policies)  
Upstream MCP: [Figma MCP server](https://developers.figma.com/docs/figma-mcp-server/) (remote `https://mcp.figma.com/mcp`)  
Upstream catalog / install: [remote server installation](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/)

## Problem

Hallmark is the kit’s UI skill (anti-slop, responsive, audit). It does not read Figma files. `hallmark study` takes a screenshot or URL. Figma appears in Hallmark only as SVG-export craft.

Figma MCP is already usable on this machine (Cursor plugin) and is a first-class design-to-code / code-to-canvas bridge. The kit never names it. `MCP-SETUP.md` and `PROVIDER-MAPPING.md` §G treat Flutter / Playwright / extra skills as out of scope. Agents that follow the kit invent Hallmark catalog themes instead of using an approved Figma frame.

The owner wants Figma as the **official visual source** for new UI in apps that use the kit, on all four mapped hosts, with Hallmark as the quality gate — not a replacement for Figma.

## Goal

Add a **thin Figma overlay policy** plus MCP install docs. Do not vendor Figma skills. Do not patch upstream Hallmark `SKILL.md`.

Success: after implementation, (1) `examples/policies/figma.md` exists and apps copy it with the other policies, (2) UI work in those apps follows spec → Figma (create if needed) → user approves frame → implement → Hallmark quality + `hallmark audit`, (3) missing Figma MCP **stops** UI work (no Hallmark-catalog fallback), (4) CHECKLIST + MCP-SETUP + PROVIDER-MAPPING name official install for Cursor, Claude Code, Codex, and an honest OpenCode constraint, (5) `examples/mcp.example.json` has a URL stub with no secrets, (6) Hallmark skill files in git are unchanged.

## Non-goals

- Vendor Figma skills, plugin, or `SKILL.md` bodies into this git (`examples/skills/figma/` is forbidden).
- Edit `examples/skills/hallmark/SKILL.md` or `references/` (Nutlope upstream).
- Create `docs/agent-policies/` or `.cursor/` in this kit repo.
- Add Figma as a first-setup **menu** item (it is a checklist requirement, like Superpowers).
- Automatic code → Figma sync after implementation.
- Make Hallmark optional or skip `hallmark audit`.
- Third-party Figma MCP servers, PATs, or API tokens in git.
- Use Figma Desktop MCP as the documented generate path (`use_figma` / `generate_figma_design` are remote-only).
- Automated tests (kit is documentation).
- CBM re-index for this docs-only change unless `[REINDEX]`.
- Mandate Figma on hosts the kit does not map. Link Figma’s catalog / README only.
- Apply this overlay by copying policies into an application from this kit session (`first-setup` still wins).

## Decisions

| ID | Choice |
|----|--------|
| 1 | Approach 1: new overlay `examples/policies/figma.md` + pointers. Not folded into Hallmark. Not a heavy vendored pack. |
| 2 | Figma is mandatory for **all new or changed user-facing visuals** in consumer apps (pages, landing, shell, visual components). Hallmark must not invent a catalog theme when no approved frame exists. |
| 3 | If spec is approved but no frame exists: agent **may create** Figma content from the spec via official MCP tools, then **wait for the user to approve** the frame (URL + node) before writing UI code. |
| 4 | After approval: Figma owns layout, IA, components, and tokens for that surface. Hallmark **must** fix slop / a11y / responsive failures **without** changing the approved macrostructure. If a Hallmark fix would change macrostructure → stop and ask. |
| 5 | Policy intent: Figma MCP required on all four kit hosts; missing MCP → stop. OpenCode is **not** on Figma’s remote OAuth allowlist today — document that and stop; do not invent a working OpenCode remote setup. |
| 6 | Ask-first visual round becomes **approve the Figma frame**. `defaults` on a spec still requires generate + frame approve; it does not skip Figma. |
| 7 | Build mode: do not re-ask design per page. Each new UI surface still needs an approved Figma node. Hallmark audit still runs. |
| 8 | Official Figma **skills on the host** (Cursor `/add-plugin figma`, Claude `figma@claude-plugins-official`, …) are required when calling Figma tools. Kit only points; it does not copy skill text. |
| 9 | `security.md` wins: OAuth in the host, never Figma tokens in git. |
| 10 | Exceptions: spelling/punctuation in **existing** strings; README Markdown (`readme-style`); non-UI work; **this kit repo** (docs/examples). New copy blocks, layout, color, type, or new components are visual → Figma. |

## Architecture

```text
this kit (git):
  examples/policies/figma.md          ← overlay (new)
  examples/policies/hallmark.md       ← quality gate after Figma
  examples/skills/hallmark/           ← unchanged upstream skill
  MCP-SETUP / CHECKLIST / mapping     ← install pointers
  examples/mcp.example.json           ← figma URL stub

consumer app (after first-setup):
  docs/agent-policies/figma.md
  Cursor: .cursor/rules/figma.mdc (alwaysApply: true) → overlay, not Figma skill body
  host plugin / MCP: Figma official (not in this git)
```

Roles:

| Piece | Job |
|-------|-----|
| Superpowers spec | What to build; brand/tone/copy intent as brief |
| Figma overlay + MCP | Visual source of truth (frame/node) |
| Official Figma host skills | How to call `use_figma`, `get_design_context`, Code Connect, … |
| Hallmark skill + policy | Anti-slop, responsive, a11y; `hallmark audit` before ship |
| `readme-style` | README Markdown only |

Priority after this change (top wins):

1. `security`, `database-readonly`, `first-setup`
2. Superpowers (features / bugs)
3. `ask-first` (first-setup + non-feature only)
4. Ponytail (diff size; does not skip Figma or Hallmark on UI)
5. **Figma overlay** (visual source after spec)
6. Hallmark (quality + audit)
7. Caveman (off during brainstorm / spec / plan review)

## Agent flow

Applies in **application** repos that copied the kit. Does **not** apply to editing this kit’s markdown/examples.

```text
non-UI / spelling in existing strings / README Markdown
  → existing Superpowers or ask-first; Figma overlay idle

user-facing visual (new or change)
  → Superpowers spec (or defaults on spec)
  → Figma MCP connected on this host?
        no  → STOP. Install per CHECKLIST / MCP-SETUP.
              OpenCode: STOP. Name Figma client catalog / waitlist.
              Do not invent PAT. Do not Hallmark-catalog the UI.
  → approved frame/node?
        no  → search_design_system first (official Figma skill).
              Create file/frame from spec (use_figma / generate_figma_design /
              create_new_file as the host skill directs).
              STOP until user approves URL + node.
        user pasted a node URL, or spec already names an approved node
              → treat as approve
        file URL without node-id → ask which node; do not implement the whole file
  → implement: Figma = layout, IA, components, tokens for that surface
  → map to existing app components; do not duplicate (Figma design-to-code: adapt)
  → Hallmark: fix slop / a11y / responsive; do not change approved macrostructure
  → hallmark audit before treating the surface as done
```

`defaults` on spec: generate Figma from the spec, then still wait for frame approval.

User rejects the frame: revise in Figma; do not implement.

`hallmark redesign` on a Figma-sourced surface: requires explicit confirmation (it breaks the visual contract).

Token scope: apply Figma tokens on the surface being built. Do not rewrite the app’s global token file unless the spec says so.

Later edits to a screen that **already has** an approved node: implement (or Hallmark-fix) from that node. If the requested look **deviates** from the approved frame, update Figma and wait for re-approve before code. Do not invent a second Hallmark theme.

## Overlay contents (`examples/policies/figma.md`)

Write in **Indonesian**, same register as `hallmark.md` / `superpowers.md`. Include:

- When it applies (user-facing visual in a **consumer app**).
- MCP missing → stop; point to CHECKLIST / MCP-SETUP / PROVIDER-MAPPING.
- OpenCode / non-catalog client → stop; do not invent credentials.
- Generate-then-approve; pasted node URL = approve.
- Figma vs Hallmark conflict rule (decision 4).
- Exceptions (decision 10).
- Plugin/skills: follow host Figma skills; kit does not vendor them.
- Do not override `security`, `database-readonly`, `first-setup`, Superpowers spec gate.

Keep it short. Long install commands live in MCP-SETUP + CHECKLIST + mapping only.

## Files to change

Match each file’s existing language. Do not rewrite a file into another language.

**Create**

- `examples/policies/figma.md` — overlay as above.

**Edit**

- `examples/policies/hallmark.md` — gate: approved Figma frame first; Hallmark does not pick catalog themes for kit-app UI; quality without macrostructure change; audit still required.
- `examples/policies/ask-first.md` — design round = approve Figma frame, not Hallmark catalog / vibe questions. `defaults` still requires Figma generate + approve.
- `examples/policies/superpowers.md` — priority list: Figma overlay after Ponytail, Hallmark after Figma. One sentence: UI after spec goes through `figma.md` then Hallmark.
- `examples/policies/first-setup.md` — Figma is **not** a menu item. Checklist requires official Figma plugin/MCP on each selected host (same sentence pattern as Superpowers). Skip setup still means no file copies.
- Root `AGENTS.md` and `examples/AGENTS.md` — UI section: Figma source + Hallmark audit; README stays `readme-style`.
- `CLAUDE.md` — architecture / agent notes: Figma overlay + Hallmark; do not create `.cursor/` in the kit.
- `MCP-SETUP.md` — new **Figma** section after RTK; renumber Troubleshooting and after-task. Header table: Figma, required for UI. Body: remote URL, OAuth, four hosts, OpenCode constraint, remote vs desktop tool conflict, `use_figma` remote-only.
- `CHECKLIST-NEW-PROJECT.md` — per-host Figma plugin/MCP checkbox in §B (required, like Superpowers — not an optional MCP next to Postgres). §D: UI without MCP → stop; without frame → generate and wait (zero UI files); after approve → implement + `hallmark audit`; spelling typo in existing copy → no Figma.
- `PROVIDER-MAPPING.md` — table row for Figma overlay + MCP; Cursor `.mdc` points at overlay; remove Figma from “out of kit” if it was implied; keep Flutter/Playwright out of kit.
- `RULES-CATALOG.md` — entry after Hallmark: Figma overlay (analogi + when + stop-gate).
- `README.md` — list overlay + MCP-SETUP blurb includes Figma for UI.
- `examples/mcp.example.json` — `"figma": { "url": "https://mcp.figma.com/mcp" }` (or host-equivalent HTTP shape). No token, no `FIGMA_ACCESS_TOKEN`.
- `examples/skills/hallmark/README.md` — one priority row: Figma overlay wins as visual source; Hallmark remains quality/audit.

**Do not edit**

- `examples/skills/hallmark/SKILL.md` and `references/`.
- `examples/CLAUDE.md.template` (no UI section today).
- Any `.cursor/` path in this repo.

## Install (checklist + MCP-SETUP + mapping)

Canonical source: [Figma remote MCP installation](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/). If upstream commands change, update CHECKLIST + MCP-SETUP + PROVIDER-MAPPING. Do not keep a fourth long copy in `AGENTS.md`.

Preferred commands **as of this spec** (replace from upstream if they drift):

| Host | Preferred | Manual |
|------|-----------|--------|
| Cursor | `/add-plugin figma` in Agent chat | MCP URL `https://mcp.figma.com/mcp` + OAuth |
| Claude Code | `claude plugin install figma@claude-plugins-official` | `claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp` then `/mcp` Authenticate |
| Codex | Figma plugin in Codex app | `codex mcp add figma --url https://mcp.figma.com/mcp` |
| OpenCode | **Not supported** on Figma’s remote client allowlist. Overlay: stop. Link catalog / waitlist. Do not document a PAT. Desktop MCP is not the generate path. |

Other hosts: link the Figma catalog. Kit does not invent waitlist clients.

Recommend **remote** server. If both desktop (`http://127.0.0.1:3845/mcp`) and remote are configured, clients may hide remote-only tools — MCP-SETUP troubleshooting: remove or rename the desktop entry when generate/write-to-canvas is required.

Verify: host shows Figma MCP connected; one tool list includes Figma tools; OAuth done in the host, not in git.

## Error handling

| Situation | Behavior |
|-----------|----------|
| Figma MCP disconnected or auth failed | Stop UI work. Install/auth. No Hallmark catalog UI. |
| OpenCode (or other non-catalog client) | Stop. Explain catalog. No PAT in git. |
| Generate Figma failed | Stop. Do not implement. |
| User rejects frame | Revise Figma. Do not implement. |
| File URL, no node-id | Ask which node. |
| Hallmark fix would change approved macrostructure | Stop and ask. |
| `hallmark redesign` on Figma-sourced UI | Need explicit user confirmation. |
| Existing `Button.tsx` vs new Figma button | Map / reuse. Do not duplicate. |
| Figma tokens vs global app tokens | Scope to this surface unless spec says rewrite globals. |
| Secret in MCP config | `security.md` wins. Example JSON is URL only. |
| Work is this kit’s docs | Figma overlay idle. |
| Feature work without Superpowers plugin | Superpowers overlay still stops first. |
| UI in build mode, no node for a new surface | Still stop / generate+approve. Do not skip Figma because build mode is on. |

## Manual verification (implementation done)

1. `grep`: overlay file exists; Figma named in hallmark/ask-first/superpowers/first-setup pointers, MCP-SETUP, CHECKLIST, PROVIDER-MAPPING, RULES-CATALOG, README, both AGENTS.md, CLAUDE.md, Hallmark README. No Figma skill tree under `examples/skills/figma/`.
2. `examples/skills/hallmark/SKILL.md` git diff empty for this work.
3. `examples/mcp.example.json` parses; `figma` stub is URL-only; no token.
4. Kit still has no tracked `.cursor/` or `docs/agent-policies/`.
5. Checklist §D includes: UI without MCP → stop; generate then wait (no UI files); audit after implement; typo without Figma.
6. OpenCode text does not claim remote OAuth works.
7. Superpowers still required for features; Figma does not replace spec.
8. Install commands match current Figma docs (or are clearly “copy from upstream”).

## Implementation notes (for the later plan, not this spec’s job)

Ponytail: one new policy file; shortest diffs on pointers. Zero dependencies. Zero `.cursor` in kit git. Zero Hallmark skill edits.

Memory after that work: MemPalace checkpoint; skip CBM `index_repository` unless `[REINDEX]`.
