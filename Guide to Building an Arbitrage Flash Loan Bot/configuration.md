# Configuration Guide

This guide provides detailed information on configuring the Arbitrage Flash Loan Trading Bot for optimal performance.

## Configuration File Structure

The main configuration file is located at `src/config.js`. This file contains all the settings needed to customize the bot's behavior.

## Network Configuration

Configure blockchain networks the bot will operate on:

```javascript
NETWORKS: {
  ethereum: {
    name: 'Ethereum Mainnet',
    chainId: 1,
    rpcUrl: process.env.ETHEREUM_RPC_URL,
    isTestnet: false,
    blockTime: 12, // Average block time in seconds
    gasStationUrl: 'https://ethgasstation.info/api/ethgasAPI.json',
    explorer: 'https://etherscan.io'
  },
  polygon: {
    name: 'Polygon Mainnet',
    chainId: 137,
    rpcUrl: process.env.POLYGON_RPC_URL,
    isTestnet: false,
    blockTime: 2,
    gasStationUrl: 'https://gasstation-mainnet.matic.network/v2',
    explorer: 'https://polygonscan.com'
  },
  arbitrum: {
    name: 'Arbitrum One',
    chainId: 42161,
    rpcUrl: process.env.ARBITRUM_RPC_URL,
    isTestnet: false,
    blockTime: 0.25,
    explorer: 'https://arbiscan.io'
  },
  // Testnets
  goerli: {
    name: 'Goerli Testnet',
    chainId: 5,
    rpcUrl: process.env.GOERLI_RPC_URL,
    isTestnet: true,
    blockTime: 15,
    explorer: 'https://goerli.etherscan.io'
  }
  // Add more networks as needed
}
```

### Network Parameters

- `name`: Human-readable network name
- `chainId`: Network chain ID
- `rpcUrl`: RPC endpoint URL (from .env file)
- `isTestnet`: Whether this is a testnet
- `blockTime`: Average block time in seconds
- `gasStationUrl`: URL for gas price API (optional)
- `explorer`: Block explorer URL

## DEX Configuration

Configure decentralized exchanges for arbitrage:

```javascript
DEXES: {
  uniswap: {
    name: 'Uniswap V2',
    router: '0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D',
    factory: '0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f',
    fee: 0.003, // 0.3%
    networks: ['ethereum', 'polygon'],
    initCodeHash: '0x96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f',
    priority: 1 // Priority for arbitrage (lower is higher priority)
  },
  sushiswap: {
    name: 'SushiSwap',
    router: '0x1b02dA8Cb0d097eB8D57A175b88c7D8b47997506',
    factory: '0xC0AEe478e3658e2610c5F7A4A2E1777cE9e4f2Ac',
    fee: 0.003, // 0.3%
    networks: ['ethereum', 'polygon', 'arbitrum'],
    initCodeHash: '0xe18a34eb0e04b04f7a0ac29a6e80748dca96319b42c54d679cb821dca90c6303',
    priority: 2
  },
  quickswap: {
    name: 'QuickSwap',
    router: '0xa5E0829CaCEd8fFDD4De3c43696c57F7D7A678ff',
    factory: '0x5757371414417b8C6CAad45bAeF941aBc7d3Ab32',
    fee: 0.003, // 0.3%
    networks: ['polygon'],
    initCodeHash: '0x96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f',
    priority: 1
  }
  // Add more DEXs as needed
}
```

### DEX Parameters

- `name`: Human-readable DEX name
- `router`: Router contract address
- `factory`: Factory contract address
- `fee`: Trading fee percentage
- `networks`: Networks where this DEX is available
- `initCodeHash`: Init code hash for pair creation
- `priority`: Priority for arbitrage (lower is higher priority)

## Token Configuration

Configure tokens for arbitrage:

```javascript
TOKENS: {
  ETH: {
    symbol: 'ETH',
    name: 'Ethereum',
    decimals: 18,
    addresses: {
      ethereum: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', // WETH
      polygon: '0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619', // WETH
      arbitrum: '0x82aF49447D8a07e3bd95BD0d56f35241523fBab1' // WETH
    },
    priority: 1 // Priority for arbitrage (lower is higher priority)
  },
  USDC: {
    symbol: 'USDC',
    name: 'USD Coin',
    decimals: 6,
    addresses: {
      ethereum: '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48',
      polygon: '0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174',
      arbitrum: '0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8'
    },
    priority: 2
  },
  DAI: {
    symbol: 'DAI',
    name: 'Dai Stablecoin',
    decimals: 18,
    addresses: {
      ethereum: '0x6B175474E89094C44Da98b954EedeAC495271d0F',
      polygon: '0x8f3Cf7ad23Cd3CaDbD9735AFf958023239c6A063',
      arbitrum: '0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1'
    },
    priority: 3
  }
  // Add more tokens as needed
}
```

