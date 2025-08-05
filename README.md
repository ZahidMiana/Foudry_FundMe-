# Foundry FundMe 💰

A decentralized crowdfunding smart contract built with Foundry, featuring real-time ETH/USD price feeds via Chainlink oracles.

![Solidity](https://img.shields.io/badge/Solidity-%23363636.svg?style=for-the-badge&logo=solidity&logoColor=white)
![Foundry](https://img.shields.io/badge/Foundry-black?style=for-the-badge&logo=ethereum&logoColor=white)
![Chainlink](https://img.shields.io/badge/Chainlink-375BD2?style=for-the-badge&logo=chainlink&logoColor=white)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Usage](#usage)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contract Architecture](#contract-architecture)
- [Scripts](#scripts)
- [Contributing](#contributing)
- [License](#license)

## 🔍 Overview

FundMe is a smart contract that allows users to fund the contract with ETH, with a minimum funding amount of $5 USD (calculated using Chainlink price feeds). Only the contract owner can withdraw the accumulated funds.

### Key Concepts Demonstrated:
- **Chainlink Price Feeds**: Real-time ETH/USD conversion
- **Custom Libraries**: Gas-efficient price conversion utilities
- **Access Control**: Owner-only withdrawal functionality
- **Foundry Testing**: Comprehensive unit and integration tests
- **Multi-network Deployment**: Support for local, testnet, and mainnet

## ✨ Features

- 💵 **Minimum USD Funding**: Enforces $5 minimum using live price feeds
- 🔒 **Owner Access Control**: Only owner can withdraw funds
- 📊 **Real-time Price Data**: Integrates Chainlink ETH/USD price feeds
- 🌐 **Multi-network Support**: Deploys on Anvil, Sepolia, and Mainnet
- ⚡ **Gas Optimized**: Efficient withdrawal patterns with `cheaperWithdraw()`
- 🧪 **Comprehensive Testing**: Unit and integration test suites
- 📜 **Interaction Scripts**: Automated fund and withdraw functionality

## 🚀 Getting Started

### Prerequisites

- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Foundry](https://getfoundry.sh/)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/ZahidMiana/Foundry_FundMe.git
cd Foundry_FundMe
```

2. **Install dependencies**
```bash
forge install
```

## 🎯 Usage

### Environment Setup

Create a `.env` file in the root directory:

```bash
SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOUR_API_KEY
MAINNET_RPC_URL=https://eth-mainnet.g.alchemy.com/v2/YOUR_API_KEY
PRIVATE_KEY=your_private_key_here
ETHERSCAN_API_KEY=your_etherscan_api_key
```

### Local Development

1. **Start Anvil (Local Blockchain)**
```bash
anvil
```

2. **Deploy to Local Network**
```bash
forge script script/DeployFundMe.s.sol --rpc-url http://localhost:8545 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --broadcast
```

3. **Fund the Contract**
```bash
forge script script/Interactions.s.sol:FundFundMe --rpc-url http://localhost:8545 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --broadcast
```

4. **Withdraw from Contract**
```bash
forge script script/Interactions.s.sol:WithdrawFundMe --rpc-url http://localhost:8545 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --broadcast
```

### Testnet Deployment

Deploy to Sepolia testnet:

```bash
forge script script/DeployFundMe.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify --etherscan-api-key $ETHERSCAN_API_KEY
```

## 🧪 Testing

### Run All Tests
```bash
forge test
```

### Run Tests with Verbosity
```bash
forge test -vv
```

### Run Specific Test
```bash
forge test --match-test testFundFailsWithoutEnoughETH -vvv
```

### Run Tests with Forked Network
```bash
forge test --fork-url $SEPOLIA_RPC_URL
```

### Generate Gas Report
```bash
forge test --gas-report
```

### Test Coverage
```bash
forge coverage
```

## 🏗️ Contract Architecture

### Core Contracts

#### `FundMe.sol`
Main contract that handles funding and withdrawals.

**Key Functions:**
- `fund()`: Accept ETH donations with minimum $5 USD requirement
- `withdraw()`: Owner-only function to withdraw all funds
- `cheaperWithdraw()`: Gas-optimized withdrawal function
- `getAddressToAmountFunded()`: Get amount funded by specific address

#### `PriceConverter.sol`
Library for ETH/USD price conversions using Chainlink price feeds.

**Key Functions:**
- `getPrice()`: Get current ETH price in USD
- `getConversionRate()`: Convert ETH amount to USD
- `getVersion()`: Get Chainlink aggregator version

### Deployment Architecture

#### `DeployFundMe.s.sol`
Main deployment script that uses HelperConfig for network-specific configurations.

#### `HelperConfig.s.sol`
Network configuration management:
- **Sepolia**: Uses live Chainlink price feed (`0x694AA1769357215DE4FAC081bf1f309aDC325306`)
- **Mainnet**: Uses live Chainlink price feed (`0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419`)
- **Anvil**: Deploys and uses MockV3Aggregator

#### `Interactions.s.sol`
Scripts for interacting with deployed contracts:
- `FundFundMe`: Fund the contract with ETH
- `WithdrawFundMe`: Withdraw funds (owner only)

## 📊 Testing Suite

### Unit Tests (`test/FundMeTest.t.sol`)
- ✅ Minimum USD validation
- ✅ Owner verification
- ✅ Price feed version check
- ✅ Funding functionality
- ✅ Withdrawal functionality
- ✅ Multiple funders scenario

### Integration Tests (`test/Integration/FundMeTestIntegration.t.sol`)
- ✅ End-to-end funding and withdrawal flow
- ✅ Script interaction testing

### Mock Contracts (`test/mocks/MockV3Aggregator.sol`)
- Price feed simulation for local testing
- Chainlink V3 interface implementation

## 📝 Scripts

| Script | Purpose | Usage |
|--------|---------|-------|
| `DeployFundMe.s.sol` | Deploy FundMe contract | `forge script script/DeployFundMe.s.sol --broadcast` |
| `Interactions.s.sol` | Fund/Withdraw operations | `forge script script/Interactions.s.sol:FundFundMe --broadcast` |
| `HelperConfig.s.sol` | Network configurations | Used by deployment script |

## 🔧 Configuration

### Network Configurations

| Network | Chain ID | Price Feed Address |
|---------|----------|-------------------|
| Ethereum Mainnet | 1 | `0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419` |
| Sepolia Testnet | 11155111 | `0x694AA1769357215DE4FAC081bf1f309aDC325306` |
| Anvil Local | 31337 | MockV3Aggregator (deployed) |

### Foundry Configuration (`foundry.toml`)

```toml
[profile.default]
src = "src"
out = "out"
libs = ["lib"]
remappings = [
    "@chainlink/contracts/=lib/chainlink-brownie-contracts/contracts/",
    "foundry-devops/=lib/foundry-devops/",
]
ffi = true
```

## 🏆 Key Learnings

This project demonstrates:

1. **Chainlink Integration**: Real-world price feed usage
2. **Library Development**: Custom `PriceConverter` library
3. **Testing Strategies**: Unit vs Integration testing
4. **Gas Optimization**: Efficient storage and memory usage
5. **Multi-network Deployment**: Local, testnet, mainnet support
6. **Script Automation**: Deployment and interaction automation
7. **Mock Development**: Testing with simulated data

## 🔍 Security Considerations

- **Access Control**: Only owner can withdraw funds
- **Minimum Funding**: Prevents spam with $5 minimum
- **Price Feed Reliability**: Uses Chainlink decentralized oracles
- **Reentrancy Protection**: Uses checks-effects-interactions pattern

## 📈 Gas Optimization

The contract includes two withdrawal patterns:
- `withdraw()`: Standard approach
- `cheaperWithdraw()`: Memory-optimized for lower gas costs

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Patrick Collins](https://github.com/PatrickAlphaC) for the foundational course material
- [Chainlink](https://chain.link/) for providing decentralized price feeds
- [Foundry](https://getfoundry.sh/) for the excellent development framework

## 📞 Contact

**Zahid Miana** - [@ZahidMiana](https://github.com/ZahidMiana)

Project Link: [https://github.com/ZahidMiana/Foundry_FundMe](https://github.com/ZahidMiana/Foundry_FundMe)

---

⭐ **Star this repository if it helped you learn Foundry and smart contract development!**

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
