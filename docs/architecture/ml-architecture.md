# Analytics implementation

Finora's current analytics are deterministic calculations and rules. The `backend/app/domain/ml/` directory name is historical; it does not mean the product uses a trained ML model.

## Current components

| Module | Current responsibility |
| --- | --- |
| `features.py` | Derives transaction-window features and data sufficiency values. |
| `forecaster.py` | Computes a deterministic month-end spending projection and interval from observed activity. |
| `change_detection.py` | Compares recent transaction periods with a prior baseline. |
| `anomaly.py` | Flags statistical outliers; current implementation is heuristic/statistical, not Isolation Forest. |
| `recommendations.py` | Applies rules to feature and shift outputs to create suggestions. |
| `historical_engine.py` | Runs empirical/statistical sampling over records for selected time windows. |
| `api/v1/analytics.py` | Scopes transactions to the current user and exposes analytics DTOs. |

The frontend's Insights screen uses the overview and selected analytics routes. It hides some fields known to be placeholders or misleading; inspect both the backend DTO and current UI before exposing more.

## Important limitations

- There is no trained model, external LLM, vector database, or RAG pipeline.
- The 10-year FinGraph on the Overview uses generated records from `frontend/src/lib/demo-data.ts`; it does not read transaction history or market prices.
- The historical endpoint currently falls back to a hard-coded ₹148,500 balance when a user has no accounts. This is a defect and must not be presented as real data.
- A privacy-named helper in the historical path does regex masking and identifier hashing; it does not enforce network isolation or certify privacy.
- Statistical intervals and rule-based scores should be described with their definitions and data-sufficiency limits. They are not financial advice.

See [product truth](../../Context-Manager/product-truth.md) for the full current status and source paths.
