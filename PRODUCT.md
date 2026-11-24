# Finora product brief

Finora helps people understand the financial records they choose to add and plan budgets and savings goals from those records. Its central workflow is: review a statement, confirm the canonical transactions to keep, then explore their timing and patterns in the workspace.

## Current product boundaries

- Public landing examples, plan cards, and the FinGraph are illustrative or synthetic.
- Supabase Auth owns hosted identity. The signed-in workspace uses user-owned records and should begin empty for a new user.
- Statement preview is temporary; only user-confirmed, validated transactions are saved.
- Bank adapters simulate data. Finora does not connect to real financial institutions.
- Analytics are deterministic statistics and rules, not a trained machine-learning model.
- No subscription, checkout, enforced plan tiers, team tenancy, or investment product is implemented.

## Audience and tone

The current brief is for students and young professionals managing everyday spending, budgets, and savings goals. Use calm, clear, non-judgmental language. Show what is observed, what is estimated, and what is only an example as separate things.

## Experience goals

1. Explain the product without implying live bank integrations or personalized outcomes from sample data.
2. Make first use honest: an empty account is a valid state, not a gap to fill with sample balances.
3. Make imported data reviewable before it persists.
4. Keep charts and recommendations understandable, with definitions for their inputs and limits.
5. Preserve keyboard use, mobile usability, readable contrast, and reduced-motion behavior.

For verified capabilities and known discrepancies, see [Context-Manager/product-truth.md](Context-Manager/product-truth.md). For visual decisions, see [DESIGN.md](DESIGN.md).
