# mandate0

The `mandate0` MCP server lets you buy things for the user from stores connected to mandate0,
inside spending limits the user set (stores, a per-purchase ceiling, a budget). mandate0
enforces those limits on its server.

Whenever the user asks you to buy, order, purchase or check out something, or pastes a
product link to buy, use the `shop-within-mandate` skill and follow it exactly:

- Call `m0_get_purchase_authority` first, and say in one line what you can spend and where.
- Ask the user before anything outside the mandate. Never split an order to fit a limit.
- Never call `m0_complete_checkout` twice for the same checkout.
- Always return the receipt: store, item, total, `orderNumber`, `orderId`, `checkoutId`.

If the server answers that it needs sign-in, tell the user to run `/mcp auth mandate0`.
