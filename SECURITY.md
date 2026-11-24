# Security policy

Finora processes private financial records. Do not put account numbers, passwords, tokens, database URLs, statement exports, or transaction details in public issues, pull requests, screenshots, or chat transcripts.

## Reporting a vulnerability

Use GitHub's private vulnerability reporting for this repository if it is enabled. Otherwise, contact the repository maintainer privately and ask for a secure reporting channel. Include the affected component, impact, and a minimal reproduction with synthetic values. Do not exploit other users' accounts or test against real financial data.

## Security boundaries contributors must preserve

- Supabase Auth owns hosted identity and passwords. Never commit credentials or service-role keys.
- Authenticate and authorize on the server. Keep queries owner-scoped and preserve Postgres RLS and the restricted application role.
- Bank adapters and account-connect UI are simulated; do not add real bank credentials or imply a live provider connection.
- Previewed statement data is temporary; persist only user-confirmed, server-validated canonical transactions.
- Do not log financial descriptions, amounts, account identifiers, raw statements, bearer tokens, or auth provider responses.
- Use only synthetic data in fixtures and browser screenshots.

See [Context-Manager/data-and-security.md](Context-Manager/data-and-security.md) for implementation details and known risks. Current code is a prototype and has documented gaps; do not claim security certification or production readiness from the presence of RLS or a privacy-named module.
