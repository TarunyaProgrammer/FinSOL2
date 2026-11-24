# First-user record workflow

This page describes the implemented onboarding path and its limits. For the rule set, see [data and security contracts](../../Context-Manager/data-and-security.md); for current product discrepancies, see [product truth](../../Context-Manager/product-truth.md).

## User journey

1. The visitor signs up or signs in through Supabase Auth. Email confirmation is disabled for the current hosted demo, so signup does not verify email ownership.
2. The protected workspace loads the user's account and transaction data. A successful empty result must remain distinct from a failed request.
3. The user creates or chooses an account label. The form accepts institution, account type, and optional last four digits; it must not ask for a bank password or full account number.
4. The user selects a statement. CSV is the normal deployed path. PDF/PNG/JPG are only extractable when optional PaddleX dependencies are installed; that OCR runtime is absent from the default Vercel backend.
5. The preview is editable and temporary. CSV mapping requires dates, descriptions, and recognizable amounts with direction from separate debit/credit columns or an explicit type. Amount-only CSV is rejected when direction is ambiguous.
6. On explicit confirmation, FastAPI checks account ownership, validates the submitted records, deduplicates, and persists canonical transactions. Preview, cancel, or parse errors should not create rows.
7. The workspace reloads relevant screens through targeted cache invalidation. Sign-out clears the user's in-memory API cache and expires session cookies.

## File limits in current code

- Maximum upload: 4 MB.
- Maximum PDF pages: 20.
- Maximum parsed candidate rows: 500.
- Supported extensions: CSV, PDF, PNG, JPG/JPEG. Extension acceptance does not mean the hosted runtime has the required OCR package.
- Files are not retained in Supabase Storage. OCR temporarily writes an input file and removes it in a `finally` block.

## Review requirements

Users must be able to check dates, amount, debit/credit direction, currency/account context, and description before saving. Treat client-edited preview data as untrusted when it reaches the import endpoint. Never log statement contents or OCR output. A valid statement with no recognized rows should show a clear empty result, not invented transactions.

## Current exception that needs correction

The authenticated Overview currently renders a generated 10-year FinGraph without passing the current user's transaction series. This violates the product goal that personal figures come from the user's records. Do not use that chart as evidence of the first user's financial history; see the [product truth gap](../../Context-Manager/product-truth.md#current-material-gaps).
