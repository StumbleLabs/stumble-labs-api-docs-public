# List Assets by Type

Paginated list of all assets of a given type (skins, emotes, animations,
footsteps).

> See also: [Asset Schema](/assets/schema) | [Search Assets](/assets/search-by-name) | [Authentication](/authentication)

## Endpoint

```http
GET /live/assets/type/:type
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | One of: `skins`, `footsteps`, `emotes`, `animations` |

## Query Parameters

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `page` | integer | `1` | 1-based page number |
| `pageSize` | integer | `50` | Items per page (max `500`) |

## Request Example

```bash
curl -G "https://api.stumblelabs.net/api/live/assets/type/skins" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "page=1" \
  --data-urlencode "pageSize=20"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [ /* Asset objects, see schema */ ],
    "total": 1376,
    "page": 1,
    "pageSize": 20,
    "totalPages": 69,
    "hasNext": true,
    "hasPrev": false
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

The `items` array follows the [Asset Schema](/assets/schema). Pagination
envelope fields (`total`, `page`, `pageSize`, `totalPages`, `hasNext`, `hasPrev`)
are standard across all paginated endpoints.

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `type` is missing or not one of the allowed values |
