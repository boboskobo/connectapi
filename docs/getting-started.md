# Getting Started with Connect AI API

Welcome to the Connect AI API documentation. This guide will help you get started with integrating Connect AI into your applications.

## Authentication

All API requests must include your API key in the Authorization header:

```bash
Authorization: Bearer YOUR_API_KEY
```

To get your API key:
1. Log into your Connect AI dashboard
2. Go to Settings > API Keys
3. Generate a new API key
4. Store this key securely - it won't be shown again

## Making Your First Request

Here's a simple example of creating a contact using cURL:

```bash
curl -X POST "https://api.yourservice.com/v1/contacts" \
-H "Authorization: Bearer YOUR_API_KEY" \
-H "Content-Type: application/json" \
-d '{"phoneNumber": "+15555555555"}'
```

## Rate Limiting

- Maximum of 10 requests per second per API key
- Exceeding this limit will result in a 429 (Too Many Requests) response
- Implement exponential backoff in your integration

## Need Help?

If you encounter any issues or need assistance, please contact our support team at support@connectsoftware.inc

© 2024 Connect Software Inc. All rights reserved. 