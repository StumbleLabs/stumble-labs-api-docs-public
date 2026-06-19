# Recommended Clans

Returns a list of recommended clans, optionally filtered by region and language.

> See also: [Search Clans](/clans/search) | [Clan Leaderboard](/clans/leaderboard) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/recommended
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `region` | string | No | — | Restrict recommendations to a region (e.g., `SA`, `EU`, `US`) |
| `language` | string | No | — | Restrict recommendations to a language (e.g., `pt`, `en`, `es`) |

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl -G "https://api.stumblelabs.net/api/clans/recommended" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "region=SA" \
  --data-urlencode "language=pt"
```

#### **JavaScript**

```javascript
const params = new URLSearchParams({ region: 'SA', language: 'pt' });
const response = await fetch(`https://api.stumblelabs.net/api/clans/recommended?${params}`, {
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

url = "https://api.stumblelabs.net/api/clans/recommended"
headers = {"x-api-key": "your-api-key-here"}
params = {"region": "SA", "language": "pt"}

response = requests.get(url, headers=headers, params=params)
data = response.json()
print(data)
```

<!-- tabs:end -->

## Success Response (200)

```json
{
  "success": true,
  "message": "Recommended clans retrieved successfully.",
  "data": {
    "recommendations": [
      {
        "clanId": "01K1NDWDD2YPVX69VGBB0X0JPP",
        "name": "stumble stumble",
        "tag": "STS",
        "logo": {
          "backgroundId": "option1",
          "foregroundId": "option1",
          "colourSchemeId": "option01",
          "level": 2,
          "emblemUrl": "https://cdn.stumblelabs.net/emblems/..."
        },
        "memberCount": 42,
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

The `recommendations` array contains simplified clan objects (without the `members[]` array). Each clan follows the standard [Clan Schema](/clans/schema), enriched with:

| Field | Type | Description |
|-------|------|-------------|
| `clanCreatedAtMs` | integer\|null | Creation timestamp (ms), decoded from the clan ULID |
| `clanCreatedAt` | string\|null | Creation date (ISO 8601), decoded from the clan ULID |
| `logo.emblemUrl` | string | Rendered emblem image URL derived from the logo fields |

> Additional fields may appear alongside `recommendations`.

## Error Responses

### 404 - No Recommendations

```json
{
  "success": false,
  "message": "No recommendations available at the moment.",
  "data": {},
  "errors": ["No recommendations found"],
  "status": 404
}
```

## Notes

- Results are cached server-side (~10 minutes) per `region`/`language` combination.
