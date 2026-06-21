# Username History

Get the list of usernames a player has used over time, ordered from most recent
to oldest.

> See also: [Search Player](/players/search) | [Authentication](/authentication) | [Error Handling](/errors)

## Endpoint

```http
GET /live/users/:userId/usernames
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userId` | integer | Yes | Player numeric ID (positive Int32, `1`–`2147483647`) |

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl "https://api.stumblelabs.net/api/live/users/193589871/usernames" \
  -H "x-api-key: your-api-key-here"
```

#### **JavaScript**

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/users/193589871/usernames', {
  headers: {
    'x-api-key': 'your-api-key-here'
  }
});

const data = await response.json();
console.log(data);
```

#### **Python**

```python
import requests

url = "https://api.stumblelabs.net/api/live/users/193589871/usernames"
headers = {
    "x-api-key": "your-api-key-here"
}

response = requests.get(url, headers=headers)
data = response.json()
print(data)
```

<!-- tabs:end -->

## Success Response (200)

```json
{
  "success": true,
  "message": "Usernames fetched successfully.",
  "data": {
    "count": 3,
    "usernames": [
      { "userName": "<#0bf>Dasw", "seenAt": "2026-04-12" },
      { "userName": "<#39e>dasw", "seenAt": "2026-01-08" },
      { "userName": "<#39e>dasw45", "seenAt": "2025-11-30" }
    ]
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `count` | integer | Number of usernames returned |
| `usernames[]` | array | Username history entries (most recent first) |
| `usernames[].userName` | string | Username as displayed (may include color and format codes) |
| `usernames[].seenAt` | string | Date the username was observed, `YYYY-MM-DD` (date only) |

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["'userId' must be a positive integer within the Int32 range."],
  "status": 400
}
```

## Notes

- Entries are ordered by `seenAt` descending, so the current username is first.
- `seenAt` is a calendar date without a time component.
- A player with no recorded history returns `count: 0` and an empty `usernames` array.
