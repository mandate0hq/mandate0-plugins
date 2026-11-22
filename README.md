# mandate0 plugins: let your AI agent shop within the limits you set

mandate0 lets an AI agent search products, build a cart and buy from real online stores for you,
without handing it your card. You set the rules once in mandate0: how much it may spend, where, and
when it must ask you first. The agent works inside those rules, and every purchase is logged.

These plugins connect your coding or chat agent to mandate0 in one step. Each one adds the mandate0
MCP server (https://platform.mandate0.com/api/mcp) and the `shop-within-mandate` skill, which teaches the agent to check
your limits before it buys.

**Works with:** Claude Code, ChatGPT and Codex, Gemini CLI, Antigravity and Cursor.

## What your agent can do

- Search connected stores for products and compare prices.
- Build a cart and check out within your spending limits.
- Ask for your approval when a purchase is over a limit you set.
- Show you what it bought, where, and why.

## Install

| Client | Install |
|---|---|
| Claude Code | `/plugin marketplace add mandate0hq/mandate0-plugins`, then `/plugin install mandate0@mandate0` |
| ChatGPT and Codex | `codex plugin marketplace add mandate0hq/mandate0-plugins`, then `codex plugin add mandate0@mandate0` |
| Gemini CLI | `gemini extensions install https://github.com/mandate0hq/mandate0-plugins` |
| Antigravity | `agy plugin install https://github.com/mandate0hq/mandate0-plugins` |
| Cursor | Cursor Marketplace, or import this repo as a team marketplace |

The first time the agent shops, it opens a mandate0 sign-in so you can connect your account. New
connections start in sandbox mode with test money; switching to real money is a deliberate choice
on the consent screen.

## Links

- Website and sign-up: https://www.mandate0.com
- Release notes: CHANGELOG.md. Each release carries the archives and SHA256SUMS.

Version 0.1.0.
