# Vibe MVP — alur ide → produk (opsional)

**Bahasa awam:** Ini resep “dari ide sampai MVP”. Bukan pengganti aturan harian Agent Kit (tanya dulu, keamanan, dll.).

Upstream (MIT): [KhazP/vibe-coding-prompt-template](https://github.com/KhazP/vibe-coding-prompt-template)

## Kapan dipakai

- Project masih ide / mau MVP cepat dengan dokumen PRD + tech design dulu.
- Mau generate `AGENTS.md` yang diisi dari PRD project (bukan hanya policy generik).

## Wajib lewat first-setup

Sebelum AI meng-clone template, menyalin prompt, atau mengisi `docs/PRD-…`, ikuti **`first-setup`**:

1. Tanya: perlu vibe sekarang?  
2. Tanya provider mana.  
3. Tunjukkan rencana → tunggu “lanjut”.  
4. Baru kerjakan.

Kalau user bilang tidak perlu: cukup arahkan ke halaman ini / repo upstream; **jangan ubah file**.

## Langkah singkat (setelah user setuju)

1. Di chat: ikuti Part 1–3 upstream (`part1-deepresearch.md` → PRD → tech design).  
2. **Desain:** satu putaran `ask-first` (brand, tone, referensi opsional, responsive) — atau `defaults` = Hallmark + tokens.  
3. Simpan hasil ke `docs/` di project.  
4. Setelah user **approve** PRD + Tech Design → masuk **build mode**: scaffold, salin Agent Kit ke app, kerjakan P0 **sampai DoD** tanpa tanya per modul.  
5. UI: baca Hallmark skill; audit sebelum ship halaman visual; desktop + mobile.  
6. Part 4 (`part4-notes-for-agent.md`) untuk `AGENTS.md` / adapter tool di **repo app**.  
7. **Overlay Agent Kit:** salin `examples/policies/` → `docs/agent-policies/`, mapping per [PROVIDER-MAPPING.md](../../../PROVIDER-MAPPING.md).  
8. **(Opsional) Stack cursorrules:** setelah tech design jelas — [../stack-cursorrules/](../stack-cursorrules/), sekali approve daftar.

## Apa yang tidak di-vendor di kit ini

Isi penuh prompt Part 1–4 **tidak** disalin ke repo Agent Kit (hindari duplikat & drift). Pakai repo upstream sebagai sumber; folder ini hanya **petunjuk + pengait** ke `first-setup`.

## Bedanya dengan policy Agent Kit

| | Vibe workflow | Agent Kit policies | Stack cursorrules |
|---|---------------|--------------------|-------------------|
| Fokus | Merancang produk (riset, PRD, stack) | Cara AI bersikap tiap hari | Tips coding per tech |
| Kapan | Awal ide / MVP | Setiap sesi coding | Setup / setelah stack jelas |
| Setelah approve PRD+TD | **Build mode** — eksekusi ke DoD | `ask-first` tidak tanya ulang per fitur | Sudah terpasang / pasang sekali |
| File tipikal | `docs/PRD-…`, `TechDesign-…` | `docs/agent-policies/*`, rules Cursor | `.cursor/rules/*stack*.mdc` + `stack-*.md` |
| UI | — | **Hallmark** + responsive | — |
