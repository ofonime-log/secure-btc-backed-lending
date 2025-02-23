# Secure BTC-Backed Lending Protocol

A decentralized lending protocol leveraging Bitcoin's security through Stacks Layer 2, enabling non-custodial BTC collateralization and compliant stablecoin loans with automated risk management and liquidation mechanisms.

## Overview

This protocol establishes a secure lending platform that combines Bitcoin's unmatched security with Stacks' programmability. It enables users to collateralize their Bitcoin holdings without surrendering custody until liquidation events occur, while maintaining full compliance with Bitcoin's operational constraints through Stacks' proof-of-transfer consensus.

## Key Features

### 1. Trustless BTC Collateralization

- Non-custodial Bitcoin locking via cryptographic proofs
- Native Bitcoin collateral without wrapping or synthetic derivatives
- Full custody retention until liquidation events

### 2. Stacks L2 Architecture

- Loan operations and interest calculations on Stacks Layer 2
- Sub-30 second transaction speeds through microblocks
- Bitcoin finality benefits

### 3. Risk Management

- Dynamic collateral ratios (minimum 150%)
- Multi-layered liquidation protection
- Automated health checks every block
- Liquidation threshold at 120%
- Real-time price feed integration

### 4. Compliance Framework

- Strict non-custodial principles
- On-chain audit trails
- UTXO-friendly accounting
- Native sBTC settlements

### 5. Decentralized Governance

- DAO-controlled protocol parameters
- Emergency circuit breakers
- Incentivized early repayment structure

## Smart Contract Functions

### Administrative Functions

\`\`\`clarity
initialize-platform
update-collateral-ratio
update-liquidation-threshold
update-price-feed
\`\`\`

### Core Lending Functions

\`\`\`clarity
deposit-collateral
request-loan
repay-loan
\`\`\`

### Read-Only Functions

\`\`\`clarity
get-loan-details
get-user-loans
get-platform-stats
get-valid-assets
\`\`\`

## Technical Parameters

### Constants

- Minimum Collateral Ratio: 150%
- Liquidation Threshold: 120%
- Platform Fee Rate: 1%
- Interest Rate: 5%
- Maximum Active Loans per User: 10

### Error Codes

- \`ERR-NOT-AUTHORIZED\` (u100): Unauthorized access attempt
- \`ERR-INSUFFICIENT-COLLATERAL\` (u101): Collateral below required ratio
- \`ERR-BELOW-MINIMUM\` (u102): Amount below minimum threshold
- \`ERR-INVALID-AMOUNT\` (u103): Invalid transaction amount
- \`ERR-ALREADY-INITIALIZED\` (u104): Platform already initialized
- \`ERR-NOT-INITIALIZED\` (u105): Platform not initialized
- \`ERR-INVALID-LIQUIDATION\` (u106): Invalid liquidation attempt
- \`ERR-LOAN-NOT-FOUND\` (u107): Loan ID not found
- \`ERR-LOAN-NOT-ACTIVE\` (u108): Loan not in active status
- \`ERR-INVALID-LOAN-ID\` (u109): Invalid loan ID
- \`ERR-INVALID-PRICE\` (u110): Invalid price feed update
- \`ERR-INVALID-ASSET\` (u111): Invalid asset type

## Data Structures

### Loans Map

```clarity
{
    loan-id: uint,
    borrower: principal,
    collateral-amount: uint,
    loan-amount: uint,
    interest-rate: uint,
    start-height: uint,
    last-interest-calc: uint,
    status: (string-ascii 20)
}
```

### User Loans Map

```clarity
{
    user: principal,
    active-loans: (list 10 uint)
}
```

### Collateral Prices Map

```clarity
{
    asset: (string-ascii 3),
    price: uint
}
```

## Interest Calculation

Interest is calculated using the following formula:

```clarity
interest-per-block = (principal * rate) / (100 * 144)  ; Daily interest divided by blocks per day
total-interest = interest-per-block * blocks
```

## Liquidation Process

1. System continuously monitors collateral ratios
2. Liquidation triggered when ratio falls below 120%
3. Position marked as liquidated
4. Collateral released for liquidation
5. User loans mapping updated

## Security Features

1. **Access Control**

   - Contract owner authorization checks
   - Borrower-specific loan access
   - Protected governance functions

2. **Input Validation**

   - Asset type verification
   - Price feed validation
   - Loan ID validation
   - Amount and ratio checks

3. **State Protection**
   - Platform initialization checks
   - Active loan status verification
   - Collateral ratio maintenance

## Usage Examples

### Depositing Collateral

```clarity
(contract-call? .lending-protocol deposit-collateral u1000000)  ; Deposit 0.01 BTC
```

### Requesting a Loan

```clarity
(contract-call? .lending-protocol request-loan u1000000 u500000)  ; Request loan with collateral
```

### Repaying a Loan

```clarity
(contract-call? .lending-protocol repay-loan u1 u550000)  ; Repay loan #1 with interest
```

## Best Practices

1. **For Borrowers**

   - Maintain healthy collateral ratios above 150%
   - Monitor BTC price movements
   - Repay loans early to reduce interest
   - Keep track of loan IDs

2. **For Integrators**
   - Always check function return values
   - Handle all error codes appropriately
   - Monitor platform stats regularly
   - Implement proper price feed validation

## Contributing

This protocol is designed for production use in the Bitcoin DeFi ecosystem. Contributions should focus on:

- Security enhancements
- Gas optimization
- Risk management improvements
- Governance mechanisms
- Integration capabilities
