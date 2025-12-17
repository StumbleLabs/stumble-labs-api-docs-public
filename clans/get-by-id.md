# Get Clan by ID

Returns detailed information about a clan, including a complete list of members with their profiles.

> See also: [Search Clans](/clans/search) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/{clanId}
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `clanId` | string | Yes | Clan ID (ULID) |


## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/clans/01K1JZ400449Q1TZERE9E719Y1" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/clans/01K1JZ400449Q1TZERE9E719Y1', {
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

url = "https://api.stumblelabs.net/api/clans/01K1JZ400449Q1TZERE9E719Y1"
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
	"message": "Clan data fetched successfully.",
	"data": {
		"clanId": "01K1JZ400449Q1TZERE9E719Y1",
		"description": "Mr. Stumble: Hello World!",
		"members": [
			{
				"userProfile": {
					"userId": 46542430,
					"userName": "_3tx",
					"title": "",
					"country": "BR",
					"trophies": 80270,
					"crowns": 286,
					"experience": 1040590,
					"hiddenRating": 0,
					"isOnline": false,
					"lastSeenDate": "2025-12-16T22:46:22",
					"skin": "SKIN123",
					"nativePlatformName": "android",
					"ranked": {
						"currentSeasonId": "LIVE_RANKED_SEASON_17",
						"currentRankId": 0,
						"currentTierIndex": 0
					},
					"flags": 0
				},
				"role": 0,
				"clanXpContribution": 0
			},
			{
				"userProfile": {
					"userId": 193589871,
					"userName": "<#0bf>Dasw",
					"title": "",
					"country": "BR",
					"trophies": 176740,
					"crowns": 5232,
					"experience": 242920,
					"hiddenRating": 0,
					"isOnline": true,
					"lastSeenDate": "2025-12-17T02:26:11",
					"skin": "SKIN958",
					"nativePlatformName": "steam",
					"ranked": {
						"currentSeasonId": "LIVE_RANKED_SEASON_17",
						"currentRankId": 0,
						"currentTierIndex": 0
					},
					"flags": 2
				},
				"role": 0,
				"clanXpContribution": 0
			},
			{
				"userProfile": {
					"userId": 522721729,
					"userName": "<#111>Drakkeo",
					"title": "",
					"country": "BR",
					"trophies": 61955,
					"crowns": 891,
					"experience": 1214550,
					"hiddenRating": 0,
					"isOnline": false,
					"lastSeenDate": "2025-12-16T22:20:05",
					"skin": "SKIN1082",
					"nativePlatformName": "steam",
					"ranked": {
						"currentSeasonId": "LIVE_RANKED_SEASON_17",
						"currentRankId": 0,
						"currentTierIndex": 0
					},
					"flags": 2
				},
				"role": 2,
				"clanXpContribution": 4980
			},
			{
				"userProfile": {
					"userId": 736121978,
					"userName": "<#B4F>froyd",
					"title": "",
					"country": "BR",
					"trophies": 125225,
					"crowns": 2646,
					"experience": 4259180,
					"hiddenRating": 0,
					"isOnline": false,
					"lastSeenDate": "2025-12-16T22:23:48",
					"skin": "SKIN1074",
					"nativePlatformName": "steam",
					"ranked": {
						"currentSeasonId": "LIVE_RANKED_SEASON_17",
						"currentRankId": 0,
						"currentTierIndex": 0
					},
					"flags": 2
				},
				"role": 1,
				"clanXpContribution": 9660
			}
		],
		"currentStreak": 0,
		"nextNameChangeIsFree": false,
		"name": "Devs",
		"tag": "DEV",
		"logo": {
			"backgroundId": "option1",
			"foregroundId": "option3",
			"colourSchemeId": "option10",
			"level": 3
		},
		"memberCount": 4,
		"memberCapacity": 50,
		"joinPolicy": 2,
		"region": "SA",
		"language": "pt",
		"clanXP": 14640,
		"clanCreatedAtMs": 1754057015300,
		"clanCreatedAt": "2025-08-01T14:03:35.300Z"
	},
	"errors": [],
	"status": 200
}
```

## Response Fields

The response follows the standard [Clan Schema](/clans/schema). See the schema documentation for complete field descriptions.

### Clan Data

Main clan object. See [Clan Schema](/clans/schema) for all fields and nested objects.

### Nested Objects

- `logo` - See [Clan Schema - logo](/clans/schema#logo)
- `members[]` - Array of clan members. Each member contains:
  - `userProfile` - Simplified player profile (see [Player Schema](/players/schema))
  - `role` - See [Clan Schema - role](/clans/schema#role)
  - `clanXpContribution` - See [Clan Schema - clanXpContribution](/clans/schema#clanxpcontribution)

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Parameter 'clanId' is required.",
  "data": {},
  "errors": ["Missing 'clanId'"],
  "status": 400
}
```

### 404 - Clan Not Found

```json
{
  "success": false,
  "message": "Clan not found",
  "data": {},
  "errors": ["Clan not found"],
  "status": 404
}
```

## Notes

- The `clanId` must be a valid ULID (e.g., "01K1JZ400449Q1TZERE9E719Y1")
- The member list includes complete profiles for each player
- The `role` field indicates the member's position in the clan:
  - `0` = Regular member
  - `1` = Administrator
  - `2` = Clan leader
- The `clanXpContribution` shows how much XP each member contributed to the clan
- The `description` field may be empty if the clan has no description set
- The `joinPolicy` can have different values:
  - `0` = Anyone can join
  - `1` = Closed (requires approval)
  - `2` = Invite only
