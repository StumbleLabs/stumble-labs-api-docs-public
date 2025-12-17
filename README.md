# StumbleLabs API

> Public API documentation to access player, clan, map, and asset data from Stumble Guys.

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

To obtain an API key, contact someone from Stumble Labs

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

### Players
- [Player Schema](/players/schema) - Standard player data structure
- [Search Player](/players/search) - Search for a player by userId or username
- [Search by Discord ID](/players/search-discord) - Search player by Discord ID

### Clans
- [Clan Schema](/clans/schema) - Standard clan data structure
- [Search Clans](/clans/search) - Search clans by name
- [Get Clan by ID](/clans/get-by-id) - Complete clan data

### Maps
- [Map Schema](/maps/schema) - Standard map data structure
- [Search Map](/maps/search) - Search map by share code
- [Search by Author](/maps/author) - List maps by an author
- [Version History](/maps/versions) - Map versions

### Leaderboards
- [Get Ranked Leaderboard](/leaderboards/ranked) - Top ranked players for current season

### Assets
- [Asset Schema](/assets/schema) - Standard asset structure and reference
- [Search Asset](/assets/search) - Information about skins, emotes, etc.
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
