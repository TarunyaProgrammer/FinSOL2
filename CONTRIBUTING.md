# Contributing to Finora

Start with the repository [agent instructions](AGENTS.md) and [Context Manager](Context-Manager/README.md). This is a fork of another repository; confirm which remote and branch a change targets before syncing or publishing it.

## Work flow

1. Check `git status -sb`, `git branch -vv`, and `git remote -v`. Keep unrelated files and changes intact.
2. Create a feature branch from the intended base. Do not work directly on `main` for normal feature work.
3. Read the scoped frontend/backend instructions and inspect current implementation before editing.
4. Make a focused change. Keep mock behavior, real user data, and future plans clearly distinct.
5. Run the relevant checks below and record any checks that could not run.
6. Update the Context-Manager page when behavior or a durable contract changes. Append a dated entry to `docs/agents/history.md` for meaningful milestones.
7. Open a pull request with the user outcome, scope, verification evidence, and known gaps.

## Checks

Backend, from `backend/`:

```bash
DEBUG=false python -m pytest -q
```

Frontend, from `frontend/`:

```bash
npm ci
npm run lint
npm run typecheck
npm run build
```

Use synthetic fixtures. A successful build does not prove deployed auth, Supabase RLS, SMTP, browser behavior, or OCR accuracy.

## Pull request content

- State what changed and why, with links to the main implementation paths.
- Include screenshots for visual changes, with browser and viewport. Never use personal statements or real financial records.
- Describe API, schema, migration, auth, owner-isolation, cache-invalidation, or environment impacts.
- Separate local checks from browser, hosted, and database verification.
- Call out behavior still simulated or unavailable.
- Do not include secrets, tokens, real bank credentials, user records, or unreviewed generated data.

## Fork synchronization

`origin` is the FinSOL2 fork and `upstream` is the original Finora repository. Fetch both and inspect the commits on each side before updating. Do not reset or force-push a published branch just to clear GitHub's ahead/behind indicator. If the maintainer requests exact alignment and discarding fork commits, preserve a recovery ref and verify both branch tips before pushing.
