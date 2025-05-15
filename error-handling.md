# Error Handling

The API uses standard HTTP status codes and provides detailed error objects:

```javascript
try {
  const signals = await agent.process();
} catch (error) {
  console.error('Error processing market data:', error);
  
  if (error.code === 'MARKET_DATA_UNAVAILABLE') {
    // Handle market data unavailability
  } else if (error.code === 'CONFIGURATION_INVALID') {
    // Handle configuration issue
  } else if (error.code === 'RATE_LIMIT_EXCEEDED') {
    // Handle rate limiting
  }
}
```

#### Common Error Codes

```
// Error Codes

// AUTHENTICATION_FAILED
// API key invalid or expired

// INSUFFICIENT_PERMISSIONS
// Lacking permissions for requested operation

// RATE_LIMIT_EXCEEDED
// API rate limit exceeded

// MARKET_DATA_UNAVAILABLE
// Required market data not available

// CONFIGURATION_INVALID
// Agent configuration invalid

// AGENT_INITIALIZATION_FAILED
// Failed to initialize agent

// TRADING_DISABLED
// Trading functionality not enabled

// EXCHANGE_ERROR
// Error communicating with exchange

// INSUFFICIENT_FUNDS
// Insufficient funds for trade execution
```
