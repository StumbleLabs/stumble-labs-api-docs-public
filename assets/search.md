# Search Asset

Search for detailed information about an asset (skin, emote, animation, footstep), including where it can be purchased and similar assets from the same category.

## Endpoint

```http
GET /live/shared/search/asset/{asset_id}
```

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `asset_id` | string | Yes | Asset ID (e.g., "SKIN999", "Steps155_FlowerPower") |

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/live/shared/search/asset/SKIN999"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/shared/search/asset/SKIN999');

const data = await response.json();
console.log(data);
```

### Python (requests)

```python
import requests

url = "https://api.stumblelabs.net/api/live/shared/search/asset/SKIN999"

response = requests.get(url)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "errors": [
    
  ],
  "data": {
    "asset": {
      "ID": "SKIN999",
      "FriendlyName": "Ugly Drawing",
      "Hidden": false,
      "NoGacha": true,
      "Version": "0.68",
      "Rarity": "SPECIAL",
      "Category": "SKIN",
      "IconUrl": "https://cdn.stumblelabs.net/skins/skin999/preview.png"
    },
    "similar": [
      {
        "ID": "SKIN905",
        "FriendlyName": "Ent",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.66",
        "Rarity": "EPIC",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin905/preview.png"
      },
      {
        "ID": "SKIN402",
        "FriendlyName": "Veloci T",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.46",
        "Rarity": "LEGENDARY",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin402/preview.png"
      },
      {
        "ID": "SKIN231",
        "FriendlyName": "Puppet",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.41",
        "Rarity": "RARE",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin231/preview.png"
      },
      {
        "ID": "SKIN324",
        "FriendlyName": "Senegal Lion",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.42",
        "Rarity": "EPIC",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin324/preview.png"
      }
    ],
    "whereIcanBuyIt": [
      {
        "Name": "12_17_2025_FAKE_Wheel",
        "prices": [
          {
            "currency": "gems",
            "amount": 999
          }
        ],
        "analyticsPrices": [
          
        ],
        "rewards": [
          {
            "min": 1,
            "type": "SKIN",
            "typeInfo": "SKIN1376",
            "chance": 3.21
          },
          {
            "min": 1,
            "type": "SKIN",
            "typeInfo": "SKIN999",
            "chance": 90
          },
          {
            "min": 1,
            "type": "SKIN",
            "typeInfo": "SKIN1176",
            "chance": 5.43
          },
          {
            "min": 1,
            "type": "SKIN",
            "typeInfo": "SKIN714",
            "chance": 1.44
          }
        ],
        "Source": "Gacha",
        "displayName": "Megaspin Wheel"
      }
    ]
  }
}
```

## Response Fields

### asset

Requested asset information. Follows the standard [Asset Schema](/assets/schema).

| Field | Type | Description |
|-------|------|-------------|
| `ID` | string | Unique asset ID (e.g., "SKIN999") |
| `FriendlyName` | string | Asset friendly name |
| `Hidden` | boolean | Whether the asset is hidden |
| `NoGacha` | boolean | Whether the asset is not available in gachas |
| `Version` | string | Asset version |
| `Rarity` | string | Asset rarity (see [Asset Schema - Rarities](/assets/schema#rarities)) |
| `Category` | string | Asset category (see [Asset Schema - Categories](/assets/schema#categories)) |
| `IconUrl` | string | Asset icon/preview URL |

### similar[]

Array with up to 4 similar assets from the same category as the requested asset. Each item follows the standard [Asset Schema](/assets/schema).

### whereIcanBuyIt[]

Array of purchasable items where the asset can be obtained (Gachas, Shop, etc).

#### General Fields

| Field | Type | Description |
|-------|------|-------------|
| `Name` | string | Internal name of the purchasable item |
| `Source` | string | Source (e.g., "Gacha", "Shop") |
| `displayName` | string | Friendly display name |
| `prices` | array | Price list |
| `analyticsPrices` | array | Prices for analytics (usually empty) |
| `rewards` | array | List of possible rewards |

#### prices[]

| Field | Type | Description |
|-------|------|-------------|
| `currency` | string | Currency (e.g., "gems") |
| `amount` | integer | Required amount |

#### rewards[]

| Field | Type | Description |
|-------|------|-------------|
| `min` | integer | Minimum quantity |
| `type` | string | Reward type (e.g., "SKIN", "EMOTE") |
| `typeInfo` | string | Reward asset ID |
| `chance` | number | Chance to obtain (in percentage) |

#### Additional Fields for Gachas

When `Source` is "Gacha", the following additional fields may be present:

| Field | Type | Description |
|-------|------|-------------|
| `header` | string | Gacha header |
| `friendlyName` | string | Gacha friendly name |
| `description` | string | Gacha description |
| `isAvailable` | boolean | Whether the gacha is currently available |
| `startDate` | string | Start date (ISO 8601) |
| `endDate` | string | End date (ISO 8601) |
| `backgroundImage` | string | Background image URL |
| `platforms` | array | Platforms where it's available |
| `rotationItemList` | array | List of items in the gacha rotation |

#### rotationItemList[] (for Gachas)

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Asset ID in the rotation |
| `isCurrentAsset` | boolean | Whether it's the current asset being searched |

## Rarities

See [Asset Schema - Rarities](/assets/schema#rarities) for the complete list of available rarities.

## Categories

See [Asset Schema - Categories](/assets/schema#categories) for the complete list of available categories.

## Errors

### 400 - Validation Error

```json
{
  "success": false,
  "message": "Unexpected content",
  "status": 400
}
```

### 404 - Asset Not Found

```json
{
  "success": false,
  "message": "Asset not found",
  "errors": [],
  "data": {
    "asset": null,
    "similar": []
  }
}
```

## Notes

- The `asset_id` is case-insensitive (can be "SKIN999" or "skin999")
- The `similar` list contains up to 4 assets from the same category, chosen randomly
- The `whereIcanBuyIt` field may be empty if the asset is not available for purchase
- For assets with `NoGacha: true`, the asset does not appear in gachas
- The `Hidden: true` field indicates the asset is hidden and may not appear in some interfaces
- Assets from different categories do not appear in the `similar` list
- The `chance` in `rewards` is in percentage (e.g., 90 = 90%)