### Token Parameters

- `symbol`: Token symbol
- `name`: Token name
- `decimals`: Token decimals
- `addresses`: Token addresses on different networks
- `priority`: Priority for arbitrage (lower is higher priority)

## Arbitrage Configuration

Configure arbitrage parameters:

```javascript
ARBITRAGE_CONFIG: {
  minProfitUsd: 10, // Minimum profit in USD to execute arbitrage
  gasLimit: {
    ethereum: 500000,
    polygon: 1000000,
    arbitrum: 3000000
  },
  maxGasPrice: {
    ethereum: 100, // in gwei
    polygon: 50, // in gwei
    arbitrum: 0.5 // in gwei
  },
  slippageTolerance: 0.5, // 0.5%
  ethUsdPrice: 3500, // Fallback ETH price in USD
  maxRetries: 3, // Maximum number of retries for failed transactions
  retryDelay: 5000, // Delay between retries in milliseconds
  monitorInterval: 60000, // Price monitoring interval in milliseconds
  executionTimeout: 300000, // Execution timeout in milliseconds
  priorityTokens: ['ETH', 'USDC', 'DAI'], // Tokens to prioritize for arbitrage
  priorityDexes: ['uniswap', 'sushiswap'], // DEXs to prioritize for arbitrage
  minLiquidityUsd: 100000, // Minimum liquidity in USD for a pair to be considered
  maxTradeSize: { // Maximum trade size in token units
    ETH: '10',
    USDC: '10000',
    DAI: '10000'
  }
}
```

### Arbitrage Parameters

- `minProfitUsd`: Minimum profit in USD to execute arbitrage
- `gasLimit`: Gas limit for transactions on different networks
- `maxGasPrice`: Maximum gas price in gwei for different networks
- `slippageTolerance`: Slippage tolerance percentage
- `ethUsdPrice`: Fallback ETH price in USD
- `maxRetries`: Maximum number of retries for failed transactions
- `retryDelay`: Delay between retries in milliseconds
- `monitorInterval`: Price monitoring interval in milliseconds
- `executionTimeout`: Execution timeout in milliseconds
- `priorityTokens`: Tokens to prioritize for arbitrage
- `priorityDexes`: DEXs to prioritize for arbitrage
- `minLiquidityUsd`: Minimum liquidity in USD for a pair to be considered
- `maxTradeSize`: Maximum trade size in token units

## Flash Loan Configuration

Configure flash loan providers:

```javascript
FLASH_LOAN_CONFIG: {
  providers: {
    aave: {
      name: 'Aave',
      addressProvider: {
        ethereum: '0xB53C1a33016B2DC2fF3653530bfF1848a515c8c5',
        polygon: '0xd05e3E715d945B59290df0ae8eF85c1BdB684744',
        arbitrum: '0xa97684ead0e402dC232d5A977953DF7ECBaB3CDb'
      },
      fee: 0.0009 // 0.09%
    },
    dydx: {
      name: 'dYdX',
      soloMargin: {
        ethereum: '0x1E0447b19BB6EcFdAe1e4AE1694b0C3659614e4e'
      },
      fee: 0 // 0%
    }
  },
  defaultProvider: 'aave', // Default flash loan provider
  maxLoanAmount: { // Maximum loan amount in token units
    ETH: '100',
    USDC: '100000',
    DAI: '100000'
  }
}
```

### Flash Loan Parameters

- `providers`: Flash loan providers configuration
- `defaultProvider`: Default flash loan provider
- `maxLoanAmount`: Maximum loan amount in token units

## Security Configuration

Configure security parameters:

```javascript
SECURITY: {
  knownContracts: [
    { address: '0x742d35Cc6634C0532925a3b844Bc454e4438f44e', name: 'ArbitrageExecutor' }
  ],
  maxSlippagePercent: 3, // Maximum acceptable slippage percentage
  minROI: 5, // Minimum ROI percentage
  circuitBreaker: {
    enabled: true,
    maxConsecutiveFailures: 3,
    cooldownPeriod: 3600000 // 1 hour in milliseconds
  },
  gasMultiplier: 1.2, // Gas price multiplier for faster confirmation
  emergencyWithdrawalAddress: '0x742d35Cc6634C0532925a3b844Bc454e4438f44e'
}
```

