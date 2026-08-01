---
name: Subscribe to Soldo webhooks
description: Register, list, and manage webhook subscriptions for Soldo events.
api: openapi/soldo-software-and-services-business-api-openapi.json
operations: [authorize, webhook-subscription-add, webhook-subscription-search, webhook-subscription-get, webhook-subscription-update, webhook-subscription-delete]
scopes: []
---

# Subscribe to Soldo webhooks

Operating instructions for setting up event-driven integrations with Soldo webhooks.

## Auth
1. `authorize` (OAuth2 client-credentials) → `Authorization: Bearer {access_token}`.

## Steps
1. `webhook-subscription-add` — create a `WebhookSubscription` for the event families you need (Card, Transaction, Wallet, User, Group, Subscription, OnlineAds, Pre-Approved Spend, Order, Expense Review, Transaction Attachment, Vehicle — see asyncapi/soldo-software-and-services-webhooks.yml).
2. `webhook-subscription-search` / `webhook-subscription-get` — list and inspect existing subscriptions.
3. `webhook-subscription-update` / `webhook-subscription-delete` — change or remove a subscription.

## Verifying deliveries
- Webhook payloads are signed by Soldo. Verify the signature with your registered public key using SHA512withRSA (the same `checkSignatureByPublicKey` crypto as advanced authentication).

## Conventions & errors
- Errors return `{ error_code, message }`; `401 invalid_token` means refresh the access token (see errors/).
- Rate limits: 600/min, 20/sec.
