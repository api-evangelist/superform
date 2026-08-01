---
name: Track a wallet's Superform portfolio and rewards
description: Read a wallet's vault positions, token and superposition balances, historical portfolio value, and claimable protocol rewards.
api: openapi/superform-openapi-original.json
operations: [user-balances, token-balances, superpositions-balances, get-claimable-rewards, get-user-earning-rewards, get-token-distribution]
---

# Track a wallet's Superform portfolio and rewards

Read-only reporting over the public Superform API (`https://api.superform.xyz`)
for a single wallet address. Send `SF-API-KEY: <your key>` on every request.

## Steps

1. **Portfolio over time.** Call `user-balances`
   (`GET /user/portfolio/{address}`) for the historical portfolio balances of an
   address. Use `start_timestamp` / `end_timestamp` / `granularity` to window the
   series and `limit` + `offset` to page.
2. **Current token balances.** Call `token-balances`
   (`GET /token/balances/{address}`) for ERC-20 balances and
   `superpositions-balances` (`GET /token/superpositions/balances/{address}`) for
   the wallet's superposition (vault-share) holdings.
3. **Rewards earned & claimable.** Call `get-user-earning-rewards`
   (`GET /protocolRewards/earning/{user}`) and `get-claimable-rewards`
   (`GET /protocolRewards/claimable/{user}`) to see accrued vs. claimable
   protocol rewards.
4. **Token distribution.** Call `get-token-distribution`
   (`GET /token-distribution/{user}`) for the user's reward-token distribution.
5. **(Optional) Build a claim.** To act on claimable rewards, call `claim-rewards`
   (`GET /protocolRewards/claim/{chain_id}/{user}`) to get the unsigned claim
   transaction, then sign and broadcast it.

## Conventions

- All list/historical endpoints paginate with `limit` + `offset`.
- Reporting endpoints are read-only; only the claim endpoint returns a
  transaction to sign (see `conventions/superform-conventions.yml`).
