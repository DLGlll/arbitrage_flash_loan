# Installation Guide

This guide provides detailed instructions for installing and setting up the Arbitrage Flash Loan Trading Bot.

## System Requirements

- **Operating System**: Linux, macOS, or Windows
- **Node.js**: v14.0.0 or higher
- **npm**: v6.0.0 or higher
- **Memory**: At least 4GB RAM
- **Storage**: At least 1GB free disk space
- **Internet Connection**: Stable broadband connection

## Dependencies Installation

1. **Install Node.js and npm**

   If you don't have Node.js installed, download and install it from [nodejs.org](https://nodejs.org/).

   Verify the installation:
   ```bash
   node --version
   npm --version
   ```

2. **Install Git**

   If you don't have Git installed, download and install it from [git-scm.com](https://git-scm.com/).

   Verify the installation:
   ```bash
   git --version
   ```

## Bot Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/arbitrage-flash-loan-bot.git
   cd arbitrage-flash-loan-bot
   ```

2. **Install Dependencies**

   ```bash
   npm install
   ```

   This will install all required Node.js packages defined in the `package.json` file.

3. **Environment Setup**

   Create a `.env` file in the root directory with the following variables:

   ```
   # Network RPC URLs
   ETHEREUM_RPC_URL=https://mainnet.infura.io/v3/your-infura-key
   POLYGON_RPC_URL=https://polygon-rpc.com
   BSC_RPC_URL=https://bsc-dataseed.binance.org
   ARBITRUM_RPC_URL=https://arb1.arbitrum.io/rpc
   OPTIMISM_RPC_URL=https://mainnet.optimism.io
   
   # Testnet RPC URLs
   GOERLI_RPC_URL=https://goerli.infura.io/v3/your-infura-key
   MUMBAI_RPC_URL=https://rpc-mumbai.maticvigil.com
   BSC_TESTNET_RPC_URL=https://data-seed-prebsc-1-s1.binance.org:8545
   
   # Wallet Configuration
   PRIVATE_KEY=your-private-key
   
   # API Keys (optional)
   ETHERSCAN_API_KEY=your-etherscan-api-key
   POLYGONSCAN_API_KEY=your-polygonscan-api-key
   BSCSCAN_API_KEY=your-bscscan-api-key
   
   # Bot Configuration
   MIN_PROFIT_USD=10
   MAX_GAS_PRICE_GWEI_ETHEREUM=100
   MAX_GAS_PRICE_GWEI_POLYGON=50
   SLIPPAGE_TOLERANCE=0.5
   ```

   Replace the placeholder values with your actual API keys and configuration.

4. **Configure the Bot**

   Review and modify the configuration in `src/config.js` according to your needs:
   
   - Add or remove networks
   - Configure DEXs for arbitrage
   - Set up token pairs
   - Adjust arbitrage parameters

## Smart Contract Deployment

1. **Compile the Contracts**

   ```bash
   npx hardhat compile
   ```

2. **Deploy to Testnet**

   Before deploying to mainnet, test on a testnet:

   ```bash
   npx hardhat run scripts/deploy.js --network goerli
   ```

3. **Deploy to Mainnet**

   Once tested, deploy to mainnet:

   ```bash
   npx hardhat run scripts/deploy.js --network ethereum
   ```

4. **Update Contract Addresses**

   After deployment, update the contract addresses in your configuration file.

## Verification

1. **Verify Installation**

   Run the verification script to ensure everything is set up correctly:

   ```bash
   node scripts/verify-installation.js
   ```

2. **Run Tests**

   Execute the test suite to verify functionality:

   ```bash
   npm test
   ```

3. **Run Integration Tests**

   ```bash
   node src/tests/integration-test.js
   ```

## Troubleshooting Installation Issues

### Common Issues and Solutions

1. **Node.js Version Compatibility**

   If you encounter errors related to Node.js version:
   ```bash
   nvm install 14
   nvm use 14
   ```

2. **Package Installation Failures**

   If npm install fails:
   ```bash
   npm cache clean --force
   rm -rf node_modules
   npm install
   ```

3. **Smart Contract Compilation Errors**

   If contract compilation fails:
   ```bash
   rm -rf artifacts cache
   npx hardhat clean
   npx hardhat compile
   ```

4. **RPC Connection Issues**

   If you can't connect to RPC endpoints:
   - Check your internet connection
   - Verify API keys are correct
   - Try alternative RPC providers

5. **Insufficient Memory**

   If you encounter out-of-memory errors:
   ```bash
   export NODE_OPTIONS=--max_old_space_size=4096
   ```

## Next Steps

After successful installation:

1. Review the [Configuration Guide](./docs/configuration.md) for detailed configuration options
2. Check the [Usage Guide](./docs/usage.md) to learn how to operate the bot
3. Understand the [Security Considerations](./docs/security.md) before running with real funds

For additional help, refer to the [Troubleshooting Guide](./docs/troubleshooting.md) or open an issue on GitHub.
