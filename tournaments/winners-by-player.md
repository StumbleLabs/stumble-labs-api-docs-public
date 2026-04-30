# Tournaments Won by Player

Returns the paginated list of classic tournaments where the given player is in
the **winning team** (i.e. appears in `winners.list[]`).

> See also: [Winners Leaderboard](/tournaments/leaderboard-winners) | [Tournament Schema](/tournaments/schema) | [Authentication](/authentication)

## Endpoint

```http
GET /tournaments/classic/winners/:identifier
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `identifier` | string | Yes | Either a `userid` or an `internal_id` (exactly 18 digits) |

The format is detected automatically:

| Pattern | Interpreted as |
|---------|----------------|
| `^\d{1,10}$` | `userid` |
| `^\d{18}$` | `internal_id` |
| anything else | `400 Bad Request` |

## Query Parameters

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `page` | integer | `1` | 1-based page number |
| `pageSize` | integer | `25` | Items per page (max `100`) |

## Request Example

```bash
# By userid
curl "https://api.stumblelabs.net/api/tournaments/classic/winners/193589871" \
  -H "x-api-key: your-api-key-here"

# By internal_id
curl "https://api.stumblelabs.net/api/tournaments/classic/winners/587639053331996716" \
  -H "x-api-key: your-api-key-here"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "identifier": "193589871",
    "matchedBy": "userid",
    "items": [ /* Tournament objects, see schema */ ],
    "total": 12,
    "page": 1,
    "pageSize": 25,
    "totalPages": 1,
    "hasNext": false,
    "hasPrev": false
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `identifier` | string | Echo of the path parameter |
| `matchedBy` | string | `userid` or `internal_id` (which column was matched) |
| `items` | array | Tournament objects, see [Tournament Schema](/tournaments/schema) |

Plus the standard pagination envelope (`total`, `page`, `pageSize`,
`totalPages`, `hasNext`, `hasPrev`).

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `identifier` missing or doesn't match the expected format |

## Notes

- Sorted by `tournament_time DESC NULLS LAST` (most recent wins first).
- Only tournaments where the player was on the **winner team** are returned.
  This endpoint does not list tournaments the player merely participated in.
