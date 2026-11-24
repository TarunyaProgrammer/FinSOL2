# Finora Context Manager

This folder is the durable, implementation-led reference for Finora. It is the quickest way for a new contributor or coding agent to understand the product, locate the code, avoid known false claims, and verify a change.

## Read in this order

1. [Product truth](product-truth.md) — what the product does now, what is simulated, and what remains only a concept.
2. [Architecture](architecture.md) — the request path, feature map, and code ownership.
3. [Data and security](data-and-security.md) — tenant boundaries, imports, money semantics, sessions, and known risks.
4. [Development and verification](development.md) — local setup, checks, and what each check does not prove.
5. [Operations](operations.md) — Vercel, Supabase, runtime limits, and hosted verification.
6. [Skills](skills.md) — project-shared agent workflows and their requirements.
7. [Milestone history](../docs/agents/history.md) — append-only decisions and project milestones. Use Git for the complete commit record.

## What counts as truth

Current source, tests, migrations, and observed runtime behavior establish implementation. A screen can be clickable and still be demo-only. A route can return a DTO while including placeholder values. A committed migration does not prove that a hosted database has applied it. Product proposals and older architecture docs must not override those checks.

When a durable fact changes, update the relevant page here and add an entry to [`docs/agents/history.md`](../docs/agents/history.md). Keep feature-specific implementation detail near its code when it is likely to change with that feature.

## Fast map

| Question | Start here |
| --- | --- |
| What can a user actually do? | [Product truth](product-truth.md) |
| Where does a request go? | [Architecture](architecture.md) |
| How are owner privacy and money direction enforced? | [Data and security](data-and-security.md) |
| How do I run checks? | [Development](development.md) |
| What is deployed or configured externally? | [Operations](operations.md) |
| Which reusable skills are included? | [Skills](skills.md) |
| Why does the repo have its current shape? | [Milestone history](../docs/agents/history.md) |
| What visual rules should a UI change preserve? | [`DESIGN.md`](../DESIGN.md) |

## Legacy document map

`docs/architecture/` contains focused implementation notes; `docs/product/ux-principles.md` contains the experience principles. Supabase and Vercel setup steps remain in [`docs/supabase/setup.md`](../docs/supabase/setup.md) and [`docs/deployment/vercel.md`](../docs/deployment/vercel.md). Older speculative architecture and roadmap files were removed rather than kept as competing references.
