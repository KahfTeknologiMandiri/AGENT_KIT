# Claude Mem — design

Date: 2026-09-21  
Status: approved in brainstorming (user `ok` on sections 1–4)  
Repo: Agent Kit (`examples/policies/` = canonical policies)  
Upstream: [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) (Apache-2.0)

## Problem

The kit’s memory stack is three layers: files on disk, Codebase Memory (CBM) graph, and MemPalace checkpoints. Claude Mem is already usable on this machine (MCP search tools) but **is not named anywhere in the kit**. Agents following `memory-refresh.md` never search Claude Mem. Consumers following `MCP-SETUP.md` never install it.

Claude Mem does a different job from MemPalace: hooks + local worker auto-record observations; agents retrieve them with a 3-layer search. It is not a code graph (CBM) and not a structured decision log (MemPalace).

## Goal

Add Claude Mem to the kit as a **recommended** fourth memory layer, beside MemPalace, without a new policy file and without vendoring the plugin.

Success: after implementation, (1) `memory-refresh.md` tells agents when to search Claude Mem vs checkpoint MemPalace, (2) `MCP-SETUP.md` + CHECKLIST + PROVIDER-MAPPING name install for Cursor, Claude Code, OpenCode, and Codex, (3) `examples/mcp.example.json` has a no-secret stub, (4) CMEM Pro is not a checklist path, (5) missing Claude Mem does not stop work.

## Non-goals

- New file `examples/policies/claude-mem.md` (rejected; approach 1).
- Vendor plugin, skills, or hook scripts into this git.
- Create `docs/agent-policies/` or `.cursor/` in this kit repo.
- Replace MemPalace, CBM, or RTK.
- Make Claude Mem a stop-gate like Superpowers.
- Add tag `[CLAUDE-MEM]`.
- Document CMEM Pro / `cmem.ai` as the install path.
- Mandate Windsurf, Antigravity, Grok Bot, or OpenClaw (link upstream README only).
- Treat Claude Mem `smart_search` / `smart_outline` / `smart_unfold` as a CBM replacement.
- Require corpus tools (`build_corpus`, `prime_corpus`, …) in the daily agent loop.
- Automated tests (kit is documentation).
- CBM re-index for this docs-only change unless `[REINDEX]`.
- Change `first-setup.md` menu (Claude Mem is not a setup menu item).
- Edit `examples/CLAUDE.md.template` or Hallmark skill files.

## Decisions

| ID | Choice |
|----|--------|
| 1a | Keep MemPalace and Claude Mem. Separate roles. No dual checkpoint. |
| 2a | Recommended like CBM / MemPalace / RTK. Missing MCP → continue without search. |
| 3 | No CMEM Pro. Source is GitHub `thedotmack/claude-mem`. Skip cloud with `--provider claude` or `CLAUDE_MEM_ONLINE_OPTIN=false`. |
| 4a | Install text for all four mapped hosts. Other hosts = upstream README link. |
| 5 | Approach 1: extend existing memory docs. No new policy file. |
| 6 | Cursor hooks: **user-level**. Kit git stays free of `.cursor/`. Apps: user-level recommended; project-level allowed, kit does not require committing hooks. |
| 7 | No `[CLAUDE-MEM]` tag. `[NO-MEMORY]` skips MemPalace **and** Claude Mem search/corpus. Hooks may still record; policy does not tell the agent to stop the worker. |
| 8 | Agreed decisions stay in MemPalace. Claude Mem is observation search, not the decision log. |

## Architecture

```text
files on disk     Read/Grep              always fresh
CBM graph         index_repository       manual re-index
MemPalace         checkpoint / search    agent writes on purpose
Claude Mem        hooks + worker         auto-write; agent searches
                      ↑
              npx claude-mem install
              (not in this git)
```

| Layer | Who writes | Agent duty | Fresh? |
|-------|------------|------------|--------|
| Files | humans / edits | Read, Grep | yes |
| CBM | agent, on purpose | `index_repository` when structure churns | no |
| MemPalace | agent, on purpose | checkpoint after meaningful work; `mempalace_search` when continuing | no |
| Claude Mem | hooks + local worker | 3-layer search when continuing a topic | no |

