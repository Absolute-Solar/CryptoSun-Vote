# CryptoSun Vote

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/github/workflow/status/cryptosun/cryptosun-vote/CI)]()
[![Latest Release](https://img.shields.io/github/v/release/cryptosun/cryptosun-vote)]()

CryptoSun Vote is the on-chain governance system that enables CSN token holders to propose, vote on, and implement changes to the CryptoSun ecosystem. This decentralized governance framework ensures that the renewable energy network evolves according to community consensus.
The CryptoSun governance system empowers token holders to participate in network decision-making proportional to their stake. The voting mechanism leverages Solana's high-throughput capabilities to enable efficient, low-cost voting while maintaining security and transparency.

### Key Governance Features

- **Proposal Creation**: Any holder with sufficient CSN stake can submit proposals
- **Voting Power**: Voting weight based on staked CSN tokens
- **Execution**: Automatic implementation of approved proposals
- **Delegation**: Ability to delegate voting power to trusted representatives
- **Timelock**: Security delay between approval and execution
- **Transparent History**: Complete on-chain record of all governance actions

## System Architecture

### Components

```
┌───────────────────┐      ┌───────────────────┐      ┌───────────────────┐
│                   │      │                   │      │                   │
│  Proposal Program │◄────►│  Vote Program     │◄────►│  Treasury Program │
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
| Proposal | Handles creation and management of governance proposals | `csv1...` |
| Vote | Manages the voting process and vote counting | `vote...` |
| Treasury | Controls access to community-owned funds | `tres...` |
| Timelock | Implements security delay for approved changes | `time...` |
| Parameter Control | Updates system parameters based on vote results | `para...` |

## Getting Started

### Prerequisites

- [Solana Tool Suite](https://docs.solana.com/cli/install-solana-cli-tools) v1.14.0 or later
- [Anchor](https://project-serum.github.io/anchor/getting-started/installation.html) v0.28.0 or later
- [Node.js](https://nodejs.org/) v16 or later
- [Yarn](https://yarnpkg.com/) v3 or later

### Installation

```bash
# Clone the repository
git clone https://github.com/cryptosun/cryptosun-vote.git
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

## Delegation

Token holders can delegate their voting power to trusted representatives:

```typescript
// Delegate voting power to another address
await vote.delegateVotes(delegateAddress);

// Return voting power to self
await vote.undelegateVotes();
```

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

## Frontend Integration

Example of integrating the voting UI into frontends:

```typescript
import { VoteSDK } from '@cryptosun/vote-sdk';

// Initialize the SDK
const voteSDK = new VoteSDK({
  rpcUrl: 'https://api.mainnet-beta.solana.com',
  programId: 'vote...'
});

// Get active proposals
const proposals = await voteSDK.getActiveProposals();

// Display proposals in UI
proposals.forEach(proposal => {
  console.log(`${proposal.id}: ${proposal.title} - ${proposal.voteCount} votes`);
});
```

## Security Considerations

- **Vote Sybil Resistance**: Votes weighted by staked tokens prevent identity-based attacks
- **Timelock Protection**: Delay between approval and execution prevents flash governance attacks
- **Critical Parameter Protection**: Higher thresholds for critical system changes
- **Vote Privacy**: Optional private voting using ZK techniques
- **Emergency Council**: Multi-sig emergency committee for critical security vulnerabilities

## Roadmap

- [x] Basic proposal and voting functionality
- [x] Treasury management
- [x] Parameter governance
- [ ] Enhanced delegation with liquid democracy
- [ ] Quadratic voting experiments
- [ ] Cross-chain governance bridge
- [ ] AI-assisted proposal analysis
- [ ] Energy-specific governance templates

## Contributing

We welcome contributions to improve the CryptoSun governance system! Please check our [Contributing Guidelines](CONTRIBUTING.md) before submitting pull requests.

### Development Workflow

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/voting-enhancement`)
3. Commit your changes (`git commit -m 'Add weighted voting feature'`)
4. Push to the branch (`git push origin feature/voting-enhancement`)
5. Open a Pull Request

## Community

- [Governance Forum](https://forum.cryptosun.io)
- [Discord Governance Channel](https://discord.gg/cryptosun-governance)
- [Snapshot Page](https://snapshot.org/#/cryptosun.eth)

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