### Security Parameters

- `knownContracts`: List of known secure contracts
- `maxSlippagePercent`: Maximum acceptable slippage percentage
- `minROI`: Minimum ROI percentage
- `circuitBreaker`: Circuit breaker configuration
- `gasMultiplier`: Gas price multiplier for faster confirmation
- `emergencyWithdrawalAddress`: Address for emergency withdrawals

## Logging Configuration

Configure logging parameters:

```javascript
LOGGING: {
  level: 'info', // Logging level (debug, info, warn, error)
  logToConsole: true, // Whether to log to console
  logToFile: true, // Whether to log to file
  logDir: 'logs', // Log directory
  maxLogSize: 10 * 1024 * 1024, // 10 MB
  maxLogFiles: 10, // Maximum number of log files
  metricsInterval: 3600000 // 1 hour in milliseconds
}
```

### Logging Parameters

- `level`: Logging level
- `logToConsole`: Whether to log to console
- `logToFile`: Whether to log to file
- `logDir`: Log directory
- `maxLogSize`: Maximum log file size
- `maxLogFiles`: Maximum number of log files
- `metricsInterval`: Metrics collection interval

## Environment Variables

The bot uses environment variables for sensitive configuration. Create a `.env` file with the following variables:

```
# Network RPC URLs
ETHEREUM_RPC_URL=https://mainnet.infura.io/v3/your-infura-key
POLYGON_RPC_URL=https://polygon-rpc.com
ARBITRUM_RPC_URL=https://arb1.arbitrum.io/rpc

# Testnet RPC URLs
GOERLI_RPC_URL=https://goerli.infura.io/v3/your-infura-key

# Wallet Configuration
PRIVATE_KEY=your-private-key

# API Keys
ETHERSCAN_API_KEY=your-etherscan-api-key
POLYGONSCAN_API_KEY=your-polygonscan-api-key

# Bot Configuration
MIN_PROFIT_USD=10
MAX_GAS_PRICE_GWEI_ETHEREUM=100
MAX_GAS_PRICE_GWEI_POLYGON=50
SLIPPAGE_TOLERANCE=0.5
```

## Advanced Configuration

### Custom Token Pairs

Define specific token pairs for arbitrage:

```javascript
TOKEN_PAIRS: [
  ['ETH', 'USDC'],
  ['ETH', 'DAI'],
  ['USDC', 'DAI']
  // Add more pairs as needed
]
```

### Custom DEX Pairs

Define specific DEX pairs for arbitrage:

```javascript
DEX_PAIRS: [
  ['uniswap', 'sushiswap'],
  ['uniswap', 'quickswap'],
  ['sushiswap', 'quickswap']
  // Add more pairs as needed
]
```

### Gas Price Strategy

Configure gas price optimization strategy:

```javascript
GAS_PRICE_STRATEGY: {
  strategy: 'dynamic', // 'static', 'dynamic', or 'aggressive'
  percentile: 60, // Gas price percentile for dynamic strategy
  maxBoostPercent: 20, // Maximum boost percentage for aggressive strategy
  refreshInterval: 30000, // Gas price refresh interval in milliseconds
  fallbackGasPrice: { // Fallback gas price in gwei
    ethereum: 50,
    polygon: 30,
    arbitrum: 0.3
  }
}
```

## Configuration Best Practices

1. **Start Conservative**: Begin with higher minimum profit thresholds and lower maximum gas prices.
2. **Network Selection**: Focus on networks with lower gas fees like Polygon or Arbitrum for higher profitability.
3. **Token Selection**: Choose tokens with high liquidity and price volatility for better arbitrage opportunities.
4. **DEX Selection**: Prioritize DEXs with higher trading volume and liquidity.
5. **Regular Updates**: Regularly update token addresses and DEX configurations as protocols evolve.
6. **Testing**: Always test configuration changes on testnets before deploying to mainnet.
7. **Monitoring**: Implement proper monitoring to track performance and adjust configuration as needed.

## Saving Custom Configurations

You can create custom configuration profiles:

1. Create a new file in the `config` directory, e.g., `config/custom.js`
2. Export your custom configuration
3. Run the bot with the custom configuration:
   ```bash
   node src/index.js --config=custom
   ```

## Configuration Validation

The bot validates your configuration at startup. If there are any issues, it will log errors and exit. Fix the reported issues before restarting the bot.
