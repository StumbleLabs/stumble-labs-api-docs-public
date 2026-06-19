# StumbleLabs API

> Public API documentation to access player, clan, map, and asset data from Stumble Guys.

> [!WARNING]
> **API key registration is currently closed.** The maximum number of API keys has already been reached, so no new keys are being issued at this time. Development focus has shifted to building the **Stumble Labs website**.

> [!NOTE]
> **About this project.** This API is still in a testing phase and runs as a controlled environment. It is a **voluntary, community-driven project by Dasw** and is **not affiliated with, endorsed by, or associated with Scopely**, nor with anyone officially involved in producing or managing Stumble Guys.
>
> Because it operates on top of data and scopes ultimately controlled by Scopely, fields and behavior can change at any time without notice, so parts of this documentation may occasionally be out of date.

## Getting Started

New to the API? Start with the [Quick Start Guide](/getting-started).

## Base URL

```
https://api.stumblelabs.net/api
```

## Documentation

- [Getting Started](/getting-started) - Quick guide for beginners
- [Authentication](/authentication) - How to use API keys and rate limiting
- [Status Codes and Errors](/errors) - Error handling

## Authentication

Most endpoints require an API key in the `x-api-key` header.

> New API key registration is currently closed (key limit reached).

[Learn more about authentication →](/authentication)

## Response Format

All responses follow the standard format:

```json
{
  "success": boolean,
  "message": string,
  "data": object,
  "errors": string[],
  "status": number
}
```

## Endpoints

### In-game
- [Get Room](/in-game/get-room) - Live state of a party room (players, config, maps)
- [Live Game Stats](/in-game/live-game) - Live concurrency and room stats by region
- [Available Maps](/in-game/maps) - Map ID to friendly name reference

### Players
- [Player Schema](/players/schema) - Standard player data structure
- [Search Player](/players/search) - Search for a player by userId or username
- [Search by Discord ID](/players/search-discord) - Search player by Discord ID

### Player Verification
- [Overview & Flow](/player-verification) - How player verification works

### Clans
- [Clan Schema](/clans/schema) - Standard clan data structure
- [Search Clans](/clans/search) - Search clans by name
- [Get Clan by ID](/clans/get-by-id) - Complete clan data
- [Recommended Clans](/clans/recommended) - Suggested clans to join
- [Clan Leaderboard](/clans/leaderboard) - Top clans by crowns or trophies
- [Current Season](/clans/season-current) - Active clan season details
- [List Seasons](/clans/seasons) - All clan seasons
- [Missions Today](/clans/missions-today) - Daily clan missions
- [Member Season XP](/clans/member-season-xp) - A member's season XP contribution
- [Member History](/clans/member-history) - Join, leave and role change events
- [Snapshots](/clans/snapshots) - Historical clan snapshots

### Maps
- [Map Schema](/maps/schema) - Standard map data structure
- [Search Map](/maps/search) - Search map by share code
- [Search by Author](/maps/author) - List maps by an author
- [Version History](/maps/versions) - Map versions

### Leaderboards
- [Get Ranked Leaderboard](/leaderboards/ranked) - Top ranked players for current season

### Classic Tournaments
- [Tournament Schema](/tournaments/schema) - Standard tournament data structure
- [List Tournaments](/tournaments/list) - Paginated list with filtering and sorting
- [Get Tournament by ID](/tournaments/get-by-id) - Complete tournament data
- [List Seasons](/tournaments/seasons) - Distinct season parts in the archive
- [Stats](/tournaments/stats) - Aggregate tournament statistics
- [Tournaments Won by Player](/tournaments/winners-by-player) - Wins for a specific player
- [Winners Leaderboard](/tournaments/leaderboard-winners) - Player ranking by tournament wins

### Assets
- [Asset Schema](/assets/schema) - Standard asset structure and reference
- [Get Asset by ID](/assets/search) - Look up a single asset by its ID
- [List by Type](/assets/by-type) - Assets filtered by type
- [Search by Name](/assets/search-by-name) - Find assets by name
- [Latest Assets](/assets/latest) - Recently added assets
- [Get Allowed Assets](/assets/allowed) - List all available assets

## Rate Limiting

All requests are subject to rate limiting. Limits vary by API key.

[Learn more about rate limiting →](/authentication#rate-limiting)

## Quick Example

```bash
curl -X POST "https://api.stumblelabs.net/api/live/users/search" \
  -H "x-api-key: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{"userid": 235378638}'
```

## Support

Need help? Contact someone from Stumble Labs, preferably the owner
