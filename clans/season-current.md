# Current Clan Season

Returns the clan season that is currently active, including its milestones and rewards.

> See also: [List Clan Seasons](/clans/seasons) | [Member Season XP](/clans/member-season-xp) | [Authentication](/authentication)

## Endpoint

```http
GET /clans/season/current
```

## Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `x-api-key` | string | Yes | Your API key |

## Request Example

<!-- tabs:start -->

#### **cURL**

```bash
curl -X GET "https://api.stumblelabs.net/api/clans/season/current" \
  -H "x-api-key: your-api-key-here"
```

#### **JavaScript**

```javascript
const response = await fetch('https://api.stumblelabs.net/api/clans/season/current', {
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

url = "https://api.stumblelabs.net/api/clans/season/current"
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
  "message": "Active clan season fetched successfully.",
  "data": {
    "seasonId": "CLANS_LIVE_SEASON_6",
    "startDate": "2025-06-01T00:00:00.000Z",
    "endDate": "2025-07-01T00:00:00.000Z",
    "totalPoints": 12000,
    "milestones": [
      {
        "milestoneId": "milestone_1",
        "pointsToClaim": 1000,
        "rewards": [
          {
            "min": 1,
            "max": 1,
            "type": "SKIN",
            "typeInfo": "SKIN999",
            "chance": 100
          }
        ]
      }
    ],
    "emblemLeveling": null
  },
  "errors": [],
  "status": 200
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `seasonId` | string | Season identifier (e.g., `CLANS_LIVE_SEASON_6`) |
| `startDate` | string | Season start date (ISO 8601) |
| `endDate` | string | Season end date (ISO 8601) |
| `totalPoints` | integer | Total points available in the season |
| `milestones` | array | Reward milestones, sorted by `pointsToClaim` ascending |
| `emblemLeveling` | array\|null | Emblem leveling configuration: array of `{ "Level": integer, "MilestoneId": string }` entries mapping each emblem level to its unlocking milestone. May be `null` for some seasons |

### milestones[]

| Field | Type | Description |
|-------|------|-------------|
| `milestoneId` | string | Milestone identifier |
| `pointsToClaim` | integer | Points required to claim this milestone |
| `rewards` | array | Rewards granted at this milestone |

### rewards[]

| Field | Type | Description |
|-------|------|-------------|
| `min` | integer | Minimum quantity |
| `max` | integer | Maximum quantity |
| `type` | string | Reward type (e.g., `SKIN`, `EMOTE`) |
| `typeInfo` | string | Reward asset ID |
| `chance` | number | Chance to obtain (percentage) |
| `source` | string | Reward source (optional) |

## Error Responses

### 404 - No Active Season

```json
{
  "success": false,
  "message": "No active clan season",
  "data": {},
  "errors": ["no active season"],
  "status": 404
}
```
