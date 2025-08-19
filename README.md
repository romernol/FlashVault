# FlashVault

FlashVault is a DeFi lending smart contract optimized for flash loan operations with advanced arbitrage protection, built on the Stacks blockchain using Clarity smart contract language.

## 🚀 Features

- **Flash Loans**: Execute uncollateralized loans that must be repaid within the same block
- **Arbitrage Protection**: Built-in safeguards against price manipulation with 5% maximum price deviation
- **Liquidity Pool Management**: Dynamic liquidity provision with proportional vault token rewards
- **Low Fees**: Competitive 0.1% (10 basis points) flash loan fee structure
- **Vault Tokens**: ERC20-like fungible tokens representing liquidity provider shares
- **Price Oracle Integration**: Real-time price tracking for arbitrage detection
- **Emergency Controls**: Pause functionality for security incidents
- **Gas Optimized**: Efficient Clarity code designed for cost-effective operations

## 📋 Technical Specifications

- **Blockchain**: Stacks
- **Language**: Clarity v2.5
- **Epoch**: 2.5
- **Token Standard**: Fungible Token (SIP-010 compatible)
- **Flash Loan Fee**: 0.1% (10 basis points)
- **Max Price Deviation**: 5% (500 basis points)
- **Architecture**: Single-contract design with integrated oracle

## 🛠 Installation

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) v1.5.0 or higher
- [Node.js](https://nodejs.org/) v16.0.0 or higher
- [Stacks Wallet](https://wallet.hiro.so/) for testnet/mainnet deployment

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/FlashVault.git
   cd FlashVault
   ```

2. **Install dependencies**
   ```bash
   cd FlashVault_contract
   npm install
   ```

3. **Run tests**
   ```bash
   npm test
   ```

4. **Start local development environment**
   ```bash
   clarinet console
   ```

## 💼 Usage Examples

### Deploy Contract

```bash
clarinet deploy --testnet
```

### Initialize Contract (Owner Only)

```clarity
;; Initialize with 1000 STX initial liquidity
(contract-call? .FlashVault initialize u1000000000)
```

### Add Liquidity

```clarity
;; Add 500 STX to the liquidity pool
(contract-call? .FlashVault add-liquidity u500000000)
```

### Execute Flash Loan

```clarity
;; Request flash loan of 100 STX
(contract-call? .FlashVault flash-loan u100000000 'SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7)

;; Must repay in the same transaction/block
(contract-call? .FlashVault repay-flash-loan)
```

### Remove Liquidity

```clarity
;; Remove liquidity by burning 250 vault tokens
(contract-call? .FlashVault remove-liquidity u250000000)
```

### Update Price Oracle (Owner Only)

```clarity
;; Update current price for arbitrage protection
(contract-call? .FlashVault update-price-oracle u2000000)
```

## 📚 Contract Functions Documentation

### Read-Only Functions

| Function | Description | Parameters | Returns |
|----------|-------------|------------|---------|
| `get-total-liquidity` | Get total STX in the vault | None | `uint` |
| `get-total-borrowed` | Get total STX currently borrowed | None | `uint` |
| `get-fees-collected` | Get total fees collected | None | `uint` |
| `get-user-liquidity` | Get user's liquidity contribution | `user: principal` | `uint` |
| `get-user-shares` | Get user's vault token balance | `user: principal` | `uint` |
| `get-flash-loan-session` | Get active flash loan session | `user: principal` | `optional` |
| `calculate-flash-loan-fee` | Calculate fee for loan amount | `amount: uint` | `uint` |
| `get-available-liquidity` | Get available liquidity for loans | None | `uint` |
| `get-current-price` | Get current oracle price | None | `optional uint` |

### Public Functions

| Function | Description | Access | Parameters |
|----------|-------------|---------|------------|
| `initialize` | Initialize contract with liquidity | Owner Only | `initial-amount: uint` |
| `update-price-oracle` | Update price oracle | Owner Only | `price: uint` |
| `add-liquidity` | Add STX to liquidity pool | Public | `amount: uint` |
| `remove-liquidity` | Remove liquidity via vault tokens | Public | `shares: uint` |
| `flash-loan` | Execute flash loan | Public | `amount: uint, recipient: principal` |
| `repay-flash-loan` | Repay active flash loan | Public | None |

### Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| u100 | `ERR_UNAUTHORIZED` | Caller not authorized |
| u101 | `ERR_INSUFFICIENT_BALANCE` | Insufficient balance |
| u102 | `ERR_LOAN_NOT_REPAID` | Flash loan not repaid properly |
| u103 | `ERR_INVALID_AMOUNT` | Invalid amount specified |
| u104 | `ERR_POOL_EMPTY` | Insufficient pool liquidity |
| u105 | `ERR_FLASH_LOAN_ACTIVE` | Flash loan already active |
| u106 | `ERR_ARBITRAGE_DETECTED` | Price manipulation detected |
| u107 | `ERR_SLIPPAGE_EXCEEDED` | Slippage tolerance exceeded |

## 🚀 Deployment Guide

### Testnet Deployment

1. **Configure testnet settings**
   ```bash
   clarinet deploy --testnet --config-file settings/Testnet.toml
   ```

2. **Verify deployment**
   ```bash
   clarinet console --testnet
   ```

### Mainnet Deployment

1. **Configure mainnet settings**
   ```toml
   # settings/Mainnet.toml
   [network]
   name = "mainnet"
   node_rpc_address = "https://stacks-node-api.mainnet.stacks.co"
   ```

2. **Deploy to mainnet**
   ```bash
   clarinet deploy --mainnet --config-file settings/Mainnet.toml
   ```

3. **Initialize contract**
   ```bash
   # Call initialize function with initial liquidity
   stx call_contract_func --testnet FlashVault initialize 1000000000
   ```

## 🔒 Security Notes

### Flash Loan Protection
- **Same-block repayment**: All flash loans must be repaid within the same block
- **Arbitrage detection**: Automatic price deviation monitoring (5% max)
- **Reentrancy protection**: State updates prevent malicious re-entries

### Access Controls
- **Owner privileges**: Only contract owner can initialize and update oracle
- **Emergency pause**: Owner can pause operations during security incidents
- **Fee collection**: Transparent fee structure with automatic collection

### Best Practices
1. **Always test on devnet/testnet** before mainnet deployment
2. **Monitor price oracle** regularly for accurate arbitrage protection
3. **Keep emergency pause** access secure and ready for incidents
4. **Regular security audits** recommended for production use
5. **Liquidity monitoring** to ensure adequate pool depth

### Known Limitations
- **Oracle dependency**: Relies on manual price updates (consider automated oracles)
- **Single asset**: Currently supports STX only
- **Block-level timing**: Flash loans limited to single-block execution

## 🧪 Testing

Run the comprehensive test suite:

```bash
# Run all tests
npm test

# Run tests with coverage
npm run test:report

# Watch mode for development
npm run test:watch
```

## 📄 License

This project is licensed under the ISC License - see the LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📞 Support

For support and questions:
- Create an issue in the GitHub repository
- Join our community Discord
- Read the [Stacks documentation](https://docs.stacks.co/)

## ⚠️ Disclaimer

FlashVault is experimental DeFi software. Use at your own risk. Always conduct thorough testing and security audits before deploying to mainnet. The developers are not responsible for any loss of funds.