# Ranked Encounters

Returns the opponents a player has faced most often in **ranked** during a given season — a "top opponents" list with encounter counts and timing.

> See also: [Search Player](/players/search) | [Ranked Score History](/players/ranked-history)

## Endpoint

```http
GET /live/users/search/encounters/{userId}
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userId` | integer | Yes | Player ID (positive Int32) |

## Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `seasonId` | string | Yes | Ranked season ID (e.g. `LIVE_RANKED_SEASON_24`) |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/live/users/search/encounters/159080158?seasonId=LIVE_RANKED_SEASON_24" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/users/search/encounters/159080158?seasonId=LIVE_RANKED_SEASON_24', {
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

url = "https://api.stumblelabs.net/api/live/users/search/encounters/159080158"
headers = {"x-api-key": "your-api-key-here"}
params = {"seasonId": "LIVE_RANKED_SEASON_24"}

response = requests.get(url, headers=headers, params=params)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
	"success": true,
	"message": "Encounters fetched successfully.",
	"data": {
		"userId": 159080158,
		"seasonId": "LIVE_RANKED_SEASON_24",
		"uniqueOpponents": 50,
		"totalEncounters": 11964,
		"opponents": [
			{
				"userId": 439387825,
				"userName": "`Riven`",
				"country": "MA",
				"skin": "SKIN137",
				"currentRankId": 7,
				"encounterCount": 498,
				"firstEncounterAt": "2026-07-02T13:52:44.020Z",
				"lastEncounterAt": "2026-07-07T23:29:27.254Z",
				"opponentPeakScore": 83850,
				"opponentBestRank": 1
			}
		]
	},
	"errors": [],
	"status": 200
}
```

> The `opponents` array is truncated above for brevity — it contains up to **50** entries, ordered by `encounterCount` (most-faced first).

## Response Fields

### data

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | The queried player ID |
| `seasonId` | string | The ranked season the data refers to |
| `uniqueOpponents` | integer | Number of distinct opponents returned (up to 50) |
| `totalEncounters` | integer | Total number of ranked encounters that season |
| `opponents` | array | Top opponents by encounter count (see below) |

### data.opponents[]

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | Opponent's player ID |
| `userName` | string | Opponent's username (may contain HTML color codes) |
| `country` | string | Opponent's country code (ISO 3166-1 alpha-2) |
| `skin` | string | Opponent's current skin ID |
| `currentRankId` | integer | Opponent's current ranked tier ID |
| `encounterCount` | integer | How many times this opponent was faced |
| `firstEncounterAt` | string | ISO 8601 time of the first encounter |
| `lastEncounterAt` | string | ISO 8601 time of the most recent encounter |
| `opponentPeakScore` | integer | Opponent's peak ranked score that season |
| `opponentBestRank` | integer | Opponent's best leaderboard rank that season |

## Players with no encounters

A valid player with no ranked encounters in the given season returns `200` with empty data:

```json
{
	"success": true,
	"message": "Encounters fetched successfully.",
	"data": {
		"userId": 117797443,
		"seasonId": "LIVE_RANKED_SEASON_24",
		"uniqueOpponents": 0,
		"totalEncounters": 0,
		"opponents": []
	},
	"errors": [],
	"status": 200
}
```

## Errors

### 400 - Validation Error

Returned when `seasonId` is missing/empty, or `userId` is not a positive Int32 integer.

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": [
    "'seasonId' is required.",
    "'seasonId' must be a string.",
    "'seasonId' must not be empty."
  ],
  "status": 400
}
```

## Notes

- `seasonId` is **required**.
- The `opponents` list is capped at the top **50** opponents by `encounterCount`.
- An unknown season ID returns `200` with empty data (not a `404`).
- Data is ranked-only; non-ranked players (or seasons with no ranked play) return empty structures.
