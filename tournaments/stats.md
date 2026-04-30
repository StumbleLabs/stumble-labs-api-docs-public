# Classic Tournament Stats

Aggregated statistics over the classic tournament archive. Accepts the same
filters as the [list endpoint](/tournaments/list), so you can scope the totals
(e.g. only season 17, only EU, only cash).

> See also: [List Tournaments](/tournaments/list) | [Authentication](/authentication)

## Endpoint

```http
GET /tournaments/classic/stats
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Query Parameters

Same filter set as [`GET /tournaments/classic`](/tournaments/list#filters):
`status`, `statusLabel`, `season`, `seasonPart`, `modeLabel`, `partySize`,
`region`, `isCash`, `isFinished`, `hasWinners`, `minPrizePool`, `maxPrizePool`,
`sinceDate`, `untilDate`, `search`.

Pagination and sorting parameters are ignored.

## Request Example

```bash
curl -G "https://api.stumblelabs.net/api/tournaments/classic/stats" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "season=17" \
  --data-urlencode "isCash=true"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "totals": {
      "total": 1342,
      "totalPrizePool": 12500000,
      "cashTournaments": 84,
      "finishedTournaments": 1300,
      "cancelledTournaments": 42,
      "avgFillRate": 0.873
    },
    "byRegion": [
      { "region": "sa", "count": 612, "prizePool": 5200000 },
      { "region": "eu", "count": 481, "prizePool": 4100000 },
      { "region": "us", "count": 249, "prizePool": 3200000 }
    ],
    "byMode": [
      { "modeLabel": "1v1", "count": 820, "prizePool": 7400000 },
      { "modeLabel": "2v2",  "count": 522, "prizePool": 5100000 }
    ]
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

### totals

| Field | Type | Description |
|-------|------|-------------|
| `total` | integer | Total tournaments matching filters |
| `totalPrizePool` | integer | Sum of `total_prize_pool` |
| `cashTournaments` | integer | Count where `is_cash = true` |
| `finishedTournaments` | integer | Count where `is_finished = true` |
| `cancelledTournaments` | integer | Count where `is_cancelled = true` |
| `avgFillRate` | number \| null | Average fill rate (0.0 - 1.0) |

### byRegion[] / byMode[]

| Field | Type | Description |
|-------|------|-------------|
| `region` / `modeLabel` | string \| null | Group key |
| `count` | integer | Tournaments in this group |
| `prizePool` | integer | Sum of `total_prize_pool` for this group |

Both arrays are sorted by `count DESC`.
