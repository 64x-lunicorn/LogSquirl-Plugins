# LogSquirl Plugin Registry

Central index of available [LogSquirl](https://github.com/64x-lunicorn/LogSquirl) plugins.

LogSquirl's **Browse Plugins** dialog fetches `plugins.json` from this repository
to display available plugins for one-click installation.

## How It Works

1. LogSquirl downloads
   `https://raw.githubusercontent.com/64x-lunicorn/LogSquirl-Plugins/main/plugins.json`
2. The dialog lists all plugins matching the current platform and API version.
3. Users click **Install** — the plugin ZIP is downloaded, verified (SHA-256),
   extracted, and loaded automatically.

## Adding a Plugin

Edit `plugins.json` and add an entry for each platform:

```json
{
  "id": "io.github.yourname.myplugin",
  "name": "My Plugin",
  "version": "1.0.0",
  "description": "Short description of what it does",
  "author": "Your Name",
  "download_url": "https://github.com/.../releases/download/v1.0.0/myplugin-1.0.0-macos-arm64.zip",
  "sha256": "abc123...",
  "platforms": ["macos"],
  "api_version": 1
}
```

**Fields:**

| Field | Required | Description |
|-------|----------|-------------|
| `id` | Yes | Reverse-DNS plugin identifier (must match `plugin.json` inside the ZIP) |
| `name` | Yes | Human-readable plugin name |
| `version` | Yes | SemVer version string |
| `description` | Yes | One-line description |
| `author` | Yes | Author or organization |
| `download_url` | Yes | Direct URL to the plugin ZIP archive |
| `sha256` | Yes | SHA-256 checksum of the ZIP (empty string disables verification) |
| `platforms` | Yes | Array of `"macos"`, `"linux"`, `"windows"` |
| `api_version` | Yes | Plugin API version (currently `1`) |

## Schema

`plugins.json` uses `schema_version: 1`:

```json
{
  "schema_version": 1,
  "plugins": [ ... ]
}
```

## Official Plugins

| Plugin | Description | Repo |
|--------|-------------|------|
| Android Logcat | Stream logcat from ADB devices | [LogSquirl-Logcat](https://github.com/64x-lunicorn/LogSquirl-Logcat) |
| Serial Monitor | Stream serial port data | [LogSquirl-Serial](https://github.com/64x-lunicorn/LogSquirl-Serial) |

## License

This repository is licensed under [GPL-3.0-or-later](https://www.gnu.org/licenses/gpl-3.0.html).
