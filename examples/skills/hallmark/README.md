# Hallmark — setup di Agent Kit

Skill desain anti-AI-slop dari [Nutlope/hallmark](https://github.com/Nutlope/hallmark) (MIT). Versi yang di-vendor di folder ini mengikuti upstream `SKILL.md` (lihat field `version` di frontmatter).

**Isi English tetap upstream.** Dokumen setup ini bahasa Indonesia.

## Apa yang ada di folder ini

| Path | Peran |
|------|--------|
| `SKILL.md` | Aturan utama (4 verb: default / audit / redesign / study) |
| `references/` | Tema, slop-test, macrostructure, dll. — dibaca skill saat perlu |

## Cara cepat update dari upstream

```text
npx skills add nutlope/hallmark
```

Atau salin ulang `skills/hallmark/` dari repo Hallmark ke folder ini (timpa), lalu sync mirror Cursor bila dipakai.

## Mapping per provider

### Cursor (disarankan di project yang pakai kit)

1. Pastikan folder skill ada di project (sudah di-vendor di kit: `examples/skills/hallmark/`).
2. Rule aktif: `.cursor/rules/hallmark.mdc` dengan `alwaysApply: true` (lihat mirror di repo agent-kit).
3. Path di rule harus mengarah ke lokasi skill di project Anda. Di agent-kit: `examples/skills/hallmark/SKILL.md`. Di app lain, biasanya salin ke `skills/hallmark/` atau biarkan di `docs/agent-kit/examples/skills/hallmark/` lalu sesuaikan path di `.mdc`.
4. Buka **Agent chat baru** setelah menambah/ubah rule.
5. Uji: `hallmark audit .` atau minta “buat landing page untuk X”.

Alternatif Cursor (tanpa mirror kit): tempel **body** `SKILL.md` (tanpa frontmatter YAML skill) ke `.cursor/rules/hallmark.mdc`, dan letakkan `references/` di tempat yang bisa di-`Read` agent (path relatif dari skill).

### Claude Code

```text
# personal
~/.claude/skills/hallmark/   ← salin SKILL.md + references/

# atau project-scoped (jika host mendukung skills di repo)
.claude/skills/hallmark/
```

Di `CLAUDE.md` boleh satu baris: “UI/landing: ikuti skill Hallmark bila terpasang.”

### Codex

```text
# personal
~/.codex/skills/hallmark/

# project
.codex/skills/hallmark/
```

Salin `SKILL.md` + `references/`.

### OpenCode / generic

- Salin folder skill ke repo (mis. `skills/hallmark/`).
- Rujuk dari `AGENTS.md` atau `opencode.json` → `instructions` ke `SKILL.md` (file besar — opsional: cukup pointer “baca skills/hallmark/SKILL.md saat kerja UI”).
- `references/` tetap di samping `SKILL.md` agar link relatif jalan.

### Install via CLI (semua host yang support skills)

```text
npx skills add nutlope/hallmark
```

Ini cara paling mudah untuk mesin pribadi; folder di kit tetap berguna agar project punya salinan terkontrol di git.

## Prioritas vs policy kit lain

| Domain | Yang menang |
|--------|-------------|
| README Markdown | `readme-style` |
| UI / landing / shell / redesign visual | **Hallmark** (+ responsive + audit sebelum ship) |
| Putaran desain / `defaults` / build mode | `ask-first` (lalu Hallmark tanpa tanya ulang tiap halaman) |
| Secret / auth / DB MCP | `security` + `database-readonly` |
| Setup multi-provider | `first-setup` |
| Jumlah kode | `ponytail` — **tidak** boleh skip Hallmark demi placeholder UI |

Policy ringkas kit: `docs/agent-policies/hallmark.md`.

Hallmark punya safety rail sendiri: jangan hapus pohon route/production tanpa izin eksplisit.

## Verb singkat

| Perintah | Efek |
|----------|------|
| (default) | Build UI baru + slop-test |
| `hallmark audit <target>` | Skor anti-pattern, **tanpa edit** |
| `hallmark redesign <target>` | Ganti struktur visual, jaga copy/IA/brand |
| `hallmark study <screenshot\|URL>` | Ekstrak DNA desain |

Demo & tema: [usehallmark.com](https://www.usehallmark.com/) · upstream: [github.com/Nutlope/hallmark](https://github.com/Nutlope/hallmark)
