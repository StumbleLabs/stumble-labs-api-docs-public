# Map Version History

Returns the version history of a map. Versions are stored via snapshots of map searches made in StumbleLabs. To play a specific version of a map, you can use the `versionShareCode`.

> See also: [Search Map](/maps/search) | [Search by Author](/maps/author)

## Endpoint

```http
GET /maps/{mapShareCode}/versions
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mapShareCode` | string | Yes | Public map share code (format: "XXXX-XXXX-XXXX") |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/maps/4980-2256-0258/versions" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/maps/4980-2256-0258/versions', {
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

url = "https://api.stumblelabs.net/api/maps/4980-2256-0258/versions"
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
	"message": "Version history retrieved successfully.",
	"data": {
		"map": {
			"mapId": "b3c7f055-5aaf-43e8-8654-0361be08959b",
			"name": "iPad",
			"description": "Find the hidden trampoline to get to the finish line!",
			"type": "RACE",
			"authorUserId": 544628594
		},
		"versions": [
			{
				"versionId": "896105da-a552-49aa-9377-372d9ea38739",
				"versionShareCode": "0907-3002-5681",
				"version": 106718224,
				"changes": {
					"name": null,
					"description": null,
					"type": null
				},
				"stats": {
					"totalProps": 164,
					"uniqueProps": 164
				}
			}
		],
		"totalVersions": 1
	},
	"errors": [],
	"status": 200
}
```

## Response Fields

### data.map

Basic map information. This follows a simplified version of the standard [Map Schema](/maps/schema) (only basic fields). See the schema documentation for complete field descriptions.

### data.versions[]

Array of map versions, ordered from newest to oldest.

| Field | Type | Description |
|-------|------|-------------|
| `versionId` | string | Unique version ID (UUID) |
| `versionShareCode` | string | This version's specific share code (format: "XXXX-XXXX-XXXX") |
| `version` | integer | Version number |
| `changes` | object | Changes relative to the previous version |
| `stats` | object | Version statistics |

### changes

Indicates which fields were changed in this version relative to the previous one.

| Field | Type | Description |
|-------|------|-------------|
| `name` | string \| null | New name (null if unchanged) |
| `description` | string \| null | New description (null if unchanged) |
| `type` | string \| null | New type (null if unchanged) |

### stats

Map version statistics.

| Field | Type | Description |
|-------|------|-------------|
| `totalProps` | integer | Total props/objects in the map |
| `uniqueProps` | integer | Number of unique props (no duplicates) |

### data.totalVersions

| Field | Type | Description |
|-------|------|-------------|
| `totalVersions` | integer | Total number of stored versions |

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["Invalid mapShareCode format"],
  "status": 400
}
```

### 404 - Map Not Found

```json
{
  "success": false,
  "message": "Verify if the map share code is the correct one (need to be the share code of the map not the version share code)",
  "data": {},
  "errors": ["Map not found"],
  "status": 404
}
```

### 404 - No Versions Found

```json
{
  "success": false,
  "message": "No versions found for this map.",
  "data": {},
  "errors": ["No versions found"],
  "status": 404
}
```

## Important Notes

- ⚠️ **Versions are stored via snapshots of map searches made in StumbleLabs**
- For a version to be stored, the map must be searched using the `/maps/search/{code}` endpoint
- The `mapShareCode` parameter must be the **map share code** (not the version share code)
- Use the `versionShareCode` to play a specific version of the map in Stumble Guys
- Share code format: `XXXX-XXXX-XXXX` (4 digits, hyphen, 4 digits, hyphen, 4 digits)
- Versions are returned ordered from newest to oldest
- The `changes` field only indicates changes relative to the previous version
- If `changes` contains only `null`, it means there were no changes to basic fields (there may have been changes to props)
- `totalProps` may be greater than `uniqueProps` if there are duplicate props in the map
- If no versions have been stored yet, the `versions` array will be empty

## Difference between Share Codes

| Type | Description | Usage |
|------|-------------|-------|
| `mapShareCode` | Public map share code | Identifies the map across all versions. Use to search the map or version history |
| `versionShareCode` | Specific version share code | Identifies a specific version. Use to play that exact version in the game |

## Usage Example

1. **Get version history:**
   ```bash
   GET /maps/4980-2256-0258/versions
   ```
   Returns all stored versions of the map.

2. **Play a specific version:**
   Use the returned `versionShareCode` (e.g., "0907-3002-5681") in the game to load that exact version.
