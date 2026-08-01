---
name: Deposit into a Superform vault
description: Discover a vault, compute the best deposit route across chains/bridges, and produce the deposit transaction to sign and broadcast.
api: openapi/superform-openapi-original.json
operations: [supported-chains, get-all-vaults, get-vault-data, deposit-calculate, deposit-start]
---

# Deposit into a Superform vault

Use the public Superform API (`https://api.superform.xyz`) to move funds into an
ERC-4626 / ERC-7540 vault (a "Superform"). All requests carry the API key header
`SF-API-KEY: <your key>` (see `authentication/superform-authentication.yml`).

## Steps

1. **Confirm the source chain is supported.** Call `supported-chains`
   (`GET /supported/chains`). Deposits can be cross-chain, so also check
   `supported` (`GET /supported`) for available bridges, DEXes, and AMBs.
2. **Find the target vault.** Browse with `get-all-vaults` (`GET /vaults`) or
   pull one directly with `get-vault-data`
   (`GET /vault/{vaultOrSuperformIDOrContractAddress}`). Read its yield type,
   chain, and TVL before committing.
3. **Calculate the route.** POST the deposit intent to `deposit-calculate`
   (`POST /deposit/calculate`) to get candidate routes (source token, amount,
   destination superform). This returns quoted routes; nothing is signed yet.
4. **Build the transaction.** Call `deposit-start` (`POST /deposit/start`) with
   the chosen route to receive the unsigned transaction payload.
5. **Sign and broadcast onchain.** The API never signs — the caller signs the
   returned transaction with their wallet and submits it to the chain.

## Conventions & safety

- The transaction-start endpoints return payloads to sign; there is no
  `Idempotency-Key` — replay safety is enforced onchain (see
  `conventions/superform-conventions.yml`).
- List/historical endpoints paginate with `limit` + `offset`.
- Always re-check `deposit-calculate` output right before `deposit-start`; routes
  and quotes are time-sensitive.
