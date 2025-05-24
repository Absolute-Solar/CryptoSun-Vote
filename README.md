# CryptoSun Vote

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Latest Release](https://img.shields.io/github/v/release/Absolute-Solar/cryptosun-vote)](https://github.com/Absolute-Solar/CryptoSun-Vote/releases/tag/Vote-V1.0.0)

## CryptoSun Vote – Decentralized Governance System

As a core part of our long-term vision, CryptoSun Vote enables CSN holders to propose, discuss, and vote on protocol upgrades, treasury allocations, energy trading rules, and maintenance automation. We provide simple blinks as an extension of our website to take polls.

### Key Governance Features

### 1.) Proposal Creation <br>
Any user with a minimum staked balance of CSN tokens can submit proposals.

### 2.) Voting Power (1 CSN = 1 Vote) <br>
Governance weight is directly proportional to the amount of CSN staked in the governance vault. Snapshot-based voting ensures fairness and prevents manipulation. Ratio of 1:1 so top stakers get top priority.

### 3.) On-Chain Execution <br>
Proposals that meet the quorum and pass voting thresholds are automatically executed via cross-program invocations, ensuring trustless and timely implementation.

### 4.) Transparent Governance Ledger <br>
Every proposal, vote, and result is permanently recorded on-chain ensuring full auditability and accountability.

### 5.) Community Integration <br>
Each proposal includes a discussion link (e.g., Discord, X, or GitHub Discussions) where the community can coordinate and debate ideas off-chain before voting on-chain.

### 6.) Upgradeable & Extensible <br>
Built using Solana’s SPL Governance Program, the system is modular and supports future enhancements.

### Participate Now
1.) Governance Dashboard: <a>https://vote.solarcrypto.ca</a> <br>
2.) Docs & How-To: <a>https://github.com/Absolute-Solar/CryptoSun-Vote/tree/main/docs</a>

## Voting Architecture

### Components

```
            ┌───────────────────┐      ┌───────────────────┐      ┌───────────────────┐
            │                   │      │                   │      │                   │
            │ Proposal Program  │◄────►│   Vote Program    │◄────►│  Treasury Program │
            │                   │      │                   │      │                   │
            └───────────┬───────┘      └────────┬──────────┘      └───────────────────┘
                        │                       │                           ▲
                        │                       │                           │
                        │                       │                           │
                        │                       │                           │
            ┌───────────▼───────────────────────▼───────────┐      ┌────────▼──────────┐
            │                                               │      │                   │
            │               Governance State                │◄────►│ Parameter Control │
            │                                               │      │                   │
            └───────────────────────┬───────────────────────┘      └───────────────────┘
                                    │
                                    │
                        ┌───────────▼───────────┐
                        │                       │
                        │  Timelock Executor    │
                        │                       │
                        └───────────────────────┘
```
<br>

<img src="https://yellow-negative-parrotfish-381.mypinata.cloud/ipfs/bafkreievvw536ke7ibxgf6xumewnfygm3cr5elbw2iy2mmfp7zgakiyppe" alt="flow2">

### Governance Process Flow

1. **Proposal Creation**: CSN holders create proposals with specific actions
2. **Discussion Period**: Community discusses proposal in designated forums
3. **Voting Period**: Token holders cast votes proportional to their stake
4. **Finalization**: Votes are tallied and outcome is recorded on-chain
5. **Timelock**: Approved proposals enter a security waiting period
6. **Execution**: Successful proposals are automatically implemented

## Programs

The CryptoSun Vote repository contains the following Solana programs:

| Program | Description | Program ID |
|---------|-------------|------------|
| Proposal | Handles creation and management of governance proposals | `csv1...TBA` |
| Vote | Manages the voting process and vote counting | `vote...TBA` |
| Treasury | Controls access to community-owned funds | `tres...TBA` |
| Timelock | Implements security delay for approved changes | `time...TBA` |
| Parameter Control | Updates system parameters based on vote results | `para...TBA` |

