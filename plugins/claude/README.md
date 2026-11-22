# mandate0 plugin for Claude

Lets Claude buy things for you from stores connected to mandate0, inside the spending limits
you set. Claude checks your limits before it shops, asks you before anything outside them, and
gives you the order number and receipt when it is done.

## Install

In Claude Code:

```
/plugin marketplace add iankiku/mandate0
/plugin install mandate0@mandate0
```

Then open `/mcp`, select `mandate0` and choose **Authenticate**. Sign in, review the consent
screen and approve. New connections use **sandbox** money; live money takes a separate,
deliberate choice on the consent screen.

## What's inside

- **MCP server** `mandate0`: the hosted mandate0 endpoint over HTTP. Sign-in is OAuth in your
  browser; the plugin carries no keys.
- **Skill** `shop-within-mandate`: how Claude shops inside your limits and reports the receipt. This is the
  source copy; the other buyer integrations ship it via `make plugin-skills-sync`.

## Optional: verified install (local bridge)

The default connection is remote and stays that way. To have stores see this install as
**verified** (identity tier 1), run the local bridge instead. It needs Node 20+ and the
`@mandate0/cli` package. On first start it opens the browser to sign in, makes a key for this
install (kept in a file only you can read) and signs every request with it.

`@mandate0/cli` is not on npm yet (coming soon). Until it is, build it from a mandate0 checkout
with `bun install && bunx turbo run build --filter=@mandate0/cli`, then use
`node /path/to/mandate0/packages/cli/dist/bin.js` wherever `m0` appears below.
How the tiers work: https://docs.mandate0.com/docs/know-your-agent

Use it in place of the plugin's server, for one project in `.mcp.json`
([format](https://code.claude.com/docs/en/mcp)):

```json
{
  "mcpServers": {
    "mandate0": {
      "command": "node",
      "args": ["/path/to/mandate0/packages/cli/dist/bin.js", "mcp"]
    }
  }
}
```

or for every project: `claude mcp add --scope user mandate0 -- node /path/to/mandate0/packages/cli/dist/bin.js mcp`.
Sign in ahead of time with `m0 mcp login`.

## Links

- Docs: https://docs.mandate0.com/docs/mcp/clients
- Privacy: https://www.mandate0.com/en/privacy
- Terms: https://www.mandate0.com/en/terms
- Support: https://github.com/iankiku/mandate0/issues
