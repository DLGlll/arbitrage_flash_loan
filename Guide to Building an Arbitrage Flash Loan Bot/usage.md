# Usage Guide

This guide provides detailed instructions on how to use the Arbitrage Flash Loan Trading Bot effectively.

## Getting Started

After completing the installation and configuration as described in the [Installation Guide](./installation.md) and [Configuration Guide](./configuration.md), you're ready to start using the bot.

## Command Line Interface

The bot provides a command-line interface with various options:

```bash
node src/index.js [options]
```

### Available Options

- `--mode`: Operating mode (`monitor` or `execute`)
  - `monitor`: Only monitors prices without executing trades
  - `execute`: Monitors prices and executes profitable trades

- `--network`: Network to use (e.g., `ethereum`, `polygon`, `arbitrum`)
  - Default: Uses all configured networks

- `--log-level`: Logging level (`debug`, `info`, `warn`, `error`)
  - Default: `info`

- `--config`: Path to custom configuration file
  - Default: Uses the default configuration

- `--dry-run`: Simulates trades without actually executing them
  - Useful for testing and validation

- `--token-pairs`: Comma-separated list of token pairs to monitor
  - Example: `ETH-USDC,ETH-DAI`

- `--dex-pairs`: Comma-separated list of DEX pairs to monitor
  - Example: `uniswap-sushiswap,uniswap-quickswap`

### Examples

1. Start in monitoring mode on Polygon:
   ```bash
   node src/index.js --mode=monitor --network=polygon
   ```

2. Execute trades on Ethereum with debug logging:
   ```bash
   node src/index.js --mode=execute --network=ethereum --log-level=debug
   ```

3. Run with a custom configuration:
   ```bash
   node src/index.js --config=custom
   ```

4. Simulate trades without execution:
   ```bash
   node src/index.js --mode=execute --dry-run
   ```

5. Monitor specific token pairs:
   ```bash
   node src/index.js --token-pairs=ETH-USDC,ETH-DAI
   ```

## Operating Modes

### Monitor Mode

In monitor mode, the bot:
- Connects to the specified blockchain networks
- Monitors prices across configured DEXs
- Identifies potential arbitrage opportunities
- Calculates potential profits
- Logs all findings without executing trades

This mode is useful for:
- Testing your configuration
- Understanding market conditions
- Identifying the most profitable token and DEX pairs
- Determining optimal trade parameters

### Execute Mode

In execute mode, the bot:
- Performs all monitoring functions
- Validates identified opportunities
- Performs security checks
- Executes profitable arbitrage trades using flash loans
- Logs all actions and results

This mode should only be used:
- After thorough testing in monitor mode
- With appropriate risk management settings
- When you're confident in your configuration

## Workflow

The bot follows this workflow:

1. **Initialization**:
   - Loads configuration
   - Establishes blockchain connections
   - Initializes price monitoring

2. **Price Monitoring**:
   - Continuously monitors prices across DEXs
   - Compares prices to identify discrepancies

3. **Opportunity Identification**:
   - Analyzes price discrepancies
   - Calculates potential profits
   - Factors in gas costs and flash loan fees

4. **Validation and Security Checks**:
   - Validates all parameters
   - Performs security checks
   - Ensures profitability after all costs

5. **Execution** (in execute mode):
   - Prepares flash loan parameters
   - Submits transaction to blockchain
   - Monitors transaction status

6. **Reporting**:
   - Logs all actions and results
   - Updates metrics and statistics
   - Provides performance summaries

## Monitoring and Management

### Logs

The bot generates detailed logs in the `logs` directory. These logs contain:
- Price monitoring data
- Identified opportunities
- Execution details
- Errors and warnings

Example log file: `logs/arbitrage_2025-03-27.log`

### Metrics

The bot collects performance metrics, which are saved periodically:
- Number of arbitrage attempts
- Success and failure rates
- Total profit and gas costs
- Average profit per trade
- Most profitable token pairs and DEXs

Metrics are saved to: `logs/metrics.json`

### Transaction History

A detailed transaction history is maintained:
- All executed trades
- Flash loan details
- Profit and loss information
- Gas costs

