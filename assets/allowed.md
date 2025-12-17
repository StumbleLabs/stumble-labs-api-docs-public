# Get Allowed Assets

Returns all available assets (skins, emotes, animations, footsteps, victory animations) along with available categories, rarities, and versions.

> 💡 **Tip:** All assets returned here can be searched individually for detailed information using the [Search Asset](/assets/search) endpoint with the asset `ID`.

## Endpoint

```http
GET /live/shared/allowedAssets
```

## Path Parameters

None. This endpoint does not require any parameters.

## Request Example

### cURL

```bash
curl -X GET "https://api.stumblelabs.net/api/live/shared/allowedAssets"
```

### JavaScript (Fetch)

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/shared/allowedAssets');

const data = await response.json();
console.log(data);
```

### Python (requests)

```python
import requests

url = "https://api.stumblelabs.net/api/live/shared/allowedAssets"

response = requests.get(url)
data = response.json()
print(data)
```

## Success Response (200)

```json
{
  "success": true,
  "message": "OK",
  "errors": [],
  "data": {
    "categories": [
      "SKIN",
      "footsteps",
      "Steps",
      "emote",
      "Emote",
      "animation",
      "Victory"
    ],
    "rarities": [
      "COMMON",
      "RARE",
      "UNCOMMON",
      "EPIC",
      "MYTHIC",
      "LEGENDARY",
      "SPECIAL"
    ],
    "versions": [
      {
        "code": "0.1"
      },
      {
        "code": "0.30"
      },
      {
        "code": "0.69"
      }
    ],
    "allowedAssets": [
      {
        "ID": "SKIN1",
        "FriendlyName": "Mr. Stumble",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.1",
        "Rarity": "COMMON",
        "Category": "SKIN",
        "IconUrl": "https://cdn.stumblelabs.net/skins/skin1/preview.png"
      },
      {
        "ID": "footsteps_banana",
        "FriendlyName": "Banana",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.1",
        "Rarity": "RARE",
        "Category": "footsteps",
        "IconUrl": "https://cdn.stumblelabs.net/footsteps/footsteps_banana/preview.png"
      },
      {
        "ID": "Steps374_VolcanicCrack",
        "FriendlyName": "Volcanic Crack",
        "Hidden": true,
        "NoGacha": true,
        "Version": "0.93.5",
        "Rarity": "EPIC",
        "Category": "Steps",
        "IconUrl": "https://cdn.stumblelabs.net/steps/steps374_volcaniccrack/preview.png"
      },
      {
        "ID": "emote_thumb2",
        "FriendlyName": "Thumbs Down",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.1",
        "Rarity": "COMMON",
        "Category": "emote",
        "IconUrl": "https://cdn.stumblelabs.net/emotes/emote_thumb2/preview.png"
      },
      {
        "ID": "Emote091_HangLooseHand",
        "FriendlyName": "Hang Loose",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.1",
        "Rarity": "COMMON",
        "Category": "Emote",
        "IconUrl": "https://cdn.stumblelabs.net/emotes/emote091_hangloosehand/preview.png"
      },
      {
        "ID": "animation_thriller",
        "FriendlyName": "Thriller Dance",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.1",
        "Rarity": "EPIC",
        "Category": "animation",
        "IconUrl": "https://cdn.stumblelabs.net/animations/animation_thriller/preview.png"
      },
      {
        "ID": "Victory030_Sobs",
        "FriendlyName": "So Sad!",
        "Hidden": false,
        "NoGacha": true,
        "Version": "0.1",
        "Rarity": "LEGENDARY",
        "Category": "Victory",
        "IconUrl": "https://cdn.stumblelabs.net/victory/victory030_sobs/preview.png"
      }
    ]
  }
}
```

## Response Fields

### data.categories

Array of available asset categories. See [Asset Schema - Categories](/assets/schema#categories) for details.

### data.rarities

Array of available asset rarities. See [Asset Schema - Rarities](/assets/schema#rarities) for details.

### data.versions

Array of game versions where assets were introduced. Each version object contains:

| Field | Type | Description |
|-------|------|-------------|
| `code` | string | Version code (e.g., "0.1", "0.69", "0.93.5") |

**Note:** Versions are not necessarily in chronological order in the response.

### data.allowedAssets[]

Array of all available assets. Each asset follows the standard [Asset Schema](/assets/schema).

## Errors

### 500 - Internal Server Error

```json
{
  "success": false,
  "message": "Internal server error",
  "data": {},
  "errors": ["An unexpected error occurred"],
  "status": 500
}
```

## Notes

- 🔍 **All assets returned in `allowedAssets` can be searched individually** using the asset `ID` via the [Search Asset](/assets/search) endpoint (`GET /live/shared/search/asset/{asset_id}`)
- The response can be very large as it includes all available assets
- The `allowedAssets` array may contain thousands of items
- Assets are not sorted in any particular order
- The `Hidden` field indicates assets that may not appear in game interfaces but are still valid
- Assets with `NoGacha: true` cannot be obtained through gacha systems
- The `Category` field may use different casing (e.g., `emote` vs `Emote`) - this is normal
- Version codes may include patch versions (e.g., "0.93.5", "0.86.6")
- Icon URLs point to CDN-hosted preview images
- Use this endpoint to get a complete list of all assets for filtering, searching, or cataloging purposes

## Use Cases

1. **Asset Catalog:** Build a complete catalog of all available assets
2. **Filtering:** Get lists of categories, rarities, or versions for filtering UI
3. **Search:** Use the asset list to implement search functionality
4. **Validation:** Verify if an asset ID exists in the game
5. **Statistics:** Analyze asset distribution by category, rarity, or version
6. **Detailed Information:** Get the asset `ID` from this endpoint, then use it with [Search Asset](/assets/search) to get detailed information including where to purchase and similar assets

## Getting Detailed Asset Information

All assets returned in `allowedAssets` can be searched individually for more detailed information:

```javascript
// First, get all allowed assets
const allowedResponse = await fetch('https://api.stumblelabs.net/api/live/shared/allowedAssets');
const allowedData = await allowedResponse.json();

// Get an asset ID (e.g., first skin)
const assetId = allowedData.data.allowedAssets.find(asset => asset.Category === 'SKIN').ID;

// Then search for detailed information about that asset
const detailResponse = await fetch(`https://api.stumblelabs.net/api/live/shared/search/asset/${assetId}`);
const detailData = await detailResponse.json();

// Now you have detailed info including where to buy it and similar assets
console.log(detailData.data);
```

See [Search Asset](/assets/search) documentation for more details on the detailed asset endpoint.

## Example: Filter Assets by Category

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/shared/allowedAssets');
const data = await response.json();

// Filter only skins
const skins = data.data.allowedAssets.filter(asset => asset.Category === 'SKIN');
console.log(`Total skins: ${skins.length}`);
```

## Example: Get Assets by Rarity

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/shared/allowedAssets');
const data = await response.json();

// Filter only LEGENDARY assets
const legendary = data.data.allowedAssets.filter(asset => asset.Rarity === 'LEGENDARY');
console.log(`Total legendary assets: ${legendary.length}`);
```

## Example: Get Assets by Version

```javascript
const response = await fetch('https://api.stumblelabs.net/api/live/shared/allowedAssets');
const data = await response.json();

// Filter assets from version 0.93
const v093Assets = data.data.allowedAssets.filter(asset => asset.Version.startsWith('0.93'));
console.log(`Assets from version 0.93: ${v093Assets.length}`);
```

