# MiKiLED Firmware Releases

Public firmware distribution for [MiKiLED](https://github.com/konkubino/MiKiLED) terrarium controllers.

Devices check these manifests automatically every 24 hours to discover new firmware updates.

## Structure

```
manifest-lolin_s2_mini.json   ← manifest for LOLIN S2 Mini board
manifest-lolin_c3_mini.json   ← manifest for LOLIN C3 Mini board
releases/
  v4.5.0/
    mikiled-lolin_s2_mini.bin
    mikiled-lolin_c3_mini.bin
```

## Manifest Format

```json
{
  "version": "4.5.0",
  "url": "https://github.com/konkubino/MiKiLED-releases/releases/download/v4.5.0/mikiled-lolin_s2_mini.bin",
  "sha256": "a3b2c1d4e5f6...",
  "notes": [
    "OTA firmware updates via web UI",
    "Improved display feedback"
  ],
  "releaseDate": "2026-04-11",
  "updateType": "regular"
}
```

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `version` | Yes | Semantic version (e.g. `"4.5.0"`) — compared against device's `App::VERSION` |
| `url` | Yes | HTTPS URL to the `.bin` firmware file |
| `sha256` | Yes | Lowercase hex SHA-256 hash of the `.bin` — verified on device before flashing |
| `notes` | Yes | Array of strings — shown as bullet points in the web UI |
| `releaseDate` | Yes | ISO date string (e.g. `"2026-04-11"`) |
| `updateType` | Yes | `"regular"` / `"major"` / `"critical"` — controls notification behavior |

### Update Types

| Type | Behavior on device |
|------|-------------------|
| `regular` | Subtle badge in Device Setup group; no popup |
| `major` | One-time popup with Install / Remind Later / Don't Show Again |
| `critical` | One-time popup with Install / Remind Later (no dismiss option) |

## How to Release

1. Build firmware for both boards: `pio run -e lolin_s2_mini` and `pio run -e lolin_c3_mini`
2. Compute SHA-256: `shasum -a 256 .pio/build/lolin_s2_mini/firmware.bin`
3. Create a GitHub Release (e.g. `v4.5.0`) and attach both `.bin` files
4. Update both manifest JSON files with the new version, URLs, sha256, notes, and updateType
5. Commit and push the manifest changes to `main`

Devices will pick up the new version within 24 hours (or immediately when the user clicks "Check for Updates").
