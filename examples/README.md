# Examples ? salin lalu sesuaikan

| File | Kegunaan |
|------|----------|
| `AGENTS.md` | Jembatan universal (Ponytail + pointer policy) |
| `CLAUDE.md.template` | Template build/arsitektur |
| `opencode.json.example` | Tarik banyak file policy di OpenCode |
| `mcp.example.json` | Contoh daftar MCP tanpa secret |
| `policies/*.md` | Kebijakan portable (tanpa frontmatter Cursor); termasuk `first-setup.md` |
| `workflows/vibe-mvp/` | Opsional: alur ide→MVP (pointer ke upstream; wajib lewat first-setup) |
| `workflows/stack-cursorrules/` | Opsional: pasang rule tech dari awesome-cursorrules sesuai stack (deteksi + konfirmasi) |
| `skills/hallmark/` | Skill UI anti-AI-slop (SKILL.md + references); setup: [skills/hallmark/README.md](skills/hallmark/README.md) |

Alur: **konfirmasi dulu** (`policies/first-setup.md`) → copy `policies/` → `docs/agent-policies/` di project baru → mapping sesuai [../PROVIDER-MAPPING.md](../PROVIDER-MAPPING.md).
Hallmark: salin `skills/hallmark/` lalu mapping per host (Cursor rule / Claude·Codex skills / pointer AGENTS).
