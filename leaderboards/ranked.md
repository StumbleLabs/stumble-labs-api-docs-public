# Get Ranked Leaderboard

Returns the ranked leaderboard for the current season, showing the top players ranked by their ranked score.

## Endpoint

```http
GET /live/leaderboards/ranked
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

None. This endpoint does not require any parameters.

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/live/leaderboards/ranked" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/leaderboards/ranked', {
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

url = "https://api.stumblelabs.net/api/live/leaderboards/ranked"
headers = {
    "x-api-key": "your-api-key-here"
}

response = requests.get(url, headers=headers)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Leaderboard found.",
  "data": {
    "seasonId": "LIVE_RANKED_SEASON_17",
    "totalEntries": 1000,
    "entries": [
      {
        "rank": 0,
        "score": 283180,
        "seasonId": "LIVE_RANKED_SEASON_17",
        "player": {
          "userId": 568180573,
          "userName": "JEXTEN7G",
          "country": "BD",
          "trophies": 584765,
          "crowns": 8539,
          "experience": 11003930,
          "hiddenRating": 0,
          "isOnline": false,
          "lastSeenDate": "2025-12-17T04:17:56",
          "skin": "SKIN1456",
          "nativePlatformName": "steam",
          "ranked": {
            "currentSeasonId": "LIVE_RANKED_SEASON_17",
            "currentRankId": 7,
            "currentTierIndex": 0
          },
          "clan": {
            "clanId": "01K2M37CVMZBZZPEHTEC3840T8",
            "name": "Kill The  Enemy",
            "tag": "ABD",
            "logo": {
              "backgroundId": "option15",
              "foregroundId": "option7",
              "colourSchemeId": "option03",
              "level": 5
            },
            "memberCount": 50,
            "memberCapacity": 50,
            "joinPolicy": 1,
            "region": "ASIA",
            "language": "en",
            "clanXP": 60000
          }
        }
      }
    ]
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

### data

| Field | Type | Description |
|-------|------|-------------|
| `seasonId` | string | Current ranked season ID (e.g., "LIVE_RANKED_SEASON_17") |
| `totalEntries` | integer | Total number of entries in the leaderboard (usually 1000) |
| `entries` | array | Array of leaderboard entries, ordered by rank (best first) |

### data.entries[]

Each entry in the leaderboard contains:

| Field | Type | Description |
|-------|------|-------------|
| `rank` | integer | Player's rank in the leaderboard (0 = #1, 1 = #2, etc.) |
| `score` | integer | Player's ranked score for this season |
| `seasonId` | string | Season ID (same as `data.seasonId`) |
| `player` | object | Player data (see below) |

### data.entries[].player

Player information. This follows a simplified version of the [Player Schema](/players/schema). The structure includes:

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | Unique player ID |
| `userName` | string | Current username |
| `country` | string | Country code (ISO 2 letters) |
| `trophies` | integer | Total trophies |
| `crowns` | integer | Total crowns |
| `experience` | integer | Total experience |
| `hiddenRating` | integer | Hidden rating (usually 0) |
| `isOnline` | boolean | Whether the player is online |
| `lastSeenDate` | string | Last seen date/time (ISO 8601) |
| `skin` | string | Current skin ID |
| `nativePlatformName` | string | Platform (steam, android, ios, webgl) |
| `ranked` | object | Ranked information (see [Player Schema - ranked](/players/schema#ranked)) |
| `clan` | object \| null | Simplified clan object (see [Clan Schema](/clans/schema)) or null if not in a clan |

## Errors

### 400 - No Leaderboard Found

```json
{
  "success": false,
  "message": "No leaderboard found.",
  "data": {},
  "errors": ["No leaderboard found"],
  "status": 400
}
```

### 500 - Internal Server Error

```json
{
  "success": false,
  "message": "Internal server error.",
  "data": {},
  "errors": ["Internal server error"],
  "status": 500
}
```

## Notes

- ⚠️ **This endpoint requires authentication** - API key is required
- The leaderboard returns the **top 1000 players** for the current ranked season
- Entries are ordered by rank (rank 0 = #1, rank 1 = #2, etc.)
- The `score` field represents the player's ranked score for the current season
- The `seasonId` is automatically determined based on the current ranked season
- The `player` object is a simplified version of the full player data (see [Player Schema](/players/schema))
- The `clan` field can be `null` if the player is not in a clan
- The `ranked` object within `player` shows the player's current ranked information
- The leaderboard is updated in real-time based on player performance

## Example: Get Top 10 Players

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/leaderboards/ranked', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key-here'
  }
});

const data = await response.json();

// Get top 10 players
const top10 = data.data.entries.slice(0, 10);
console.log('Top 10 Players:');
top10.forEach((entry, index) => {
  console.log(`#${entry.rank + 1}: ${entry.player.userName} - Score: ${entry.score}`);
});
```

## Example: Find Player Position

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/leaderboards/ranked', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key-here'
  }
});

const data = await response.json();

// Find a specific player
const targetUserId = 568180573;
const playerEntry = data.data.entries.find(entry => entry.player.userId === targetUserId);

if (playerEntry) {
  console.log(`Player rank: #${playerEntry.rank + 1}`);
  console.log(`Score: ${playerEntry.score}`);
} else {
  console.log('Player not in top 1000');
}
```

