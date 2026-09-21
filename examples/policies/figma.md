# Figma — sumber visual UI (overlay kit)

MCP resmi: [Figma MCP server](https://developers.figma.com/docs/figma-mcp-server/) (remote `https://mcp.figma.com/mcp`). Kit **tidak** men-vendor skill Figma. Pasang plugin/MCP di host — perintah: [CHECKLIST-NEW-PROJECT.md](../../CHECKLIST-NEW-PROJECT.md), [MCP-SETUP.md](../../MCP-SETUP.md), [PROVIDER-MAPPING.md](../../PROVIDER-MAPPING.md).

Salin file ini ke `docs/agent-policies/figma.md` di project app bersama policy lain.

## Kapan wajib

Setiap pekerjaan yang **membuat atau mengubah** halaman, landing, shell, atau komponen visual user-facing di **repo aplikasi** (bukan repo Agent Kit / docs-only).

## Pengecualian (Figma idle)

- Ejaan / tanda baca di string yang **sudah ada**
- README Markdown (`readme-style`)
- Kerja non-UI (config, backend, policy)
- Repo **Agent Kit** ini (markdown / examples)

Copy baru, layout, warna, tipe, atau komponen baru = visual → Figma wajib.

## Gerbang sebelum tulis UI

1. Spec Superpowers sudah ada (atau `defaults` pada spec). Brand / tone / copy di spec = brief Figma.
2. MCP Figma terhubung di host ini. Jika tidak: **berhenti**. Suruh pasang per CHECKLIST / MCP-SETUP. Jangan tulis UI. Jangan jatuh ke tema catalog Hallmark.
3. Host tidak di [katalog klien Figma](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/) (OpenCode hari ini): **berhenti**. Jelaskan waitlist. Jangan PAT / token di git. Desktop MCP (`http://127.0.0.1:3845/mcp`) **bukan** jalur generate (`use_figma` / `generate_figma_design` hanya di remote).
4. Ada frame/node yang sudah di-approve (URL + node)?
   - Tidak: ikuti skill Figma resmi di host (`search_design_system` dulu; lalu `use_figma` / `generate_figma_design` / `create_new_file`). **Tunggu approve** URL + node sebelum kode UI.
   - User menempel URL **node**, atau spec sudah menamai node yang di-approve → itu approve.
   - URL file tanpa `node-id` → tanya node; jangan implement seluruh file.
5. User menolak frame → revisi di Figma; jangan implementasi.
6. Implementasi: Figma = layout, IA, komponen, token permukaan itu. Petakan ke komponen app yang sudah ada; jangan duplikat.
7. Hallmark: perbaiki slop / a11y / responsive **tanpa** ganti makrostruktur yang di-approve. Jika perbaikan akan ganti kerangka → **berhenti** dan tanya.
8. `hallmark audit` sebelum anggap permukaan selesai.
9. `hallmark redesign` pada permukaan bersumber Figma butuh konfirmasi eksplisit.

`defaults` pada spec: tetap generate Figma + tunggu approve frame. `langsung kerjakan` **tidak** melewati gerbang ini.

**Build mode:** jangan tanya ulang desain tiap halaman. Setiap permukaan UI **baru** tetap butuh node Figma yang sudah di-approve. Audit Hallmark tetap.

Layar yang sudah punya node: implement / perbaiki dari node itu. Jika tampilan **menyimpang** dari frame → update Figma, tunggu approve ulang. Jangan invent tema Hallmark kedua.

Token Figma berlaku di permukaan yang dikerjakan. Jangan rewrite file token global app kecuali spec bilang.

## Yang dilarang

- Men-vendor skill Figma ke git kit (`examples/skills/figma/` dilarang).
- Secret / PAT / `FIGMA_ACCESS_TOKEN` di git (`security.md` menang). Auth = OAuth host.
- Sync otomatis kode → Figma.
- Menimpa `security`, `database-readonly`, `first-setup`, atau overlay Superpowers.
- Skip spec Superpowers untuk fitur UI.

## Plugin / skill host

Ikuti skill Figma resmi di host saat memanggil tool Figma (Cursor: `/add-plugin figma`; Claude Code: `claude plugin install figma@claude-plugins-official`). Overlay ini bukan tubuh skill. Perintah panjang hanya di CHECKLIST / MCP-SETUP / PROVIDER-MAPPING.
