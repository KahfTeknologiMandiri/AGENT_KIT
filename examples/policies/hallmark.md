# Hallmark — UI anti-AI-slop (policy kit)

Skill penuh: `examples/skills/hallmark/SKILL.md` (+ `references/`). Setup: `examples/skills/hallmark/README.md`. Spec dulu: overlay Superpowers (`examples/policies/superpowers.md`). Sumber visual: overlay Figma (`examples/policies/figma.md`).

Salin file ini ke `docs/agent-policies/hallmark.md` di project app bersama policy lain.

## Kapan wajib

Setiap pekerjaan yang **membuat atau mengubah** halaman, landing, shell app, atau komponen visual user-facing **di repo aplikasi**.

## Gerbang sebelum tulis UI

1. Spec Superpowers sudah ada (atau `defaults` pada spec).
2. Ikuti `figma.md`: MCP Figma terhubung; frame/node di-approve (agent boleh generate dulu, lalu **tunggu**). Jangan pilih tema catalog Hallmark sebagai pengganti Figma.
3. **Baca** `SKILL.md` (dan `references/` yang diminta skill) — jangan hanya mengingat ringkasan bridge.
4. Tulis UI dari frame Figma (layout / IA / komponen / token permukaan itu). Hallmark **bukan** izin ganti makrostruktur yang sudah di-approve.
5. **Responsive / a11y / anti-slop:** perbaiki kegagalan audit **tanpa** ganti kerangka yang di-approve. Jika perbaikan akan ganti makrostruktur → berhenti dan tanya (lihat `figma.md`).
6. Sebelum anggap halaman visual selesai: jalankan **`hallmark audit`** (atau checklist setara dari skill) — perbaiki slop kritis dulu.

## Yang dilarang

- Ship placeholder / “shell sementara AI” sebagai hasil akhir untuk permukaan yang user lihat.
- Mengabaikan Hallmark karena Ponytail “kode minimal” — minimal yang benar untuk UI = Figma + Hallmark + responsive, bukan file CSS kosong bermerek.
- Invent tema catalog karena MCP Figma absen — itu **berhenti**, bukan fallback Hallmark.
- Menimpa `security`, `database-readonly`, first-setup, overlay Superpowers, atau overlay Figma.
- `hallmark redesign` pada permukaan bersumber Figma tanpa konfirmasi eksplisit.

## Verb

| Perintah | Efek |
|----------|------|
| (default) | Build UI + slop-test, **setelah** frame Figma di-approve |
| `hallmark audit` | Skor anti-pattern, tanpa edit |
| `hallmark redesign` | Ganti struktur visual, jaga copy/IA/brand — pada sumber Figma butuh izin user |
| `hallmark study` | Ekstrak DNA dari screenshot/URL — boleh jadi **brief** generate Figma, bukan skip Figma |

## Build mode

Saat **build mode** aktif (spec + plan Superpowers disetujui): tetap wajib Figma (node di-approve per permukaan baru) + Hallmark + responsive; **jangan** tanya ulang desain tiap halaman jika frame untuk permukaan itu sudah di-approve.
