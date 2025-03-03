# Webhooks

## Overview
Connect AI webhooks allow you to receive real-time updates when specific events occur in your account.

## Available Events
- `contact.created` - Triggered when a new contact is created
- `contact.updated` - Triggered when a contact's information is updated
- `message.sent` - Triggered when a message is sent
- `message.received` - Triggered when a message is received

## Setting Up Webhooks
1. Go to your Connect AI dashboard
2. Navigate to Settings > Webhooks
3. Click "Add Webhook"
4. Enter your endpoint URL
5. Select the events you want to receive
6. Save your webhook configuration

## Webhook Format

### Headers
```
Content-Type: application/json
X-Connect-Signature: sha256=...
X-Connect-Event: contact.created
```

### Example Payload
```json
{
  "event": "contact.created",
  "timestamp": "2024-03-02T12:00:00Z",
  "data": {
    "contactId": "cont_123abc",
    "phoneNumber": "+15555555555",
    "createdAt": "2024-03-02T12:00:00Z"
  }
}
```

## Security
- Verify webhook signatures using your webhook secret
- Only accept requests from Connect AI IP addresses
- Implement retry logic for failed webhook deliveries
- Set up monitoring for webhook delivery status

## Signature Verification
```python
import hmac
import hashlib

def verify_webhook_signature(payload, signature, secret):
    expected = hmac.new(
        secret.encode('utf-8'),
        payload.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(signature, expected)
```

## Best Practices
- Respond quickly to webhook requests (under 3 seconds)
- Implement proper error handling
- Store webhook data before processing
- Set up monitoring and alerting
- Use HTTPS endpoints only 