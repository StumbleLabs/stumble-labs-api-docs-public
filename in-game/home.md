# Home Feed

Aggregated payload that powers the public landing / dashboard: a featured (spotlight) player, the latest skins, headline site statistics, and carousel slots for players and maps. Handy for building a "front page" without calling several endpoints.

> See also: [Live Game Stats](/in-game/live-game) | [Latest Assets](/assets/latest) | [Players Leaderboard](/leaderboards/players) | [Authentication](/authentication)

## Endpoint

```http
GET /live/home
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

This endpoint takes **no query parameters**; unrecognized ones (e.g. `?region=eu`) are ignored and return the same payload.

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl -X GET "https://api.stumblelabs.net/api/live/home" \
  -H "x-api-key: your-api-key-here"
```

#### **JavaScript**

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/home', {
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

url = "https://api.stumblelabs.net/api/live/home"
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
    "player_container": {
      "userId": 193589871,
      "userName": "<#39e>dasw",
      "country": "BR",
      "trophies": 176420,
      "crowns": 5229,
      "experience": 199580,
      "hiddenRating": 148152,
      "isOnline": false,
      "skin": "SKIN1155",
      "nativePlatformName": "steam",
      "ranked": {
        "currentSeasonId": "LIVE_RANKED_SEASON_2",
        "currentTierIndex": 0
      }
    },
    "players_slide": [
      { "icon": "", "name": "", "id": "" }
    ],
    "maps_slide": [],
    "community_maps": [],
    "trending_maps": [],
    "skins_latest": [
      {
        "ID": "SKIN1787",
        "FriendlyName": "Sythra",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.100",
        "Rarity": "EPIC",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin1787/preview.png"
      }
    ],
    "stats": {
      "accounts_tracked": "10M+",
      "ugc_maps_found": "82.392",
      "assets_files": "3242",
      "players_online_delay_10": null
    }
  },
  "errors": [],
  "status": 200
}
```

> The sample trims long arrays: `skins_latest` typically returns several items and `players_slide` several slots.

## Response Fields

### data

| Field | Type | Description |
|-------|------|-------------|
| `player_container` | object | A single featured / spotlight player (see below) |
| `players_slide` | array | Carousel slots for highlighted players (see below) |
| `maps_slide` | array | Carousel slots for highlighted maps |
| `community_maps` | array | Featured community (UGC) maps |
| `trending_maps` | array | Currently trending maps |
| `skins_latest` | array | Most recently added skins — standard [Asset](/assets/schema) objects (`Category: SKIN`) |
| `stats` | object | Headline site statistics, pre-formatted for display (see below) |

### data.player_container

A subset of a player profile spotlighting one account. Follows the [Player Schema](/players/schema) but only carries the fields below.

| Field | Type | Description |
|-------|------|-------------|
| `userId` | integer | Unique player ID |
| `userName` | string | Username (may contain HTML color codes) |
| `country` | string | Country code (ISO 3166-1 alpha-2) |
| `trophies` | integer | Total trophies |
| `crowns` | integer | Total crowns |
| `experience` | integer | Total experience |
| `hiddenRating` | integer | Hidden matchmaking rating — **may require elevated access** (removed for lower-tier keys; see [Access Levels](/authentication#access-levels)) |
| `isOnline` | boolean | Whether the player is currently online |
| `skin` | string | Current skin ID |
| `nativePlatformName` | string | Platform (`steam`, `android`, `ios`, `webgl`) |
| `ranked` | object | Ranked season info: `currentSeasonId` (string), `currentTierIndex` (integer) |

### data.players_slide[]

| Field | Type | Description |
|-------|------|-------------|
| `icon` | string | Player avatar / icon URL |
| `name` | string | Display name |
| `id` | string | Player ID |

### data.stats

Display-ready statistic strings for the landing page (already formatted — not raw numbers).

| Field | Type | Description |
|-------|------|-------------|
| `accounts_tracked` | string | Number of tracked accounts, abbreviated (e.g. `"10M+"`) |
| `ugc_maps_found` | string | Count of discovered UGC maps (e.g. `"82.392"`) |
| `assets_files` | string | Count of catalogued asset files (e.g. `"3242"`) |
| `players_online_delay_10` | string \| null | Players online (10-minute-delayed figure); `null` when unavailable |

## Notes

- **`player_container` does not rotate per request** — the same featured player was returned across repeated back-to-back calls. It appears to be a curated / periodically-rotated spotlight rather than a per-call random pick.
- **`skins_latest`** returns the newest skins (stable between calls) and follows the standard [Asset Schema](/assets/schema); refer to that page rather than treating these as a distinct shape.
- The `*_slide`, `community_maps`, and `trending_maps` arrays were **empty or placeholder** during observation (`players_slide` contained a single slot with empty `icon`/`name`/`id` strings; the map arrays were empty). They are content slots that may be unpopulated depending on what is currently featured.
- `stats` values are **pre-formatted display strings**, not integers (note the abbreviations and locale-style separators); parse accordingly if you need numbers.
- `players_online_delay_10` was `null` at observation time.
- `hiddenRating` inside `player_container` is sensitive and is stripped for lower access-level keys — do not rely on it for public integrations.

## Errors

| Status | Reason |
|--------|--------|
| `401` | Invalid API key (`x-api-key` present but not recognized) |
| `403` | Missing `x-api-key` header |
