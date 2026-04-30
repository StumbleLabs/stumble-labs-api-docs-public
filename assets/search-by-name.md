# Search Assets by Name

Paginated text search across asset names and IDs. Optionally restrict the
search to a specific asset type.

> See also: [Asset Schema](/assets/schema) | [List by Type](/assets/by-type) | [Authentication](/authentication)

## Endpoint

```http
GET /live/assets/search
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `q` | string | Yes | — | Search term (max 100 chars). Matches against `FriendlyName` **or** `ID` (case-insensitive substring) |
| `type` | string | No | — | Restrict to one type: `skins`, `footsteps`, `emotes`, `animations` |
| `page` | integer | No | `1` | 1-based page number |
| `pageSize` | integer | No | `50` | Items per page (max `500`) |

## Request Example

```bash
# Search across all types
curl -G "https://api.stumblelabs.net/api/live/assets/search" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "q=dragon"

# Restrict to skins only
curl -G "https://api.stumblelabs.net/api/live/assets/search" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "q=dragon" \
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
        "ID": "SKIN2",
        "FriendlyName": "Ent",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.1",
        "Rarity": "EPIC",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin2/preview.png"
      }
    ],
    "total": 12,
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

The `items` array follows the [Asset Schema](/assets/schema). Pagination
envelope is standard across paginated endpoints.

## Notes

- Matches both `FriendlyName` and `ID` — searching for `SKIN905` works just
  like searching for the friendly name.
- Match is case-insensitive substring (no fuzzy matching).
- Use [Get Asset by ID](/assets/search) to retrieve a single asset with
  detailed purchase info and similar items.

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `q` missing/empty, or `type` not in allowed values |
