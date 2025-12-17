# Search Player by Discord ID

Search for player data using their Discord ID.

## Endpoint

```http
GET /live/users/search/discord/{discordId}
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `discordId` | string | Yes | Player's Discord ID |

## Request Example

```bash
curl -X GET "https://api.stumblelabs.net/api/live/users/search/discord/211186121571303425" \
  -H "x-api-key: your-api-key-here"
```

## Response

The response follows the same format as the [Search Player](/players/search) endpoint, but searches by Discord ID. The player data structure follows the standard [Player Schema](/players/schema).
