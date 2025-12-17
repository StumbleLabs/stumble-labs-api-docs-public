# Clan Schema

Standard schema for clan data used across the API.

## Clan Object Structure

Clan data follows this standard structure:

| Field | Type | Description |
|-------|------|-------------|
| `clanId` | string | Unique clan ID (ULID) |
| `name` | string | Clan name |
| `tag` | string | Clan tag (max 3-4 characters) |
| `description` | string | Clan description |
| `memberCount` | integer | Current number of members |
| `memberCapacity` | integer | Maximum member capacity (usually 50) |
| `joinPolicy` | integer | Join policy (see [Join Policy](#join-policy)) |
| `region` | string | Clan region (e.g., "SA", "EU", "US") |
| `language` | string | Clan language (e.g., "pt", "en", "es") |
| `clanXP` | integer | Total clan XP |
| `currentStreak` | integer | Current win streak |
| `nextNameChangeIsFree` | boolean | Whether next name change is free |
| `clanCreatedAtMs` | integer | Creation timestamp in milliseconds |
| `clanCreatedAt` | string | Formatted creation date (ISO 8601) |

## Nested Objects

### logo

| Field | Type | Description |
|-------|------|-------------|
| `backgroundId` | string | Logo background ID (e.g., "option1") |
| `foregroundId` | string | Logo foreground ID (e.g., "option3") |
| `colourSchemeId` | string | Color scheme ID (e.g., "option10") |
| `level` | integer | Logo level (1-5) |

### members[]

Array of objects containing information about each clan member.

#### userProfile

Member's player profile. This follows a simplified version of the [Player Schema](/players/schema).

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | Unique player ID |
| `userName` | string | Username |
| `title` | string | Player title |
| `country` | string | Country code (ISO 2 letters) |
| `trophies` | integer | Total trophies |
| `crowns` | integer | Total crowns |
| `experience` | integer | Total experience |
| `hiddenRating` | integer | Hidden rating (usually 0) |
| `isOnline` | boolean | Whether the player is online |
| `lastSeenDate` | string | Last seen date/time (ISO 8601) |
| `skin` | string | Current skin ID |
| `nativePlatformName` | string | Platform (steam, android, ios, webgl) |
| `flags` | integer | Player flags |
| `ranked` | object | Ranked information (see [Player Schema - ranked](/players/schema#ranked)) |

#### role

Member's role in the clan:

| Value | Description |
|-------|-------------|
| `0` | Member |
| `1` | Admin |
| `2` | Leader |

#### clanXpContribution

| Field | Type | Description |
|-------|------|-------------|
| `clanXpContribution` | integer | Member's XP contribution to the clan |

## Join Policy

| Value | Description |
|-------|-------------|
| `0` | Open - Anyone can join |
| `1` | Closed - Requires approval |
| `2` | Invite only |

## Where Clan Data Appears

Clan data appears in various API responses:

1. **[Get Clan by ID](/clans/get-by-id)** - Returns complete clan data with full member profiles
2. **[Search Clans](/clans/search)** - Returns simplified clan objects (without members)
3. **[Search Player](/players/search)** - Returns simplified clan object in `clan` field (when player is in a clan)

## Example Clan Object (Complete)

```json
{
  "clanId": "01K1JZ400449Q1TZERE9E719Y1",
  "name": "Devs",
  "tag": "DEV",
  "description": "Mr. Stumble: Hello World!",
  "memberCount": 4,
  "memberCapacity": 50,
  "joinPolicy": 2,
  "region": "SA",
  "language": "pt",
  "clanXP": 14640,
  "currentStreak": 0,
  "nextNameChangeIsFree": false,
  "clanCreatedAtMs": 1754057015300,
  "clanCreatedAt": "2025-08-01T14:03:35.300Z",
  "logo": {
    "backgroundId": "option1",
    "foregroundId": "option3",
    "colourSchemeId": "option10",
    "level": 3
  },
  "members": [
    {
      "userProfile": {
        "userId": 193589871,
        "userName": "<#0bf>Dasw",
        "country": "BR",
        "trophies": 176740,
        "crowns": 5232,
        "experience": 242920,
        "isOnline": true,
        "lastSeenDate": "2025-12-17T02:26:11",
        "skin": "SKIN958",
        "nativePlatformName": "steam",
        "ranked": {
          "currentSeasonId": "LIVE_RANKED_SEASON_17",
          "currentRankId": 0,
          "currentTierIndex": 0
        },
        "flags": 2
      },
      "role": 0,
      "clanXpContribution": 0
    }
  ]
}
```

## Example Clan Object (Simplified)

Used in search results and player data:

```json
{
  "clanId": "01K1JZ400449Q1TZERE9E719Y1",
  "name": "Devs",
  "tag": "DEV",
  "logo": {
    "backgroundId": "option1",
    "foregroundId": "option3",
    "colourSchemeId": "option10",
    "level": 3
  },
  "memberCount": 4,
  "memberCapacity": 50,
  "joinPolicy": 2,
  "region": "SA",
  "language": "pt",
  "clanXP": 14640
}
```

## Notes

- The `clanId` must be a valid ULID (e.g., "01K1JZ400449Q1TZERE9E719Y1")
- The `description` field may be empty if the clan has no description set
- Member profiles in `members[]` follow a simplified version of the [Player Schema](/players/schema)
- The `role` field indicates the member's position in the clan
- The `clanXpContribution` shows how much XP each member contributed to the clan

