# Elevra release repo

Hosts the Elevra Windows installer and the update manifest that the desktop
app polls to know when a newer build is available.

## Layout

- `latest.json` — update manifest read by `updater.py` in the app.
- `ElevraSetup.exe` (added on each release) — the installer asset.

## Manifest schema

```json
{
  "version": "1.1.0",
  "notes": "What changed in this release",
  "installer_url": "https://github.com/TheRedOps/elevra-release/releases/latest/download/ElevraSetup.exe",
  "sha256": "<hex sha256 of the installer, optional>",
  "published_at": "2026-08-14"
}
```

The app compares `version` (a semver-ish string) against its own
`APP_VERSION` in `app.py` and prompts the user to update when the manifest is
newer.

## How the app finds it

The updater is configured through `ELEVRA_UPDATE_URL` (a URL or a local file
path — handy for testing) or the baked-in `DEFAULT_MANIFEST_URL` in
`updater.py`. Point either at the raw URL for this repo's `latest.json`, e.g.:

```
https://raw.githubusercontent.com/TheRedOps/elevra-release/main/latest.json
```

## For local testing

Serve the manifest locally and point the app at it:

```powershell
$env:ELEVRA_UPDATE_URL = "C:\path\elevra\release\latest.json"
python elevra.py --ui
```

Bump `version` in `release/latest.json` above `APP_VERSION` to see the update
prompt. Run the app with `ELEVRA_UPDATE_OFF=1` to disable the checker.