Worker data lives in `~/.claude-mem/` (SQLite, logs, settings). Not in git. MCP search talks to that worker.

```text
search(query) → index + IDs
timeline(anchor) → nearby context
get_observations([ids]) → full text for filtered IDs only
```

Do not fetch full observations before filtering.

This kit: agents follow `examples/policies/memory-refresh.md`. Apps copy that file to `docs/agent-policies/memory-refresh.md` with the rest of the policies.

## Agent flow

### Continue a past topic / new session that continues yesterday’s work

1. Claude Mem `search` (short query) → `timeline` on interesting IDs → `get_observations` only for filtered IDs.
2. `mempalace_search` with topic keywords (existing rule).
3. Summarize hits; continue open todos.

### After meaningful work

Meaningful = code change, brainstorm, root-cause debug, business-pattern review, or existing MemPalace tags.

1. Finish the work + relevant tests.
2. MemPalace checkpoint (existing format). **Do not** manually “checkpoint” Claude Mem; hooks already recorded.
3. CBM `index_repository` only under existing re-index rules (`[REINDEX]`, new routes/services, migrations, structural refactors, ≥5 code files). Policy-docs-only → MemPalace checkpoint, **not** CBM re-index, **not** Claude Mem corpus rebuild.
4. One-line report: `Memory: skip ✓` / `Session: checkpoint ✓` / `re-index ✓`. Add `Claude Mem: search ✓` only if this session actually searched.

Skip MemPalace checkpoint only if: 1–2 short info questions, `[NO-MEMORY]`, or exact duplicate within 24h (unchanged).

### Tags

| Tag | Meaning after this change |
|-----|---------------------------|
| `[REINDEX]` | CBM `index_repository` only |
| `[MEMPALACE]` | Must checkpoint MemPalace |
| `[SESSION-END]` | Must checkpoint MemPalace |
| `[BRAINSTORM]` | MemPalace checkpoint + diary (even with no code) |
| `[NO-MEMORY]` | Skip MemPalace save **and** skip Claude Mem search/corpus |

Do not add `[CLAUDE-MEM]`.

## Files to change

Match the language of each file (English `memory-refresh.md`; Indonesian MCP-SETUP / CHECKLIST / catalog where that is already the register). Do not rewrite a file into another language.

**Edit**

- `examples/policies/memory-refresh.md` — four-layer table; continue-topic order; after-task order; `[NO-MEMORY]` as above; one sentence: `smart_*` is not CBM.
- `MCP-SETUP.md` — insert **§3 Claude Mem** after MemPalace. Renumber Postgres → §4, RTK → §5, Troubleshooting → §6, after-task → §7. Header table: add Claude Mem, recommended. Body: install, four hosts, skip CMEM Pro, MCP tools, troubleshooting rows.
- `examples/mcp.example.json` — stub `claude-mem` with placeholder path, empty/no secrets. Typical shape after plugin install: `node` + `…/plugin/scripts/mcp-server.cjs`. Use `<path-to-claude-mem-plugin>/scripts/mcp-server.cjs`, same style as the MemPalace placeholder.
- `CHECKLIST-NEW-PROJECT.md` — §C: Claude Mem connected; worker up; one `search`. §D Memory: continue-topic searches Claude Mem; `[NO-MEMORY]` skips MemPalace and Claude Mem search.
- `PROVIDER-MAPPING.md` — MCP row includes Claude Mem; per-host install bullets (commands from upstream, not invented). Cursor: user-level.
- `RULES-CATALOG.md` — memory entry lists CBM + MemPalace + Claude Mem.
- `README.md` — MCP-SETUP blurb includes Claude Mem.
- `CLAUDE.md` — after-work line: MemPalace checkpoint; Claude Mem search on continue; CBM re-index unchanged.
- `AGENTS.md` (root) and `examples/AGENTS.md` — keep pointer at `memory-refresh.md` (that file carries the new layer). One short clause is enough if the current bullet would otherwise still say “MemPalace only”.

