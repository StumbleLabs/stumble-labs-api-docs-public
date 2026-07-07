# Get Player Profiles (Batch)

Fetch multiple player profiles in a single request by their user IDs. Returns the same profile objects as [Search Player](/players/search), one per ID.

> See also: [Search Player](/players/search) | [Player Schema](/players/schema)

## Endpoint

```http
POST /live/users/profiles
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |
| `Content-Type` | string | Yes | `application/json` |

## Request Body

```json
{ "userIds": [193589871, 374303791] }
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `userIds` | integer[] | Yes | 1 to 32 player IDs (each within the Int32 range) |

## Request Example

### cURL

```bash
curl -X POST "https://api.stumblelabs.net/api/live/users/profiles" \
  -H "x-api-key: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{"userIds":[193589871,374303791]}'
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/users/profiles', {
  method: 'POST',
  headers: {
    'x-api-key': 'your-api-key-here',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ userIds: [193589871, 374303791] })
});

const data = await response.json();
console.log(data);
```

### Python (requests)

```python
import requests

url = "https://api.stumblelabs.net/api/live/users/profiles"
headers = {
    "x-api-key": "your-api-key-here",
    "Content-Type": "application/json"
}
payload = {"userIds": [193589871, 374303791]}

response = requests.post(url, headers=headers, json=payload)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
	"success": true,
	"message": "Player profiles fetched successfully.",
	"data": [
		{
			"userId": 193589871,
			"userName": "<#0bf>Dasw",
			"country": "BR",
			"trophies": 176740,
			"crowns": 5232,
			"experience": 242920,
			"isOnline": false,
			"skin": "SKIN958",
			"nativePlatformName": "steam",
			"ranked": {
				"name": "Unranked",
				"icon": "https://cdn.stumblelabs.net/ranked/generic.png",
				"currentSeasonId": "LIVE_RANKED_SEASON_24",
				"currentRankId": 0,
				"currentTierIndex": 0
			},
			"clan": {
				"clanId": "01K1JZ400449Q1TZERE9E719Y1",
				"name": "Devs",
				"tag": "DEV"
			},
			"isFallback": false,
			"isCache": false
		}
	],
	"errors": [],
	"status": 200
}
```

## Response Fields

`data` is an **array** of player objects, one per requested ID, in the **same order** as `userIds`. Each object follows the standard [Player Schema](/players/schema).

- Duplicated IDs are returned as duplicates (not de-duplicated).
- Unknown-but-valid IDs may return a placeholder profile flagged with `isFallback: true`.

## Errors

### 400 - Validation Error

Returned when `userIds` is missing, empty, contains more than 32 items, or contains non-integer / out-of-range values.

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["'userIds' must be an array with 1 to 32 items."],
  "status": 400
}
```

Other validation messages include:

- `"'userIds' is required."`
- `"each userId must be an integer within the Int32 range."`

## Notes

- Up to **32** IDs per request.
- Results preserve the order of `userIds`; duplicates are returned as-is.
- The endpoint occasionally returns a transient `500`; retrying usually succeeds.
- This is the most efficient way to fetch many profiles at once (one call instead of one per player).
