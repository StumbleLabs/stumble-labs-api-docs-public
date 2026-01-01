# Search Map

Search for a map using the public share code or version share code. Returns complete map information, including author data, current version, and metadata.

> See also: [Search by Author](/maps/author) | [Version History](/maps/versions) | [Authentication](/authentication)

## Endpoint

```http
GET /maps/search/{code}
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `code` | string | Yes | Map share code or version share code (e.g., "2051-0533-6189") |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/maps/search/2051-0533-6189" \
  -H "x-api-key: your-api-key-here"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/maps/search/2051-0533-6189', {
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

url = "https://api.stumblelabs.net/api/maps/search/2051-0533-6189"
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
  "message": "Map data fetched successfully.",
  "data": {
    "mapId": "d2667c74-54ae-4a56-bdc8-47aa5db426b8",
    "mapShareCode": "2051-0533-6189",
    "name": "<#FF0>BRICK <#fff> so que diferente 🤡💀",
    "description": "dasw test",
    "type": "RACE",
    "authorUserId": 193589871,
    "thumbnails": {
      "detail": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/1d264c98-a642-46ca-a2d8-9ab47cd8250d_532x400.png?Expires=1766801203&Key-Pair-Id=K1ZWU1LRABT2S3&Signature=...",
      "list": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/592df3fc-ac35-4980-bd9a-50af6df514ea_532x400.png?Expires=1766801203&Key-Pair-Id=K1ZWU1LRABT2S3&Signature=..."
    },
    "latestVersion": null,
    "author": {
      "userId": 193589871,
      "username": "<#0bf>Dasw",
      "country": "BR",
      "trophies": 176740,
      "crowns": 5232,
      "skin": "SKIN958"
    },
    "version": {
      "current": 3134,
      "shareCode": "1724-2532-3225",
      "versionId": "b3e9fd7c-abd4-4017-b301-77686bf93174"
    },
    "metadata": {
      "minClientVersion": "",
      "stateFlags": 0,
      "customMapSchemaVersion": 0,
      "objects_version" : "2",
      "propsSummary": [
        {
          "name": "spawn",
          "type": 3001,
          "count": 1,
          "objectId": "763216006"
        },
        {
          "name": "Square",
          "type": 2021,
          "count": 1,
          "objectId": "1130366521"
        }
      ]
    }
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

The response follows the standard [Map Schema](/maps/schema). See the schema documentation for complete field descriptions.

### Map Data

Main map object. See [Map Schema](/maps/schema) for all fields and nested objects.

### Nested Objects

- `thumbnails` - See [Map Schema - thumbnails](/maps/schema#thumbnails)
- `author` - Simplified player profile (see [Player Schema](/players/schema))
- `version` - See [Map Schema - version](/maps/schema#version)
- `metadata` - See [Map Schema - metadata](/maps/schema#metadata)
- `propsSummary[]` - See [Map Schema - propsSummary](/maps/schema#propssummary)

## Map Types

See [Map Schema - Map Types](/maps/schema#map-types) for the complete list.

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["Invalid map share code format"],
  "status": 400
}
```

### 404 - Map Not Found

```json
{
  "success": false,
  "message": "Map not found",
  "data": {},
  "errors": ["Map not found"],
  "status": 404
}
```

## Notes

- The `code` parameter accepts both the **map share code** and the **version share code**
- The map share code is permanent and identifies the map across all versions
- The version share code is specific to a version and changes when the map is updated
- Share code format: `XXXX-XXXX-XXXX` (4 digits, hyphen, 4 digits, hyphen, 4 digits)
- The `name` field may contain HTML color codes (e.g., `<#FF0>` for yellow)
- Thumbnail URLs contain temporary signatures and expire after a period
- The `latestVersion` field may be `null` if no versions are registered
- The `propsSummary` lists all unique objects used in the map and their quantities
- The `stateFlags` is a bitmask indicating map states (published, private, etc.)
- The `customMapSchemaVersion` indicates the custom map data format version
