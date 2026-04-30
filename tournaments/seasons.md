# List Classic Seasons

Lists all distinct `(season, season_part)` pairs in the classic tournament
archive, with totals and date windows. Use it to populate dropdowns without
scanning the full dataset.

> See also: [List Tournaments](/tournaments/list) | [Authentication](/authentication)

## Endpoint

```http
GET /tournaments/classic/seasons
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Request Example

```bash
curl "https://api.stumblelabs.net/api/tournaments/classic/seasons" \
  -H "x-api-key: your-api-key-here"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "seasons": [
      {
        "season": 17,
        "seasonPart": 2,
        "tournamentCount": 412,
        "firstTournament": "2026-03-01T00:00:00.000Z",
        "lastTournament": "2026-04-29T22:00:00.000Z"
      },
      {
        "season": 17,
        "seasonPart": 1,
        "tournamentCount": 388,
        "firstTournament": "2026-01-05T00:00:00.000Z",
        "lastTournament": "2026-02-28T22:00:00.000Z"
      }
    ],
    "count": 2
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `seasons[].season` | integer | Season number |
| `seasons[].seasonPart` | integer \| null | Season part |
| `seasons[].tournamentCount` | integer | Total tournaments in this season part |
| `seasons[].firstTournament` | ISO string \| null | Earliest tournament time |
| `seasons[].lastTournament` | ISO string \| null | Latest tournament time |
| `count` | integer | Total distinct season parts returned |

## Notes

- Sorted by `season DESC, seasonPart DESC` — newest first.
- Tournaments without a season are excluded.
