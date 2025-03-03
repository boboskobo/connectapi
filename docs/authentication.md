# Authentication

## Overview
Connect AI uses API keys to authenticate all requests. You must include your API key in the `Authorization` header of every request.

## Getting Your API Key
1. Log into your Connect AI dashboard
2. Navigate to Settings > API Keys
3. Click "Generate New API Key"
4. Copy and store your API key securely

## Using Your API Key

### HTTP Header Format
```bash
Authorization: Bearer YOUR_API_KEY
```

### Example Request
```bash
curl -X POST "https://api.connectsoftware.inc/v1/contacts" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "phoneNumber": "+15555555555"
  }'
```

## Security Best Practices
- Never share your API key or commit it to version control
- Rotate your API keys periodically
- Use different API keys for development and production
- Monitor your API key usage for suspicious activity
- Implement IP whitelisting when possible

## Rate Limiting
- 10 requests per second per API key
- Exceeding this limit results in a 429 error
- Implement exponential backoff in your code

## Error Responses
If your authentication fails, you'll receive one of these responses:

### Invalid API Key
```json
{
  "status": "error",
  "statusCode": 401,
  "message": "Invalid API key provided"
}
```

### Missing API Key
```json
{
  "status": "error",
  "statusCode": 401,
  "message": "No API key provided"
}
```

### Rate Limit Exceeded
```json
{
  "status": "error",
  "statusCode": 429,
  "message": "Rate limit exceeded. Please try again in 60 seconds"
}
``` 