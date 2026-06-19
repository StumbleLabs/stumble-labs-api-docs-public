# Clan Snapshots

Returns paginated historical snapshots of a clan's state over time (newest first), optionally bounded by a date range.

> See also: [Get Clan by ID](/clans/get-by-id) | [Member History](/clans/member-history) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/{clanId}/snapshots
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `clanId` | string | Yes | Clan ID (26-character Crockford Base32 ULID) |

## Query Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `from` | string | No | — | Lower bound for `capturedAt` (ISO 8601 date) |
| `to` | string | No | — | Upper bound for `capturedAt` (ISO 8601 date) |
| `page` | integer | No | `1` | 1-based page number |
| `pageSize` | integer | No | `50` | Items per page (max `500`) |

## Request Example

```bash
curl -G "https://api.stumblelabs.net/api/clans/01K1JZ400449Q1TZERE9E719Y1/snapshots" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "from=2025-06-01T00:00:00Z" \
  --data-urlencode "to=2025-06-30T23:59:59Z"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Clan snapshots fetched successfully.",
  "data": {
    "items": [
      {
        "capturedAt": "2025-06-20T00:00:00.000Z",
        "memberCount": 50,
        "memberCapacity": 50,
        "clanXP": 14640,
        "currentStreak": 0,
        "joinPolicy": 2,
        "aggregates": {
          "avgTrophies": 178000,
          "avgCrowns": 2490,
          "avgHiddenRating": 0,
          "totalTrophies": 8900000,
          "totalCrowns": 124500,
          "onlineCount": 12,
          "platforms": {
            "steam": 28,
            "android": 18,
            "ios": 4
          }
        }
      }
    ],
    "total": 2,
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

Standard pagination envelope. The `items` array contains one entry per captured point in time, ordered by `capturedAt` descending.

### items[]

| Field | Type | Description |
|-------|------|-------------|
| `capturedAt` | string\|null | Snapshot timestamp (ISO 8601) |
| `memberCount` | integer | Member count at capture time |
| `memberCapacity` | integer | Member capacity at capture time |
| `clanXP` | integer | Total clan XP at capture time |
| `currentStreak` | integer | Win streak at capture time |
| `joinPolicy` | integer | Join policy at capture time, see [Clan Schema - Join Policy](/clans/schema#join-policy) |
| `aggregates` | object | Aggregated member stats at capture time (see below) |

### aggregates

| Field | Type | Description |
|-------|------|-------------|
| `avgTrophies` | integer | Average trophies across members |
| `avgCrowns` | integer | Average crowns across members |
| `avgHiddenRating` | integer | Average hidden rating across members |
| `totalTrophies` | integer | Sum of member trophies |
| `totalCrowns` | integer | Sum of member crowns |
| `onlineCount` | integer | Number of members online at capture time |
| `platforms` | object | Member counts keyed by platform (e.g., `steam`, `android`, `ios`, `unknown`) |

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `clanId` not a valid ULID, or `from`/`to` not valid ISO 8601 dates |

## Notes

- Both `from` and `to` are optional. Provide one, both, or neither.
- The `aggregates` object holds the rolled-up member metrics used by the [Clan Leaderboard](/clans/leaderboard) (`totalCrowns`, `totalTrophies`).
- An unknown or never-snapshotted clan returns `200` with an empty `items` array (not a 404).
