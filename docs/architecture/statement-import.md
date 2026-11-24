# Statement import

The current import path separates transient preview from explicit persistence. See [first-use workflow](../agents/first-user-workflow.md) for the user journey and [Context-Manager/data-and-security.md](../../Context-Manager/data-and-security.md) for the data contracts.

## Supported formats in code

| Format | Current handling | Hosted status |
| --- | --- | --- |
| CSV | Header/column mapping for a date, description, and amount with debit/credit direction. Amount-only files without direction are rejected. | Supported default path; still requires review. |
| PDF | File signature/page checks then optional local PaddleX table recognition. | OCR dependency is absent from the default Vercel runtime. |
| PNG/JPG | Signature and image-dimension checks then optional local PaddleX table recognition. | OCR dependency is absent from the default Vercel runtime. |
| XLSX | No upload reader in the current API. | Unsupported. |

Limits in `backend/app/domain/statement_import/document_reader.py` include 4 MB, 20 PDF pages, and 500 candidate transactions. OCR files are copied to a temporary file for the PaddleX call and removed in `finally`.

## Request sequence

1. Authenticated `/statements/preview` reads and parses the upload. It never writes to the database.
2. The frontend lets the user inspect and edit candidate rows.
3. `/statements/import` requires an owned account, validated submitted rows, and explicit confirmation.
4. The backend deduplicates and writes canonical transactions under the current user.

Preview edits are untrusted input at confirmation time; keep server-side validation and owner checks. Do not preserve source files in logs or storage. OCR output is a draft and has no accuracy guarantee.
