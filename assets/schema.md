# Asset Schema

Standard schema for assets (skins, emotes, animations, footsteps, victory animations) used across the API.

## Asset Object Structure

All assets follow this standard structure:

| Field | Type | Description |
|-------|------|-------------|
| `ID` | string | Unique asset ID (e.g., "SKIN1", "footsteps_banana", "Victory030_Sobs") |
| `FriendlyName` | string | Human-readable asset name |
| `Hidden` | boolean | Whether the asset is hidden (may not appear in some interfaces) |
| `NoGacha` | boolean | Whether the asset is not available in gachas |
| `Version` | string | Game version when the asset was introduced |
| `Rarity` | string | Asset rarity (see [Rarities](#rarities)) |
| `Category` | string | Asset category (see [Categories](#categories)) |
| `IconUrl` | string | URL to the asset's preview icon |

## Asset ID Patterns

Different asset types follow different ID patterns:

- **Skins:** `SKIN{number}` (e.g., `SKIN1`, `SKIN999`)
- **Footsteps (old):** `footsteps_{name}` (e.g., `footsteps_banana`)
- **Footsteps (new):** `Steps{number}_{Name}` (e.g., `Steps374_VolcanicCrack`)
- **Emotes (old):** `emote_{name}` (e.g., `emote_thumb2`)
- **Emotes (new):** `Emote{number}_{Name}` (e.g., `Emote091_HangLooseHand`)
- **Animations:** `animation_{name}` (e.g., `animation_thriller`)
- **Victory:** `Victory{number}_{Name}` (e.g., `Victory030_Sobs`)

## Rarities

Available asset rarities:

| Value | Description |
|-------|-------------|
| `COMMON` | Common |
| `RARE` | Rare |
| `UNCOMMON` | Uncommon |
| `EPIC` | Epic |
| `MYTHIC` | Mythic |
| `LEGENDARY` | Legendary |
| `SPECIAL` | Special |

## Categories

Available asset categories:

| Value | Description |
|-------|-------------|
| `SKIN` | Character skins |
| `footsteps` | Footstep sounds (lowercase, old format) |
| `Steps` | Footstep sounds (capitalized, new format) |
| `emote` | Emotes (lowercase, old format) |
| `Emote` | Emotes (capitalized, new format) |
| `animation` | Animations |
| `Victory` | Victory animations |

**Note:** Some categories appear in both lowercase and capitalized forms (e.g., `footsteps` and `Steps`, `emote` and `Emote`). This is due to historical naming conventions.

## Where Assets Appear

Assets appear in various API responses:

1. **[Get Allowed Assets](/assets/allowed)** - Returns all available assets with their basic information
2. **[Search Asset](/assets/search)** - Returns detailed information about a specific asset, including where to purchase and similar assets
3. **[Search Player](/players/search)** - Returns `skinInformation` object which follows the asset schema (Category: SKIN)

## Example Asset Object

```json
{
  "ID": "SKIN958",
  "FriendlyName": "Inflatable Joe",
  "Hidden": false,
  "NoGacha": true,
  "Version": "0.69",
  "Rarity": "MYTHIC",
  "Category": "SKIN",
  "IconUrl": "https://cdn.stumblelabs.net/skins/skin958/preview.png"
}
```

## Notes

- The `asset_id` is case-insensitive when searching (can be "SKIN999" or "skin999")
- The `Hidden` field indicates assets that may not appear in game interfaces but are still valid
- Assets with `NoGacha: true` cannot be obtained through gacha systems
- Version codes may include patch versions (e.g., "0.93.5", "0.86.6")
- Icon URLs point to CDN-hosted preview images
- All assets returned in [Get Allowed Assets](/assets/allowed) can be searched individually using [Search Asset](/assets/search) with the asset `ID`

