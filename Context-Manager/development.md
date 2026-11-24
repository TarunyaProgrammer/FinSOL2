# Development and verification

## Requirements

- Python **3.13** for the backend (`backend/.python-version`). The optional OCR dependency set also targets Python 3.13.
- Node.js **20** for the frontend CI workflow.
- Two terminals for normal local work: Next.js on port `3000`, FastAPI on port `8000`.
- Supabase project values for hosted-style sign-in and database access. Keep them in ignored local environment files.

## Local setup

```bash
cd backend
python3.13 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
DEBUG=false python -m pytest -q
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

In a second terminal:

```bash
cd frontend
npm ci
cp .env.local.example .env.local
npm run dev -- --hostname 127.0.0.1
```

Fill in only the values described by [`docs/supabase/setup.md`](../docs/supabase/setup.md). Do not copy real values into this file, issues, screenshots, or chat. The frontend rewrite targets `http://127.0.0.1:8000/api/v1` by default. `ECONNREFUSED` in the Next log usually means the API process is not listening there.

Useful local URLs:

- Web: `http://127.0.0.1:3000`
- Demo shell: `http://127.0.0.1:3000/dashboard/demo`
- API health: `http://127.0.0.1:8000/health`
- OpenAPI docs: `http://127.0.0.1:8000/api/v1/docs`

The demo shell can prove layout behavior only; it does not prove auth, API owner isolation, persistence, hosted email, or database configuration.

## Checks

The current CI workflow runs:

```bash
# backend, from backend/
DEBUG=false python -m pytest -q

# frontend, from frontend/
npm ci
npm run lint
npm run typecheck
npm run build
```

Use focused backend tests for changed behavior and preserve the isolated test database from `backend/tests/conftest.py`. The frontend has no checked-in unit or end-to-end suite today. Do not describe a lint/build pass as a browser or hosted smoke test.

## Verification

For meaningful workspace changes, use synthetic data such as the root `sample-statement-3-months.csv` and check:

1. Empty workspace and populated workspace separately.
2. Loading, successful empty response, failed read/retry, and post-write refresh.
3. Desktop and narrow mobile widths, no horizontal overflow, keyboard focus, and reduced motion.
4. User scoping across account/transaction changes and a fresh protected request after logout.
5. Import preview before confirmation, edits across review pages, deduplication, and no writes on cancel/failure.
6. Overview month boundaries, separate currencies, and transfer treatment.
7. Network response status for local assets and API requests when investigating a loading issue.

Capture viewport/browser and what data was used. Keep screenshots synthetic; never use a real user's account or statement.

## Fork workflow

- Check `git status -sb`, `git branch -vv`, and `git remote -v` before touching refs.
- `origin` is the FinSOL2 fork; `upstream` is the original Finora repository.
- Create a feature branch for changes. Fetch both remotes before comparing or syncing. Inspect both sides of a divergence before deciding which commits to keep.
- Never use a force push to make counts look clean. Rewriting `main` is a maintainer decision and requires an explicit request.

## Documentation updates

For durable behavior changes, update the relevant `Context-Manager/` page and append a dated summary to [`docs/agents/history.md`](../docs/agents/history.md). Update README setup only when the contributor path changes. Distinguish code changes from database, hosted-setting, and deployment work.
