# Deployment and operations

## Hosting shape

The root [`vercel.json`](../vercel.json) defines a frontend service rooted at `frontend/` and a FastAPI service rooted at `backend/`. It routes `/api/v1` and `/api/v1/*` plus `/health` to FastAPI; the remaining app paths go to Next.js. The deployed demo URL currently used by the README is `https://finora-sem3.vercel.app/`.

Hosted deployment also depends on Vercel dashboard settings, Supabase project settings, database credentials, Auth redirect URLs, and email provider configuration. Those values are not represented by `vercel.json`; check the dashboard and environment after each deployment instead of inferring success from a local build.

## Environment variables

Variable names are documented in the committed templates and deployment guide. Typical frontend settings are `NEXT_PUBLIC_API_URL`, `NEXT_PUBLIC_SUPABASE_URL`, and `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`. The backend needs `AUTH_PROVIDER=supabase`, `SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`, `DATABASE_URL`, and `SECRET_KEY`, along with runtime settings.

- `NEXT_PUBLIC_*` values are public client configuration; never put a secret there.
- The database URL and `SECRET_KEY` are private server values.
- Do not set a Supabase service-role or secret key in this application; the intended backend path uses the publishable key for Auth validation and a direct Postgres pooler connection.
- Keep `frontend/.env.local` and `backend/.env` ignored. Committed templates contain placeholders only.

See [`docs/deployment/vercel.md`](../docs/deployment/vercel.md) for the exact dashboard setup and [`docs/supabase/setup.md`](../docs/supabase/setup.md) for Auth and schema setup. Confirm the docs against current provider dashboards before applying operational changes.

## Database operations

- `supabase/migrations/` is the hosted schema source. Apply schema changes through reviewed migrations and keep them aligned with SQLAlchemy models.
- Hosted Supabase mode skips local metadata creation and demo seeding. It depends on the migration being applied and the `finora_app` role/RLS setup being intact.
- Local auth/database setup has a demo seed path. Never point it at production records.
- Test role permissions and tenant isolation after migrations. A successful connection alone does not prove that RLS is enforcing ownership.

## Image delivery

The frontend's checked-in images are under `frontend/public/images/`. On 2026-11-24, the deployed direct static URLs returned `200`, while generated `/_next/image?...` URLs returned the app's `404` page. `frontend/next.config.mjs` now sets `images.unoptimized: true` so Next `<Image>` uses those direct paths. If hosting routing changes, verify both the direct asset response and rendered image before re-enabling optimization.

## Runtime constraints

- Statement uploads are capped at 4 MB to fit the current Vercel request limit.
- CSV is the default hosted extraction path. Optional PaddleX/PaddlePaddle dependencies are not included in the standard Vercel backend runtime, so do not claim scanned OCR works there.
- OCR dependencies and accuracy need a separately evaluated runtime before enabling this flow for real users.
- Vercel health status, successful frontend build, Supabase persistence, Auth emails, and statement import are separate facts. Record each verification independently.

## Deployment verification checklist

After a user-authorized deployment, verify and report:

1. The deployment commit and production/preview URL.
2. `/` and `/login` render without console errors; local images and fonts return successful responses.
3. `/health` and `/api/v1/docs` reach FastAPI.
4. Auth redirects use the intended domain. Test sign-in and recovery with an isolated account you control.
5. A synthetic account and statement exercise preview, confirmation, persistence, and tenant scoping.
6. Vercel runtime and Supabase logs show no secrets or financial content.

Do not report deployment verification unless these checks were actually run. Use synthetic data and stop if provider rate limits or credentials block the smoke test.
