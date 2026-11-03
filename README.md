# Ha Tang plugin for Claude Code

Teaches Claude Code how to build with [Ha Tang](https://hatang.dev) (Hạ Tầng): transactional email, the support chat
widget and SDKs, VietQR payments, e-invoices, webhooks and the AI assistant's knowledge.

- Skill `hatang`: the integration rules (SDK per platform, keys, `{ data, error }`, idempotency, development
  payments, webhook and IPN verification) and how to find the right guide.
- MCP server `@hatang/mcp`: searches and reads the docs; with `HATANG_API_KEY` set, also calls the API.

## Install

In Claude Code:

```
/plugin marketplace add sota-labs/hatang-claude-plugin
/plugin install hatang@hatang
```

Set `HATANG_API_KEY` in your environment for the API tools (Settings → API Keys in the Ha Tang dashboard); the docs
tools work without it. `HATANG_ENVIRONMENT` defaults to `development`.

Docs: https://hatang.dev/docs/guides/getting-started/ai-agents · Support: https://hatang.dev/contact