Transaction history is saved to: `logs/transactions.json`

## Advanced Usage

### Custom Scripts

You can create custom scripts to interact with the bot's modules:

```javascript
// custom-script.js
const { createBlockchainConnection } = require('./src/blockchain/connection');
const { PriceMonitor } = require('./src/arbitrage/price-monitor');
const config = require('./src/config');

async function main() {
  // Create blockchain connection
  const { provider } = await createBlockchainConnection('ethereum');
  
  // Initialize price monitor
  const priceMonitor = new PriceMonitor(provider, 'ethereum');
  
  // Monitor specific token pair
  const token0 = config.TOKENS.ETH.addresses.ethereum;
  const token1 = config.TOKENS.USDC.addresses.ethereum;
  const amountIn = ethers.utils.parseEther('1.0');
  
  const results = await priceMonitor.monitorPrices(
    ['uniswap', 'sushiswap'],
    token0,
    token1,
    amountIn
  );
  
  console.log(results);
}

main().catch(console.error);
```

Run custom script:
```bash
node custom-script.js
```

### Scheduled Execution

You can use cron jobs to schedule the bot's execution:

```bash
# Run every hour
0 * * * * cd /path/to/bot && node src/index.js --mode=execute >> logs/cron.log 2>&1
```

### Multiple Instances

You can run multiple instances of the bot with different configurations:

```bash
# Instance 1: Ethereum
node src/index.js --network=ethereum --config=ethereum-config

# Instance 2: Polygon
node src/index.js --network=polygon --config=polygon-config
```

## Troubleshooting

### Common Issues

1. **No Profitable Opportunities**:
   - Check minimum profit threshold in configuration
   - Verify token and DEX configurations
   - Ensure gas price settings are appropriate

2. **Transaction Failures**:
   - Check gas price and gas limit settings
   - Verify contract addresses
   - Check network connectivity

3. **Flash Loan Failures**:
   - Verify flash loan provider configuration
   - Check token approvals
   - Ensure contract has necessary permissions

### Debugging

For detailed debugging:

1. Set log level to debug:
   ```bash
   node src/index.js --log-level=debug
   ```

2. Check debug logs:
   ```bash
   tail -f logs/arbitrage_2025-03-27.log | grep DEBUG
   ```

3. Run in dry-run mode:
   ```bash
   node src/index.js --mode=execute --dry-run
   ```

## Best Practices

1. **Start Small**:
   - Begin with small trade amounts
   - Focus on well-established token pairs
   - Use networks with lower gas fees

2. **Monitor Before Executing**:
   - Run in monitor mode for several days
   - Analyze logs and metrics
   - Adjust configuration based on findings

3. **Risk Management**:
   - Set appropriate profit thresholds
   - Implement circuit breakers
   - Monitor gas prices closely

4. **Regular Maintenance**:
   - Update token and DEX configurations
   - Monitor contract upgrades
   - Keep dependencies up to date

5. **Security**:
   - Regularly rotate private keys
   - Monitor contract balances
   - Implement withdrawal limits

## Advanced Strategies

### Multi-Network Arbitrage

Configure the bot to monitor opportunities across multiple networks:

```bash
node src/index.js --mode=monitor --network=ethereum,polygon,arbitrum
```

### Token Pair Optimization

Analyze which token pairs provide the most profitable opportunities:

```bash
node scripts/analyze-token-pairs.js
```

### Gas Price Optimization

Fine-tune gas price strategies for different networks:

```javascript
// config/gas-optimization.js
module.exports = {
  GAS_PRICE_STRATEGY: {
    ethereum: {
      strategy: 'dynamic',
      percentile: 60
    },
    polygon: {
      strategy: 'aggressive',
      maxBoostPercent: 30
    }
  }
};
```

## Conclusion

The Arbitrage Flash Loan Trading Bot is a powerful tool for identifying and executing profitable arbitrage opportunities. By following this guide and best practices, you can maximize your chances of success while minimizing risks.

Remember that cryptocurrency trading involves significant risks, and past performance is not indicative of future results. Always use this tool responsibly and only with funds you can afford to lose.
