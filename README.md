<div align="center">
  <img src="header.png" alt="TempMailChecker API" width="100%">
</div>

# TempMailChecker API Documentation

[![API Status](https://img.shields.io/badge/API-Online-brightgreen?style=flat-square)](https://tempmailchecker.com)
[![Free Tier](https://img.shields.io/badge/Free-25%20req%2Fday-blue?style=flat-square)](https://tempmailchecker.com)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

> **Detect disposable email addresses in real-time.** Protect your platform from fake signups, spam, and abuse with our fast, reliable email validation API.

## 🚀 Quick Start

```bash
curl "https://tempmailchecker.com/check?email=test@tempmail.com" \
  -H "X-API-Key: YOUR_API_KEY"
```

**Response:**
```json
{"temp": true}
```

Get your free API key at [tempmailchecker.com](https://tempmailchecker.com)

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [Getting Started](docs/getting-started.md) | Quick setup guide and first API call |
| [API Reference](docs/api-reference.md) | Complete endpoint documentation |
| [SDKs](docs/sdks.md) | Official libraries for PHP, Node.js, Python, Go |
| [Examples](docs/examples.md) | Code examples in multiple languages |
| [Error Handling](docs/errors.md) | Error codes and troubleshooting |
| [Rate Limits](docs/rate-limits.md) | Usage limits and best practices |

---

## 🔌 Official SDKs

Install our official SDK for your language:

| Language | Package | Installation |
|----------|---------|--------------|
| **PHP** | [php-disposable-email-checker](https://github.com/Fushey/php-disposable-email-checker) | `composer require tempmailchecker/php-sdk` |
| **Node.js** | [nodejs-disposable-email-checker](https://github.com/Fushey/nodejs-disposable-email-checker) | `npm install @tempmailchecker/node-sdk` |
| **Python** | [python-disposable-email-checker](https://github.com/Fushey/python-disposable-email-checker) | `pip install tempmailchecker` |
| **Go** | [go-disposable-email-checker](https://github.com/Fushey/go-disposable-email-checker) | `go get github.com/Fushey/go-disposable-email-checker` |

---

## 🌍 Global Endpoints

Choose the endpoint closest to your users for lowest latency:

| Region | Base URL | Best For |
|--------|----------|----------|
| 🇪🇺 Europe | `https://tempmailchecker.com` | EU, Africa, Middle East |
| 🇺🇸 United States | `https://us.tempmailchecker.com` | Americas |
| 🇸🇬 Asia | `https://asia.tempmailchecker.com` | Asia-Pacific, Australia, Japan |

All endpoints share the same API key and database.

---

## 📋 API Overview

### Check Email

```http
GET /check?email=user@example.com
```

**Headers:**
```
X-API-Key: your_api_key
```

**Response:**
```json
{"temp": true}   // Disposable email
{"temp": false}  // Legitimate email
```

### Check Domain

```http
GET /check?domain=tempmail.com
```

### Check Usage

```http
GET /usage?key=your_api_key
```

**Response:**
```json
{
  "usage_today": 15,
  "limit": 25,
  "reset": "midnight UTC"
}
```

---

## ⚡ Features

- **Fast** — Average response time under 50ms
- **Accurate** — 200,000+ disposable domains tracked
- **Reliable** — 99.9% uptime SLA
- **Global** — 3 regional endpoints for low latency
- **Simple** — RESTful API, JSON responses
- **Free Tier** — 25 requests/day, no credit card

---

## 🛡️ Use Cases

- **User Registration** — Block fake signups
- **Newsletter Forms** — Keep mailing lists clean
- **E-commerce** — Reduce fraud risk
- **Promotions** — Prevent coupon abuse
- **Lead Generation** — Ensure quality leads

---

## 📊 Rate Limits

| Tier | Requests/Day | Price |
|------|--------------|-------|
| Free | 25 | $0 |
| Pro | 1,000+ | Contact us |

Rate limit exceeded? You'll receive a `429` response:

```json
{
  "error": "Rate limit exceeded",
  "message": "Daily limit reached",
  "reset": "midnight UTC"
}
```

---

## 🔗 Links

- **Website:** [tempmailchecker.com](https://tempmailchecker.com)
- **Get API Key:** [tempmailchecker.com](https://tempmailchecker.com)
- **Support:** support@tempmailchecker.com

---

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

