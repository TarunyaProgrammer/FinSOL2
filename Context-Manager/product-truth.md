# Product truth and capability status

Reviewed against `main` source and tests on 2026-11-24. Recheck before repeating these claims after a feature change.

## Product in one sentence

Finora is a personal-finance workspace for reviewing transaction records and exploring their timing, spending patterns, budgets, and savings goals. It is a prototype, not a bank, brokerage, credit product, or investment adviser.

## User-facing capability matrix

| Surface | Current behavior | Status and boundary |
| --- | --- | --- |
| Public landing page | Explains statement review and presents interactive examples and plan cards. | Example figures and plan tiers are illustrative. No billing or entitlement enforcement exists. |
| Sign-up and sign-in | Supabase Auth handles identity; Next.js SSR manages cookies and protects `/dashboard`. | Hosted email confirmation is disabled for the current demo. Signup therefore does not verify email ownership. Recovery still needs working email delivery. |
| Workspace | Shows Overview, ledger, accounts/import, calendar, budgets, goals, and insights. | A newly signed-in account should begin without fabricated user records. Some newly added visualizations below are exceptions and must be labelled synthetic. |
| Accounts | Saves user-owned account labels and optional last four digits. | A label or button labelled “Connect” does not establish a bank connection. |
| Bank sync | FastAPI calls a deterministic/demo adapter and maps generated records into canonical transactions. The bank-connect card also creates a local account label with a generated last-four value. | Simulated only. No bank credentials, provider authorization, or live institution feed is used. |
| Statement import | CSV rows can be previewed, edited, selected, deduplicated, and persisted only after explicit confirmation. | CSV is the dependable hosted path. PDF/PNG/JPG code exists behind optional PaddleX OCR dependencies; the default Vercel runtime does not include those dependencies and OCR accuracy has not been benchmarked. XLSX is not supported by the current upload flow. |
| Transactions | User-scoped list/filter, detail, manual debit/credit, deletion, and monthly summary endpoints exist. | Categories are mostly normalized/review metadata rather than a fully managed category catalog. Transfers and currencies must retain their own semantics. |
| Calendar | Month/week/day data is derived from canonical transactions. API accepts date selection. | Verify navigation wiring before assuming local controls request a selected day/week from the API. |
| Budgets | CRUD and spending totals/burn projections exist. | No implemented per-category budget allocation behavior should be promised without checking the relevant route and screen. |
| Goals | CRUD and projection logic exist. | Values are user-entered assumptions; do not present them as investment returns or guarantees. |
| Standard insights | Rules and deterministic statistics produce score/archetype, forecasts, shifts, recommendations, and outlier summaries. | Not a trained model, not an LLM, and not investment advice. Several backend DTO fields are placeholders or misleading and should not be presented as validated findings. |
| Historical insights API | `/api/v1/analytics/historical` runs an empirical sampling/statistical routine over stored transactions. | It is not a learned predictive model. The route currently substitutes a hard-coded ₹148,500 balance when the user has no accounts; treat this as a defect, never as a real balance. |
| FinGraph | `StockMarketFinancialChart` renders a 2017–2026 synthetic 120-month record set when no explicit records are passed. The Overview currently renders it without passing user transaction records. | This can appear in an authenticated workspace but is not that user's financial history, live market data, a portfolio, or a forecast. The UI copy risks implying otherwise. Do not use it to make claims about personal performance. |
| Theme | Fin, Dark, and AMOLED themes are selectable and saved as a local preference. | Theme state is presentation-only; it grants no roles or access. |
| Plans and roles | Plans are public concept cards; role controls are presentation/demo behavior. | No checkout, subscription, server-side entitlements, or RBAC product feature exists. |

## Claims the UI or docs must not make

- “Live bank connection”, “verified account”, or “connected to HDFC/SBI/ICICI/Axis” for the current mock adapter flow.
- “Live market”, “stock-market data”, “investment tracking”, “returns”, or “personal 10-year trajectory” for the synthetic FinGraph records.
- “AI-powered”, “trained ML”, “Isolation Forest”, “RAG”, or “personalized AI” for current deterministic analytics.
- “Kiteworks”, “zero trust”, “air-gapped”, “zero leakage”, “privacy certificate”, or “tamper-proof audit” as implemented security guarantees. The `privacy_gate.py` module performs a few regex replacements and hashes an identifier with a hard-coded default salt; it does not establish an isolated perimeter or prove zero data egress.
- “Accurate OCR” or production-ready scanned statement import.
- “Plans include X” as an enforceable entitlement.

## Current material gaps

1. The Overview shows generated 10-year chart data in place of actual user history.
2. The historical analytics endpoint can return a fabricated fallback balance.
3. Several analytics fields use placeholders, static text, or aggregate assumptions. The Insights screen hides some known misleading fields; recheck backend and frontend together before exposing more.
4. The bank-connect UI wording overstates what its simulated account-creation action does.
5. Hosted email verification is off for the demo; password recovery email delivery is not verified.
6. Optional OCR is not installed in the normal deployment and has no documented accuracy benchmark.
7. Frontend unit and end-to-end coverage is absent.

These gaps are not roadmap promises. Fix the implementation and verify the behavior before changing this page to call them shipped.

## Primary source paths

- Public site: `frontend/src/components/landing/`
- Auth and route protection: `frontend/src/app/login/page.tsx`, `frontend/src/app/dashboard/page.tsx`, `frontend/src/proxy.ts`, `frontend/src/lib/supabase/`
- Workspace mode, account/import flow: `frontend/src/components/prototype/PrototypeDashboard.tsx`, `frontend/src/components/prototype/LiveBankConnectCard.tsx`
- FinGraph data: `frontend/src/components/charts/StockMarketFinancialChart.tsx`, `frontend/src/lib/demo-data.ts`, `frontend/src/components/prototype/WorkspaceOverview.tsx`
- REST API: `backend/app/api/v1/`
- Analytics and bank adapters: `backend/app/domain/ml/`, `backend/app/domain/bank_adapters/`
- Persisted schema: `backend/app/db/models/`, `supabase/migrations/`
