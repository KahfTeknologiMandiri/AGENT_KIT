# Superpowers — proses fitur dan bug (overlay kit)

Plugin: [obra/superpowers](https://github.com/obra/superpowers) (MIT). Kit **tidak** men-vendor skill. Pasang plugin di host yang dipakai — perintah: [CHECKLIST-NEW-PROJECT.md](../../CHECKLIST-NEW-PROJECT.md) dan [PROVIDER-MAPPING.md](../../PROVIDER-MAPPING.md).

Salin file ini ke `docs/agent-policies/superpowers.md` di project app bersama policy lain.

## Kapan wajib

Fitur baru, perbaikan bug, atau kerja multi-langkah yang mengubah perilaku produk. Bukan typo 1 baris. Bukan first-setup (itu `first-setup.md`).

## Plugin wajib

Skill Superpowers harus terpasang di host sesi ini. Jika tidak: **berhenti**. Suruh user pasang per [CHECKLIST-NEW-PROJECT.md](../../CHECKLIST-NEW-PROJECT.md) / [PROVIDER-MAPPING.md](../../PROVIDER-MAPPING.md). Jangan implementasi fitur. Jangan putaran 3–7 A/B/C `ask-first` untuk fitur.

Jangan skip overlay ini kecuali user mengubah policy kit.

## Skill wajib (nama persis)

1. `using-superpowers`
2. `brainstorming` — fitur baru / kerja berbentuk produk
3. `systematic-debugging` — bug
4. `writing-plans` — setelah spec disetujui
5. `executing-plans` — setelah plan disetujui
6. `test-driven-development` — saat eksekusi
7. `verification-before-completion` — sebelum klaim selesai

Skill Superpowers lain (worktree, review, subagent, writing-skills, …) opsional, tidak dilarang.

## Prioritas (atas menang)

1. `security`, `database-readonly`, `first-setup` — selalu. Superpowers tidak boleh skip konfirmasi setup, secret di git, atau tulis DB lewat MCP.
2. Fitur / bug / multi-langkah — tujuh skill di atas.
3. `ask-first` — hanya first-setup dan kerja **non-fitur** (config, rename). Typo 1 baris tetap langsung. Bukan 3–7 A/B/C untuk fitur baru.
4. Ponytail — ukuran diff. Tidak skip TDD. Tidak skip Figma atau Hallmark pada UI user-facing.
5. Figma (`figma.md`) — setelah spec, sumber visual (MCP + frame di-approve) sebelum tulis UI. MCP absen → berhenti, bukan catalog Hallmark.
6. Hallmark — kualitas + `hallmark audit` sebelum ship; jangan ganti makrostruktur Figma yang di-approve.
7. Caveman — lepas saat brainstorm / spec / plan review. Balik setelah eksekusi mulai. Security / aksi irreversible / user bingung: selalu lepas.

## Spec dan plan

Ikuti path plugin yang terpasang (default app: `docs/superpowers/specs/`, plans). Jangan invent root spec kit untuk app.

**Build mode:** user sudah approve **spec dan plan** Superpowers → eksekusi sampai DoD (`executing-plans` + TDD). Jangan tanya ulang per modul. Bukan PRD KhazP / vibe-MVP. UI: tetap `figma.md` (node di-approve per permukaan baru) lalu Hallmark.

## Refresh plugin

Update = pasang ulang plugin di host. Bukan vendor bump di repo kit.
