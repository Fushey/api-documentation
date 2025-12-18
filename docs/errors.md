# Error Handling

How to handle errors from the TempMailChecker API.

## HTTP Status Codes

| Code | Description | Action |
|------|-------------|--------|
| `200` | Success | Process the response |
| `400` | Bad Request | Check your parameters |
| `401` | Unauthorized | Check your API key |
| `429` | Too Many Requests | Wait and retry, or upgrade |
| `500` | Server Error | Retry with exponential backoff |

---

## Error Responses

### Missing Parameter (400)

```json
{
  "error": "Missing email or domain parameter"
}
```

**Cause:** Neither `email` nor `domain` was provided.

**Solution:** Include either `email` or `domain` in your request.

```bash
# Correct
curl "https://tempmailchecker.com/check?email=test@example.com" -H "X-API-Key: YOUR_KEY"

# Also correct
curl "https://tempmailchecker.com/check?domain=example.com" -H "X-API-Key: YOUR_KEY"
```

---

### Invalid API Key (401)

```json
{
  "error": "Invalid API key",
  "temp": null
}
```

**Cause:** The API key is missing, malformed, or doesn't exist.

**Solution:**
1. Check that you included the `X-API-Key` header
2. Verify your API key is correct
3. Get a new key at [tempmailchecker.com](https://tempmailchecker.com)

---

### Rate Limit Exceeded (429)

```json
{
  "error": "Rate limit exceeded",
  "message": "You have exceeded your daily limit of 25 requests",
  "limit": 25,
  "used": 25,
  "reset": "midnight UTC"
}
```

**Cause:** You've exceeded your daily request limit.

**Solution:**
1. Wait until midnight UTC for the counter to reset
2. Contact us to increase your limit
3. Implement caching to reduce requests

---

## Handling Errors in SDKs

### PHP

```php
use TempMailChecker\TempMailChecker;
use Exception;

$checker = new TempMailChecker('your_api_key');

try {
    $result = $checker->check($email);
} catch (Exception $e) {
    if (strpos($e->getMessage(), 'Rate limit') !== false) {
        // Handle rate limit
        echo "Rate limit exceeded, try again later";
    } elseif (strpos($e->getMessage(), 'Invalid API key') !== false) {
        // Handle invalid key
        echo "Invalid API key";
    } else {
        // Handle other errors
        error_log($e->getMessage());
    }
}
```

### Node.js

```javascript
import { TempMailChecker } from '@tempmailchecker/node-sdk';

const checker = new TempMailChecker('your_api_key');

try {
    const result = await checker.check(email);
} catch (error) {
    if (error.message.includes('Rate limit')) {
        console.log('Rate limit exceeded, try again later');
    } else if (error.message.includes('Invalid API key')) {
        console.log('Invalid API key');
    } else {
        console.error('Error:', error.message);
    }
}
```

### Python

```python
from tempmailchecker import TempMailChecker

checker = TempMailChecker('your_api_key')

try:
    result = checker.check(email)
except Exception as e:
    if 'Rate limit' in str(e):
        print('Rate limit exceeded, try again later')
    elif 'Invalid API key' in str(e):
        print('Invalid API key')
    else:
        print(f'Error: {e}')
```

### Go

```go
import tempmailchecker "github.com/Fushey/go-disposable-email-checker"

checker := tempmailchecker.MustNew("your_api_key")

result, err := checker.Check(email)
if err != nil {
    if tempmailchecker.IsRateLimitError(err) {
        fmt.Println("Rate limit exceeded, try again later")
    } else if tempmailchecker.IsAPIError(err) {
        fmt.Printf("API error: %v\n", err)
    } else {
        fmt.Printf("Error: %v\n", err)
    }
    return
}
```

---

## Best Practices

### 1. Implement Retries

```javascript
async function checkWithRetry(email, maxRetries = 3) {
    for (let i = 0; i < maxRetries; i++) {
        try {
            return await checker.check(email);
        } catch (error) {
            if (error.message.includes('Rate limit')) {
                throw error; // Don't retry rate limits
            }
            if (i === maxRetries - 1) throw error;
            await sleep(1000 * Math.pow(2, i)); // Exponential backoff
        }
    }
}
```

### 2. Cache Results

```javascript
const cache = new Map();

async function checkCached(email) {
    const domain = email.split('@')[1];
    
    if (cache.has(domain)) {
        return cache.get(domain);
    }
    
    const result = await checker.check(email);
    cache.set(domain, result);
    
    return result;
}
```

### 3. Graceful Degradation

```javascript
async function validateEmail(email) {
    try {
        const result = await checker.check(email);
        return !result.temp;
    } catch (error) {
        // If the API is unavailable, allow the email
        console.error('Email check failed, allowing:', error);
        return true;
    }
}
```

