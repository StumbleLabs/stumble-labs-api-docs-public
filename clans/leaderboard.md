# Clan Leaderboard

Returns a ranked leaderboard of clans (top 100), built from the latest clan snapshots. Optionally sorted by a metric and filtered by region.

> See also: [Search Clans](/clans/search) | [Recommended Clans](/clans/recommended) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/leaderboard
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `sort` | string | No | `crowns` | Sort metric. One of: `crowns`, `trophies` |
| `region` | string | No | — | Restrict results to a region (e.g., `SA`, `EU`, `US`) |

## Request Example

```bash
# Top clans by crowns (default)
curl -G "https://api.stumblelabs.net/api/clans/leaderboard" \
  -H "x-api-key: your-api-key-here"

# Top clans by trophies in South America
curl -G "https://api.stumblelabs.net/api/clans/leaderboard" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "sort=trophies" \
  --data-urlencode "region=SA"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Clan leaderboard by crowns fetched successfully.",
  "data": {
    "sort": "crowns",
    "region": null,
    "items": [
      {
        "rank": 1,
        "clanId": "01K1NDWDD2YPVX69VGBB0X0JPP",
        "name": "stumble stumble",
        "tag": "STS",
        "region": "SA",
        "memberCount": 50,
        "logo": {
          "backgroundId": "option1",
          "foregroundId": "option1",
          "colourSchemeId": "option01",
          "level": 2,
          "emblemUrl": "https://cdn.stumblelabs.net/emblems/..."
        },
        "totalCrowns": 124500,
        "totalTrophies": 8900000,
        "statsCapturedAt": "2025-06-20T00:00:00.000Z"
      }
    ],
    "total": 1,
    "page": 1,
    "pageSize": 100,
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
| `sort` | string | The metric used to rank (`crowns` or `trophies`) |
| `region` | string\|null | Region filter applied (`null` when unfiltered) |
| `items` | array | Ranked clan entries (up to 100) |
| `total` | integer | Number of entries returned |
| `page` | integer | Always `1` (single-page result) |
| `pageSize` | integer | Always `100` (the leaderboard limit) |
| `totalPages` | integer | Always `1` |
| `hasNext` | boolean | Always `false` |
| `hasPrev` | boolean | Always `false` |

### items[]

| Field | Type | Description |
|-------|------|-------------|
| `rank` | integer | Position in the leaderboard (1-based) |
| `clanId` | string | Clan ID (ULID) |
| `name` | string | Clan name |
| `tag` | string\|null | Clan tag |
| `region` | string\|null | Clan region |
| `memberCount` | integer | Member count at last snapshot |
| `logo` | object | Clan logo, including `emblemUrl` |
| `totalCrowns` | integer | Aggregated clan crowns |
| `totalTrophies` | integer | Aggregated clan trophies |
| `statsCapturedAt` | string\|null | Timestamp of the snapshot the stats came from (ISO 8601) |

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `sort` not in `crowns`/`trophies`, or `region` is an empty string |

## Notes

- The leaderboard is derived from the **latest snapshot per clan**; clans without snapshots do not appear.
- Results are capped at 100 entries and cached server-side (~5 minutes).
