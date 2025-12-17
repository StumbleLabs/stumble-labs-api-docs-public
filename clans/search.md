# Search Clans

Search for clans that match the provided name.

> See also: [Get Clan by ID](/clans/get-by-id) | [Authentication](/authentication)

## Endpoint

```http
POST /clans/search
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Request Body

```json
{
  "name": "stumble"
}
```

## Request Example

```bash
curl -X POST "https://api.stumblelabs.net/api/clans/search" \
  -H "x-api-key: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{"name": "stumble"}'
```

## Success Response (200)

```json
{
	"success": true,
	"message": "Clans found successfully.",
	"data": {
		"results": [
			{
				"clanId": "01K1NDWDD2YPVX69VGBB0X0JPP",
				"name": "stumble stumble",
				"tag": "STS",
				"logo": {
					"backgroundId": "option1",
					"foregroundId": "option1",
					"colourSchemeId": "option01",
					"level": 2
				},
				"memberCount": 50,
				"memberCapacity": 50,
				"joinPolicy": 0,
				"region": "SA",
				"language": "pt-BR",
				"clanXP": 8055,
				"clanCreatedAtMs": 1754139604386,
				"clanCreatedAt": "2025-08-02T13:00:04.386Z"
			}
		]
	},
	"errors": [],
	"status": 200
}
```

## Response Fields

The `results` array contains simplified clan objects. Each clan follows the standard [Clan Schema](/clans/schema), but without the `members[]` array. See the schema documentation for complete field descriptions.
