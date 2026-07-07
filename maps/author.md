# Search Maps by Author

> ⚠️ **Deprecated — UGC Maps (Stumble Workshop)**
>
> The in-game custom map creator — known in-game as **Stumble Workshop** and shown as **UGC Maps** on StumbleLabs — has been **removed from Stumble Guys**. This endpoint only returns **legacy data** kept for historical reference and is **no longer updated**. Users who never created maps return `404` ("No maps found for this author.").

Returns a list of maps previously searched in StumbleLabs related to a specific user. This indicates that the returned maps were previously searched and associated with the user.

> See also: [Search Map](/maps/search) | [Version History](/maps/versions)

## Endpoint

```http
GET /maps/author/{authorUserId}
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `authorUserId` | integer | Yes | Author/user ID |

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl -X GET "https://api.stumblelabs.net/api/maps/author/193589871" \
  -H "x-api-key: your-api-key-here"
```

#### **JavaScript**

```javascript
const response = await fetch('https://api.stumblelabs.net/api/maps/author/193589871', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key-here'
  }
});

const data = await response.json();
console.log(data);
```

#### **Python**

```python
import requests

url = "https://api.stumblelabs.net/api/maps/author/193589871"
headers = {
    "x-api-key": "your-api-key-here"
}

response = requests.get(url, headers=headers)
data = response.json()
print(data)
```

<!-- tabs:end -->

## Success Response (200)

```json
{
	"success": true,
	"message": "Maps retrieved successfully.",
	"data": {
		"items": [
			{
				"mapId": "d2667c74-54ae-4a56-bdc8-47aa5db426b8",
				"mapShareCode": "2051-0533-6189",
				"name": "<#FF0>BRICK <#fff> so que diferente 🤡💀",
				"description": "dasw test",
				"type": "RACE",
				"authorUserId": 193589871,
				"thumbnails": {
					"detail": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/1d264c98-a642-46ca-a2d8-9ab47cd8250d_532x400.png?Expires=1766801203&Key-Pair-Id=K1ZWU1LRABT2S3&Signature=HY0BKEcp3zLh0-25M3~H2RsR9Vb~bxeuK1zffeu7spB2vDFZjUa1PdHJiNvFyx1tRJr5BjfegbzHQ9L5V-9KksrAWorTTT2P2qvPwgqdJRX9MjQeqCDzUlP~Krt6KCNeyuQwgfg6uM8ik6oRyvjlcyTyl-8pbySZt1R~P5j~LI71GV2H9LDCMJAJOkEsBiVFxSabdtvvFW~LVz-b2URAJvJjV4eNdCLvkikDs9A8W07MOxgVVM4jJIhg6vI363t1GF86FcXifywVGTYJTBHrswR0dGMstW7cCLT4iFMvblp2jOGD0n4CT42chtHuNL~z81VTqnirzyfHfygzWT-vCg__",
					"list": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/592df3fc-ac35-4980-bd9a-50af6df514ea_532x400.png?Expires=1766801203&Key-Pair-Id=K1ZWU1LRABT2S3&Signature=Lp6FNQ9Tsx9iqIENrpIT89k4QWpguU~X7sv9Mqbz9HGzXsGbk~O~4zmGSn6H-E9dZoJ8uYEjFJFxtISA-9UkhvL3SDMfMfVmebYGdkI0raUdr8fVZs3meHFR3-edALe7ra5MkppTexM7nhlFxOl6yYvVPqfJd7RG4nuOngMoL2IT4Wp27KfpXgV5acWvQ0b-psOs5-YeQsQdBGJW6cuylbPoCEkaEJxWqsakO53cRcdVPWLlfHVkQvmcA1liWCtcT6v4FSrI7DEk-Jqv0oXX09~yGtEhGWuUcrfkNcTeiy141o3ZrIs0i6dWr0ukUMdJxa~6TfN-p~M~K0QDg7zUHg__"
				},
				"latestVersion": null
			}
		],
		"total": 1,
		"page": 1,
		"pageSize": 50,
		"totalPages": 1,
		"hasNext": false,
		"hasPrev": false
	},
	"errors": [],
	"status": 200
}
```

## Response Fields

The `data` object is **paginated**. The maps are inside the `items` array, alongside pagination metadata.

### data

| Field | Type | Description |
|-------|------|-------------|
| `items` | array | Array of map objects (see below) |
| `total` | integer | Total number of maps for this author |
| `page` | integer | Current page number (1-based) |
| `pageSize` | integer | Number of items per page |
| `totalPages` | integer | Total number of pages |
| `hasNext` | boolean | Whether a next page exists |
| `hasPrev` | boolean | Whether a previous page exists |

### data.items[]

Array of maps associated with the author. Each map follows a simplified version of the standard [Map Schema](/maps/schema) (without `author`, `version`, and `metadata` fields). See the schema documentation for complete field descriptions.

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Validation failed",
  "data": {},
  "errors": ["Invalid authorUserId"],
  "status": 400
}
```

### 404 - No Maps Found

```json
{
  "success": false,
  "message": "No maps found for this author.",
  "data": {},
  "errors": ["No maps found"],
  "status": 404
}
```

## Important Notes

- ⚠️ **This endpoint only returns maps that have been previously searched in StumbleLabs**
- Maps are associated with the author when they are first searched using the `/maps/search/{code}` endpoint
- If a map has never been searched, it will not appear in this list, even if the `authorUserId` matches
- The list may be empty if no maps by the author have been previously searched
- The `latestVersion` field may be `null` if no versions are registered
- Thumbnail URLs contain temporary signatures and expire after a period
- The `name` field may contain HTML color codes (e.g., `<#FF0>` for yellow)
- Maps are returned sorted by creation date (newest first)

## Difference between endpoints

| Endpoint | Description |
|----------|-------------|
| `/maps/search/{code}` | Search for a specific map by share code |
| `/maps/author/{authorUserId}` | List maps by author that have been previously searched |

To ensure a map appears in the author list, you must first search it using `/maps/search/{code}`.
