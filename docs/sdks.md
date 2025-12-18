# Official SDKs

Official client libraries for the TempMailChecker API.

## Available SDKs

| Language | Package | Version | Links |
|----------|---------|---------|-------|
| PHP | `tempmailchecker/php-sdk` | 1.0.0 | [GitHub](https://github.com/Fushey/php-disposable-email-checker) · [Packagist](https://packagist.org/packages/tempmailchecker/php-sdk) |
| Node.js | `@tempmailchecker/node-sdk` | 1.0.0 | [GitHub](https://github.com/Fushey/nodejs-disposable-email-checker) · [npm](https://www.npmjs.com/package/@tempmailchecker/node-sdk) |
| Python | `tempmailchecker` | 1.0.0 | [GitHub](https://github.com/Fushey/python-disposable-email-checker) · [PyPI](https://pypi.org/project/tempmailchecker/) |
| Go | `go-disposable-email-checker` | 1.0.0 | [GitHub](https://github.com/Fushey/go-disposable-email-checker) |

---

## PHP

### Installation

```bash
composer require tempmailchecker/php-sdk
```

### Usage

```php
<?php
require_once 'vendor/autoload.php';

use TempMailChecker\TempMailChecker;

// Create client
$checker = new TempMailChecker('your_api_key');

// Check email
$result = $checker->check('user@tempmail.com');
if ($result['temp']) {
    echo "Disposable email!";
}

// Check domain
$result = $checker->checkDomain('tempmail.com');

// Use regional endpoint
$checker = new TempMailChecker('your_api_key', TempMailChecker::ENDPOINT_US);

// Get usage
$usage = $checker->getUsage();
echo "Used: {$usage['usage_today']} / {$usage['limit']}";
```

📖 [Full PHP Documentation](https://github.com/Fushey/php-disposable-email-checker#readme)

---

## Node.js

### Installation

```bash
npm install @tempmailchecker/node-sdk
# or
yarn add @tempmailchecker/node-sdk
# or
pnpm add @tempmailchecker/node-sdk
```

### Usage

```javascript
import { TempMailChecker, ENDPOINT_US } from '@tempmailchecker/node-sdk';

// Create client
const checker = new TempMailChecker('your_api_key');

// Check email
const result = await checker.check('user@tempmail.com');
if (result.temp) {
    console.log('Disposable email!');
}

// Check domain
const domainResult = await checker.checkDomain('tempmail.com');

// Use regional endpoint
const usChecker = new TempMailChecker('your_api_key', { endpoint: ENDPOINT_US });

// Get usage
const usage = await checker.getUsage();
console.log(`Used: ${usage.usage_today} / ${usage.limit}`);
```

📖 [Full Node.js Documentation](https://github.com/Fushey/nodejs-disposable-email-checker#readme)

---

## Python

### Installation

```bash
pip install tempmailchecker
```

### Usage

```python
from tempmailchecker import TempMailChecker, ENDPOINT_US

# Create client
checker = TempMailChecker('your_api_key')

# Check email
result = checker.check('user@tempmail.com')
if result['temp']:
    print('Disposable email!')

# Check domain
result = checker.check_domain('tempmail.com')

# Use regional endpoint
checker = TempMailChecker('your_api_key', endpoint=ENDPOINT_US)

# Get usage
usage = checker.get_usage()
print(f"Used: {usage['usage_today']} / {usage['limit']}")
```

📖 [Full Python Documentation](https://github.com/Fushey/python-disposable-email-checker#readme)

---

## Go

### Installation

```bash
go get github.com/Fushey/go-disposable-email-checker
```

### Usage

```go
package main

import (
    "fmt"
    tempmailchecker "github.com/Fushey/go-disposable-email-checker"
)

func main() {
    // Create client
    checker := tempmailchecker.MustNew("your_api_key")

    // Check email
    result, _ := checker.Check("user@tempmail.com")
    if result.Temp {
        fmt.Println("Disposable email!")
    }

    // Check domain
    result, _ = checker.CheckDomain("tempmail.com")

    // Use regional endpoint
    checker, _ = tempmailchecker.New("your_api_key",
        tempmailchecker.WithEndpoint(tempmailchecker.EndpointUS),
    )

    // Get usage
    usage, _ := checker.GetUsage()
    fmt.Printf("Used: %d / %d\n", usage.UsageToday, usage.Limit)
}
```

📖 [Full Go Documentation](https://github.com/Fushey/go-disposable-email-checker#readme)

---

## Other Languages

Don't see your language? The TempMailChecker API is a simple REST API that works with any HTTP client.

```bash
curl "https://tempmailchecker.com/check?email=test@example.com" \
  -H "X-API-Key: your_api_key"
```

See the [API Reference](api-reference.md) for complete documentation.

---

## SDK Features Comparison

| Feature | PHP | Node.js | Python | Go |
|---------|-----|---------|--------|-----|
| Check Email | ✅ | ✅ | ✅ | ✅ |
| Check Domain | ✅ | ✅ | ✅ | ✅ |
| Get Usage | ✅ | ✅ | ✅ | ✅ |
| Regional Endpoints | ✅ | ✅ | ✅ | ✅ |
| Custom Timeout | ✅ | ✅ | ✅ | ✅ |
| TypeScript Support | - | ✅ | - | - |
| Type Hints | - | - | ✅ | ✅ |

