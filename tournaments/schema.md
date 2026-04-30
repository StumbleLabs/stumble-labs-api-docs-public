# Classic Tournament Schema

Standard schema for the classic tournament objects returned by the
`/tournaments/classic/*` endpoints.

## Tournament Object Structure

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Tournament unique ID |
| `type` | integer | Tournament type identifier |
| `status` | integer | Status code (see [Status](#status)) |
| `schedule` | object | Schedule / timing fields |
| `flags` | object | Boolean flags (`isFinished`, `isCancelled`, `isCash`) |
| `format` | object | Format definition (party size, mode, phases, rounds) |
| `participation` | object | `maxInvites`, `invites`, `fillRate` |
| `branding` | object | `name`, `icon`, `image`, `themeColor` |
| `sponsor` | object \| null | Sponsor `name` and `image` (null when not sponsored) |
| `media` | object | `streamUrl`, `highlightsUrl` |
| `season` | object | `season` and `seasonPart` |
| `regions` | object | `primary` region + full `all` array |
| `entryFee` | object \| null | Entry fee details (null when free) |
| `prizes` | object | `totalPrizePool`, `currencyId`, `distribution[]` |
| `winners` | object | `hasData` flag + `list[]` (see [Winners](#winners)) |
| `maps` | object | Global map pool + per-tournament overrides |
| `emotes` | object | Disabled / enabled emote IDs and names |
| `extraProperties` | object \| null | Free-form metadata |

### schedule

| Field | Type | Description |
|-------|------|-------------|
| `tournamentTime` | ISO string | Tournament start time |
| `invitationOpens` | ISO string | When invitations open |
| `invitationCloses` | ISO string | When invitations close |
| `registrationWindowSeconds` | integer | Registration window duration |
| `timeToStartSeconds` | integer | Time to start after fill |
| `maxWaitTimeSeconds` | integer | Max wait time |

### format

| Field | Type | Description |
|-------|------|-------------|
| `partySize` | integer | Players per team (1 = solo, 2 = duos, ...) |
| `modeLabel` | string | Game mode label (e.g. `"2v2"`) |
| `phaseCount` | integer | Number of phases |
| `roundCount` | integer | Total number of rounds |
| `overrideMaxQualified` | integer \| null | Custom qualifier override |

### participation

| Field | Type | Description |
|-------|------|-------------|
| `maxInvites` | integer | Maximum number of invitations |
| `invites` | integer | Actual invitations issued |
| `fillRate` | number | Ratio between `0.0` and `1.0` (`invites / maxInvites`) |

### branding

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Tournament display name |
| `icon` | string \| null | Icon image URL |
| `image` | string \| null | Banner image URL |
| `themeColor` | string \| null | Hex color **without** `#` (e.g. `"0000ff"`) |

### regions

| Field | Type | Description |
|-------|------|-------------|
| `primary` | string \| null | Primary region code, lowercase (e.g. `"us"`, `"eu"`, `"sa"`) |
| `all` | string[] | All region codes the tournament was available in |

### entryFee

`null` when the tournament is free. Otherwise:

| Field | Type | Description |
|-------|------|-------------|
| `amount` | integer | Quantity required to enter |
| `currencyId` | string | Currency ID |
| `externalId` | string | Currency external ID |
| `currencyName` | string | Human-readable currency name (e.g. `"gems"`) |

### prizes

| Field | Type | Description |
|-------|------|-------------|
| `totalPrizePool` | integer | Total prize pool across all positions |
| `currencyId` | string \| null | Main currency ID for the pool |
| `distribution[]` | array | Awarded positions (see below) |

#### prizes.distribution[]

Each entry awards one **placement bracket**. Positions are **not contiguous** —
they represent the lowest-placed player in the bracket. Example: a tournament
might pay positions `1`, `2`, `4`, `8`, `16`, `32`... meaning everyone from 3rd
to 4th place gets the position-4 reward, 5th–8th get the position-8 reward,
and so on.

| Field | Type | Description |
|-------|------|-------------|
| `position` | integer | Bracket cap (1 = winner) |
| `items[]` | array | Items awarded for this bracket |
| `items[].type` | string | Item type code |
| `items[].amount` | integer | Quantity |
| `items[].currency_id` | string | Currency ID |
| `items[].external_id` | string | External currency ID |
| `items[].currency_name` | string | Human-readable currency name |

### winners

`winners.list[]` is the **winning team** (position 1). Length matches
`format.partySize`.

| Field | Type | Description |
|-------|------|-------------|
| `nick` | string | Display nickname |
| `userid` | integer | Player numeric ID |
| `internal_id` | string | 18-digit internal ID |

`winners.hasData` is `true` when the tournament has ingested winner data.

### maps

| Field | Type | Description |
|-------|------|-------------|
| `globalPoolIds` | string[] | Map IDs in the global rotation pool |
| `globalPoolNames` | string[] | Friendly names matching `globalPoolIds` |
| `overrides[]` | array | Per-phase / per-round map overrides (see below) |
| `hasOverrides` | boolean | `true` if `overrides[]` is non-empty |

#### maps.overrides[]

| Field | Type | Description |
|-------|------|-------------|
| `phase` | integer \| null | Phase number (null if not phase-specific) |
| `round` | integer \| null | Round number (null if applies to whole phase) |
| `map_id` | string | Map identifier |
| `map_name` | string | Friendly map name |

### emotes

| Field | Type | Description |
|-------|------|-------------|
| `disabledIds` | string[] | IDs of emotes disabled in the tournament |
| `disabledNames` | string[] | Friendly names matching `disabledIds` |
| `enabledNames` | string[] | Friendly names of emotes explicitly enabled |

## Status

| Value | Label |
|-------|-------|
| `3` | finished |
| `4` | cancelled |

## Example

```json
{
  "id": "825785997152232522",
  "type": 1,
  "status": 3,
  "schedule": {
    "tournamentTime": "2024-03-30T01:00:00.000Z",
    "invitationOpens": "2024-03-30T00:00:00.000Z",
    "invitationCloses": "2024-03-30T00:58:00.000Z",
    "registrationWindowSeconds": 3480,
    "timeToStartSeconds": 120,
    "maxWaitTimeSeconds": 30
  },
  "flags": { "isFinished": true, "isCancelled": false, "isCash": false },
  "format": {
    "partySize": 2,
    "modeLabel": "2v2",
    "phaseCount": 3,
    "roundCount": 28,
    "overrideMaxQualified": 1
  },
  "participation": { "maxInvites": 30000, "invites": 1984, "fillRate": 0.0661 },
  "branding": {
    "name": "SMOREKYLE",
    "icon": "IMAGE_URL_HERE",
    "image": null,
    "themeColor": "0000ff"
  },
  "sponsor": null,
  "media": {
    "streamUrl": "https://www.twitch.tv/smorekyle",
    "highlightsUrl": null
  },
  "season": { "season": 26, "seasonPart": 1 },
  "regions": { "primary": "us", "all": ["us"] },
  "entryFee": {
    "amount": 20,
    "currencyId": "386434335026193365",
    "externalId": "4",
    "currencyName": "gems"
  },
  "prizes": {
    "totalPrizePool": 34990,
    "currencyId": "386434335026193365",
    "distribution": [
      {
        "position": 1,
        "items": [
          {
            "type": "10",
            "amount": 16600,
            "currency_id": "386434335026193365",
            "external_id": "4",
            "currency_name": "gems"
          }
        ]
      },
      {
        "position": 2,
        "items": [
          {
            "type": "10",
            "amount": 8740,
            "currency_id": "386434335026193365",
            "external_id": "4",
            "currency_name": "gems"
          }
        ]
      },
      {
        "position": 4,
        "items": [
          {
            "type": "10",
            "amount": 4600,
            "currency_id": "386434335026193365",
            "external_id": "4",
            "currency_name": "gems"
          }
        ]
      }
    ]
  },
  "winners": {
    "hasData": true,
    "list": [
      { "nick": "Gmz KnG:/",           "userid": 51608703,  "internal_id": "520743233312008428" },
      { "nick": "Rtk KnG :/<#7CF>[W]", "userid": 321745936, "internal_id": "622875044481082963" }
    ]
  },
  "maps": {
    "globalPoolIds": [],
    "globalPoolNames": [],
    "overrides": [
      { "phase": 1, "round": null, "map_id": "level21_pillar",               "map_name": "Lava Land" },
      { "phase": 2, "round": null, "map_id": "eventlevel13_block_legendary", "map_name": "Legendary Block Dash" },
      { "phase": 3, "round": null, "map_id": "level24_streamtiles",          "map_name": "Rush Hour" }
    ],
    "hasOverrides": true
  },
  "emotes": {
    "disabledIds":   ["8", "124", "55", "122"],
    "disabledNames": ["Hug", "Charged Hug", "Banana", "Golden Banana"],
    "enabledNames":  ["#1 Boss", "2024", "Aerial", "Angry Zongzi"]
  },
  "extraProperties": {}
}
```

> `prizes.distribution[]` and `emotes.*` arrays are **truncated** in the
> example above for readability — real responses contain the full lists.

## Notes

- All timestamps are ISO 8601 (UTC).
- `themeColor` is a hex color **without** the leading `#`.
- `regions.primary` and `regions.all[]` use lowercase codes.
- `winners.list` always represents the **champion team only**, not a top-N
  ranking. Per-player prize is approximately
  `prizes.distribution[position=1].items[0].amount / format.partySize`.
- `prizes.distribution[]` positions are bracket caps, not literal placements:
  e.g. `position: 4` rewards everyone placed 3rd or 4th.
- `extraProperties` may be an empty object (`{}`) or `null`.
