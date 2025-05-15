# API Reference - Agent API

## Agent API Reference

### Overview

The INTUE Agent API provides standardized interfaces for interacting with AI trading agents. This API enables developers to initialize, configure, and utilize agents for market analysis and trade execution.

### Authentication

```javascript
// Initialize API client with authentication
const intueClient = new IntueClient({
  apiKey: 'YOUR_API_KEY',
  apiSecret: 'YOUR_API_SECRET'
});

// Verify authentication
const authStatus = await intueClient.verifyAuth();
console.log('Authentication status:', authStatus);
```

### Core Agent Methods

#### Initializing an Agent

```javascript
// Initialize agent with configuration
const agent = await intueClient.initializeAgent({
  agentType: 'momentum',  // Options: momentum, arbitrage, meta, custom
  configuration: {
    sensitivity: 0.7,
    timeframes: ['1h', '4h', '1d'],
    assets: ['BTC', 'ETH', 'SOL', 'AVAX']
  },
  mcpOptions: {
    sentiment: {
      sources: ['twitter', 'reddit'],
      updateInterval: 10 * 60 * 1000  // 10 minutes
    },
    volume: {
      granularity: '1h'
    }
  }
});

// Get agent status
const status = await agent.getStatus();
console.log('Agent status:', status);
```

#### Processing Market Data

```javascript
// Process current market data
const signals = await agent.process({
  timeframe: '1h',
  forceFresh: true,  // Force fresh data fetch
  assets: ['BTC', 'ETH', 'SOL']  // Override configured assets
});

console.log('Generated signals:', signals);

// Process historical data
const historicalAnalysis = await agent.processHistorical({
  startTime: new Date('2023-01-01T00:00:00Z'),
  endTime: new Date('2023-06-30T23:59:59Z'),
  timeframe: '1d',
  assets: ['BTC', 'ETH']
});

console.log('Historical analysis:', historicalAnalysis);
```

#### Configuring Agent Parameters

```javascript
// Update agent configuration
await agent.updateConfiguration({
  sensitivity: 0.8,  // Increase sensitivity
  timeframes: ['4h', '1d'],  // Change timeframes
  addAssets: ['LINK', 'MATIC'],  // Add assets to existing list
  removeAssets: ['AVAX']  // Remove assets from existing list
});

// Update specific MCP options
await agent.updateMCPConfiguration('sentiment', {
  sources: ['twitter', 'reddit', 'discord'],
  nlpModel: 'advanced'
});

// Get current configuration
const config = await agent.getConfiguration();
console.log('Current configuration:', config);
```

#### Trade Execution

```javascript
// Initialize trading functionality
await agent.initializeTrading({
  exchange: 'binance',  // Options: binance, hyperliquid, custom
  credentials: {
    apiKey: process.env.EXCHANGE_API_KEY,
    secretKey: process.env.EXCHANGE_SECRET_KEY
  },
  riskManagement: {
    maxRiskPerTrade: 0.02,  // 2% per trade
    stopLossPercent: 0.05,  // 5% stop loss
    takeProfitPercent: 0.15  // 15% take profit
  },
  mode: 'live'  // Options: live, paper, simulation
});

// Execute trades based on signals
const trades = await agent.executeTrades({
  confidenceThreshold: 0.75,  // Minimum signal confidence
  maxPositions: 5,  // Maximum concurrent positions
  assets: ['BTC', 'ETH', 'SOL']  // Limit to specific assets
});

console.log('Executed trades:', trades);

// Get active positions
const positions = await agent.getPositions();
console.log('Active positions:', positions);

// Close specific position
const closeResult = await agent.closePosition({
  asset: 'BTC',
  percentage: 100  // Close entire position
});

console.log('Position closed:', closeResult);
```

### Agent Monitoring

#### Performance Metrics

```javascript
// Get agent performance metrics
const performance = await agent.getPerformanceMetrics({
  timeframe: '30d',  // Options: 1d, 7d, 30d, 90d, all
  includeOpenPositions: true
});

console.log('Performance metrics:', performance);

// Get signal accuracy metrics
const accuracy = await agent.getSignalAccuracy({
  timeframe: '90d',
  minimumConfidence: 0.7
});

console.log('Signal accuracy:', accuracy);
```

#### Audit Logs