## Getting Started

### Prerequisites

- [Solana Tool Suite](https://docs.solana.com/cli/install-solana-cli-tools) v1.14.0 or later
- [Anchor](https://project-serum.github.io/anchor/getting-started/installation.html) v0.28.0 or later
- [Node.js](https://nodejs.org/) v16 or later
- [Yarn](https://yarnpkg.com/) v3 or later

### Installation

```bash
# Clone the repository
git clone https://github.com/Absolute-Solar/CryptoSun-Vote.git
cd cryptosun-vote

# Install dependencies
yarn install

# Build all programs
yarn build
```

### Local Testing

```bash
# Start a local validator
solana-test-validator

# Deploy governance programs
anchor deploy

# Initialize the governance system
yarn run init-governance
```

## Voting Process

### Creating a Proposal

```typescript
import { Connection, PublicKey } from '@solana/web3.js';
import { CryptoSunVote } from '@cryptosun/vote';

// Initialize the client
const connection = new Connection('https://api.devnet.solana.com');
const vote = new CryptoSunVote(connection);

// Create a proposal
const proposalId = await vote.createProposal({
  title: "Increase Energy Oracle Update Frequency",
  description: "Change energy data update interval from 1 hour to 15 minutes",
  actions: [
    {
      program: new PublicKey("eorg..."),
      function: "updateInterval",
      parameters: [15 * 60] // 15 minutes in seconds
    }
  ],
  votingPeriod: 7 * 24 * 60 * 60 // 7 days in seconds
});

console.log(`Created proposal ${proposalId}`);
```

### Casting a Vote

```typescript
// Vote on a proposal (1 = yes, 0 = no)
await vote.castVote(proposalId, 1, "Support increased oracle frequency for better energy tracking");
```

### Executing an Approved Proposal

```typescript
// Once voting period is complete and proposal is approved
await vote.executeProposal(proposalId);
```

## Proposal Types

CryptoSun governance handles several types of proposals:

| Type | Description | Approval Threshold | Timelock |
|------|-------------|-------------------|----------|
| Parameter Update | Change system parameters | 51% | 24 hours |
| Treasury Action | Move funds from treasury | 67% | 48 hours |
| Program Upgrade | Update smart contracts | 75% | 72 hours |
| Emergency Action | Critical security fixes | 80% | 6 hours |
| Renewable Integration | Add new energy sources | 51% | 48 hours |


## Governance Parameters

Key parameters that control the voting system:

| Parameter | Value | Description |
|-----------|-------|-------------|
| Proposal Threshold | 100,000 CSN | Minimum stake required to create a proposal |
| Voting Period | 7 days | Duration proposals remain open for voting |
| Quorum | 10% | Minimum participation required for valid vote |
| Execution Delay | 24-72 hours | Timelock before execution (depends on proposal type) |
| Vote Delegation Limit | 5 hops | Maximum depth of delegation chains |

## Energy-Specific Governance

Special governance features for the renewable energy network:

- **Energy Provider Proposals**: Special category for energy integration
- **Regional Parameters**: Governance of region-specific energy parameters
- **Hardware Certification**: Community voting on approved solar hardware


## Security Considerations

- **Vote Sybil Resistance**: Votes weighted by staked tokens prevent identity-based attacks
- **Timelock Protection**: Delay between approval and execution prevents flash governance attacks
- **Critical Parameter Protection**: Higher thresholds for critical system changes
- **Vote Privacy**: Optional private voting using ZK techniques
- **Emergency Council**: Multi-sig emergency committee for critical security vulnerabilities


## Contributing

We welcome contributions to improve the CryptoSun governance system! Please check our [Contributing Guidelines](CONTRIBUTING.md) before submitting pull requests.


## Community

- [Governance Forum](https://forum.cryptosun.io)
- [Discord Governance Channel](https://discord.gg/cryptosun-governance)
- [Snapshot Page](https://snapshot.org/#/cryptosun.eth)

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
