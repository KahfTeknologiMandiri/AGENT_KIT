# Stack cursorrules — pasang rule sesuai tech stack

**Bahasa awam:** Seperti memilih buku tips yang cocok pekerjaanmu — bukan membawa seluruh perpustakaan. AI deteksi stack project, usulkan beberapa rule dari komunitas, kamu setuju dulu, baru dipasang.

Upstream (CC0): [PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)

## Kapan dipakai

- Setup pertama (bersama policy Agent Kit / vibe), **atau**
- Project lama yang sudah jalan — sesuaikan dengan stack yang **benar-benar ada** di repo.

## Wajib lewat first-setup / ask-first

**Jangan** langsung clone atau salin puluhan file.

1. Deteksi stack (lihat bawah).  
2. Usulkan daftar rule (nama + alasan + path upstream).  
3. Tunjukkan rencana file.  
4. Tunggu “lanjut” / “apply”.  
5. Baru salin.

## Deteksi stack (urut prioritas)

Gabungkan sinyal; jangan tebak buta.

| Sumber | Cara |
|--------|------|
| **Tanya user** | “Stack utama apa?” (contoh: Next.js + Postgres + Playwright) |
| **PRD / Tech Design** | Baca `docs/PRD-*`, `docs/*TechDesign*`, atau hasil vibe-mvp |
| **Scan project lama** | `package.json` / `pnpm-lock.yaml` / `pubspec.yaml` / `go.mod` / `Cargo.toml` / `composer.json` / folder `lib/` `app/` `src/` / CI config |

Tulis ringkas ke user: “Kelihatannya stack = X, Y, Z — benar?”

## Pilih rule (semua kategori boleh, jumlah dibatasi)

Boleh pertimbangkan **semua** kategori di README upstream (frontend, backend, mobile, CSS, state, DB/API, testing, deploy, language, security komunitas, docs, …).

**Batas keras:** usulkan **maks 5–7** rule per project (lebih sedikit lebih baik). Prioritas:

1. Framework / bahasa utama (1–2)  
2. Data / API yang nyata dipakai (0–1)  
3. Testing atau styling jika sudah ada di repo (0–2)  
4. Lainnya hanya jika jelas relevan  

Jangan pasang rule untuk stack yang **tidak** dipakai.

### Petunjuk cepat sinyal → jenis rule

| Sinyal di project / PRD | Cari di awesome-cursorrules (contoh) |
|-------------------------|--------------------------------------|
| `next`, `react`, `vue`, `svelte`, `angular`, `astro` | Frontend Frameworks |
| `express`, `fastapi`, `django`, `nestjs`, `laravel`, `go` | Backend and Full-Stack |
| `flutter`, `expo`, `swiftui`, `jetpack` | Mobile Development |
| `tailwind`, `chakra`, `styled-components` | CSS and Styling |
| `redux`, `zustand`, `tanstack-query`, `pinia` | State Management |
| `graphql`, `prisma`, `supabase`, axios-heavy | Database and API |
| `playwright`, `cypress`, `vitest`, `jest` | Testing |
| `vercel`, `netlify`, `cloudflare` | Hosting and Deployments |
| Unity / game | Games and Graphics |

Sumber kebenaran daftar file: folder [`rules/`](https://github.com/PatrickJS/awesome-cursorrules/tree/main/rules) di upstream (bukan salinan di Agent Kit).

## Setelah user setuju — pasang dual format

Untuk **setiap** rule yang disetujui:

1. Ambil isi dari upstream (`rules/…/*.mdc` atau setara).  
2. **Cursor:** simpan di `.cursor/rules/` sebagai `.mdc`.  
   - Prefer `alwaysApply: false` + `globs` sempit (mis. `**/*.{tsx,ts}`).  
   - Jangan biarkan rule luar menimpa ask-first / security / ponytail / database-readonly / first-setup.  
3. **Portable:** salin tubuh Markdown **tanpa** frontmatter `---` ke `docs/agent-policies/stack-<nama>.md` (atau `imported/`).  
4. Rujuk file `.md` dari `AGENTS.md` / `opencode.json` / `CLAUDE.md` sesuai [PROVIDER-MAPPING.md](../../../PROVIDER-MAPPING.md).  
5. Catat sumber: URL file upstream + tanggal ambil (1 baris di README project atau komentar atas file).

Detail strip frontmatter: [PROVIDER-MAPPING.md](../../../PROVIDER-MAPPING.md) §H.

## Prioritas konflik (wajib)

**Peraturan rumah Agent Kit selalu menang.**  
Rule stack = tips pekerjaan. Kalau tips bilang “langsung ubah tanpa tanya” atau melemahkan security / DB read-only → **abaikan** bagian itu; biarkan policy kit.

## Jangan lakukan

- Commit seluruh isi awesome-cursorrules ke project.  
- Pasang >7 rule “untuk berjaga-jaga”.  
- `alwaysApply: true` pada rule luar tanpa permintaan eksplisit user.  
- Menimpa `ask-first`, `ponytail`, `security`, `database-readonly`, `first-setup`, `memory-refresh`.

## Uji singkat

- Cursor: Agent chat baru → “List active project rules” / “Rule stack apa yang aktif?”  
- Minta tugas kecil sesuai stack (mis. “buat komponen React”) → pastikan tips stack terpakai **dan** ask-first tetap jalan untuk refactor besar.

## Bedanya dengan policy Agent Kit & vibe

| | Stack cursorrules | Agent Kit policies | Vibe MVP |
|---|-------------------|--------------------|----------|
| Fokus | Tips coding per tech | Sikap AI tiap hari | Ide → PRD → MVP |
| Sumber | PatrickJS (pilih tipis) | `docs/agent-policies/` | Upstream KhazP |
| Kapan | Setup / project lama ber-stack | Setiap sesi | Awal produk |
