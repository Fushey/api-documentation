# API Reference

Complete documentation for the TempMailChecker REST API.

## Base URLs

| Region | Base URL |
|--------|----------|
| 🇪🇺 Europe (Default) | `https://tempmailchecker.com` |
| 🇺🇸 United States | `https://us.tempmailchecker.com` |
| 🇸🇬 Asia | `https://asia.tempmailchecker.com` |

All endpoints share the same API key and database.

---

## Authentication

Include your API key in the request header:

```
X-API-Key: your_api_key
```

Get your free API key at [tempmailchecker.com](https://tempmailchecker.com)

---

## Endpoints

### Check Email

Verify if an email address is from a disposable email provider.

```http
GET /check?email={email}
```

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | Yes* | Full email address to check |
| `domain` | string | Yes* | Or just the domain (alternative) |

*Use either `email` or `domain`, not both.

**Headers:**

| Header | Required | Description |
|--------|----------|-------------|
| `X-API-Key` | Yes | Your API key |

**Example Request:**

```bash
curl "https://tempmailchecker.com/check?email=user@tempmail.com" \
  -H "X-API-Key: tmck_your_api_key"
```

**Success Response (200 OK):**

```json
{
  "temp": true
}
```

| Field | Type | Description |
|-------|------|-------------|
| `temp` | boolean | `true` if disposable, `false` if legitimate |

---

### Check Domain

Check if a domain is a disposable email provider.

```http
GET /check?domain={domain}
```

**Example Request:**

```bash
curl "https://tempmailchecker.com/check?domain=guerrillamail.com" \
  -H "X-API-Key: tmck_your_api_key"
```

**Response:**

```json
{
  "temp": true
}
```

---

### Get Usage

Check your current API usage statistics.

```http
GET /usage?key={api_key}
```

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | Yes | Your API key |

**Note:** No `X-API-Key` header required for this endpoint.

**Example Request:**

```bash
curl "https://tempmailchecker.com/usage?key=tmck_your_api_key"
```

**Success Response (200 OK):**

```json
{
  "usage_today": 15,
  "limit": 25,
  "reset": "midnight UTC"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `usage_today` | integer | Requests made today |
| `limit` | integer | Daily request limit |
| `reset` | string | When the counter resets |

---

## Error Responses

### Invalid API Key (401)

```json
{
  "error": "Invalid API key",
  "temp": null
}
```

### Missing Parameter (400)

```json
{
  "error": "Missing email or domain parameter"
}
```

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

---

## Rate Limits

| Tier | Requests/Day |
|------|--------------|
| Free | 25 |
| Pro | Contact us |

Rate limits reset at midnight UTC.

---

## Response Codes

| Code | Description |
|------|-------------|
| `200` | Success |
| `400` | Bad request (missing parameters) |
| `401` | Invalid API key |
| `429` | Rate limit exceeded |
| `500` | Server error |

