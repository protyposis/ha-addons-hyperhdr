# HyperHDR for Home Assistant

[HyperHDR](https://github.com/awawa-dev/HyperHDR) provides ambient lighting based on video and audio analysis, including HDR tone mapping.

## Installation

Requires Home Assistant with Supervisor (for example, Home Assistant OS), on amd64 or aarch64.

[![Add this repository to Home Assistant](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fprotyposis%2Fha-addons-hyperhdr)

Or add `https://github.com/protyposis/ha-addons-hyperhdr` under **Settings → Apps → App store → ⋮ → Repositories** (called **Add-ons → Add-on Store** in older Home Assistant versions). Refresh the store, select HyperHDR from **protyposis Home Assistant Apps**, and install it. Open its web UI on port 8090.

Images must be published and public before installation; see below.

## Publishing images

Docker Hub is not required. Home Assistant pulls the public images directly from GitHub Container Registry (GHCR):

- `ghcr.io/protyposis/hyperhdr-addon-amd64:<version>`
- `ghcr.io/protyposis/hyperhdr-addon-aarch64:<version>`

The **Build HyperHDR** workflow builds both architectures on native runners. Pull requests only build; pushes to `master` affecting the app/workflow publish the version in `addon-hyperhdr/config.json`. You can also run the workflow manually on `master`. It uses `GITHUB_TOKEN` with `packages: write`; no Docker Hub account or extra registry secret is needed. Enable GitHub Actions in the fork if disabled.

After the first publish, open each package under the GitHub account's **Packages**, then **Package settings → Change visibility → Public**. Public repository visibility does not automatically make a new GHCR package public. Verify both version tags can be pulled without credentials before installing in Home Assistant. If an existing package rejects the workflow token, grant this repository Actions access in that package's settings.

The workflow does not automatically upgrade HyperHDR or commit version changes. The configured version must match an upstream `v<version>` release with Bookworm tarballs for both architectures. The image uses a versioned Home Assistant Debian Bookworm base and checks that the downloaded HyperHDR executable can run during the build. Review upgrades explicitly before publishing, especially when sharing existing settings.

## Migrate from NoName93D/ha-addons

The upstream app maps Home Assistant's shared configuration directory to `/config` and starts HyperHDR with `-u /config/hyperhdr`. This fork deliberately keeps the same mapping, path, and HyperHDR version. **The two apps therefore use the same settings files**, despite having different repository-specific app IDs. Do not run them simultaneously.

1. Export a configuration backup from the running upstream HyperHDR web UI. Record its installed HyperHDR version and Home Assistant app settings (network port mappings, start on boot, watchdog, protection mode, and any local customizations).
2. Disable **Start on boot** and **Watchdog** for upstream, then stop it. With HyperHDR stopped, copy the **entire** Home Assistant configuration folder's `hyperhdr` directory to a safe location outside that directory. Include hidden files, the database, calibration/LUT files, and any custom files. This is `/config/hyperhdr` in the upstream app and in editors exposing Home Assistant's configuration directory; other tools may call the parent `/homeassistant`. Also create and download a Home Assistant backup including **Home Assistant configuration** and the upstream app. An app-only backup is not sufficient for this shared directory.
3. Confirm this fork's HyperHDR version matches the installed upstream version before starting it. This fork initially uses `21.0.0.0`. If versions differ, plan and back up for that upgrade separately; do not downgrade a newer configuration database.
4. Add this repository and install its HyperHDR app. Keep upstream installed but stopped. Copy the recorded app-level settings to the fork; they do not transfer automatically because the app ID changes. Any automations referencing the old app ID must also be updated.
5. Start only the fork. It should load the existing `/config/hyperhdr` directory without an import. Check all instances, LED layout, capture device, serial output, calibration, effects, and any authentication/network integrations. Restore any externally stored custom files to their original paths if necessary.
6. Enable start on boot/watchdog on the fork once verified. Only then optionally uninstall upstream, retaining your backups and the shared `hyperhdr` directory.

**Rollback:** stop the fork and disable its start-on-boot/watchdog first. If settings were changed or a newer HyperHDR version touched the database, restore the complete stopped-state directory backup before restarting upstream. Never start both against the shared database. A backup restore of the upstream app does not automatically convert it into the fork's app ID.

## Credits

- [awawa-dev](https://github.com/awawa-dev/HyperHDR) for HyperHDR
- [NoName93D](https://github.com/NoName93D/ha-addons) for the upstream Home Assistant app
- [ihrapsa](https://github.com/ihrapsa/hassio-addons) for the original add-on
