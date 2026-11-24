# Security and privacy implementation

This page summarizes current code controls. It is not a security certification. The authoritative invariants and known risks are in [Context-Manager/data-and-security.md](../../Context-Manager/data-and-security.md).

## Current path

- Supabase Auth owns hosted identity and passwords.
- Next.js SSR verifies claims before serving the authenticated dashboard.
- The browser sends a Supabase access token to FastAPI.
- FastAPI validates that token with Supabase Auth, resolves the app profile, and supplies the verified user ID to the async DB session.
- Postgres sessions use the restricted `finora_app` role and transaction-local JWT subject, with RLS on user-owned tables.
- Application reads and writes still filter by the current user's ID.

## Data handling

- Local secrets belong in ignored environment files or hosted secret settings.
- Statement preview parses temporarily and does not persist rows. Confirmation validates ownership and persists reviewed canonical transactions.
- Browser API responses stay in bounded, user-scoped memory; sign-out clears the cache.
- Bank adapters are simulated and must not accept real bank credentials.

## Limitations

- Supabase tokens are checked over HTTP per API request; this adds latency.
- Email confirmation is disabled for the current hosted demo, so signup does not prove email ownership.
- The `privacy_gate.py` names and outputs do not guarantee air-gapped processing, zero network egress, or a cryptographic compliance certificate.
- Review the current schema and hosted RLS settings after each migration. Local tests do not prove hosted isolation.
