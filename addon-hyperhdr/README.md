# HyperHDR

[HyperHDR](https://github.com/awawa-dev/HyperHDR) provides ambient lighting based on video and audio analysis, including HDR tone mapping.

This app is maintained in [protyposis/ha-addons-hyperhdr](https://github.com/protyposis/ha-addons-hyperhdr). Install it from `https://github.com/protyposis/ha-addons-hyperhdr` in Home Assistant's app/add-on store. Images for amd64 and aarch64 are hosted publicly on GHCR; no Docker Hub account is required.

## Migrating from upstream

**Back up the entire Home Assistant configuration directory's `hyperhdr` folder and stop the upstream app before starting this fork.** Both apps use the same `/config/hyperhdr` directory. Do not run both at once, and do not downgrade a newer HyperHDR database.

Follow the [complete migration and rollback instructions](https://github.com/protyposis/ha-addons-hyperhdr#migrate-from-noname93dha-addons) to preserve settings, calibration, and custom files. Home Assistant app-level settings and automations using the old app ID must be transferred separately.

## Credits

[awawa-dev](https://github.com/awawa-dev/HyperHDR), [NoName93D](https://github.com/NoName93D/ha-addons), and [ihrapsa](https://github.com/ihrapsa/hassio-addons).
