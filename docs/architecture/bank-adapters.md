# Simulated bank adapters

The adapter interface makes synthetic bank activity reusable by the demo API. It is not a live open-banking integration.

## Implementation

- Interface: `backend/app/domain/bank_adapters/base.py`
- Factory: `backend/app/domain/bank_adapters/factory.py`
- Current deterministic demo: `backend/app/domain/bank_adapters/hsbc_demo.py`
- API: `backend/app/api/v1/bank.py`
- Frontend account/connect surface: `frontend/src/components/prototype/LiveBankConnectCard.tsx`

`POST /api/v1/bank/sync` verifies ownership of the selected account, invokes a registered adapter, normalizes generated rows into canonical transactions, deduplicates them, and updates demo sync metadata. The UI's “Connect” action creates an account label with a generated last-four value; it does not authenticate with the named bank.

## Contract

- Keep all adapters deterministic or synthetic and label them as demos.
- Never request or store a real bank password, PIN, OTP, or provider access token.
- An institution logo/name or account label does not prove an external connection.
- Before adding a real provider, require a separate product, legal, data-security, credential, and provider-integration design. Do not retrofit real credentials into this interface without that review.

See [product truth](../../Context-Manager/product-truth.md) for the current UI overclaim and [data/security contracts](../../Context-Manager/data-and-security.md).
