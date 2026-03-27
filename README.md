# LogSquirl Plugin Registry

[![Validate plugins.json](https://github.com/64x-lunicorn/LogSquirl-Plugins/actions/workflows/validate.yml/badge.svg)](https://github.com/64x-lunicorn/LogSquirl-Plugins/actions/workflows/validate.yml)
[![License: GPL-3.0-or-later](https://img.shields.io/badge/License-GPL--3.0--or--later-blue.svg)](LICENSE)

Central index of available [LogSquirl](https://github.com/64x-lunicorn/LogSquirl)
plugins.  LogSquirl's **Browse Plugins** dialog fetches `plugins.json` from this
repository to list and install plugins with a single click.

> **This repo contains only the plugin index — no plugin source code.**
> Each plugin lives in its own repository and publishes release ZIPs.
> This registry points LogSquirl to those ZIPs.

---

## How It Works

```
┌──────────────┐       GET plugins.json        ┌─────────────────────┐
│  LogSquirl   │  ─────────────────────────►    │  LogSquirl-Plugins  │
│  (app)       │                                │  (this repo)        │
└──────┬───────┘                                └─────────────────────┘
       │
       │  download_url from matching entry
       ▼
┌──────────────────────────────┐
│  Plugin Release ZIP          │
│  (e.g. LogSquirl-Logcat)     │
│  github.com/.../releases/... │
└──────────────────────────────┘
```

1. LogSquirl downloads `plugins.json` from this repo's `main` branch.
2. The dialog filters entries by platform and `api_version`.
3. User clicks **Install** → the plugin ZIP is downloaded, verified
   (SHA-256), extracted, and loaded automatically.

---

## Adding or Updating a Plugin

All changes go through **pull requests**.  Direct pushes to `main` are not
allowed.

### Step-by-Step

1. **Fork** this repository (or create a branch if you have write access).

2. **Edit `plugins.json`** — add one entry per platform your plugin supports:

   ```json
   {
     "id": "io.github.yourname.myplugin",
     "name": "My Plugin",
     "version": "1.0.0",
     "description": "Short description of what it does",
     "author": "Your Name",
     "download_url": "https://github.com/.../releases/download/v1.0.0/myplugin-1.0.0-macos-arm64.zip",
     "sha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
     "platforms": ["macos"],
     "api_version": 1
   }
   ```

3. **Open a pull request** against `main`.

4. **CI validates** your change automatically:
   - `plugins.json` must be valid JSON
   - Every entry must have all required fields
   - `id` must be a reverse-DNS identifier
   - `version` must be a valid SemVer string
   - `download_url` must be a valid HTTPS URL
   - `platforms` must contain only `"macos"`, `"linux"`, or `"windows"`
   - `api_version` must be a supported version (currently `1`)

5. A maintainer **reviews and merges** the PR.  The updated index is
   live immediately (served via GitHub raw content).

### Updating an Existing Plugin

To publish a new version, update the `version` and `download_url` (and
`sha256`) of the existing entries for your plugin.  Do **not** remove
older entries — users on older LogSquirl versions may still reference them.

### Removing a Plugin

Open a PR that removes your plugin's entries from `plugins.json` and
explain the reason in the PR description.

---

## Entry Schema

`plugins.json` uses `schema_version: 1`:

```json
{
  "schema_version": 1,
  "plugins": [ ... ]
}
```

### Required Fields

| Field          | Type       | Description |
|----------------|------------|-------------|
| `id`           | `string`   | Reverse-DNS plugin identifier (must match `plugin.json` inside the ZIP) |
| `name`         | `string`   | Human-readable plugin name |
| `version`      | `string`   | SemVer version (e.g. `"1.0.0"`, `"0.2.0-beta.1"`) |
| `description`  | `string`   | One-line description |
| `author`       | `string`   | Author name or organization |
| `download_url` | `string`   | Direct HTTPS URL to the plugin ZIP archive |
| `sha256`       | `string`   | SHA-256 hex digest of the ZIP (empty string disables verification) |
| `platforms`    | `string[]` | One or more of `"macos"`, `"linux"`, `"windows"` |
| `api_version`  | `number`   | Plugin API version the plugin was built against (currently `1`) |

### SHA-256 Checksum

Generate the checksum for your release ZIP:

```bash
# macOS / Linux
shasum -a 256 myplugin-1.0.0-macos-arm64.zip

# Windows (PowerShell)
Get-FileHash myplugin-1.0.0-macos-arm64.zip -Algorithm SHA256
```

---

## Official Plugins

| Plugin | Version | Description | Repo |
|--------|---------|-------------|------|
| Android Logcat | 0.2.0 | Stream logcat from ADB devices | [LogSquirl-Logcat](https://github.com/64x-lunicorn/LogSquirl-Logcat) |
| Serial Monitor | 0.2.0 | Stream serial port data | [LogSquirl-Serial](https://github.com/64x-lunicorn/LogSquirl-Serial) |

---

## Repository Structure

```
LogSquirl-Plugins/
├── .github/
│   ├── workflows/
│   │   └── validate.yml          # CI: validates plugins.json on every PR
│   └── PULL_REQUEST_TEMPLATE.md  # PR checklist for plugin submissions
├── .gitignore
├── CONTRIBUTING.md               # Contribution guidelines
├── LICENSE                       # GPL-3.0-or-later
├── README.md                     # This file
└── plugins.json                  # The plugin index (the only file that matters)
```

## License

This repository is licensed under the
[GNU General Public License v3.0 or later](LICENSE).
