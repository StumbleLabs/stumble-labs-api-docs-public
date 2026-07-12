# Live Game Stats

Fetch live concurrency and room statistics for the game, broken down by region plus a global aggregate. Useful for population dashboards, region pickers, and health monitoring.

> See also: [Get Room](/in-game/get-room) | [Available Maps](/in-game/maps) | [Authentication](/authentication)

## Endpoint

```http
GET /live/game
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl -X GET "https://api.stumblelabs.net/api/live/game" \
  -H "x-api-key: your-api-key-here"
```

#### **JavaScript**

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/game', {
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

url = "https://api.stumblelabs.net/api/live/game"
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
  "message": "Region Information Success",
  "data": {
    "success": true,
    "message": "Region Information Success",
    "retrievedAtUtc": "2026-06-19T18:42:11Z",
    "regions": {
      "sa": {
        "success": true,
        "returnCode": 0,
        "retrievedAtUtc": "2026-06-19T18:42:11Z",
        "region": "sa",
        "regionInfo": {
          "code": "sa",
          "displayName": "South America",
          "serverLocation": "São Paulo, Brazil"
        },
        "dataSource": "masterServerRegionTotals_azure",
        "ccu": {
          "playersInGameRooms": 4210,
          "playersOnMasterNotInRoom": 380,
          "estimatedTotalConnected": 4590
        },
        "rooms": {
          "totalRoomsInRegion": 612
        },
        "derived": {
          "avgPlayersPerRoom": 6.88
        },
        "defaultLobby": {
          "name": "default",
          "isDefault": true,
          "roomCount": 612,
          "playerCount": 4210,
          "scope": "entireRegion"
        }
      }
    },
    "aggregatedCcu": {
      "playersInGameRooms": 38120,
      "playersOnMasterNotInRoom": 3110,
      "estimatedTotalConnected": 41230
    },
    "aggregatedRooms": {
      "totalRoomsInRegion": 5870
    },
    "aggregatedDerived": {
      "avgPlayersPerRoom": 6.49
    }
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

### Payload (`data`)

| Field | Type | Description |
|-------|------|-------------|
| `success` | bool | Whether the statistics were retrieved successfully |
| `message` | string | Status message |
| `retrievedAtUtc` | string | UTC timestamp when the snapshot was taken |
| `regions` | object | Map of region code → per-region statistics (see below) |
| `aggregatedCcu` | object | Global concurrency totals across all regions (see `ccu`) |
| `aggregatedRooms` | object | Global room totals (see `rooms`) |
| `aggregatedDerived` | object | Global derived metrics (see `derived`) |

### Region (`regions[code]`)

| Field | Type | Description |
|-------|------|-------------|
| `success` | bool | Whether stats for this region were retrieved |
| `returnCode` | int | Provider return code for this region's fetch (`0` = success) |
| `retrievedAtUtc` | string | UTC timestamp for this region's snapshot |
| `region` | string | Region code (e.g. `sa`, `us`, `eu`) |
| `regionInfo` | object | Region metadata: `code`, `displayName`, `serverLocation` |
| `dataSource` | string | Origin of the statistics (e.g. `masterServerRegionTotals_azure`) |
| `ccu` | object | Concurrency breakdown (see below) |
| `rooms` | object | `totalRoomsInRegion` — number of active rooms in the region |
| `derived` | object | `avgPlayersPerRoom` — average players per room, or `null` when there are no rooms |
| `defaultLobby` | object | Default lobby stats (see below) |

### Concurrency (`ccu` / `aggregatedCcu`)

| Field | Type | Description |
|-------|------|-------------|
| `playersInGameRooms` | int | Players currently inside game rooms |
| `playersOnMasterNotInRoom` | int | Players connected but not in a room |
| `estimatedTotalConnected` | int | Estimated total connected players (`inRooms + onMaster`) |

### Default Lobby (`defaultLobby`)

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Lobby name |
| `isDefault` | bool | Whether this is the default lobby |
| `roomCount` | int | Number of rooms counted |
| `playerCount` | int | Number of players counted |
| `scope` | string | Counting scope: `entireRegion` or `defaultLobbyOnly` |

## Notes

- Statistics are a snapshot at request time and update continuously.
- A region entry may report `success: false` if its stats are momentarily unavailable; the global aggregates only include successful regions.
- `derived.avgPlayersPerRoom` is `null` when a region has no active rooms.
- This endpoint is cached briefly to stay fast under heavy traffic, so values may be a few seconds old.
