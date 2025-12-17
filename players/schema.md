# Player Schema

Standard schema for player data used across the API.

## Player Object Structure

Player data follows this standard structure:

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | Unique player ID |
| `userName` | string | Current username (may contain HTML color codes) |
| `country` | string | Country code (ISO 2 letters) |
| `trophies` | integer | Total trophies |
| `crowns` | integer | Total crowns |
| `experience` | integer | Total experience |
| `isOnline` | boolean | Whether the player is online |
| `skin` | string | Current skin ID |
| `skinIcon` | string | Skin icon URL |
| `nativePlatformName` | string | Platform (steam, android, ios, webgl) |
| `isFallback` | boolean | Whether data is from fallback |
| `isCache` | boolean | Whether data came from cache |

## Nested Objects

### lastActivityInfo

| Field | Type | Description |
|-------|------|-------------|
| `lastInteraction` | string | Last interaction date/time (ISO 8601) |
| `lastSeenType` | string | Last seen type |

### skinInformation

Information about the player's current skin. This object follows the standard [Asset Schema](/assets/schema) with `Category: SKIN`.

| Field | Type | Description |
|-------|------|-------------|
| `ID` | string | Skin ID |
| `FriendlyName` | string | Friendly name |
| `Rarity` | string | Rarity (see [Asset Schema - Rarities](/assets/schema#rarities)) |
| `Category` | string | Category (always "SKIN") |
| `Hidden` | boolean | Whether it's hidden |
| `NoGacha` | boolean | Whether it's not available in gacha |
| `Version` | string | Asset version |
| `IconUrl` | string | Asset icon/preview URL |

### xpRoadInfo

| Field | Type | Description |
|-------|------|-------------|
| `level` | integer | Current level |
| `currentXp` | integer | Current XP |
| `requiredXpForCurrentLevel` | integer | XP required for current level |
| `requiredXpForNextLevel` | integer | XP required for next level |
| `progressToNextLevel` | integer | Progress to next level (0-100) |

### ranked

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Rank ID |
| `name` | string | Rank name |
| `minScore` | integer | Minimum score |
| `maxScore` | integer | Maximum score |
| `icon` | string | Rank icon URL |
| `currentRankId` | integer | Current rank ID |
| `currentSeasonId` | string | Current season ID |
| `currentTierIndex` | integer | Current tier index |

### clan (simplified)

When a player is in a clan, a simplified clan object is included. For full clan details, see [Clan Schema](/clans/schema).

| Field | Type | Description |
|-------|------|-------------|
| `clanId` | string | Clan ID |
| `name` | string | Clan name |
| `tag` | string | Clan tag |
| `memberCount` | integer | Number of members |
| `memberCapacity` | integer | Maximum capacity |
| `joinPolicy` | integer | Join policy (0=open, 1=closed) |
| `region` | string | Clan region |
| `language` | string | Clan language |
| `clanXP` | integer | Total clan XP |
| `logo` | object | Clan logo (see [Clan Schema - logo](/clans/schema#logo)) |

## Additional Fields (may vary by access level)

Depending on the API key's `accessLevel`, additional fields may be present:

- `usernameHistory` - Array of previous usernames
- `xpRoadProgress` - Detailed XP road progress
- `actionEmotes` - Equipped action emotes
- `inventory` - Player inventory
- `flags` - Player flags
- `hiddenRating` - Hidden rating
- `creationDate` - Account creation date

## Where Player Data Appears

Player data appears in various API responses:

1. **[Search Player](/players/search)** - Returns complete player data
2. **[Search by Discord ID](/players/search-discord)** - Returns player data (same structure)
3. **[Get Clan by ID](/clans/get-by-id)** - Returns `userProfile` objects in `members[]` array (similar structure)

## Example Player Object

```json
{
  "userId": 193589871,
  "userName": "<#0bf>Dasw",
  "country": "BR",
  "trophies": 176740,
  "crowns": 5232,
  "experience": 242920,
  "isOnline": false,
  "lastActivityInfo": {
    "lastInteraction": "2025-12-17T02:26:11",
    "lastSeenType": "unknown"
  },
  "skin": "SKIN958",
  "skinIcon": "https://cdn.stumblelabs.net/skins/skin958/preview.png",
  "skinInformation": {
    "ID": "SKIN958",
    "FriendlyName": "Inflatable Joe",
    "Hidden": false,
    "NoGacha": true,
    "Version": "0.69",
    "Rarity": "MYTHIC",
    "Category": "SKIN",
    "IconUrl": "https://cdn.stumblelabs.net/skins/skin958/preview.png"
  },
  "xpRoadInfo": {
    "level": 25,
    "currentXp": 242920,
    "requiredXpForCurrentLevel": 232700,
    "requiredXpForNextLevel": 252700,
    "progressToNextLevel": 51
  },
  "nativePlatformName": "steam",
  "ranked": {
    "id": 0,
    "name": "Unranked",
    "minScore": 0,
    "maxScore": 0,
    "icon": "https://s3.amazonaws.com/production-assetsbucket-8ljvyr1xczmb/765fe95c-162f-4f42-89c2-ab8129941709/Wood.png",
    "currentRankId": 0,
    "currentSeasonId": "LIVE_RANKED_SEASON_17",
    "currentTierIndex": 0
  },
  "isFallback": false,
  "isCache": false
}
```

## Notes

- The `userName` field may contain HTML color codes (e.g., `<#0bf>Dasw`)
- The `clan` field can be `null` if the player is not in a clan
- The `maps` field is deprecated and always returns an empty array
- Returned data may vary depending on your API key's `accessLevel`
- The `skinInformation` object follows the [Asset Schema](/assets/schema)

