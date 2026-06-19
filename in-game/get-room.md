# Get Room

Fetch the live state of an in-game party room: the players currently in it, the party configuration, and the map of every round.

> See also: [Live Game Stats](/in-game/live-game) | [Available Maps](/in-game/maps) | [Authentication](/authentication)

## Endpoint

```http
GET /live/room/{region}/{roomCode}
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `region` | string | Yes | Server region. One of: `asia`, `eu`, `inw`, `sa`, `us`, `ussc` (case-insensitive) |
| `roomCode` | string | Yes | The 6-character party invite code shown in-game (case-insensitive) |

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl -X GET "https://api.stumblelabs.net/api/live/room/sa/AB12CD" \
  -H "x-api-key: your-api-key-here"
```

#### **JavaScript**

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/room/sa/AB12CD', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key-here'
  }
});

const data = await response.json();
console.log(data.data);
```

#### **Python**

```python
import requests

url = "https://api.stumblelabs.net/api/live/room/sa/AB12CD"
headers = {
    "x-api-key": "your-api-key-here"
}

response = requests.get(url, headers=headers)
data = response.json()
print(data["data"])
```

<!-- tabs:end -->

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "name": "PARTY_AB12CD",
    "playerCount": 3,
    "maxPlayers": 32,
    "isOpen": true,
    "isVisible": false,
    "autoCleanUp": true,
    "masterClientId": 1,
    "playerTtl": 0,
    "emptyRoomTtl": 30000,
    "expectedUsers": [],
    "customProperties": {
      "PARTY_MODE": 0,
      "PLAYER_NUMBER": 32,
      "ROUNDS_NUMBER": 3,
      "ROUND": 1,
      "PLAY_WITH_BOTS": true,
      "CROSSPLAY_ENABLED": true,
      "ROOM_CODE": "AB12CD",
      "ROUND1_MAP_ID": "eventlevel1_dash",
      "ROUND1_MAP_NAME": "Laser Dash",
      "ROUND2_MAP_ID": "level19_block",
      "ROUND2_MAP_NAME": "Block Dash",
      "ROUND3_MAP_ID": "level15_laser",
      "ROUND3_MAP_NAME": "Laser Tracer"
    },
    "players": [
      {
        "actorNumber": 1,
        "userId": "235378638",
        "userName": "Dasw",
        "isMasterClient": true,
        "isInactive": false,
        "hasRejoined": false,
        "customProperties": {}
      }
    ]
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

### Room (`data`)

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Internal room name (includes the `PARTY_` prefix) |
| `playerCount` | int | Number of real players currently in the room |
| `maxPlayers` | int | Maximum room capacity |
| `isOpen` | bool | Whether the room accepts new players |
| `isVisible` | bool | Whether the room appears in public listings |
| `autoCleanUp` | bool | Whether the room clears its cache when emptied |
| `masterClientId` | int | `actorNumber` of the room host |
| `playerTtl` | int | Time (ms) before an inactive player is removed |
| `emptyRoomTtl` | int | Time (ms) the room persists while empty before being destroyed |
| `expectedUsers` | string[] | Reserved/expected user IDs (slots) |
| `customProperties` | object | Party configuration — see below |
| `players` | object[] | Players currently in the room — see below |

### Player (`players[]`)

| Field | Type | Description |
|-------|------|-------------|
| `actorNumber` | int | Player identifier within the room |
| `userId` | string | The player's Stumble Guys user ID |
| `userName` | string | The player's display nickname |
| `isMasterClient` | bool | Whether the player is the room host |
| `isInactive` | bool | Whether the player is temporarily disconnected |
| `hasRejoined` | bool | Whether the player rejoined the room |
| `customProperties` | object | The player's cosmetic and status data. Keys vary per player |

### Party (`customProperties`)

Values depend on the party. The most relevant keys:

| Key | Description |
|-----|-------------|
| `PARTY_MODE` | Party mode: `0` = Classic Custom, `2` = Grand Prix |
| `PLAYER_NUMBER` | Configured player capacity |
| `ROUNDS_NUMBER` | Number of rounds |
| `ROUND` | Current round |
| `ROUND{n}_MAP_ID` | Map ID of round `n` (e.g. `ROUND1_MAP_ID`) |
| `ROUND{n}_MAP_NAME` | Friendly map name of round `n` (e.g. `Laser Dash`) — see [Available Maps](/in-game/maps) |
| `PLAY_WITH_BOTS` | Whether the party plays with bots |
| `ROOM_CODE` | The room's 6-character code |
| `CROSSPLAY_ENABLED` | Whether crossplay is enabled |

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Invalid region",
  "data": {},
  "errors": ["Region must be one of: asia, eu, inw, sa, us, ussc"],
  "status": 400
}
```

### 404 - Room Not Found

```json
{
  "success": false,
  "message": "Game does not exist",
  "data": {},
  "errors": ["The requested game room was not found"],
  "status": 404
}
```

## Notes

- The `roomCode` is the 6-character invite code shown in-game. Do **not** include the `PARTY_` prefix — it is added automatically.
- The response reflects the room state **at the moment of the request**; it is not a stream. Polling in very quick succession may show small variations as players join and leave.
- `region` and `roomCode` are case-insensitive and normalized internally.
- The `players[].customProperties` object holds per-player data (cosmetics, status, etc.); its keys are not fixed and vary between players.
- Friendly map names returned in `ROUND{n}_MAP_NAME` follow the [Available Maps](/in-game/maps) reference.
