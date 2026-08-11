# Memory refresh ? Codebase graph + MemPalace + session

| Layer | Tool | Always fresh? |
|-------|------|---------------|
| Files on disk | Read, Grep | Yes ? source of truth |
| Codebase graph | `index_repository`, `search_graph`, `trace_path` | No ? manual re-index |
| Long-term notes | MemPalace (`checkpoint`, `search`, `diary`) | No ? manual save |

## User tags (override defaults)

| Tag | Meaning |
|-----|---------|
| `[REINDEX]` | Must `index_repository` after the task |
| `[MEMPALACE]` | Must update MemPalace after the task |
| `[SESSION-END]` | Must checkpoint the session |
| `[BRAINSTORM]` | Checkpoint + diary ? even with no code changes |
| `[NO-MEMORY]` | Skip refresh/save |

## A. Session checkpoint (MemPalace)

Required for meaningful sessions: code change, brainstorm, root-cause debug, business-pattern review, or tags above.

**Brainstorm with no code still needs a checkpoint.**

Prefer structured `mempalace_checkpoint`. Avoid routine full-transcript mining.

### Checkpoint format

```text
[<project-id>] <topic one line>
Tipe: fix | brainstorm | review | debug | tanya-jawab
Keputusan: <agreed decisions ? required for brainstorm>
Kode: <files + short note ? or "tidak ada">
Belum: <open todos ? or "selesai">
Tanggal: <YYYY-MM-DD>
```

Skip only if: 1?2 short info questions, `[NO-MEMORY]`, or exact duplicate within 24h.

### Continuing a past topic

1. `mempalace_search` with topic keywords
2. Summarize hits
3. Continue open todos

## B. Re-index Codebase Memory

Re-index if: new/renamed main route/service/view files; API endpoint changes; new migrations; structural refactors; ?5 code files changed; `[REINDEX]`.

Usually skip: fix 1?3 files; typo/docs/rules-only; `[NO-MEMORY]`.

Changing agent policy files ? MemPalace checkpoint only, not graph re-index.

## C. After-task order

1. Finish fix/feature + relevant tests
2. Meaningful session? ? checkpoint
3. Need re-index? ? `index_repository`
4. Tell user one line: `Memory: skip ?` / `Session: checkpoint ?` / `re-index ?`
