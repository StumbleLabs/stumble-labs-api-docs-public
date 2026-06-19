# Clan Member History

Returns the paginated membership event history for a clan (joins, leaves, and role changes), newest first. Can be filtered by player and/or event type.

> See also: [Get Clan by ID](/clans/get-by-id) | [Member Season XP](/clans/member-season-xp) | [Snapshots](/clans/snapshots) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/{clanId}/member-history
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
| `userId` | integer | No | — | Filter events for a single player (positive Int32) |
| `event` | string | No | — | Filter by event type. One of: `join`, `leave`, `role_change` |
| `page` | integer | No | `1` | 1-based page number |
| `pageSize` | integer | No | `50` | Items per page (max `500`) |

## Request Example

```bash
# All membership events for a clan
curl -G "https://api.stumblelabs.net/api/clans/01K1JZ400449Q1TZERE9E719Y1/member-history" \
  -H "x-api-key: your-api-key-here"

# Only role changes for a specific player
curl -G "https://api.stumblelabs.net/api/clans/01K1JZ400449Q1TZERE9E719Y1/member-history" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "userId=193589871" \
  --data-urlencode "event=role_change"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Clan member history fetched successfully.",
  "data": {
    "items": [
      {
        "userId": 193589871,
        "event": "role_change",
        "roleBefore": 0,
        "roleAfter": 1,
        "at": "2025-08-10T09:30:00.000Z"
      },
      {
        "userId": 193589871,
        "event": "join",
        "roleBefore": null,
        "roleAfter": null,
        "at": "2025-08-01T14:05:00.000Z"
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

Standard pagination envelope. The `items` array contains one entry per membership event, ordered by `at` descending.

### items[]

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | Player ID involved in the event |
| `event` | string | Event type: `join`, `leave`, or `role_change` |
| `roleBefore` | integer\|null | Previous role (typically only for `role_change`), see [Clan Schema - role](/clans/schema#role) |
| `roleAfter` | integer\|null | New role (typically only for `role_change`), see [Clan Schema - role](/clans/schema#role) |
| `at` | string | When the event occurred (ISO 8601) |

## Error Responses

| Status | Reason |
|--------|--------|
| `400` | `clanId` not a valid ULID, `userId` not a positive Int32, or `event` not in the allowed list |

## Notes

- An unknown or empty-history clan returns `200` with an empty `items` array (not a 404).
