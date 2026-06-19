# List Clan Seasons

Returns the list of archived clan seasons (most recent first).

> See also: [Current Clan Season](/clans/season-current) | [Member Season XP](/clans/member-season-xp) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/seasons
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/clans/seasons" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/clans/seasons', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key-here'
  }
});

const data = await response.json();
console.log(data);
```

### Python (requests)

```python
import requests

url = "https://api.stumblelabs.net/api/clans/seasons"
headers = {"x-api-key": "your-api-key-here"}

response = requests.get(url, headers=headers)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Clan seasons fetched successfully.",
  "data": {
    "items": [
      {
        "seasonId": "CLANS_LIVE_SEASON_6",
        "startDate": "2025-06-01T00:00:00.000Z",
        "endDate": "2025-07-01T00:00:00.000Z",
        "totalPoints": 12000,
        "isActive": true
      },
      {
        "seasonId": "CLANS_LIVE_SEASON_5",
        "startDate": "2025-05-01T00:00:00.000Z",
        "endDate": "2025-06-01T00:00:00.000Z",
        "totalPoints": 11000,
        "isActive": false
      }
    ],
    "count": 2
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `items` | array | List of seasons, ordered by `startDate` descending |
| `count` | integer | Number of seasons returned |

### items[]

| Field | Type | Description |
|-------|------|-------------|
| `seasonId` | string | Season identifier (e.g., `CLANS_LIVE_SEASON_6`) |
| `startDate` | string | Season start date (ISO 8601) |
| `endDate` | string | Season end date (ISO 8601) |
| `totalPoints` | integer | Total points available in the season |
| `isActive` | boolean | Whether the season is currently active |

## Notes

- Use [Current Clan Season](/clans/season-current) to retrieve the active season with its full milestone list.
