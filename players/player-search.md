# Search Players by Name

Search for players whose username matches a query string. Returns lightweight results (username, skin, user ID). Use the returned `userid` with [Search Player](/players/search) or [Get Player Profiles (Batch)](/players/profiles) to fetch full profile data.

> See also: [Search Player](/players/search) | [Get Player Profiles (Batch)](/players/profiles)

## Endpoint

```http
GET /live/users/search/player-search
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `string` | string | Yes | Search query. Case-insensitive substring match on the username. **Minimum 3 characters.** |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/live/users/search/player-search?string=wallid" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/users/search/player-search?string=wallid', {
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

url = "https://api.stumblelabs.net/api/live/users/search/player-search"
headers = {"x-api-key": "your-api-key-here"}
params = {"string": "wallid"}

response = requests.get(url, headers=headers, params=params)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
	"success": true,
	"message": "Player data fetched successfully.",
	"data": [
		{
			"username": "wallid",
			"userid": 515204265,
			"skin": "https://cdn.stumblelabs.net/skins/skin493/preview.png",
			"profileUrl": "/profile/wallid"
		},
		{
			"username": "<#E08>Wallid",
			"userid": 117797443,
			"skin": "https://cdn.stumblelabs.net/skins/skin1404/preview.png",
			"profileUrl": "/profile/%3C%23E08%3EWallid"
		}
	],
	"status": 200
}
```

## Response Fields

`data` is an **array** of up to **12** matching players (most relevant first). Each entry is a lightweight result:

| Field | Type | Description |
|-------|------|-------------|
| `username` | string | Username (may contain HTML color codes, e.g. `<#E08>`) |
| `userid` | integer | Unique player ID — use it with [Search Player](/players/search) or [Get Player Profiles (Batch)](/players/profiles) |
| `skin` | string | Full preview image URL of the player's current skin |
| `profileUrl` | string | Relative, URL-encoded profile path on the StumbleLabs website |

## Errors

### 400 - Invalid Search String

Returned when `string` is missing or shorter than 3 characters.

```json
{
  "success": false,
  "message": "Search string must be at least 3 characters long",
  "data": [],
  "errors": ["Invalid search string"],
  "status": 400
}
```

## Notes

- The query must be at least **3 characters** long.
- Matching is **case-insensitive** and matches the username as a substring.
- At most **12** results are returned; there is no pagination.
- This endpoint returns lightweight data only. To get trophies, crowns, rank, clan, etc., pass the returned `userid` to [Search Player](/players/search) (single) or [Get Player Profiles (Batch)](/players/profiles) (many at once).
- On success the response envelope omits the `errors` field (present only on errors).
