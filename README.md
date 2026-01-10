# BitVault: Bitcoin-Backed Stablecoin Protocol

## Overview

**BitVault** is a decentralized finance (DeFi) protocol built on the **Stacks blockchain**, enabling users to mint Bitcoin-backed stablecoins (BVUSD) through **over-collateralized vaults**.  
It bridges Bitcoin's unmatched security and value preservation with DeFi composability, offering a secure and algorithmically managed stablecoin ecosystem fully anchored to Bitcoin.

BitVault provides:

- Trust-minimized, Bitcoin-collateralized stablecoin issuance
- Dynamic risk management via automated liquidations
- Robust oracle-based price feeds
- On-chain governance for key protocol parameters
- Direct Bitcoin settlement guarantees through Stacks

## Features

✅ **Bitcoin Collateralization:** Mint stablecoins using BTC locked via Stacks smart contracts.

✅ **Over-Collateralized Vaults:** Maintain stability and solvency through enforced collateral ratios.

✅ **Algorithmic Liquidations:** Automated liquidation of undercollateralized positions ensures systemic stability.

✅ **Dynamic Fee Model:** Adjustable minting and redemption fees support responsive economic incentives.

✅ **Robust Oracle System:** Decentralized and updatable Bitcoin price feed system for accurate collateral valuation.

✅ **Governance Controls:** Permissioned updates to key risk parameters such as collateralization ratios.

## System Architecture

### Core Concepts

| Component | Description |
|:----------|:------------|
| **Vaults** | Isolated, user-owned accounts holding BTC collateral and tracking minted BVUSD. |
| **BVUSD Token** | A Bitcoin-backed stablecoin conforming to SIP-010 (Stacks Token Standard). |
| **Oracle System** | Whitelisted oracles post up-to-date BTC/USD price feeds used to calculate vault health. |
| **Liquidation Mechanism** | Under-collateralized vaults are liquidated to protect solvency and ensure BVUSD stability. |
| **Governance Parameters** | Admin-adjustable settings for mint fees, collateralization ratios, etc., ensuring adaptability over time. |

## Contract Details

### SIP-010 Token Interface

BitVault implements the standard SIP-010 trait, ensuring seamless integration with Stacks DeFi tools, wallets, and dApps.

- `transfer`
- `get-name`
- `get-symbol`
- `get-decimals`
- `get-balance`
- `get-total-supply`

### Vault System

Users interact with **vaults** to deposit Bitcoin and mint BVUSD:

- **`create-vault(collateral-amount)`**: Open a new vault by depositing BTC.
- **`mint-stablecoin(vault-owner, vault-id, mint-amount)`**: Mint new BVUSD up to the allowed collateralized limit.
- **`redeem-stablecoin(vault-owner, vault-id, redeem-amount)`**: Burn BVUSD to reclaim BTC collateral.

**Vault State:**

- `collateral-amount`: Amount of BTC locked.
- `stablecoin-minted`: BVUSD minted against the vault.
- `created-at`: Timestamp of vault creation.

### Liquidation Process

- **`liquidate-vault(vault-owner, vault-id)`**: Anyone can trigger liquidation if a vault's collateralization ratio falls below the **Liquidation Threshold** (default: 125%).

Upon liquidation:

- Vault is closed.
- Collateral is seized.
- BVUSD supply is adjusted.

### Oracle System

- **`add-btc-price-oracle(oracle)`**: Admin adds trusted price feed addresses.
- **`update-btc-price(price, timestamp)`**: Authorized oracles update the BTC price.

The BTC price is retrieved via:

- **`get-latest-btc-price()`**

### Risk Parameters & Governance

Admin-only, secure updates:

- **Collateralization Ratio**: (`update-collateralization-ratio(new-ratio)`)
  - Default: **150%**
- **Mint/Redemption Fees**: (Basis points format, 50bps = 0.5%)
- **Max Mint Limit**: Cap on the amount of BVUSD minted per vault.

## Constants and Configuration

| Parameter | Default Value | Purpose |
|:----------|:--------------|:--------|
| `stablecoin-name` | `"BitVault Protocol Token"` | Name of the BVUSD stablecoin |
| `stablecoin-symbol` | `"BVUSD"` | Ticker symbol |
| `collateralization-ratio` | `150%` | Minimum collateralization for minting |
| `liquidation-threshold` | `125%` | Collateralization ratio triggering liquidation |
| `mint-fee-bps` | `50` | 0.5% mint fee |
| `redemption-fee-bps` | `50` | 0.5% redemption fee |
| `max-mint-limit` | `1,000,000 BVUSD` | Vault-level mint cap |

## Error Codes

| Error | Description |
|:------|:------------|
| `ERR-NOT-AUTHORIZED` | Caller lacks required permissions |
| `ERR-INSUFFICIENT-BALANCE` | Not enough stablecoins for redemption |
| `ERR-INVALID-COLLATERAL` | Supplied collateral is invalid |
| `ERR-UNDERCOLLATERALIZED` | Vault would fall below required collateral |
| `ERR-ORACLE-PRICE-UNAVAILABLE` | No valid BTC price available |
| `ERR-LIQUIDATION-FAILED` | Liquidation not allowed at this time |
| `ERR-MINT-LIMIT-EXCEEDED` | Requested mint exceeds allowed limit |
| `ERR-INVALID-PARAMETERS` | One or more parameters are invalid |
| `ERR-UNAUTHORIZED-VAULT-ACTION` | Caller is not owner of the vault |

## Security Considerations

- **Price Feed Trust:** Only authorized oracles can update BTC price.
- **Over-Collateralization:** Prevents systemic insolvency.
- **On-Chain Governance:** Risk parameter changes require contract owner privileges.
- **Hard Limits:** Maximum acceptable BTC price and timestamp limits mitigate overflow attacks.

## Future Enhancements (Roadmap Ideas)

- Multi-Oracle Aggregation with Medianized Pricing
- Automated Fee Adjustment Based on Volatility
- Cross-chain BTC Collateralization (e.g., via Lightning Network channels)
- Vault Insurance Pools
- DAO Governance Upgrade (decentralized ownership)

## Contributing

Contributions, ideas, and feedback are welcome!  
Please open issues, suggest improvements, or create pull requests to improve BitVault's resilience and utility.
