# mandate0 plugin for ChatGPT and Codex

Lets ChatGPT and Codex buy things for you from stores connected to mandate0, inside the
spending limits you set. The assistant checks your limits before it shops, asks you before
anything outside them, and gives you the order number and receipt when it is done.

This is one package in the [Agent Plugins](https://agent-plugins.org) format, which OpenAI
uses for both ChatGPT and Codex.

## Install in Codex

```
codex plugin marketplace add iankiku/mandate0
codex plugin add mandate0@mandate0
codex mcp login mandate0
```

Sign in, review the consent screen and approve. New connections use **sandbox** money; live
money takes a separate, deliberate choice on the consent screen.

## Optional: verified install (local bridge)

The default connection is remote and stays that way. To have stores see this install as
**verified** (identity tier 1), run the local bridge instead. It needs Node 20+ and the
`@mandate0/cli` package. On first start it opens the browser to sign in, makes a key for this
install (kept in a file only you can read) and signs every request with it.

`@mandate0/cli` is not on npm yet (coming soon). Until it is, build it from a mandate0 checkout
with `bun install && bunx turbo run build --filter=@mandate0/cli`, then use
`node /path/to/mandate0/packages/cli/dist/bin.js` wherever `m0` appears below.
How the tiers work: https://docs.mandate0.com/docs/know-your-agent

In Codex ([format](https://learn.chatgpt.com/docs/extend/mcp?surface=cli)):

```
codex mcp add mandate0 -- node /path/to/mandate0/packages/cli/dist/bin.js mcp
```

ChatGPT on the web can't run a local bridge; it connects remotely.

## Install in ChatGPT

Until the plugin is listed in the ChatGPT plugin directory, add the MCP server as a custom
connector (see the docs link below) and leave the OAuth client id and secret blank.

## What's inside

- `plugin.json`: package identity and the ChatGPT/Codex listing (`extensions.com.openai`).
- `mcp.json`: the hosted mandate0 MCP server over streamable HTTP. Sign-in is OAuth in your
  browser (discovery and dynamic client registration); the package carries no keys.
- `skills/shop-within-mandate`: how the assistant shops inside your limits and reports the receipt. A copy of
  `integrations/claude-plugin/skills`: edit it there, then run `make plugin-skills-sync`.

## Links

- Docs: https://docs.mandate0.com/docs/mcp/clients
- Privacy: https://www.mandate0.com/en/privacy
- Terms: https://www.mandate0.com/en/terms
- Support: https://github.com/iankiku/mandate0/issues
