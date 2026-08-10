# settlement-contracts

Non-custodial settlement for Verity Protocol.

The matching engine runs off-chain. When two orders cross, the engine emits a signed batch that both counterparties have already authorized via EIP-712. On-chain, the settlement contract verifies signatures and atomically transfers assets between them. No funds ever pass through Verity.

## Architecture

Pure CLOB. Price-time priority. No AMM, no pools, no LPs. Every fill is a resting limit order that got hit.

## Contracts

### `Settlement.sol`

Settles matched CLOB trades. Both buyer and seller sign an EIP-712 `SettlementBatch`; the contract verifies signatures and atomically transfers assets. No funds pass through Verity — transfers go directly between counterparties.

- Chain: Robinhood Chain (chain ID 4663)
- EIP-712 domain: `Verity Protocol`

## Flow

1. Two traders sign orders off-chain (EIP-712)
2. Matching engine crosses them at price-time priority
3. Engine constructs a `SettlementBatch` with both signatures
4. Anyone can submit the batch on-chain
5. Contract verifies both signatures, transfers assets atomically

No custody. No sequencer trust. If the engine misbehaves, signatures still hold — the trader authorized only that price and size.

## Tests

```bash
forge test
```

## License

MIT.