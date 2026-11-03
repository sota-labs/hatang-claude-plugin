# Ha Tang plugin for Claude Code

Teaches Claude Code how to build with [Ha Tang](https://hatang.dev) (Hạ Tầng): transactional email, the support chat
widget and SDKs, VietQR payments, e-invoices, webhooks and the AI assistant's knowledge.

- Skill `hatang`: the integration rules (SDK per platform, keys, `{ data, error }`, idempotency, development
  payments, webhook and IPN verification) and how to find the right guide.
- MCP server `@hatang/mcp`: searches and reads the docs; with an API key, also sends email, creates VietQR payments, reads e-invoices and works the support inbox.

## Install

In Claude Code:

```
/plugin marketplace add sota-labs/hatang-claude-plugin
/plugin install hatang@hatang
```

When you enable the plugin, Claude Code asks for two settings (change them later in `/config`):

| Setting | |
|---|---|
| Ha Tang API key | Optional, stored in secure storage. Create one in the Ha Tang dashboard under Settings → API Keys with only the scopes the agent needs. Without it only the docs tools work. |
| Payments environment | `development` (default: no money moves, test transfers can be simulated) or `production` |

## What it runs, sends and fetches

- **Runs** `npx -y @hatang/mcp@0.1.1` (the [npm package](https://www.npmjs.com/package/@hatang/mcp)) as a local stdio
  MCP server. npx downloads it from npm the first time.
- **Fetches** `https://hatang.dev/docs/llms.txt` and Markdown pages under `https://hatang.dev/docs/` when the agent
  calls `search_docs` or `read_doc`. These requests carry no key.
- **Sends** API requests to `https://api.hatang.dev` only when the agent calls an API tool and you set a key, with
  `Authorization: Bearer <your key>`. What the tool sends is what you or the agent asked for: an email's fields, a
  payment's amount and description, a message reply, a knowledge document. Tools that send, create or change
  something are annotated so Claude Code asks before running them.
- **Stores** nothing itself. The key stays in Claude Code's secure storage and is passed to the server as
  `HATANG_API_KEY`. The server never prints it.
- The skill is text only: rules and links, no code runs.

Privacy policy: https://hatang.dev/legal/privacy · Terms: https://hatang.dev/legal/terms

Docs: https://hatang.dev/docs/guides/getting-started/ai-agents · Support: https://hatang.dev/contact
