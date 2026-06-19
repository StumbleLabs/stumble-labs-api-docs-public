# Clan Missions Today

Returns the active clan missions for the current daily cycle, along with the next daily reset timestamp.

> See also: [Current Clan Season](/clans/season-current) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/missions/today
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl -X GET "https://api.stumblelabs.net/api/clans/missions/today" \
  -H "x-api-key: your-api-key-here"
```

#### **JavaScript**

```javascript
const response = await fetch('https://api.stumblelabs.net/api/clans/missions/today', {
  method: 'GET',
  headers: {
    'x-api-key': 'your-api-key-here'
  }
});

const data = await response.json();
console.log(data);
```

#### **Python**

```python
import requests

url = "https://api.stumblelabs.net/api/clans/missions/today"
headers = {"x-api-key": "your-api-key-here"}

response = requests.get(url, headers=headers)
data = response.json()
print(data)
```

<!-- tabs:end -->

## Success Response (200)

```json
{
  "success": true,
  "message": "Clan missions for today fetched successfully.",
  "data": {
    "dailyResetAt": "2026-06-19T10:00:00.000Z",
    "missions": [
      {
        "missionId": "CLAN_DAILY_WIN_RACES",
        "type": "DAILY",
        "category": "RACE",
        "description": "Win races as a clan",
        "endDate": "2026-06-19T10:00:00.000Z",
        "missionActive": true,
        "rewards": []
      }
    ]
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `dailyResetAt` | string | Next daily reset (ISO 8601). Missions reset at 10:00:00 UTC |
| `missions` | array | Active clan missions for the current cycle |

### missions[]

| Field | Type | Description |
|-------|------|-------------|
| `missionId` | string | Mission identifier (prefixed with `CLAN_`) |
| `type` | string\|null | Mission type (e.g., `DAILY`, `WEEKLY`) |
| `category` | string\|null | Mission category |
| `description` | string\|null | Human-readable mission description |
| `endDate` | string\|null | When the mission ends (ISO 8601) |
| `missionActive` | boolean | Whether the mission is currently active |
| `rewards` | array | Reward definitions for the mission |

## Notes

- Missions reset daily at **10:00:00 UTC**; `dailyResetAt` reflects the next reset.
- Missions are sorted by type (`DAILY` → `WEEKLY` → others), then by `missionId`.
- Only missions whose ID starts with `CLAN_` and that are currently active are returned.
