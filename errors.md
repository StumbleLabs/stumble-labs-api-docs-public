# Status Codes and Errors

The StumbleLabs API returns standard HTTP status codes and descriptive error messages.

## HTTP Status Codes

| Code | Meaning | Description |
|------|---------|-------------|
| `200` | OK | Request successful |
| `400` | Bad Request | Validation error or invalid parameters |
| `401` | Unauthorized | Invalid or missing API key |
| `403` | Forbidden | Access denied |
| `404` | Not Found | Resource not found |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | Internal server error |

## Error Format

All errors follow the standard format:

```json
{
  "success": false,
  "message": "Error description",
  "data": {},
  "errors": ["List of specific errors"],
  "status": 400
}
```

## Common Errors

### 400 - Bad Request

**Common causes:**
- Missing required parameters
- Invalid data format
- Validation failed

**Example:**
```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["userid/userId or username required"],
  "status": 400
}
```

### 401 - Unauthorized

**Causes:**
- Missing API key
- Invalid API key
- Expired API key

**Example:**
```json
{
  "success": false,
  "message": "Invalid API key",
  "data": {},
  "errors": ["Unauthorized"],
  "status": 401
}
```

### 403 - Forbidden

**Causes:**
- Access denied for this endpoint
- Insufficient permissions

**Example:**
```json
{
  "success": false,
  "message": "Unauthorized",
  "data": {},
  "errors": ["Unauthorized"],
  "status": 403
}
```

### 404 - Not Found

**Causes:**
- Resource does not exist
- Invalid ID/parameter
- Resource not found in database

**Examples:**

Player not found:
```json
{
  "success": false,
  "message": "Player not found at the moment, you can try again later.",
  "data": {},
  "errors": ["No player found with the provided userid or username"],
  "status": 404
}
```

Clan not found:
```json
{
  "success": false,
  "message": "Clan not found",
  "data": {},
  "errors": ["Clan not found"],
  "status": 404
}
```

### 429 - Too Many Requests

**Cause:**
- Rate limit exceeded

**Example:**
```json
{
  "success": false,
  "message": "Rate limit exceeded",
  "data": {},
  "errors": ["Too many requests"],
  "status": 429
}
```

**Response headers:**
```
Retry-After: 60
```

### 500 - Internal Server Error

**Cause:**
- Internal server error
- Temporary issue

**Example:**
```json
{
  "success": false,
  "message": "Internal server error",
  "data": {},
  "errors": ["An unexpected error occurred"],
  "status": 500
}
```

## Error Handling

### JavaScript

```javascript
try {
  const response = await fetch('https://api.stumblelabs.net/api/live/users/search', {
    method: 'POST',
    headers: {
      'x-api-key': 'your-api-key',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ userid: 123456 })
  });

  const data = await response.json();

  if (!data.success) {
    console.error('Error:', data.message);
    console.error('Details:', data.errors);
    
    if (response.status === 429) {
      const retryAfter = response.headers.get('Retry-After');
      console.log(`Rate limit. Try again in ${retryAfter} seconds`);
    }
  }
} catch (error) {
  console.error('Network error:', error);
}
```

### Python

```python
import requests
import time

def make_request(url, headers, data):
    try:
        response = requests.post(url, json=data, headers=headers)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.HTTPError as e:
        if e.response.status_code == 429:
            retry_after = int(e.response.headers.get('Retry-After', 60))
            print(f"Rate limit exceeded. Wait {retry_after} seconds")
            time.sleep(retry_after)
            return make_request(url, headers, data)  # Retry
        else:
            error_data = e.response.json()
            print(f"Error {e.response.status_code}: {error_data.get('message')}")
            print(f"Details: {error_data.get('errors')}")
    except requests.exceptions.RequestException as e:
        print(f"Network error: {e}")
```

## Best Practices

1. **Always check the `success` field** before processing data
2. **Read the error message** - it usually explains the problem
3. **Check the `errors` array** for specific details
4. **Implement retry logic** for 429 and 500 errors
5. **Use try-catch** to catch network errors
6. **Log errors** for debugging
