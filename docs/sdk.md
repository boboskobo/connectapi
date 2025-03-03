# Connect AI SDKs

## Official SDKs

### Node.js
```bash
npm install @connect-ai/sdk
```

```javascript
const Connect = require('@connect-ai/sdk');
const client = new Connect('YOUR_API_KEY');

// Create a contact
async function createContact() {
  try {
    const contact = await client.contacts.create({
      phoneNumber: '+15555555555'
    });
    console.log('Contact created:', contact);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

### Python
```bash
pip install connect-ai
```

```python
from connect_ai import Client

client = Client('YOUR_API_KEY')

# Create a contact
try:
    contact = client.contacts.create(
        phone_number='+15555555555'
    )
    print(f'Contact created: {contact}')
except Exception as e:
    print(f'Error: {e}')
```

### PHP
```bash
composer require connect-ai/sdk
```

```php
<?php
require_once 'vendor/autoload.php';

$client = new ConnectAI\Client('YOUR_API_KEY');

// Create a contact
try {
    $contact = $client->contacts->create([
        'phoneNumber' => '+15555555555'
    ]);
    echo "Contact created: " . json_encode($contact);
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
```

## SDK Features
- Automatic retry handling
- Rate limiting protection
- Type safety
- Promise/async support
- Error handling
- Webhook signature verification
- Logging and debugging tools

## Best Practices
- Keep your SDK version up to date
- Use environment variables for API keys
- Implement proper error handling
- Set reasonable timeouts
- Enable logging in development

## Example: Error Handling
```javascript
try {
  const contact = await client.contacts.create({
    phoneNumber: '+15555555555'
  });
} catch (error) {
  if (error instanceof Connect.RateLimitError) {
    // Handle rate limiting
    await delay(error.retryAfter);
    // Retry the request
  } else if (error instanceof Connect.AuthenticationError) {
    // Handle authentication issues
    console.error('Invalid API key');
  } else {
    // Handle other errors
    console.error('Unexpected error:', error);
  }
}
``` 