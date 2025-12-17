# Map Schema

Standard schema for map data used across the API.

## Map Object Structure

Map data follows this standard structure:

| Field | Type | Description |
|-------|------|-------------|
| `mapId` | string | Unique map ID (UUID) |
| `mapShareCode` | string | Public map share code (format: "XXXX-XXXX-XXXX") |
| `name` | string | Map name (may contain HTML color codes) |
| `description` | string | Map description |
| `type` | string | Map type (see [Map Types](#map-types)) |
| `authorUserId` | integer | Map author ID |
| `latestVersion` | integer \| null | Latest version (may be null) |

## Nested Objects

### thumbnails

| Field | Type | Description |
|-------|------|-------------|
| `detail` | string | Thumbnail URL in detail size (532x400) |
| `list` | string | Thumbnail URL in list size (532x400) |

**Note:** URLs contain temporary signatures and expire after a period.

### author

Map author information. This is a simplified player profile.

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | Author ID |
| `username` | string | Author username |
| `country` | string | Country code (ISO 2 letters) |
| `trophies` | integer | Author's total trophies |
| `crowns` | integer | Author's total crowns |
| `skin` | string | Author's current skin ID |

### version

Current map version information.

| Field | Type | Description |
|-------|------|-------------|
| `current` | integer | Current version number |
| `shareCode` | string | This version's specific share code |
| `versionId` | string | Unique version ID (UUID) |

### metadata

Technical map metadata.

| Field | Type | Description |
|-------|------|-------------|
| `minClientVersion` | string | Minimum required client version (may be empty) |
| `stateFlags` | integer | Map state flags |
| `customMapSchemaVersion` | integer | Custom map schema version |
| `propsSummary` | array | Summary of props/objects used in the map |

### propsSummary[]

Array of objects summarizing the props/objects used in the map.

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Prop/object name |
| `type` | integer | Object type (numeric code) |
| `count` | integer | Quantity of this object in the map |
| `objectId` | string | Unique object ID |

## Map Types

| Value | Description |
|-------|-------------|
| `RACE` | Race - first to finish wins |
| `SURVIVAL` | Survival - last survivor wins |
| `HUNT` | Hunt - eliminate other players |

## Share Codes

There are two types of share codes:

| Type | Description | Usage |
|------|-------------|-------|
| `mapShareCode` | Public map share code | Identifies the map across all versions. Use to search the map or version history |
| `versionShareCode` | Specific version share code | Identifies a specific version. Use to play that exact version in the game |

**Format:** `XXXX-XXXX-XXXX` (4 digits, hyphen, 4 digits, hyphen, 4 digits)

## Where Map Data Appears

Map data appears in various API responses:

1. **[Search Map](/maps/search)** - Returns complete map data with author, version, and metadata
2. **[Search by Author](/maps/author)** - Returns simplified map objects in `maps[]` array
3. **[Version History](/maps/versions)** - Returns simplified map object in `map` field, plus version history

## Example Map Object (Complete)

```json
{
  "mapId": "d2667c74-54ae-4a56-bdc8-47aa5db426b8",
  "mapShareCode": "2051-0533-6189",
  "name": "<#FF0>BRICK <#fff> so que diferente 🤡💀",
  "description": "dasw test",
  "type": "RACE",
  "authorUserId": 193589871,
  "latestVersion": null,
  "thumbnails": {
    "detail": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/...",
    "list": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/..."
  },
  "author": {
    "userId": 193589871,
    "username": "<#0bf>Dasw",
    "country": "BR",
    "trophies": 176740,
    "crowns": 5232,
    "skin": "SKIN958"
  },
  "version": {
    "current": 3134,
    "shareCode": "1724-2532-3225",
    "versionId": "b3e9fd7c-abd4-4017-b301-77686bf93174"
  },
  "metadata": {
    "minClientVersion": "",
    "stateFlags": 0,
    "customMapSchemaVersion": 0,
    "propsSummary": [
      {
        "name": "spawn",
        "type": 3001,
        "count": 1,
        "objectId": "763216006"
      }
    ]
  }
}
```

## Example Map Object (Simplified)

Used in search by author results:

```json
{
  "mapId": "d2667c74-54ae-4a56-bdc8-47aa5db426b8",
  "mapShareCode": "2051-0533-6189",
  "name": "<#FF0>BRICK <#fff> so que diferente 🤡💀",
  "description": "dasw test",
  "type": "RACE",
  "authorUserId": 193589871,
  "thumbnails": {
    "detail": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/...",
    "list": "https://prod-alchemy.web.stumbleguys.com/map_thumbnails/v1/..."
  },
  "latestVersion": null
}
```

## Notes

- The `name` field may contain HTML color codes (e.g., `<#FF0>` for yellow)
- Thumbnail URLs contain temporary signatures and expire after a period
- The `latestVersion` field may be `null` if no versions are registered
- The `propsSummary` lists all unique objects used in the map and their quantities
- The `stateFlags` is a bitmask indicating map states (published, private, etc.)
- The `customMapSchemaVersion` indicates the custom map data format version
- The `author` object is a simplified player profile (see [Player Schema](/players/schema))
- The `mapShareCode` is permanent and identifies the map across all versions
- The `versionShareCode` is specific to a version and changes when the map is updated

