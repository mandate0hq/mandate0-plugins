# mandate0 plugin for Cursor

Lets Cursor's agent buy things for you from stores connected to mandate0, inside the spending
limits you set. It checks your limits before it shops, asks you before anything outside them,
and gives you the order number and receipt when it is done.

Prepared, not published: the plugin is not in the Cursor Marketplace yet. Release steps are in
`docs/CONNECTOR-LAUNCH.md` ("Cursor") and in the `RELEASE.md` that `make plugins-release` writes.

## Test it locally

1. Copy this folder (not a symlink: Cursor skips symlinked plugins) to
   `~/.cursor/plugins/local/mandate0`.
2. Run **Developer: Reload Window**, or restart Cursor.
3. Check **Customize**: the `mandate0` MCP server and the `shop-within-mandate` skill appear.
4. Open the MCP server and sign in. The browser opens the mandate0 consent screen. New
   connections use **sandbox** money; live money takes a separate choice on that screen.

On Enterprise, local plugins load only when **Allow Local Plugin Imports** is on.

## What's inside

- **MCP server** `mandate0` (`mcp.json`): the hosted mandate0 endpoint. Sign-in is OAuth in
  your browser; the plugin carries no keys.
- **Skill** `shop-within-mandate`: a copy of `integrations/claude-plugin/skills`, kept in step by
  `make plugin-skills-sync`.

## Links

- Docs: https://docs.mandate0.com/docs/mcp/clients
- Privacy: https://www.mandate0.com/en/privacy
- Terms: https://www.mandate0.com/en/terms
- Support: https://github.com/iankiku/mandate0/issues
