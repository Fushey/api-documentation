# Rate Limits

Understanding and working with TempMailChecker API rate limits.

## Current Limits

| Tier | Requests/Day | Price |
|------|--------------|-------|
| **Free** | 25 | $0 |
| **Pro** | Custom | Contact us |

Rate limits reset at **midnight UTC** every day.

---

## Checking Your Usage

Use the `/usage` endpoint to check your current usage:

```bash
curl "https://tempmailchecker.com/usage?key=YOUR_API_KEY"
```

**Response:**

```json
{
  "usage_today": 15,
  "limit": 25,
  "reset": "midnight UTC"
}
```

### Using SDKs

```php
// PHP
$usage = $checker->getUsage();
echo "Used: {$usage['usage_today']} / {$usage['limit']}";
```

```javascript
// Node.js
const usage = await checker.getUsage();
console.log(`Used: ${usage.usage_today} / ${usage.limit}`);
```

```python
# Python
usage = checker.get_usage()
print(f"Used: {usage['usage_today']} / {usage['limit']}")
```

```go
// Go
usage, _ := checker.GetUsage()
fmt.Printf("Used: %d / %d\n", usage.UsageToday, usage.Limit)
```

---

## Rate Limit Response

When you exceed your limit, you'll receive a `429 Too Many Requests` response:

```json
{
  "error": "Rate limit exceeded",
  "message": "You have exceeded your daily limit of 25 requests",
  "limit": 25,
  "used": 25,
  "reset": "midnight UTC"
}
```

---

## Best Practices

### 1. Cache Results by Domain

Most disposable email services use the same domains. Cache results to avoid repeated checks:

```javascript
const domainCache = new Map();

async function checkEmail(email) {
    const domain = email.split('@')[1].toLowerCase();
    
    // Check cache first
    if (domainCache.has(domain)) {
        return { temp: domainCache.get(domain) };
    }
    
    // Make API call
    const result = await checker.check(email);
    
    // Cache the result
    domainCache.set(domain, result.temp);
    
    return result;
}
```

### 2. Check Domain Instead of Email

If you're checking multiple emails from the same domain, check the domain once:

```javascript
// Instead of checking each email
await checker.check('user1@tempmail.com');
await checker.check('user2@tempmail.com');
await checker.check('user3@tempmail.com');

// Check the domain once
const result = await checker.checkDomain('tempmail.com');
// Apply result to all emails from that domain
```

### 3. Batch Processing

When processing large lists, implement rate limiting:

```python
import time

emails = [...]  # Large list

for email in emails:
    result = checker.check(email)
    process(result)
    
    time.sleep(0.1)  # 100ms between requests = 600/minute max
```

### 4. Skip Known Legitimate Domains

Don't waste API calls on domains you know are legitimate:

```javascript
const KNOWN_LEGITIMATE = new Set([
    'gmail.com', 'yahoo.com', 'outlook.com', 'hotmail.com',
    'icloud.com', 'protonmail.com', 'aol.com'
]);

async function checkEmail(email) {
    const domain = email.split('@')[1].toLowerCase();
    
    // Skip known legitimate domains
    if (KNOWN_LEGITIMATE.has(domain)) {
        return { temp: false };
    }
    
    return await checker.check(email);
}
```

### 5. Implement Graceful Degradation

If you hit rate limits, don't block users:

```javascript
async function validateEmail(email) {
    try {
        const result = await checker.check(email);
        return { valid: !result.temp, checked: true };
    } catch (error) {
        if (error.message.includes('Rate limit')) {
            // Allow the email but flag for later review
            return { valid: true, checked: false, needsReview: true };
        }
        throw error;
    }
}
```

---

## Need More Requests?

The free tier includes 25 requests per day — perfect for testing and small projects.

**Need more?** Contact us at support@tempmailchecker.com and we'll set you up with a higher limit during beta.

