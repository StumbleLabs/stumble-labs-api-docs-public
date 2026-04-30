# Latest Assets

Returns the most recently added assets, optionally filtered by type. Useful for
"new this week" sections.

> See also: [Asset Schema](/assets/schema) | [List by Type](/assets/by-type) | [Authentication](/authentication)

## Endpoint

```http
GET /live/assets/latest
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `limit` | integer | No | `4` | How many to return (max `500`) |
| `type` | string | No | — | Restrict to one type: `skins`, `footsteps`, `emotes`, `animations` |

## Request Example

```bash
# 10 latest from any type
curl -G "https://api.stumblelabs.net/api/live/assets/latest" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "limit=10"

# 5 latest skins
curl -G "https://api.stumblelabs.net/api/live/assets/latest" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "limit=5" \
  --data-urlencode "type=skins"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [
      {
        "ID": "SKIN1376",
        "FriendlyName": "Cosmic Pup",
        "Hidden": false,
        "NoGacha": false,
        "Version": "0.93.5",
        "Rarity": "LEGENDARY",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin1376/preview.png"
      }
    ],
    "count": 1
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `items` | array | Asset objects, see [Asset Schema](/assets/schema) |
| `count` | integer | Number of items returned (≤ `limit`) |

> Unlike paginated endpoints, this one returns a flat `{ items, count }`
> shape — there's no `total` / `page` envelope.

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `limit` not a positive integer or above `500`, or `type` invalid |
