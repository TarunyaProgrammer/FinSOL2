# Database model and migrations

The current schema is defined by SQLAlchemy models under `backend/app/db/models/` and the hosted SQL migration under `supabase/migrations/`. The migration is authoritative for hosted structure; this page is a map, not a second schema definition.

## Application entities

- `users`: app profile mirror for a Supabase Auth identity; does not own hosted passwords.
- `bank_accounts`: user-owned account labels, type, masked/last-four display, currency, demo balance, and simulated adapter metadata.
- `transactions`: user/account-owned canonical financial rows with absolute amount, type/direction, currency, raw description, dedup hash, source, and metadata.
- `categories`, `merchants`: shared reference data used by normalization and classification.
- `budgets`, `budget_categories`: user budget and optional category allocation rows.
- `financial_goals`, `goal_milestones`: user goal plans and milestones.
- `ml_profiles`: schema storage shape retained by the model; its presence does not imply trained ML.

## Hosted safety model

The migration creates owner-scoped RLS policies and the restricted `finora_app` role. FastAPI sets a verified user identity transaction-locally and uses both ORM owner filters and database RLS. Keep those aligned when adding tables or changing ownership rules.

## Change procedure

1. Inspect the existing model and migration before editing.
2. Add a reviewed SQL migration; do not rely on `Base.metadata.create_all()` for hosted schema changes.
3. Update API schemas, owner checks, RLS/grants, and isolated tests together.
4. Verify migration and RLS behavior in an isolated test setup. Hosted application is a separate operational action and must be reported separately.

See [the checked-in migration](../../supabase/migrations/20261121220000_initial_finora_schema.sql) for exact columns, constraints, grants, and policies.
