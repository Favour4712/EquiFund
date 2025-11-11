# EquiFund 🌟

> **Democratizing Public Goods Funding Through Quadratic Matching**

EquiFund is a decentralized quadratic funding platform built on Base Sepolia that amplifies the impact of small donations through mathematical fairness. Where your voice matters more than your wallet size.

[![Built on Base](https://img.shields.io/badge/Built%20on-Base%20Sepolia-blue)](https://base.org)
[![Foundry](https://img.shields.io/badge/Built%20with-Foundry-orange)](https://getfoundry.sh)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📖 What is Quadratic Funding?

Quadratic Funding (QF) is a mathematically optimal way to fund public goods in democratic communities. It works by:

- **Amplifying Small Donations**: A project with 100 people donating $1 receives more matching funds than one person donating $100
- **Preventing Whale Dominance**: Large donors can't monopolize the matching pool
- **Community Signal**: Broad support matters more than total capital
- **Math-Enforced Fairness**: The quadratic formula ensures democratic distribution

### The Formula

```
Project Match = (Σ√contribution_i)² 
```

Each project's share of the matching pool is proportional to the square of the sum of square roots of contributions.

---

## 🎯 Features

### MVP (Current)
- ✅ **Funding Rounds**: Time-bound contribution periods with matching pools
- ✅ **Project Registry**: Verified projects eligible for funding
- ✅ **Quadratic Matching**: Automated calculation and distribution
- ✅ **Sybil Resistance**: Basic cooldown mechanism to prevent gaming
- ✅ **Transparent**: All contributions and matches on-chain
- ✅ **Base Sepolia**: Deployed on L2 for low gas fees

### Roadmap
- 🔜 Multi-token support (ERC20)
- 🔜 Advanced Sybil resistance (BrightID/Gitcoin Passport integration)
- 🔜 Governance voting on project eligibility
- 🔜 Automated round scheduling
- 🔜 Grant streaming/vesting
- 🔜 Multi-chain deployment

---

## 🏗️ Architecture

### Smart Contracts

```
src/
├── EquiFundPool.sol      # Main funding pool & round management
├── ProjectRegistry.sol    # Whitelisted projects registry
├── QuadraticMath.sol      # QF formula implementation
└── SybilGuard.sol         # Anti-gaming mechanisms
```

#### Contract Overview

| Contract | Purpose | Key Functions |
|----------|---------|---------------|
| **EquiFundPool** | Manages funding rounds, contributions, and matching distribution | `contribute()`, `finalizeRound()`, `withdrawMatching()` |
| **ProjectRegistry** | Maintains verified projects list | `registerProject()`, `isProjectActive()` |
| **QuadraticMath** | Pure math library for QF calculations | `calculateMatch()`, `sqrt()` |
| **SybilGuard** | Prevents multiple contributions from same entity | `checkEligibility()`, `recordContribution()` |

---

## 🚀 Getting Started

### Prerequisites

- [Foundry](https://getfoundry.sh/) installed
- Node.js v18+ (for frontend)
- Base Sepolia ETH ([faucet](https://www.coinbase.com/faucets/base-ethereum-sepolia-faucet))
- MetaMask or compatible wallet

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/equifund.git
cd equifund

# Install Foundry dependencies
forge install

# Install OpenZeppelin contracts
forge install OpenZeppelin/openzeppelin-contracts

# Install frontend dependencies (if applicable)
cd frontend
npm install
```

### Environment Setup

Create a `.env` file in the project root:

```env
# Network
BASE_SEPOLIA_RPC_URL=https://sepolia.base.org
CHAIN_ID=84532

# Deployment
PRIVATE_KEY=your_private_key_here
ETHERSCAN_API_KEY=your_basescan_api_key

# Contract Addresses (after deployment)
EQUIFUND_POOL=0x...
PROJECT_REGISTRY=0x...
SYBIL_GUARD=0x...
```

---

## 🔨 Development

### Compile Contracts

```bash
forge build
```

### Run Tests

```bash
# Run all tests
forge test

# Run with verbosity
forge test -vvv

# Run specific test
forge test --match-test testContribution

# Generate gas report
forge test --gas-report
```

### Test Coverage

```bash
forge coverage
```

### Deploy to Base Sepolia

```bash
# Deploy all contracts
forge script script/Deploy.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --broadcast --verify

# Deploy with mock data for testing
forge script script/MockSetup.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --broadcast
```

### Local Development

```bash
# Start local Anvil node
anvil

# Deploy to local node (new terminal)
forge script script/Deploy.s.sol --rpc-url http://localhost:8545 --broadcast
```

---

## 📝 Usage

### For Contributors

1. **Connect Wallet**: Connect to Base Sepolia network
2. **Browse Projects**: View active projects in current round
3. **Contribute**: Donate ETH to projects you support
4. **Track Impact**: See your contribution + projected match amount

### For Project Owners

1. **Register**: Submit project for admin approval
2. **Get Verified**: Admin verifies project legitimacy
3. **Receive Funds**: Collect contributions during round
4. **Claim Match**: Withdraw matched funds after round finalization

### For Administrators

1. **Create Round**: Set duration and add matching pool
2. **Register Projects**: Approve eligible projects
3. **Finalize Round**: Calculate and distribute matches
4. **Monitor**: Track participation and prevent abuse

---

## 🧪 Testing Scenarios

### Test Cases Covered

- ✅ Single vs multiple contributors
- ✅ Whale dominance prevention (1 donor $100 vs 100 donors $1)
- ✅ Quadratic formula accuracy
- ✅ Sybil guard cooldown enforcement
- ✅ Round finalization idempotency
- ✅ Access control (admin-only functions)
- ✅ Edge cases (zero contributions, single project)
- ✅ Reentrancy protection
- ✅ Emergency withdrawal

### Example Test

```solidity
function testQuadraticAmplification() public {
    // 1 whale donates 100 ETH
    vm.prank(whale);
    pool.contribute{value: 100 ether}(projectA);
    
    // 100 smols donate 1 ETH each
    for(uint i = 0; i < 100; i++) {
        vm.prank(smols[i]);
        pool.contribute{value: 1 ether}(projectB);
    }
    
    pool.finalizeRound();
    
    // ProjectB should receive more matching funds
    assert(pool.getProjectMatchAmount(1, projectB) > 
           pool.getProjectMatchAmount(1, projectA));
}
```

---

## 🎨 Frontend

The frontend is built with modern web3 tooling:

- **Framework**: React + Vite / Next.js
- **Styling**: Tailwind CSS + Framer Motion
- **Web3**: wagmi + viem
- **UI**: Glass-morphism design with animated elements
- **Charts**: Recharts for data visualization

### Run Frontend

```bash
cd frontend
npm run dev
```

Visit `http://localhost:5173`

---

## 🔒 Security

### Audits
- ⚠️ **Not audited yet** - MVP for testnet only
- DO NOT use on mainnet without professional audit

### Security Features
- OpenZeppelin battle-tested contracts
- ReentrancyGuard on all payable functions
- Access control (Ownable pattern)
- Pausable emergency stops
- Checks-effects-interactions pattern
- SafeMath for overflow protection

### Known Limitations (MVP)
- Basic Sybil resistance (time-based cooldown only)
- Centralized admin control
- No dispute resolution mechanism
- Single token support (ETH only)

---

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Write tests for new features
- Follow Solidity style guide
- Comment complex logic
- Update documentation
- Run `forge fmt` before committing

---

## 📚 Resources

### Learn About Quadratic Funding
- [Gitcoin's QF Explanation](https://wtfisqf.com)
- [Vitalik's Original Post](https://vitalik.ca/general/2019/12/07/quadratic.html)
- [Quadratic Funding Paper](https://arxiv.org/abs/1809.06421)

### Technical Documentation
- [Foundry Book](https://book.getfoundry.sh/)
- [Base Developer Docs](https://docs.base.org)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)
- [Solidity Documentation](https://docs.soliditylang.org/)

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Gitcoin**: For pioneering quadratic funding in crypto
- **Vitalik Buterin**: For the quadratic funding mechanism
- **Base**: For the scalable L2 infrastructure
- **OpenZeppelin**: For secure smart contract libraries
- **Foundry**: For the blazing fast development toolkit

---

## 📞 Contact & Community

- **Twitter**: [@EquiFund](https://twitter.com/equifund)
- **Discord**: [Join our community](https://discord.gg/equifund)
- **GitHub**: [github.com/yourusername/equifund](https://github.com/yourusername/equifund)
- **Email**: hello@equifund.xyz

---

## 💡 Why EquiFund?

> "In traditional funding, $1 from a billionaire equals $1 from a student. In EquiFund, community consensus matters more than capital size. We're building a funding mechanism where **democracy meets math**."

**Built with ❤️ for public goods**

---

## 🎯 Quick Links

- [Live Demo](https://equifund.xyz) *(coming soon)*
- [Documentation](https://docs.equifund.xyz) *(coming soon)*
- [Contract Addresses](https://github.com/yourusername/equifund/wiki/Deployments)
- [Roadmap](https://github.com/yourusername/equifund/projects)
- [Bug Bounty](https://github.com/yourusername/equifund/security) *(post-audit)*

---

**Star ⭐ this repo if you believe in democratizing public goods funding!**
