---
name: Reconcile Soldo transactions and attach receipts
description: Search settled transactions, read details, categorise, and attach receipt documents.
api: openapi/soldo-software-and-services-business-api-openapi.json
operations: [authorize, transaction-search, transaction-get, transaction-update, transaction-attachment-upload, transaction-attachment-upload-confirm, transaction-expense-category-set]
scopes: [transaction_read, transaction_write, expense_category_read]
---

# Reconcile Soldo transactions and attach receipts

Operating instructions for pulling transactions, enriching them, and attaching receipts.

## Auth
1. `authorize` (OAuth2 client-credentials) → `Authorization: Bearer {access_token}`.
2. Reading/searching transactions requires advanced authentication — compute `X-Soldo-Fingerprint` (SHA-512 of the ordered field list on the endpoint's reference page, lowercase) and `X-Soldo-Fingerprint-Signature` (SHA512withRSA, Base64). See conventions/ and authentication/.

## Steps
1. `transaction-search` — filter by `fromDate`/`toDate`/`dateType`/`category`/`status` (advanced-auth required). Page with `p`/`s`.
2. `transaction-get` — read a specific `Transaction`.
3. `transaction-expense-category-set` — assign an `ExpenseCategory`.
4. `transaction-attachment-upload` — request an `UPLOAD_URL`, PUT the receipt file to it, then `transaction-attachment-upload-confirm` to finalise (pre-signed URL pattern).
5. `transaction-update` — update the assignee where applicable (company card transactions only; assignee must be a card assignee).

## Conventions & errors
- Errors: `{ error_code, message }`; `400 INVALID_FINGERPRINT`/`INVALID_FINGERPRINT_SIGNATURE` mean the advanced-auth headers are wrong (see errors/).
- Rate limits: 600/min, 20/sec.
