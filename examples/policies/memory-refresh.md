# Memory refresh — disk + CBM + MemPalace + Claude Mem

| Layer | Tool | Always fresh? |
|-------|------|---------------|
| Files on disk | Read, Grep | Yes — source of truth |
| Codebase graph | CBM `index_repository`, `search_graph`, `trace_path` | No — manual re-index |
| Decisions / session notes | MemPalace `checkpoint`, `search`, `diary` | No — agent writes on purpose |
| Session observations | Claude Mem `search` → `timeline` → `get_observations` | No — hooks + worker auto-write; agent searches |

Claude Mem is not a code graph (use CBM / Read / Grep). Claude Mem `smart_search` / `smart_outline` / `smart_unfold` are **not** a CBM replacement. Agreed decisions go to MemPalace, not only to Claude Mem. Corpus tools (`build_corpus`, …) are not part of the daily loop.

Missing Claude Mem MCP/worker: continue the task without search. Do not stop (not a Superpowers gate).

## User tags (override defaults)

| Tag | Meaning |
|-----|---------|
| `[REINDEX]` | Must CBM `index_repository` after the task |
| `[MEMPALACE]` | Must update MemPalace after the task |
| `[SESSION-END]` | Must checkpoint MemPalace |
| `[BRAINSTORM]` | MemPalace checkpoint + diary — even with no code changes |
| `[NO-MEMORY]` | Skip MemPalace save **and** skip Claude Mem search/corpus |

Do not add `[CLAUDE-MEM]`. Search Claude Mem when continuing a past topic (section A). `[NO-MEMORY]` does not tell the agent to stop the worker; hooks may still record.

## A. Continue a past topic

1. Claude Mem `search` (short query) → `timeline` on interesting IDs → `get_observations` only for filtered IDs. Do not fetch full observations first.
2. `mempalace_search` with topic keywords.
3. Summarize hits; continue open todos.

If Claude Mem is down or empty: skip step 1, still do step 2, do not block the task.

## B. Session checkpoint (MemPalace)

Required for meaningful sessions: code change, brainstorm, root-cause debug, business-pattern review, or tags above.

**Brainstorm with no code still needs a MemPalace checkpoint.**

Prefer structured `mempalace_checkpoint`. Avoid routine full-transcript mining. **Do not** manually checkpoint Claude Mem — hooks already recorded.

### Checkpoint format

```text
[<project-id>] <topic one line>
Tipe: fix | brainstorm | review | debug | tanya-jawab
Keputusan: <agreed decisions — required for brainstorm>
Kode: <files + short note — or "tidak ada">
Belum: <open todos — or "selesai">
Tanggal: <YYYY-MM-DD>
```

Skip MemPalace checkpoint only if: 1–2 short info questions, `[NO-MEMORY]`, or exact duplicate within 24h.

## C. Re-index Codebase Memory

Re-index if: new/renamed main route/service/view files; API endpoint changes; new migrations; structural refactors; ≥5 code files changed; `[REINDEX]`.

Usually skip: fix 1–3 files; typo/docs/rules-only; `[NO-MEMORY]`.

Changing agent policy files → MemPalace checkpoint only, not CBM re-index, not Claude Mem corpus rebuild.

## D. After-task order

1. Finish fix/feature + relevant tests
2. Meaningful session? → MemPalace checkpoint
3. Need re-index? → CBM `index_repository`
4. Tell user one line: `Memory: skip ✓` / `Session: checkpoint ✓` / `re-index ✓`. Add `Claude Mem: search ✓` only if this session actually searched.
