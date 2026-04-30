# List Classic Tournaments

Paginated list of classic tournaments with rich filtering and sorting.

> See also: [Tournament Schema](/tournaments/schema) | [Get by ID](/tournaments/get-by-id) | [Authentication](/authentication)

## Endpoint

```http
GET /tournaments/classic
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

All parameters are optional.

### Pagination

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `page` | integer | `1` | 1-based page number |
| `pageSize` | integer | `25` | Items per page (max `100`) |

### Filters

| Param | Type | Description |
|-------|------|-------------|
| `status` | integer | Raw status code |
| `statusLabel` | string | `finished` or `cancelled` (preferred over `status`) |
| `season` | integer | Filter by season number |
| `seasonPart` | integer | Filter by season part |
| `modeLabel` | string | Exact match on game mode (e.g. `2v2`) |
| `partySize` | integer | `1` for solo, `>1` for teams |
| `region` | string | Matches `primary_region` (case-insensitive) **or** entry in `regions[]` |
| `isCash` | boolean | Filter cash tournaments |
| `isFinished` | boolean | Only finished |
| `hasWinners` | boolean | Only tournaments with ingested winner data |
| `minPrizePool` | integer | Min `total_prize_pool` |
| `maxPrizePool` | integer | Max `total_prize_pool` |
| `sinceDate` | ISO 8601 | `tournament_time >=` filter |
| `untilDate` | ISO 8601 | `tournament_time <=` filter |
| `search` | string | Full-text search on tournament name (max 100 chars) |

### Sorting

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `sortBy` | string | `tournament_time` | One of: `tournament_time`, `invitation_opens`, `fill_rate`, `total_prize_pool`, `invites` |
| `order` | string | `desc` | `asc` or `desc` |

## Request Example

```bash
curl -G "https://api.stumblelabs.net/api/tournaments/classic" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "season=17" \
  --data-urlencode "isCash=true" \
  --data-urlencode "page=1" \
  --data-urlencode "pageSize=10" \
  --data-urlencode "sortBy=total_prize_pool" \
  --data-urlencode "order=desc"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [ /* Tournament objects, see schema */ ],
    "total": 1342,
    "page": 1,
    "pageSize": 10,
    "totalPages": 135,
    "hasNext": true,
    "hasPrev": false
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

The `items` array follows the [Tournament Schema](/tournaments/schema). Pagination
envelope fields (`total`, `page`, `pageSize`, `totalPages`, `hasNext`, `hasPrev`)
are standard across all paginated endpoints.

## Notes

- The `search` parameter supports phrases, `OR`, and `-exclusion` syntax
  (web-style search).
- `region` matches both the tournament's primary region and any region in its
  full region list — passing `EU` returns tournaments where EU is primary or
  among the available regions.
