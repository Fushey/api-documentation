# Code Examples

Real-world examples of using the TempMailChecker API.

## Registration Form Validation

### PHP (Laravel)

```php
<?php
// app/Http/Controllers/RegisterController.php

use TempMailChecker\TempMailChecker;

class RegisterController extends Controller
{
    public function register(Request $request)
    {
        $email = $request->input('email');
        
        // Check for disposable email
        $checker = new TempMailChecker(config('services.tempmailchecker.key'));
        $result = $checker->check($email);
        
        if ($result['temp']) {
            return back()->withErrors([
                'email' => 'Please use a permanent email address.'
            ]);
        }
        
        // Continue with registration...
    }
}
```

### Node.js (Express)

```javascript
// routes/auth.js
import { TempMailChecker } from '@tempmailchecker/node-sdk';

const checker = new TempMailChecker(process.env.TEMPMAILCHECKER_API_KEY);

app.post('/register', async (req, res) => {
    const { email, password } = req.body;
    
    // Check for disposable email
    try {
        const result = await checker.check(email);
        
        if (result.temp) {
            return res.status(400).json({
                error: 'Please use a permanent email address.'
            });
        }
    } catch (error) {
        console.error('Email check failed:', error);
        // Optionally allow registration if check fails
    }
    
    // Continue with registration...
});
```

### Python (Django)

```python
# views.py
from django.http import JsonResponse
from tempmailchecker import TempMailChecker
import os

checker = TempMailChecker(os.environ['TEMPMAILCHECKER_API_KEY'])

def register(request):
    email = request.POST.get('email')
    
    # Check for disposable email
    try:
        result = checker.check(email)
        
        if result['temp']:
            return JsonResponse({
                'error': 'Please use a permanent email address.'
            }, status=400)
    except Exception as e:
        print(f'Email check failed: {e}')
    
    # Continue with registration...
```

### Go (Gin)

```go
// handlers/auth.go
package handlers

import (
    "github.com/gin-gonic/gin"
    tempmailchecker "github.com/Fushey/go-disposable-email-checker"
)

var checker = tempmailchecker.MustNew(os.Getenv("TEMPMAILCHECKER_API_KEY"))

func Register(c *gin.Context) {
    email := c.PostForm("email")
    
    // Check for disposable email
    result, err := checker.Check(email)
    if err == nil && result.Temp {
        c.JSON(400, gin.H{
            "error": "Please use a permanent email address.",
        })
        return
    }
    
    // Continue with registration...
}
```

---

## Batch Validation

### Python

```python
from tempmailchecker import TempMailChecker
import time

checker = TempMailChecker('your_api_key')

emails = [
    'user1@gmail.com',
    'user2@tempmail.com',
    'user3@yahoo.com',
    'user4@10minutemail.com',
]

results = []
for email in emails:
    try:
        result = checker.check(email)
        results.append({
            'email': email,
            'disposable': result['temp']
        })
        time.sleep(0.1)  # Rate limiting
    except Exception as e:
        print(f'Error checking {email}: {e}')

# Filter disposable emails
disposable = [r for r in results if r['disposable']]
legitimate = [r for r in results if not r['disposable']]

print(f'Found {len(disposable)} disposable emails')
print(f'Found {len(legitimate)} legitimate emails')
```

---

## Real-time Form Validation (JavaScript)

```html
<form id="signup-form">
    <input type="email" id="email" placeholder="Enter email">
    <span id="email-status"></span>
    <button type="submit">Sign Up</button>
</form>

<script>
const emailInput = document.getElementById('email');
const status = document.getElementById('email-status');
let timeout;

emailInput.addEventListener('input', () => {
    clearTimeout(timeout);
    timeout = setTimeout(checkEmail, 500);
});

async function checkEmail() {
    const email = emailInput.value;
    if (!email || !email.includes('@')) return;
    
    status.textContent = 'Checking...';
    
    try {
        const response = await fetch(`/api/check-email?email=${encodeURIComponent(email)}`);
        const data = await response.json();
        
        if (data.temp) {
            status.textContent = '⚠️ Please use a permanent email';
            status.style.color = 'red';
        } else {
            status.textContent = '✓ Email looks good';
            status.style.color = 'green';
        }
    } catch (error) {
        status.textContent = '';
    }
}
</script>
```

---

## Webhook Integration

### Node.js

```javascript
// Validate email when webhook receives new user
app.post('/webhook/new-user', async (req, res) => {
    const { email, userId } = req.body;
    
    const result = await checker.check(email);
    
    if (result.temp) {
        // Flag user for review
        await db.users.update(userId, {
            flagged: true,
            flagReason: 'Disposable email detected'
        });
        
        // Send alert
        await slack.send(`⚠️ User ${userId} registered with disposable email: ${email}`);
    }
    
    res.json({ processed: true });
});
```

---

## CLI Tool

### Bash

```bash
#!/bin/bash
# check-email.sh - Check if an email is disposable

API_KEY="your_api_key"
EMAIL="$1"

if [ -z "$EMAIL" ]; then
    echo "Usage: ./check-email.sh <email>"
    exit 1
fi

RESULT=$(curl -s "https://tempmailchecker.com/check?email=$EMAIL" \
    -H "X-API-Key: $API_KEY")

if echo "$RESULT" | grep -q '"temp": true'; then
    echo "⚠️  $EMAIL is a disposable email"
    exit 1
else
    echo "✓ $EMAIL is legitimate"
    exit 0
fi
```

Usage:
```bash
chmod +x check-email.sh
./check-email.sh test@tempmail.com
```

