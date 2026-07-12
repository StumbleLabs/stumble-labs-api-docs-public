# Players Leaderboard

Returns the global — or per-country — player leaderboard, ranked by trophies or crowns. Limited to the top 100.

> See also: [Get Ranked Leaderboard](/leaderboards/ranked) | [Search Player](/players/search)

## Endpoint

```http
GET /live/leaderboards/players
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `sort` | string | No | Ranking metric: `trophies` (default) or `crowns`. Any other value returns `400`. |
| `country` | string | No | ISO 3166-1 alpha-2 country code (e.g. `IT`) for a country leaderboard. Omit for the global leaderboard. |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/live/leaderboards/players?country=IT&sort=trophies" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/leaderboards/players?country=IT&sort=trophies', {
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

url = "https://api.stumblelabs.net/api/live/leaderboards/players"
headers = {"x-api-key": "your-api-key-here"}
params = {"country": "IT", "sort": "trophies"}

response = requests.get(url, headers=headers, params=params)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
	"success": true,
	"message": "Leaderboard fetched successfully.",
	"data": {
		"sort": "trophies",
		"country": "IT",
		"items": [
			{
				"rank": 1,
				"userId": 626950275,
				"userName": "fabio#8rnkd",
				"country": "IT",
				"trophies": 946250,
				"crowns": 2371,
				"skin": "SKIN1070",
				"nativePlatformName": "android"
			},
			{
				"rank": 2,
				"userId": 226391634,
				"userName": "GRRRR!!! _",
				"country": "IT",
				"trophies": 907175,
				"crowns": 12059,
				"skin": "SKIN137",
				"nativePlatformName": "android"
			}
		],
		"total": 100,
		"page": 1,
		"pageSize": 100,
		"totalPages": 1,
		"hasNext": false,
		"hasPrev": false
	},
	"errors": [],
	"status": 200
}
```

## Response Fields

### data

| Field | Type | Description |
|-------|------|-------------|
| `sort` | string | The metric the leaderboard is sorted by (`trophies` or `crowns`) |
| `country` | string \| null | The country filter applied, or `null` for the global leaderboard |
| `items` | array | The ranked players (see below) |
| `total` | integer | Number of entries returned (up to 100) |
| `page` | integer | Always `1` |
| `pageSize` | integer | Always `100` |
| `totalPages` | integer | Always `1` |
| `hasNext` | boolean | Always `false` |
| `hasPrev` | boolean | Always `false` |

### data.items[]

| Field | Type | Description |
|-------|------|-------------|
| `rank` | integer | Leaderboard position (starts at 1) |
| `userId` | integer | Unique player ID |
| `userName` | string | Username (may contain HTML color codes) |
| `country` | string | Player country code (ISO 3166-1 alpha-2) |
| `trophies` | integer | Total trophies |
| `crowns` | integer | Total crowns |
| `skin` | string | Current skin ID |
| `nativePlatformName` | string | Platform (`steam`, `android`, `ios`, `webgl`) |

## Errors

### 400 - Validation Error

Returned when `sort` is set to a value other than `trophies` or `crowns`.

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["'sort' must be 'crowns' or 'trophies'."],
  "status": 400
}
```

## Notes

- The leaderboard is capped at the **top 100**; there is no real pagination (`pageSize` is always 100 and `totalPages` is always 1).
- `sort` accepts only `trophies` or `crowns`.
- Provide `country` for a single country's leaderboard; omit it for the global leaderboard.
