---
name: Issue and control a Soldo company card
description: Authenticate, create a card on a wallet, and apply spending-limit and category rules.
api: openapi/soldo-software-and-services-business-api-openapi.json
operations: [authorize, wallet-search, card-add, card-get, card-rules-set, card-rule-spending-limits-set]
scopes: [wallet_read, card_read, card_write]
---

# Issue and control a Soldo company card

Operating instructions for an agent using the Soldo Business API to issue a company card and set its controls. Ground every call in a real `operationId` from the spec.

## Auth
1. Get an access token with `authorize` (OAuth2 client-credentials): POST `https://api.soldo.com/oauth/authorize` with `client_id` and `client_secret` as `application/x-www-form-urlencoded`. Send `Authorization: Bearer {access_token}` on every call. Tokens expire after 7200s.
2. Card creation and rule changes are account-changing operations — include the advanced-authentication headers `X-Soldo-Fingerprint` and `X-Soldo-Fingerprint-Signature` where the endpoint's reference page requires them (see conventions/).

## Steps
1. `wallet-search` — find the target `Wallet`; the new card inherits the wallet's currency.
2. `card-add` — create the `Card` on that wallet.
3. `card-get` — confirm the card details/status.
4. `card-rule-spending-limits-set` — set spending-limit rules for the card.
5. `card-rules-set` — set the broader `CardRules` (countries, merchant categories, cash points) as needed.

## Conventions & errors
- Pagination on search: `p`, `s` (max 50), `d`, `props` (see conventions/).
- Errors return `{ error_code, message }`; `403` means the credential lacks the `card_write`/`wallet_read` scope (see errors/).
- Respect rate limits: 600/min, 20/sec (headers `x-ratelimit-*`).
