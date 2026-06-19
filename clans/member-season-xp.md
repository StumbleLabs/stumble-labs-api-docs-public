# Clan Member Season XP

Returns a member's XP contribution for a given clan season, derived from periodic snapshots. Optionally includes the full snapshot history.

> See also: [Get Clan by ID](/clans/get-by-id) | [Current Clan Season](/clans/season-current) | [Member History](/clans/member-history) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/{clanId}/members/{userId}/season-xp
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `clanId` | string | Yes | Clan ID (26-character Crockford Base32 ULID) |
| `userId` | integer | Yes | Player ID (positive Int32) |

## Query Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `seasonId` | string | Yes | — | Season identifier (e.g., `CLANS_LIVE_SEASON_6`) |
| `includeSnapshots` | boolean | No | `false` | When `true`, includes the `snapshots` array |

## Request Example

```bash
curl -G "https://api.stumblelabs.net/api/clans/01K1JZ400449Q1TZERE9E719Y1/members/193589871/season-xp" \
  -H "x-api-key: your-api-key-here" \
  --data-urlencode "seasonId=CLANS_LIVE_SEASON_6" \
  --data-urlencode "includeSnapshots=true"
```

## Success Response (200)

```json
{
  "success": true,
  "message": "Member season XP contribution fetched successfully.",
  "data": {
    "clanId": "01K1JZ400449Q1TZERE9E719Y1",
    "userId": 193589871,
    "userName": "<#0bf>Dasw",
    "seasonId": "CLANS_LIVE_SEASON_6",
    "totalXpContribution": 4980,
    "snapshotCount": 2,
    "firstCapturedAt": "2025-06-10T00:00:00.000Z",
    "lastCapturedAt": "2025-06-15T00:00:00.000Z",
    "trackingStartedAfterSeasonStart": true,
    "snapshots": [
      {
        "clanXpContribution": 3200,
        "capturedAt": "2025-06-10T00:00:00.000Z"
      },
      {
        "clanXpContribution": 4980,
        "capturedAt": "2025-06-15T00:00:00.000Z"
      }
    ]
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `clanId` | string | Clan ID (ULID) |
| `userId` | integer | Player ID |
| `userName` | string\|null | Player username (`null` if unknown locally) |
| `seasonId` | string | Season identifier |
| `totalXpContribution` | integer\|null | Highest recorded XP contribution for the season. `null` when no snapshots exist |
| `snapshotCount` | integer | Number of snapshots recorded |
| `firstCapturedAt` | string\|null | Timestamp of the first snapshot (ISO 8601) |
| `lastCapturedAt` | string\|null | Timestamp of the last snapshot (ISO 8601) |
| `trackingStartedAfterSeasonStart` | boolean\|null | `true` if the first snapshot was recorded more than a minute after the season started (i.e., early contribution may be missing) |
| `snapshots` | array | Snapshot history — present only when `includeSnapshots=true` |

### snapshots[]

| Field | Type | Description |
|-------|------|-------------|
| `clanXpContribution` | integer | Member's XP contribution at capture time |
| `capturedAt` | string | Snapshot timestamp (ISO 8601) |

## Notes

- `totalXpContribution` is the **maximum** contribution observed across snapshots, not a sum.
- When the member has no snapshots for the season, the response is still `200` with `totalXpContribution: null`, `snapshotCount: 0`, and the message *"No XP snapshots recorded for this member in the requested season."*

## Error Responses

### 404 - Season Not Found

```json
{
  "success": false,
  "message": "Clan season not found",
  "data": {},
  "errors": ["seasonId not found"],
  "status": 404
}
```

| Status | Reason |
|--------|--------|
| `400` | `clanId` not a valid ULID, `userId` not a positive Int32, or `seasonId` missing |
| `404` | The `seasonId` could not be resolved |
