# Data, identity, and security contracts

Finora handles private financial records. These rules apply even when a feature is presented as a prototype.

## Identity and authorization

- Hosted identity and passwords belong to Supabase Auth. The browser receives only the Supabase project URL and publishable key.
- Next.js uses `@supabase/ssr` cookie sessions and checks server-side claims before rendering `/dashboard`.
- Frontend API calls attach the current access token as `Authorization: Bearer …`.
- FastAPI validates a hosted token through Supabase Auth, resolves the matching app profile, and puts the verified user ID into the DB session context.
- For Postgres transactions, `backend/app/core/database.py` switches to the restricted `finora_app` role and sets `request.jwt.claim.sub` transaction-locally. RLS policies use the verified subject.
- Every query and mutation over a user-owned account, transaction, budget, or goal still needs an explicit owner filter. RLS is defense in depth, not a replacement for application checks.
- Local `AUTH_PROVIDER=local` is demo-only. A missing bearer token can fall back to the first seeded user in that mode; never deploy it as a hosted auth configuration.

## Password and session boundaries

- Supabase Auth stores hosted passwords; the application must not copy them into `public.users` or app logs.
- Logout uses a same-origin server route to expire local session cookies and attempt provider revocation. It clears the private response cache; open same-browser tabs are notified where `BroadcastChannel` is available.
- Current hosted demo configuration has email confirmation turned off. Signup can accept an address the user does not control. This is not an acceptable public-launch policy without an explicit product/security decision and end-to-end verification.
- Password recovery depends on working Supabase email delivery. Local build success does not verify SMTP.

## Financial data invariants

- Amounts are absolute values; direction is `DEBIT`, `CREDIT`, or `TRANSFER`.
- Currency is attached to each record. Do not add different currencies without an explicit exchange-rate source and conversion timestamp.
- A cash-flow summary counts transfers as activity but excludes transfers from credits/debits and spending totals, as documented by the endpoint.
- A bank-account balance is distinct from transaction-derived cash flow. Never infer or invent one when it is absent.
- Statement preview is non-persistent. Only confirmed, server-validated rows for an owned account may be saved.
- Deduplication hashes use account and transaction identity fields. Preserve same-account scope and allow direction/currency distinctions where needed.
- Account display values may contain a label and optional last four digits; never accept/store full account numbers for this prototype.

## File handling

- Hosted function uploads are limited to 4 MB by the current flow. The statement source is processed temporarily; the app does not retain it in Supabase Storage.
- CSV is the supported default deployed parsing path. OCR for PDF/image requires the optional `requirements-ocr.txt` dependencies; installed-runtime correctness and accuracy remain unverified.
- Keep preview edits server-validated at confirmation. Parse failure, cancel, or OCR failure must not persist rows.
- Do not log filenames, source contents, account identifiers, descriptions, OCR text, or transaction values.

## Browser cache

- Successful authenticated GET responses may be cached in bounded tab memory, partitioned by Supabase user ID. Writes invalidate affected data groups; sign-out clears the cache.
- Do not move private financial data into `localStorage`, `sessionStorage`, service-worker caches, analytics, URLs, or shared demo fixtures.
- The separate TanStack Query cache and API response cache have different invalidation behavior. Check both before changing query ownership.

## Claims that are not security controls

The newer `backend/app/domain/ml/privacy_gate.py` source contains descriptive “Kiteworks”, “air-gapped”, “zero leakage”, and certificate terminology. The implemented regex masking, fixed default salt, and generated hash do not prove data isolation, prevent network egress, or produce an auditable security certification. Do not repeat those claims as guarantees. A real security perimeter requires separate design, implementation, and review.

## Known risks to check before production

1. Supabase bearer tokens are introspected over HTTP on each API request, adding latency and availability dependency.
2. Auth confirmation is disabled for the demo; account ownership of email addresses is not established.
3. Password recovery email delivery has not been confirmed end to end.
4. The historical analytics route fabricates a fallback balance of ₹148,500 for users with no accounts. This is a data-integrity defect.
5. The authenticated Overview currently renders a 10-year sample FinGraph instead of deriving the series from the current user's data.
6. Several analytics DTO outputs or copy are static, placeholder, or based on simplified assumptions. Audit the field and screen before exposing them.
7. Hosted Supabase RLS was verified at setup time; repeat verification after schema/role migrations and before production changes.

For incident handling, see [`SECURITY.md`](../SECURITY.md). Do not put credentials or real financial records in tickets or public commits.
