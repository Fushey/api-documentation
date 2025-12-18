# Getting Started

Get up and running with the TempMailChecker API in under 5 minutes.

## Step 1: Get Your API Key

1. Visit [tempmailchecker.com](https://tempmailchecker.com)
2. Enter your email address
3. Check your inbox for your API key

Your API key looks like: `tmck_xxxxxxxxxxxxxxxxxxxxxxxxxxxx`

## Step 2: Make Your First Request

### Using cURL

```bash
curl "https://tempmailchecker.com/check?email=test@tempmail.com" \
  -H "X-API-Key: YOUR_API_KEY"
```

### Using an SDK

**PHP:**
```php
use TempMailChecker\TempMailChecker;

$checker = new TempMailChecker('YOUR_API_KEY');
$result = $checker->check('test@tempmail.com');

if ($result['temp']) {
    echo "Disposable email detected!";
}
```

**Node.js:**
```javascript
import { TempMailChecker } from '@tempmailchecker/node-sdk';

const checker = new TempMailChecker('YOUR_API_KEY');
const result = await checker.check('test@tempmail.com');

if (result.temp) {
    console.log('Disposable email detected!');
}
```

**Python:**
```python
from tempmailchecker import TempMailChecker

checker = TempMailChecker('YOUR_API_KEY')
result = checker.check('test@tempmail.com')

if result['temp']:
    print('Disposable email detected!')
```

**Go:**
```go
import tempmailchecker "github.com/Fushey/go-disposable-email-checker"

checker := tempmailchecker.MustNew("YOUR_API_KEY")
result, _ := checker.Check("test@tempmail.com")

if result.Temp {
    fmt.Println("Disposable email detected!")
}
```

## Step 3: Handle the Response

The API returns a simple JSON response:

```json
{"temp": true}   // Email is disposable
{"temp": false}  // Email is legitimate
```

## Step 4: Choose Your Endpoint

For lowest latency, use the endpoint closest to your users:

| Region | Base URL |
|--------|----------|
| 🇪🇺 Europe | `https://tempmailchecker.com` |
| 🇺🇸 USA | `https://us.tempmailchecker.com` |
| 🇸🇬 Asia | `https://asia.tempmailchecker.com` |

## Next Steps

- [API Reference](api-reference.md) — Full endpoint documentation
- [SDKs](sdks.md) — Install an official library
- [Examples](examples.md) — More code examples
- [Error Handling](errors.md) — Handle errors gracefully

