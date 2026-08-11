# Stack example (LEGACY SHAPE ? Mongo)

> Meniru bentuk file lama template Cursor (Node + Mongo + JWT).
> **Jangan pakai buta** jika project Anda Postgres (seperti Koala HRIS).
> Lihat `stack-backend.postgres.example.md` untuk adaptasi.

## Tech stack (example)

- Backend: Node.js + Express.js
- Database: MongoDB + Mongoose ODM
- Frontend: React.js (admin panel, if required)
- Authentication: JWT
- Version control: Git
- Deployment: Docker (optional)

## Working style

- Follow specified user flow and business rules strictly.
- Before coding: plain-language flow + pseudocode for endpoints and logic.
- Secure REST; validate input; explicit errors.
- Prefer clear state transitions (pending ? approved ? ?).

## Adapt checklist

- [ ] Ganti Database jika bukan Mongo
- [ ] Hapus asumsi Mongoose / collection jika pakai SQL
- [ ] Sesuaikan contoh flow dengan domain project Anda
