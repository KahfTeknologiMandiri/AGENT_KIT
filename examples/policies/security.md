# DevSecOps + SSDLC + AppSec

## General

- Never hardcode secrets. Use env vars or vaults.
- Do not commit `.env` or secret config.
- Never log secrets or session tokens.
- Validate and sanitize input. Escape output in HTML/JS/SQL contexts.
- Avoid `exec` / `eval` and similar dynamic execution.

## Database

- Parameterized queries or ORM only.
- Least-privilege DB users.

## Dependencies

- Verified sources only.
- Do not add dependencies without explicit approval and security review.
- Keep updated; scan vulnerabilities (SCA).

## Auth

- Established auth frameworks; no custom crypto/auth.
- Strong salted password hashes (Argon2, bcrypt).
- RBAC + least privilege.

## Secure SDLC

- Prefer SAST, SCA, secret scanning, IaC scanning, DAST where pipeline allows.
- Document security-relevant decisions.
