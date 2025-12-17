# Search Player

Search for complete player data using userId or username.

> See also: [Authentication](/authentication) | [Error Handling](/errors)

## Endpoint

```http
POST /live/users/search
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `accountFrom` | string | No | Account origin identifier |

## Request Body

You can search by **userId** or **username**:

### By User ID

```json
{
  "userid": 235378638
}
```

### By Username

```json
{
  "username": "Blade :/<#FF0>[SG]"
}
```

## Request Example

### cURL

```bash
curl -X POST "https://api.stumblelabs.net/api/live/users/search" \
  -H "x-api-key: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "userid": 235378638
  }'
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/users/search', {
  method: 'POST',
  headers: {
    'x-api-key': 'your-api-key-here',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    userid: 235378638
  })
});

const data = await response.json();
console.log(data);
```

### Python (requests)

```python
import requests

url = "https://api.stumblelabs.net/api/live/users/search"
headers = {
    "x-api-key": "your-api-key-here",
    "Content-Type": "application/json"
}
payload = {
    "userid": 235378638
}

response = requests.post(url, json=payload, headers=headers)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
	"success": true,
	"message": "Player data fetched successfully.",
	"data": {
		"userId": 193589871,
		"userName": "<#0bf>Dasw",
		"country": "BR",
		"trophies": 176740,
		"crowns": 5232,
		"experience": 242920,
		"isOnline": false,
		"lastActivityInfo": {
			"lastInteraction": "2025-12-17T02:26:11",
			"lastSeenType": "unknown"
		},
		"skin": "SKIN958",
		"skinIcon": "https://cdn.stumblelabs.net/skins/skin958/preview.png",
		"skinInformation": {
			"ID": "SKIN958",
			"FriendlyName": "Inflatable Joe",
			"Hidden": false,
			"NoGacha": true,
			"Version": "0.69",
			"Rarity": "MYTHIC",
			"Category": "SKIN",
			"IconUrl": "https://cdn.stumblelabs.net/skins/skin958/preview.png"
		},
		"xpRoadInfo": {
			"level": 25,
			"currentXp": 242920,
			"requiredXpForCurrentLevel": 232700,
			"requiredXpForNextLevel": 252700,
			"progressToNextLevel": 51
		},
		"nativePlatformName": "steam",
		"ranked": {
			"id": 0,
			"name": "Unranked",
			"minScore": 0,
			"maxScore": 0,
			"icon": "https://s3.amazonaws.com/production-assetsbucket-8ljvyr1xczmb/765fe95c-162f-4f42-89c2-ab8129941709/Wood.png",
			"currentRankId": 0,
			"currentSeasonId": "LIVE_RANKED_SEASON_17",
			"currentTierIndex": 0
		},
		"usernameHistory": [
			"<#0bf>Dasw",
			"<#39e>dasw",
			"<#39e>dasw45"
		],
		"maps": [],
		"clan": {
			"clanId": "01K1JZ400449Q1TZERE9E719Y1",
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
			"clanXP": 14640
		},
		"isFallback": false,
		"isCache": false
	},
	"errors": [],
	"status": 200
}
```

## Response Fields

The response follows the standard [Player Schema](/players/schema). See the schema documentation for complete field descriptions.

### Player Data

Main player object. See [Player Schema](/players/schema) for all fields and nested objects.

### Nested Objects

- `lastActivityInfo` - See [Player Schema - lastActivityInfo](/players/schema#lastactivityinfo)
- `skinInformation` - See [Player Schema - skinInformation](/players/schema#skininformation) (follows [Asset Schema](/assets/schema))
- `xpRoadInfo` - See [Player Schema - xpRoadInfo](/players/schema#xproadinfo)
- `ranked` - See [Player Schema - ranked](/players/schema#ranked)
- `clan` - Simplified clan object (see [Clan Schema](/clans/schema) for full structure)

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["userid/userId or username required"],
  "status": 400
}
```

### 404 - Player Not Found

```json
{
  "success": false,
  "message": "Player not found at the moment, you can try again later.",
  "data": {},
  "errors": ["No player found with the provided userid or username"],
  "status": 404
}
```

## Notes

- Returned data may vary depending on your API key's `accessLevel`
- The `maps` field is deprecated and always returns an empty array
- The `clan` field can be `null` if the player is not in a clan
