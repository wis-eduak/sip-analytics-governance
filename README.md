# SIP Analytics Governance Protocol (SAGP)

A decentralized protocol for analytics token distribution and governance on Stacks, enabling STX holders to participate in protocol decisions while earning rewards.

## Overview

SAGP is a Clarity smart contract that implements a comprehensive staking and governance system with the following key features:

- Multi-tiered staking system with variable lock periods
- Quadratic voting power based on stake duration and amount
- Time-weighted rewards with multipliers
- Governance proposal creation and voting mechanisms
- Emergency pause functionality for protocol safety

## Contract Architecture

### Token System

- Native fungible token: ANALYTICS-TOKEN
- Earned through staking STX tokens
- Used for governance participation

### Staking Mechanism

#### Tiers

1. **Tier 1**

   - Minimum Stake: 1,000,000 μSTX
   - Reward Multiplier: 1x
   - Basic features enabled

2. **Tier 2**

   - Minimum Stake: 5,000,000 μSTX
   - Reward Multiplier: 1.5x
   - Additional governance features

3. **Tier 3**
   - Minimum Stake: 10,000,000 μSTX
   - Reward Multiplier: 2x
   - Full feature access

#### Lock Periods

- No Lock: 1x multiplier
- 1 Month (4,320 blocks): 1.25x multiplier
- 2 Months (8,640 blocks): 1.5x multiplier

### Reward System

- Base reward rate: 5% annually
- Additional bonus rate: 1% for extended staking
- Rewards calculated based on:
  - Stake amount
  - Lock period multiplier
  - Tier multiplier
  - Time staked

## Governance Features

### Proposal Creation

- Minimum voting power required: 1,000,000
- Description length: 10-256 characters
- Configurable voting period: 100-2,880 blocks

### Voting System

- One vote per staked token
- Voting power scales with tier level
- Votes can be cast for or against proposals
- Active proposals tracked on-chain

## Security Features

### Access Control

- Contract owner privileges for critical functions
- Tiered access based on stake amount
- Cooldown periods for unstaking

### Safety Mechanisms

- Emergency pause functionality
- Minimum stake requirements
- Valid period checks for proposals
- Cooldown enforcement for unstaking

## Public Functions

### Staking Operations

```clarity
(define-public (stake-stx (amount uint) (lock-period uint)))
(define-public (initiate-unstake (amount uint)))
(define-public (complete-unstake))
```

### Governance Operations

```clarity
(define-public (create-proposal (description (string-utf8 256)) (voting-period uint)))
(define-public (vote-on-proposal (proposal-id uint) (vote-for bool)))
```

### Administrative Functions

```clarity
(define-public (pause-contract))
(define-public (resume-contract))
```

### Read-Only Functions

```clarity
(define-read-only (get-contract-owner))
(define-read-only (get-stx-pool))
(define-read-only (get-proposal-count))
```

## Error Codes

| Code | Description                 |
| ---- | --------------------------- |
| 1000 | Not authorized              |
| 1001 | Invalid protocol parameters |
| 1002 | Invalid amount              |
| 1003 | Insufficient STX            |
| 1004 | Cooldown active             |
| 1005 | No stake found              |
| 1006 | Below minimum stake         |
| 1007 | Contract paused             |

## Data Storage

### Maps

- `Proposals`: Stores governance proposals
- `UserPositions`: Tracks user account data
- `StakingPositions`: Manages staking positions
- `TierLevels`: Defines tier system configuration

## Implementation Notes

### Staking Process

1. User calls `stake-stx` with amount and lock period
2. Contract validates parameters and transfers STX
3. Updates user position and tier information
4. Calculates and applies reward multipliers

### Unstaking Process

1. User initiates unstake with `initiate-unstake`
2. Cooldown period begins (1440 blocks ≈ 24 hours)
3. After cooldown, user calls `complete-unstake`
4. Contract returns staked STX to user

### Governance Process

1. User creates proposal with description and voting period
2. Stakeholders vote during active period
3. Votes weighted by staking position and tier
4. Results recorded on-chain

