# Contributing to LogSquirl-Plugins

Thank you for your interest in contributing to the LogSquirl plugin registry!

## How It Works

This repository contains `plugins.json` — the central index that LogSquirl uses to discover
and install plugins. Every plugin listed here is available to all LogSquirl users through the
built-in plugin manager.

## Adding a New Plugin

1. **Build your plugin** against the [LogSquirl Plugin API](https://github.com/64x-lunicorn/LogSquirl).
2. **Create a GitHub Release** (or other HTTPS-accessible host) with a ZIP archive
   containing your compiled plugin for each platform you support.
3. **Fork this repository** and add an entry to `plugins.json` for each platform build.
4. **Open a Pull Request** — the PR template will guide you through the checklist.

### Entry Format

Each entry in the `plugins` array must contain:

| Field          | Type     | Description                                         |
|----------------|----------|-----------------------------------------------------|
| `id`           | string   | Reverse-DNS identifier (e.g. `io.github.logsquirl.serial`) |
| `name`         | string   | Human-readable display name                         |
| `version`      | string   | SemVer version (e.g. `1.0.0`)                      |
| `description`  | string   | Short description of what the plugin does           |
| `author`       | string   | Author or organization name                         |
| `download_url` | string   | HTTPS URL to the ZIP archive                        |
| `sha256`       | string   | SHA-256 hash of the ZIP file                        |
| `platforms`    | string[] | Target platforms: `macos`, `linux`, `windows`       |
| `api_version`  | integer  | Plugin API version the plugin was built against     |

### SHA-256 Hash

Generate the hash for your ZIP file:

```bash
# macOS / Linux
shasum -a 256 your-plugin.zip

# Windows (PowerShell)
Get-FileHash your-plugin.zip -Algorithm SHA256
```

## Updating an Existing Plugin

1. Update the `version`, `download_url`, and `sha256` fields for each platform entry.
2. Open a PR with a clear description of what changed.

## Removing a Plugin

Open a PR that removes the corresponding entries from `plugins.json` and explain why.

## Guidelines

- **One plugin per PR** — keep changes focused and reviewable.
- **HTTPS only** — all download URLs must use HTTPS.
- **Stable URLs** — use GitHub Releases or similar permanent hosting.
  Do not use URLs that might change or expire.
- **Accurate hashes** — the SHA-256 hash must match the file at the download URL.
  LogSquirl verifies this before installing.
- **Supported API** — your `api_version` must match a version supported by the current
  LogSquirl release.

## CI Validation

Every PR triggers a GitHub Actions workflow that validates `plugins.json`:

- Valid JSON syntax
- All required fields present and correct types
- `id` is reverse-DNS format
- `version` is valid SemVer
- `download_url` starts with `https://`
- `platforms` only contain recognized values
- `api_version` is a positive integer

Your PR must pass validation before it can be merged.

## Questions?

Open an [issue](https://github.com/64x-lunicorn/LogSquirl-Plugins/issues) if you have
questions about the plugin registry or need help getting your plugin listed.
