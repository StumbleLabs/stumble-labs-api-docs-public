# Ranked Score History

Returns a player's ranked **score history over time**: per-season summary stats plus a time-series of score snapshots. Useful for plotting a player's ranked progression.

> See also: [Search Player](/players/search) | [Player Schema](/players/schema)

## Endpoint

```http
GET /live/users/search/ranked-history/{userId}
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userId` | integer | Yes | Player ID (positive Int32) |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/live/users/search/ranked-history/374303791" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/users/search/ranked-history/374303791', {
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

url = "https://api.stumblelabs.net/api/live/users/search/ranked-history/374303791"
headers = {"x-api-key": "your-api-key-here"}

response = requests.get(url, headers=headers)
data = response.json()
print(data)
```

## Success Response (200)

> ⚠️ Unlike most endpoints, a successful response returns the history object **directly** — there is no `success` / `data` envelope. (Only error responses use the standard envelope.)

```json
{
	"userId": 374303791,
	"isActiveInTop": false,
	"currentSeason": "LIVE_RANKED_SEASON_24",
	"lastUpdated": "2026-05-24T13:20:16.849Z",
	"lastKnownSnapshot": {
		"snapshotTime": "2026-05-24T13:20:16.849Z",
		"score": 12950,
		"seasonId": "LIVE_RANKED_SEASON_22"
	},
	"seasonStats": {
		"LIVE_RANKED_SEASON_22": {
			"highestScore": 13150,
			"lowestScore": 12710,
			"averageScore": 12913,
			"totalSnapshots": 16,
			"firstSeen": "2026-05-10T01:54:18.164Z",
			"lastSeen": "2026-05-24T13:20:16.849Z"
		}
	},
	"scoreHistoryBySeason": {
		"LIVE_RANKED_SEASON_22": [
			{ "snapshotTime": "2026-05-24T13:20:16.849Z", "score": 12950 },
			{ "snapshotTime": "2026-05-13T22:41:15.177Z", "score": 12880 }
		]
	},
	"totalSeasonsTracked": 2,
	"loadedSeasons": ["LIVE_RANKED_SEASON_22"]
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | The queried player ID |
| `isActiveInTop` | boolean | Whether the player is currently in the ranked top leaderboard |
| `currentSeason` | string | The active ranked season ID |
| `lastUpdated` | string \| null | ISO 8601 time of the most recent snapshot (`null` if none) |
| `lastKnownSnapshot` | object \| null | The most recent snapshot: `{ snapshotTime, score, seasonId }` (`null` if none) |
| `seasonStats` | object | Per-season summary, keyed by season ID (covers **all** tracked seasons). See below |
| `scoreHistoryBySeason` | object | Per-season time-series, keyed by season ID. Only includes seasons listed in `loadedSeasons`. See below |
| `totalSeasonsTracked` | integer | Number of seasons with tracked data for this player |
| `loadedSeasons` | string[] | Seasons whose full snapshot history is included in `scoreHistoryBySeason` |

### seasonStats[seasonId]

| Field | Type | Description |
|-------|------|-------------|
| `highestScore` | integer | Highest score recorded that season |
| `lowestScore` | integer | Lowest score recorded that season |
| `averageScore` | integer | Average across all snapshots that season |
| `totalSnapshots` | integer | Number of snapshots recorded that season |
| `firstSeen` | string | ISO 8601 time of the first snapshot that season |
| `lastSeen` | string | ISO 8601 time of the last snapshot that season |

### scoreHistoryBySeason[seasonId][]

| Field | Type | Description |
|-------|------|-------------|
| `snapshotTime` | string | ISO 8601 time of the snapshot |
| `score` | integer | Ranked score at that time |

Snapshots are ordered **newest first** within each season.

## Players with no ranked history

A valid player who has never been tracked in ranked returns `200` with empty structures:

```json
{
	"userId": 117797443,
	"isActiveInTop": false,
	"currentSeason": "LIVE_RANKED_SEASON_24",
	"lastUpdated": null,
	"lastKnownSnapshot": null,
	"seasonStats": {},
	"scoreHistoryBySeason": {},
	"totalSeasonsTracked": 0,
	"loadedSeasons": []
}
```

## Errors

### 400 - Validation Error

Returned when `userId` is not a positive Int32 integer.

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

- A successful response is the history object itself (no `success` / `data` wrapper); errors use the standard envelope.
- `seasonStats` summarizes **every** tracked season, while `scoreHistoryBySeason` contains the full per-snapshot series only for the seasons in `loadedSeasons` — so a player can have more entries in `seasonStats` than in `scoreHistoryBySeason`.
- Snapshots are captured periodically by StumbleLabs; `totalSnapshots` reflects how many were recorded that season.
- Untracked players return `200` with empty structures (not a `404`).
