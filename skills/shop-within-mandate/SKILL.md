---
name: shop-within-mandate
description: Set up a connected Mandate0 agent for buying or selling, and shop inside human-confirmed limits. Use when the user asks to set up their agent or store, buy, order, check out, or find a product with Mandate0 tools connected.
---

# Shop inside the user's mandate

The user has given you a mandate: a set of spending limits (stores, a per-purchase ceiling, a
budget). mandate0 enforces it on the server. Your job is to stay inside it, to ask before
anything outside it, and to always hand back proof of what you bought.

## 1. Connect, experience, then authorize

Searching needs no mandate. Do not stop discovery because the user has no grants.
On setup requests, ask whether the user wants to buy or sell, then call
`m0_start_setup` with `journey: buy` or `journey: sell`. Resume with
`m0_get_setup_status`; use its server facts and human-action links.

For buying, retain the returned agent and wallet IDs. Before completing a purchase,
call `m0_get_purchase_authority`. If no suitable grant exists, agree the exact store,
limit, currency, expiry and cart-confirmation rule with the user, then call
`m0_prepare_mandate` with those terms and a stable idempotency key. A draft cannot spend.
Show its embedded review panel if the host supports MCP Apps; otherwise give its
`reviewUrl`. Poll `m0_get_mandate_draft` after the person returns. Continue only when
server state confirms approval and the purchase still fits the mandate.

Never ask for SMS codes, card details, bank details or recovery codes in chat. The
review component sends encrypted approval data through MCP. Do not call the app-only
verification tools yourself. Phone enrollment alone does not approve a mandate.
Never claim an embedded popup works in a host until that host actually renders it.

For selling, follow the returned store and payout handoffs, then check status again.
Do not claim store setup is complete from a clicked link or a user's "done".
Existing tokens missing `setup:write` need reconnection and explicit setup consent.
If the new tools are absent, explain that this server has not deployed agent-first
setup yet; use its existing secure dashboard handoff. Do not invent tool success.

Tell the user when the connection is sandbox. It moves no real money.

## 2. Find the product

- A product link: `m0_buy_from_link` with the `url`. It refuses links from stores that are not
  connected, and says why in `code`.
- A description: `m0_search_products`, or `m0_list_merchants` then `m0_get_merchant_profile`
  to get a `merchantId`. Never guess a `merchantId` from a store name.
- Confirm the exact item, variant, quantity and price with the user if anything is ambiguous.
- If you can fetch web pages, read the store's `mandate.json` before the first purchase there:
  `https://{store}/.well-known/mandate.json` (Shopify: `https://{store}/apps/mandate/mandate.json`)
  is a pointer, and its `document` is the signed file. If `store.domain_verified` is false,
  tell the user in one line that mandate0 has not yet confirmed this domain belongs to the
  connected store. It is not a reason to refuse; the user's limits still apply at checkout.
  Its `agents.policy` says which agents the store accepts, and live payments need identity tier
  `agents.live_payments_min_tier` (2) or better. If this connection cannot meet them, tell the
  user before you start a checkout there.

## 3. Ask before anything outside the mandate

Stop and ask the user before you create a checkout when any of these is true:

- The store is not one of the stores the grant covers.
- The total (with tax and shipping) is above the per-purchase ceiling or the remaining budget.
- The item is not what the user asked for (a substitute, a different size, a subscription).

Never split one order into smaller ones to fit under a ceiling. Never pick a different store
just because it is inside the mandate without telling the user.

## 4. Check out

1. `m0_create_checkout` with the store domain and items. Read the `status`:
   - `policy_denied`: the mandate refused it. Tell the user the `reason`. Do not retry.
   - `requires_approval`: above the approval threshold. Offer to ask the owner.
   - `agent_not_accepted`: the store, or live money, needs a more strongly verified agent
     (`requiredTier`). Nothing was charged. Tell the user in one line. Do not retry with another
     name or header: the tier does not change.
2. To get the owner's sign-off, call `m0_request_owner_confirmation` for that checkout. The
   owner gets a one-tap link. Poll `m0_get_owner_confirmation` until it is approved or
   declined.
3. `m0_complete_checkout` with the `checkoutId` (plus `approvalId` if the owner approved, or
   `grantId` if several grants cover the store).

Never call `m0_complete_checkout` twice for the same checkout. If a call fails or times out,
read the state with `m0_get_checkout` before doing anything else.

## 5. Always return the receipt

After every completed purchase, give the user:

- the store, the item and quantity
- the total and currency
- the `orderNumber` and `orderId`
- the `checkoutId` and the mandate it was charged under (`_governance.mandateId`)

If the answer has `unresolved` and a `continueUrl`, the order is not finished: give the user
the link and say what is left. To show an order again later, use `m0_get_transaction`.

If nothing was bought, say so plainly and say why. Never report a purchase you did not
complete.
