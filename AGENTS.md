# Finora agent instructions

Finora is a personal-finance web app. Preserve the boundary between illustrative public content and private user records. The source code, tests, migrations, and observed runtime behavior define what exists; roadmaps and design docs may describe proposals.

## Start every task

1. Run `git status -sb` and `git branch -vv`. Preserve local and untracked work.
2. Read this file and the nearest scoped file: [frontend/AGENTS.md](frontend/AGENTS.md) or [backend/AGENTS.md](backend/AGENTS.md).
3. Read [Context-Manager/README.md](Context-Manager/README.md), then the linked product, architecture, security, or operations page for the task.
4. Inspect the current implementation and relevant tests before planning. Verify any claim that comes from a roadmap, a commit message, or a demo screen.
5. For frontend design work, read [DESIGN.md](DESIGN.md) and select a relevant repo skill from [.agents/skills](.agents/skills/).

## Truth and documentation hierarchy

When sources disagree, use this order:

1. Current source and behavior.
2. Tests and checked-in database migrations/configuration.
3. [Context-Manager](Context-Manager/README.md), which records verified behavior and known limits.
4. Feature-specific implementation docs.
5. Product requirements, roadmaps, mockups, and commit messages, which can be aspirational.

Update the canonical Context-Manager page when durable implementation facts change. Keep `docs/agents/history.md` append-only. Do not duplicate the same technical fact across multiple reference docs; link to its source instead.

## Product and data contracts

- Supabase Auth owns hosted user identity and passwords. Keep authentication and authorization on the server. A role selector or theme mode never grants access.
- Every query and mutation over private records must be scoped to the authenticated owner. Preserve the restricted Postgres role and RLS context in hosted mode.
- Bank adapters and bank-connect UI are simulated. Never request, store, or transmit real bank credentials. Do not call the simulation a live connection.
- Imported statements normalize to canonical transactions. Preview is temporary; persist only explicitly confirmed, validated rows. Preserve owner checks, deduplication, and source/reviewed descriptions.
- Amount is non-negative; direction is represented by `DEBIT`, `CREDIT`, or `TRANSFER`. Keep currencies separate unless a conversion rate and conversion time are implemented.
- A successful empty read is not an error. Do not manufacture balances, transactions, risk, or forecasts to make a screen look populated. On failure, show a calm retry state.
- Analytics are statistics and rules. Do not describe them as trained ML, LLM output, RAG, personalized investment advice, or a security certification.
- Keep financial data out of persistent browser storage. Reuse the API client and scoped invalidation. Clear in-memory private data at sign-out.
- Keep plan tiers illustrative until billing and server-side entitlements exist. Keep plans on the public landing page.
- Use synthetic data for tests, screenshots, logs, and demos. Never commit real statements or personal financial details.

## Branch and remote safety

- This is a fork: `origin` is `TarunyaProgrammer/FinSOL2`; `upstream` is the original Finora repository. Check with `git remote -v` if unsure.
- Feature work belongs on a feature branch. Before syncing, fetch both remotes and inspect `git log --left-right --graph origin/main...upstream/main`.
- Never use `reset --hard`, force push, or rewrite a published branch unless the maintainer explicitly requested that exact outcome. If a sync requires discarding commits, inspect and preserve a recovery ref first.
- Before pushing, verify the target branch, commit range, worktree, checks, and whether the push triggers a deployment. Report hosted state only when observed.

## Implementation and review

- Make a focused change in the layer that owns it. Keep HTTP concerns in `backend/app/api`, validation in `app/schemas`, models in `app/db/models`, and reusable business logic in `app/domain`.
- Keep frontend API/query code in `frontend/src/lib` and `frontend/src/hooks`; keep presentation components focused. Maintain strict TypeScript.
- Keep SQLAlchemy async sessions injected through `get_db`. ORM changes and Supabase migrations must agree; schema changes require a migration plan.
- Preserve loading, populated, empty, error, keyboard, mobile, focus, and reduced-motion behavior relevant to the changed flow.
- Add or update focused coverage for behavior changes where practical. Do not expand a documentation-only task into a test-suite project. Run the listed project checks for implementation changes.
- Do not start long-running development servers on the user's behalf unless asked. Browser verification should use synthetic data and record viewport/browser evidence.

## Required checks

From `backend/`:

```bash
DEBUG=false python -m pytest -q
```

From `frontend/`:

```bash
npm run lint
npm run typecheck
npm run build
```

Follow [Context-Manager/development.md](Context-Manager/development.md) for environment setup and verification boundaries. A local build does not prove hosted auth, email, database, or deployment behavior.

## Leave the repository understandable

- For meaningful product or architecture changes, update the relevant Context-Manager page, user-facing setup/usage docs, and append a dated entry to `docs/agents/history.md`.
- Link the code or migration that owns a contract. Record known gaps and what was not verified.
- Keep public setup examples free of secrets. Do not paste local environment values into docs.
- Shared skills live in `.agents/skills/<skill-name>/SKILL.md`; list their purpose and external-tool requirements in [Context-Manager/skills.md](Context-Manager/skills.md).
