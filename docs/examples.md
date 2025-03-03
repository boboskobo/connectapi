# Code Examples

## ROCK RMS Integration

### Webhook Setup
```csharp
// In your ROCK RMS workflow
public class ConnectWebhook : IWorkflowAction
{
    public void Execute(WorkflowAction action, IEntity entity)
    {
        var phoneNumber = entity.GetAttributeValue("PhoneNumber");
        
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_API_KEY");
        
        var content = new StringContent(
            JsonConvert.SerializeObject(new { phoneNumber = phoneNumber }),
            Encoding.UTF8,
            "application/json"
        );
        
        var response = await client.PostAsync(
            "https://api.connectsoftware.inc/v1/contacts",
            content
        );
        
        if (!response.IsSuccessStatusCode)
        {
            throw new Exception($"Connect API Error: {response.StatusCode}");
        }
    }
}
```

## Common Use Cases

### Create Contact with Custom Fields
```javascript
const contact = await client.contacts.create({
  phoneNumber: '+15555555555',
  firstName: 'John',
  lastName: 'Doe',
  customFields: {
    churchLocation: 'Main Campus',
    memberSince: '2024-01-01'
  }
});
```

### Bulk Contact Creation
```python
contacts = [
    {'phoneNumber': '+15555555551'},
    {'phoneNumber': '+15555555552'},
    {'phoneNumber': '+15555555553'}
]

results = await client.contacts.bulk_create(contacts)
```

### Update Contact Information
```php
$contact = $client->contacts->update('cont_123abc', [
    'firstName' => 'Jane',
    'lastName' => 'Smith',
    'tags' => ['VIP', 'NewMember']
]);
```

### Send Message to Contact
```javascript
const message = await client.messages.send({
  to: '+15555555555',
  text: 'Welcome to our church! Let us know if you have any questions.',
  template: 'welcome_message'
});
```

### Handle Webhook Events
```python
from flask import Flask, request
import hmac
import hashlib

app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def handle_webhook():
    # Verify signature
    signature = request.headers.get('X-Connect-Signature')
    payload = request.get_data()
    
    if not verify_signature(payload, signature):
        return 'Invalid signature', 401
    
    # Process webhook
    event = request.json
    if event['event'] == 'contact.created':
        handle_new_contact(event['data'])
    
    return 'OK', 200

def verify_signature(payload, signature):
    expected = hmac.new(
        'YOUR_WEBHOOK_SECRET'.encode(),
        payload,
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(f'sha256={expected}', signature)
```

## Error Handling Examples

### Rate Limiting
```javascript
const rateLimitRetry = async (fn, maxRetries = 3) => {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (error instanceof Connect.RateLimitError) {
        await new Promise(resolve => 
          setTimeout(resolve, error.retryAfter * 1000)
        );
        continue;
      }
      throw error;
    }
  }
  throw new Error('Max retries exceeded');
};

// Usage
await rateLimitRetry(() => 
  client.contacts.create({ phoneNumber: '+15555555555' })
);
```

### Batch Processing
```python
async def process_batch(phone_numbers):
    batch_size = 100
    results = []
    
    for i in range(0, len(phone_numbers), batch_size):
        batch = phone_numbers[i:i + batch_size]
        try:
            result = await client.contacts.bulk_create([
                {'phoneNumber': number} for number in batch
            ])
            results.extend(result)
        except Exception as e:
            print(f'Error processing batch {i}: {e}')
    
    return results
``` 