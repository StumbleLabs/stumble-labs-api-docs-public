# Authentication

Most StumbleLabs API endpoints require authentication through an API key.

## How to Use the API Key

Send your API key in the `x-api-key` header for all requests that require authentication:

```bash
curl -X GET "https://api.stumblelabs.net/api/live/users/search" \
  -H "x-api-key: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{"userid": 123456}'
```

## Access Levels

API keys have different access levels that determine which data is returned:

| Level | Description |
|-------|-------------|
| **0 - Public** | Basic data only (userId, userName, trophies, crowns, skin, ranked) |
| **1 - Basic** | Basic data + some additional data (removes sensitive data like inventory, flags, hiddenRating) |
| **2 - Premium** | Most data (removes only very sensitive data) |
| **3 - Admin** | Full access to all data |

### Example Data by Level

**Level 0 (Public):**
```json
{
  "userId": 123456,
  "userName": "Player",
  "trophies": 5000,
  "crowns": 100,
  "skin": "SKIN1",
  "ranked": { ... }
}
```

**Level 1+ (Basic/Premium/Admin):**
Includes all the above data + inventory, emotes, xpRoadInfo, usernameHistory, etc.

## Rate Limiting

All requests are subject to rate limiting based on your API key.

## Endpoints that DO NOT Require Authentication

Some endpoints are public and do not require an API key:

- `GET /live/shared/search/asset/{asset_id}`

## Authentication Errors

### 401 - Invalid API Key

```json
{
  "success": false,
  "message": "Invalid API key",
  "data": {},
  "errors": ["Unauthorized"],
  "status": 401
}
```

### 403 - Unauthorized

```json
{
  "success": false,
  "message": "Unauthorized",
  "data": {},
  "errors": ["Unauthorized"],
  "status": 403
}
```

## Security

- **Never share your API key publicly**
- **Contact immediately if your key is compromised**
