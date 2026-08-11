# Stack example (adaptasi Postgres ? pola Koala-like)

Gunakan sebagai dasar `stack-backend.md` jika API mirip Koala.

## Tech stack

- Backend: Node.js + Express.js (modular monolith OK)
- Database: **PostgreSQL** (parameterized SQL / migrations ? bukan Mongoose)
- Auth: JWT (plus token revocation if you have it)
- Client: Flutter and/or web as applicable
- Background jobs: separate worker unless explicitly in-process
- Version control: Git

## Working style

- Before coding: plain-language flow + endpoint/state pseudocode.
- REST + explicit validation/errors.
- Shared workflows: fix shared service once; grep callers.
- Schema changes only via numbered migrations ? never via AI MCP writes.
- Sensitive uploads: authenticated serve, not blind public static.

## Anti-patterns

- Jangan menyalin rule Mongo/Mongoose ke repo Postgres.
- Jangan string-concatenate SQL.
- Jangan asumsikan microservices jika repo satu Express entrypoint.
