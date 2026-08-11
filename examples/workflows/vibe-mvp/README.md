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

1. Di chat (ChatGPT / Claude / Gemini): ikuti Part 1–3 di repo upstream (`part1-deepresearch.md` → PRD → tech design).  
2. Simpan hasil ke `docs/` di project (nama seperti di README upstream).  
3. Di AI IDE: ikuti Part 4 (`part4-notes-for-agent.md`) untuk `AGENTS.md` / adapter tool.  
4. **Overlay Agent Kit:** salin `examples/policies/` → `docs/agent-policies/`, mapping per [PROVIDER-MAPPING.md](../../../PROVIDER-MAPPING.md). Policy kit (ask-first, security, first-setup, …) tetap menang atas instruksi generik dari vibe.
5. **(Opsional) Stack cursorrules:** setelah tech design jelas, ikuti [../stack-cursorrules/](../stack-cursorrules/) agar rule framework cocok PRD — tetap lewat konfirmasi user.

## Apa yang tidak di-vendor di kit ini

Isi penuh prompt Part 1–4 **tidak** disalin ke repo Agent Kit (hindari duplikat & drift). Pakai repo upstream sebagai sumber; folder ini hanya **petunjuk + pengait** ke `first-setup`.

## Bedanya dengan policy Agent Kit

| | Vibe workflow | Agent Kit policies | Stack cursorrules |
|---|---------------|--------------------|-------------------|
| Fokus | Merancang produk (riset, PRD, stack) | Cara AI bersikap tiap hari | Tips coding per tech |
| Kapan | Awal ide / MVP | Setiap sesi coding | Setup / setelah stack jelas |
| File tipikal | `docs/PRD-…`, `TechDesign-…` | `docs/agent-policies/*`, rules Cursor | `.cursor/rules/*stack*.mdc` + `docs/agent-policies/stack-*.md` |