```javascript
// Get agent activity logs
const logs = await agent.getActivityLogs({
  startTime: new Date('2023-06-01T00:00:00Z'),
  endTime: new Date('2023-06-30T23:59:59Z'),
  types: ['signal', 'trade', 'configuration'],
  limit: 100
});

console.log('Activity logs:', logs);

// Get specific signal history
const signalHistory = await agent.getSignalHistory({
  asset: 'BTC',
  timeframe: '30d',
  minimumConfidence: 0.6
});

console.log('Signal history:', signalHistory);
```

### Advanced Agent Operations

#### Backtesting

```javascript
// Run backtest with current configuration
const backtestResults = await agent.runBacktest({
  startTime: new Date('2022-01-01T00:00:00Z'),
  endTime: new Date('2023-01-01T00:00:00Z'),
  initialCapital: 10000,
  assets: ['BTC', 'ETH', 'SOL'],
  feeStructure: {
    maker: 0.001,  // 0.1%
    taker: 0.002  // 0.2%
  }
});

console.log('Backtest results:', backtestResults);

// Get detailed backtest metrics
const backtestMetrics = await agent.getBacktestMetrics({
  backtestId: backtestResults.id,
  includeTradeBreakdown: true
});

console.log('Backtest metrics:', backtestMetrics);
```

#### Agent Composition

```javascript
// Create composed agent from multiple agents
const composedAgent = await intueClient.createComposedAgent({
  name: 'Hybrid Strategy',
  agents: [
    {
      agentId: momentumAgent.id,
      weight: 0.6
    },
    {
      agentId: sentimentAgent.id,
      weight: 0.4
    }
  ],
  compositionMethod: 'weighted',  // Options: weighted, filtered, sequential
  conflictResolution: 'highestConfidence'  // How to resolve conflicting signals
});

// Process with composed agent
const composedSignals = await composedAgent.process();
console.log('Composed agent signals:', composedSignals);
```

#### Custom Agent Deployment

```javascript
// Deploy custom agent
const customAgent = await intueClient.deployCustomAgent({
  name: 'My Custom Agent',
  sourceCode: sourceCodeString,
  entryPoint: 'src/index.js',
  configSchema: {
    // JSON schema for configuration
  },
  dependencies: {
    '@intue/core': '^1.0.0',
    'technicalindicators': '^3.1.0'
  }
});

// Initialize custom agent
await customAgent.initialize({
  // Custom configuration
});

// Use custom agent
const customSignals = await customAgent.process();
console.log('Custom agent signals:', customSignals);
```

### Response Formats

#### Signal Object

```javascript
{
  "asset": "BTC",
  "direction": "up",  // or "down"
  "confidence": 0.85,
  "timeframe": "4h",
  "timestamp": 1623451200000,
  "expiryTimestamp": 1623480000000,
  "signalType": "momentum",
  "metadata": {
    "indicators": {
      "rsi": 72.5,
      "macd": {
        "line": 0.42,
        "signal": 0.18,
        "histogram": 0.24
      }
    },
    "supportingFactors": [
      "volume_increase",
      "price_breakout",
      "sentiment_positive"
    ],
    "conflictingFactors": []
  },
  "id": "sig_1623451200000_BTC_up"
}
```

#### Trade Result Object

```javascript
{
  "tradeId": "trd_98765432",
  "status": "executed",  // executed, failed, partial
  "signalId": "sig_1623451200000_BTC_up",
  "asset": "BTC",
  "direction": "buy",
  "quantity": 0.05,
  "price": 28500.75,
  "timestamp": 1623451230000,
  "exchange": "binance",
  "fees": 2.85,
  "metadata": {
    "orderId": "binance_ord_12345",
    "executionLatency": 125,  // ms
    "slippage": 0.0012  // 0.12%
  }
}
```

#### Performance Metrics Object

```javascript
{
  "timeframe": "30d",
  "roi": 0.172,  // 17.2%
  "absoluteReturn": 1720.50,  // USD
  "winRate": 0.68,  // 68%
  "averageWin": 0.027,  // 2.7%
  "averageLoss": 0.012,  // 1.2%
  "maxDrawdown": 0.085,  // 8.5%
  "sharpeRatio": 2.3,
  "sortinoRatio": 3.1,
  "calmarRatio": 5.2,
  "tradesExecuted": 22,
  "signalsGenerated": 31,
  "highConfidenceAccuracy": 0.75,  // 75% accuracy for high confidence signals
  "assetBreakdown": {
    "BTC": {
      "roi": 0.21,
      "trades": 8
    },
    "ETH": {
      "roi": 0.15,
      "trades": 7
    },
    // Other assets...
  }
}
```
