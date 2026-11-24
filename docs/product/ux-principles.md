# Finora experience principles

These principles describe how current product surfaces should communicate. The implementation status for each surface is in [Context-Manager/product-truth.md](../../Context-Manager/product-truth.md); visual tokens are in [`DESIGN.md`](../../DESIGN.md).

## 1. Keep observed facts separate from examples and estimates

Use confirmed user transactions for personal totals. Label public, demo, synthetic, and forecast values at the point of use. Never present a generated balance or synthetic chart as a user's history.

## 2. Make first use honest

An empty workspace is a successful state. Explain how to add an account label or import data, but do not prefill a private account with sample activity. A failed read needs an error/retry path, not zero values.

## 3. Put review before persistence

Show the extracted statement rows, their debit/credit direction, and the target account. Keep editing and exclusion available. Say clearly when the confirm action writes transactions.

## 4. Explain money and uncertainty

Use labels for currency and transaction direction. Separate transfers from inflows and spending. Define a metric's inputs and time window. Distinguish observed totals from forecasts, rules, and synthetic examples.

## 5. Avoid financial pressure and unsupported advice

Use calm, non-judgmental copy. Do not use urgency patterns to push a financial action. Suggestions should state their evidence and avoid claiming guaranteed returns or investment outcomes.

## 6. Keep interfaces usable in real conditions

Preserve keyboard operation, visible focus, readable contrast, mobile layouts, and reduced-motion behavior. Loading, empty, error, and success states are part of the feature contract.
