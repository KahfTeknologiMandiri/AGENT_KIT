# Hallmark — UI anti-AI-slop (policy kit)

Skill penuh: `examples/skills/hallmark/SKILL.md` (+ `references/`). Setup: `examples/skills/hallmark/README.md`. Spec dulu: overlay Superpowers (`examples/policies/superpowers.md`).

Salin file ini ke `docs/agent-policies/hallmark.md` di project app bersama policy lain.

## Kapan wajib

Setiap pekerjaan yang **membuat atau mengubah** halaman, landing, shell app, atau komponen visual user-facing.

## Gerbang sebelum tulis UI

1. Spec Superpowers sudah ada (atau `defaults` pada spec). Baru **putaran desain** Hallmark di `ask-first` — atau `defaults` (Hallmark + tokens merek).
2. **Baca** `SKILL.md` (dan `references/` yang diminta skill) — jangan hanya mengingat ringkasan bridge.
3. Tulis UI mengikuti Hallmark; **bukan** template “3 kartu + ungu / cream serif generik”.
4. **Responsive:** layout harus usable di desktop dan mobile (bukan hanya lebar laptop).
5. Sebelum anggap halaman visual selesai: jalankan **`hallmark audit`** (atau checklist setara dari skill) — perbaiki slop kritis dulu.

## Yang dilarang

- Ship placeholder / “shell sementara AI” sebagai hasil akhir untuk permukaan yang user lihat.
- Mengabaikan Hallmark karena Ponytail “kode minimal” — minimal yang benar untuk UI = ikut Hallmark + responsive, bukan file CSS kosong bermerek.
- Menimpa `security`, `database-readonly`, first-setup, atau overlay Superpowers.

## Verb

| Perintah | Efek |
|----------|------|
| (default) | Build UI + slop-test |
| `hallmark audit` | Skor anti-pattern, tanpa edit |
| `hallmark redesign` | Ganti struktur visual, jaga copy/IA/brand |
| `hallmark study` | Ekstrak DNA dari screenshot/URL |

## Build mode

Saat **build mode** aktif (spec + plan Superpowers disetujui): tetap wajib Hallmark + responsive; **jangan** tanya ulang desain tiap halaman jika sudah `defaults` / putaran desain selesai.
