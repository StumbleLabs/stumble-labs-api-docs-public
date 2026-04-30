# Classic Tournaments Winners Leaderboard

Player ranking aggregated from all classic tournament champion teams.

> See also: [Tournaments Won by Player](/tournaments/winners-by-player) | [Authentication](/authentication)

## Endpoint

```http
GET /tournaments/classic/leaderboard/winners
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
| `season` | integer | Restrict to a season |
| `seasonPart` | integer | Restrict to a season part |
| `modeLabel` | string | Exact match on game mode |
| `partySize` | integer | `1` for solo, `>1` for teams |
| `isCash` | boolean | Only cash tournaments |
| `sinceDate` | ISO 8601 | `tournament_time >=` filter |
| `untilDate` | ISO 8601 | `tournament_time <=` filter |

> Region filter is **not** available on this endpoint.

### Sorting

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `sortBy` | string | `wins` | One of: `wins`, `cashWins`, `soloWins`, `teamWins`, `estimatedPrize`, `lastWinAt` |
| `order` | string | `desc` | `asc` or `desc` |

Tie-breakers (always applied): `wins DESC`, then `estimated_prize DESC`.

## Request Example

```bash
curl -G "https://api.stumblelabs.net/api/tournaments/classic/leaderboard/winners" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "season=17" \
  --data-urlencode "isCash=true" \
  --data-urlencode "sortBy=estimatedPrize" \
  --data-urlencode "pageSize=10"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [
      {
        "userid": 193589871,
        "internalId": "587639053331996716",
        "nick": "Dasw",
        "wins": 12,
        "soloWins": 7,
        "teamWins": 5,
        "cashWins": 3,
        "estimatedPrize": 145000,
        "firstWinAt": "2025-09-12T18:00:00.000Z",
        "lastWinAt": "2026-04-30T20:00:00.000Z"
      }
    ],
    "total": 8421,
    "page": 1,
    "pageSize": 10,
    "totalPages": 843,
    "hasNext": true,
    "hasPrev": false
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `userid` | integer \| null | Player numeric ID (null on legacy rows) |
| `internalId` | string \| null | 18-digit internal ID |
| `nick` | string \| null | Most recent nickname seen |
| `wins` | integer | Total tournament wins |
| `soloWins` | integer | Wins where `party_size = 1` |
| `teamWins` | integer | Wins where `party_size > 1` |
| `cashWins` | integer | Wins where `is_cash = true` |
| `estimatedPrize` | integer | Sum of per-player prize shares (see [Prize Calculation](#prize-calculation)) |
| `firstWinAt` | ISO string \| null | Earliest win timestamp |
| `lastWinAt` | ISO string \| null | Most recent win timestamp |

Plus the standard pagination envelope.

## Prize Calculation

For each tournament won by the player, the prize share is:

```
prize_share = prize_distribution[position=1].items[0].amount / party_size
```

That is, the first-place pool of the **first listed item** (typically gems),
divided equally among the team members. `estimatedPrize` is the sum of all such
shares for the matching tournaments.

> Multi-currency rewards are not aggregated — only the first item of position 1
> contributes to `estimatedPrize`.
> Idk if my calc related to estimatedPrize is right but ok.

## Notes

- Players are grouped by `(userid, internalId)`. Legacy entries without
  `internalId` are grouped by `userid` alone.
