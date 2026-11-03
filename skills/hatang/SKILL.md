---
name: hatang
description: Use when building with Ha Tang (hatang.dev, Hạ Tầng) — sending transactional email, adding the support chat widget or a chat SDK, accepting VietQR bank-transfer payments, issuing e-invoices, handling Ha Tang webhooks or IPN, or adding knowledge for its AI assistant.
---

# Ha Tang (hatang.dev)

This project uses [Ha Tang](https://hatang.dev) (Hạ Tầng) for email, customer support chat and/or VietQR payments.

- Read the docs before writing Ha Tang code; don't guess endpoints or fields. Index: https://hatang.dev/docs/llms.txt. Every docs page has a Markdown copy at its URL plus `.md`. The REST API is described by https://hatang.dev/docs/openapi.json (base URL `https://api.hatang.dev`).
- If the `hatang` MCP server is available (`npx -y @hatang/mcp`), use its `search_docs` and `read_doc` tools.
- Use the official SDKs: `@hatang/sdk` (Node.js, server), `hatang` (Python, server), `@hatang/chat-react` (React/Next.js widget), `@hatang/chat-react-native`, `hatang_chat` (Flutter), `HatangChat` (iOS, Swift Package), `dev.hatang:chat` (Android), or the `https://chat.hatang.dev/widget.js` script tag on plain websites.
- The API key (`ht_...`) is a server-side secret: read it from `HATANG_API_KEY`, never put it in client code, mobile apps or commits. Chat SDKs use the public site id (`site_...`) instead.
- SDK calls return `{ data, error }` and do not throw for API errors: check `error` (branch on `error.name`).
- Send an `Idempotency-Key` on `POST /emails` and `POST /payments` (`idempotencyKey` option in the Node SDK; `payments.createOrder` uses the order id).
- Email: `from` must be on a domain verified in Ha Tang.
- Payments: build against the development environment (`HATANG_ENVIRONMENT=development` or the `Hatang-Environment` header), where `payments.simulateTransfer` settles a payment. Before fulfilling an order, confirm on the server with `payments.verify(id, { amount, order_id })`; never trust the browser redirect.
- Webhooks: verify the signature on the raw request body with `constructWebhookEvent(rawBody, headers, secret)` (Python: `construct_webhook_event`). IPN calls for payments: `constructIpnEvent(rawBody, headers, secretKey)` (Python: `construct_ipn_event`). Don't parse the body as JSON before verifying.

## How to work

1. Find the guide: fetch https://hatang.dev/docs/llms.txt (or call `search_docs` on the `hatang` MCP server) and read the pages that match the task as Markdown (`<page URL>.md`).
2. Pick the SDK for the platform from the list above and install it; on a server, read `HATANG_API_KEY` from the environment and ask the user to add it if missing.
3. Write the integration from the guide's examples, handle `error` on every call, and verify webhook or IPN signatures on the raw body.
4. Test against development: send to a test address, use `HATANG_ENVIRONMENT=development` and `simulateTransfer` for payments.