**Do not edit**

- `examples/policies/first-setup.md` (not a menu item).
- `examples/policies/superpowers.md`.
- `examples/CLAUDE.md.template`.
- `examples/skills/hallmark/**`.
- Any `.cursor/` path in this repo.

## Install (checklist + MCP-SETUP + mapping)

Canonical source: [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) and [docs.claude-mem.ai/installation](https://docs.claude-mem.ai/installation). If upstream README changes, update CHECKLIST + MCP-SETUP + PROVIDER-MAPPING to match. Do not keep a fourth copy of long commands in `AGENTS.md`.

**Do**

```bash
npx claude-mem install
```

Select Cursor, Claude Code, OpenCode, and/or Codex CLI when the installer lists apps. Skip CMEM Pro: `--provider claude` (does not talk to `cmem.ai`) or `CLAUDE_MEM_ONLINE_OPTIN=false`. Gemini / OpenRouter are allowed as the user’s own keys; kit does not put keys in git and does not make them the checklist default.

Host-specific `--ide` flags: copy from current upstream docs only when that README shows them (example already documented: `--ide opencode`). Cursor: choose Cursor in the installer, then user-level hooks with `claude-mem cursor install user`. Codex CLI: select it in the interactive installer; do not invent a `--ide` value.

Claude Code alternative (also upstream):

```text
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem
```

Then restart the host.

**Do not**

- `npm install -g claude-mem` (SDK only; no hooks, no worker).
- Clone the repo just to install (clone is for people changing Claude Mem).
- Commit API keys, `~/.claude-mem/settings.json`, or worker paths with usernames into kit git.
- Write `.cursor/hooks` or `.cursor/rules` into **this** kit repo.

Verify: worker health (`claude-mem status` or `http://127.0.0.1:${port}/api/health`); restart Cursor/Claude once after install; one MCP `search`.

## Error handling

| Situation | Behavior |
|-----------|----------|
| Claude Mem MCP / worker down | Continue the task. No search. Do not stop (not Superpowers). |
| Empty search | Try without `project` filter; check worker; wrong project name. Still do not block the task. |
| User used `npm install -g` | Tell them to run `npx claude-mem install`. |
| Cursor hooks not firing | User-level install + restart host. Do not add `.cursor/` to kit git to “fix” it. |
| `[NO-MEMORY]` | Skip MemPalace and skip Claude Mem search/corpus. Do not stop the worker. |
| Decision from brainstorm | Write it to MemPalace. Searching Claude Mem is not a substitute. |
| Code-structure question | CBM / Read / Grep. Not Claude Mem `smart_search` as the required path. |
| Secret in MCP config | `security.md` wins. Example JSON uses placeholders only. |

## Manual verification (implementation done)

1. `grep` in the kit: Claude Mem named in memory-refresh, MCP-SETUP, CHECKLIST, PROVIDER-MAPPING, RULES-CATALOG, README. No CMEM Pro as a required step. No `examples/policies/claude-mem.md`.
2. `examples/mcp.example.json` parses; `claude-mem` stub has no secret.
3. Kit still has no tracked `.cursor/` or `docs/agent-policies/`.
4. Continue-topic text: search Claude Mem then MemPalace.
5. After-task text: MemPalace checkpoint; no manual Claude Mem checkpoint.
6. `[NO-MEMORY]` covers both MemPalace and Claude Mem search.
7. Superpowers overlay unchanged: feature work still brainstorm/spec, not ask-first.
8. Four hosts named; other hosts are a README link.

## Implementation notes (for the later plan, not this spec’s job)

Ponytail: shortest diffs on existing docs. Zero new policy files. Zero new dependencies. Zero `.cursor` in kit git.

Memory after that work: MemPalace checkpoint; skip CBM `index_repository` unless `[REINDEX]`. Claude Mem search in later sessions will pick up this spec via hooks if the worker is running.
