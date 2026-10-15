# Exynos 9820/9825 Repartitioner & Partition Cleaner

Open-source, recovery-flashable ZIPs for Samsung Exynos 9820/9825 devices (S10 / Note10 series). Everything is built from the source in this repo by GitHub Actions, so you can read exactly what runs on your phone.

> **WARNING: this COMPLETELY DESTROYS ALL DATA on the device.** Back up everything first. You flash this at your own risk.

## What's in it

| ZIP | What it does |
| --- | --- |
| `Exynos9820_Repartitioner.zip` | Resizes `system` and recreates `vendor`, `product`, `odm`, `prism`, `optics`, `cache`, `omr`, `cp_debug`, `cp2_debug`, `spu` and `userdata` using the sizes in `repartition.sh`. |
| `Exynos9820_Partition_Cleaner.zip` | Formats the new partitions (ext4, and f2fs for `userdata`). |

## Usage

1. Flash **Exynos9820_Repartitioner.zip** in recovery. There is a 15 second window to hard reboot (hold VOLUME DOWN + POWER) before anything is touched. The phone reboots to recovery when it finishes.
2. Flash **Exynos9820_Partition_Cleaner.zip**. The phone reboots to recovery again.

Downloads are on the [Releases](../../releases) page.

## Default sizes (MiB)

| Partition | Size |
| --- | --- |
| system | 7000 |
| vendor | 1500 |
| product | 1500 |
| odm | 4 |
| prism | 600 |
| optics | 30 |
| cache | 600 |
| omr | 50 |
| cp_debug / cp2_debug | 5 |
| spu | 50 |

`userdata` takes all remaining space.

## Building

Go to **Actions → Build flashable ZIPs → Run workflow** and pick a mode:

- **default**: builds with the sizes above.
- **manual**: builds with the values you type into the size fields (whole numbers, in MiB). The output is named `Exynos9820_Repartitioner_Custom.zip`. Only change these if you know what you're doing, and make sure the total fits on your device.

Tick **Publish a GitHub release** to publish the ZIPs. Default builds go to the `Exynos-9820-Repartitioner` release and manual builds go to `Exynos-9820-Repartitioner-Custom`; each is updated in place. A push to `main` builds the default ZIPs and publishes them.

## Repo layout

```
.github/workflows/build.yml          build + release workflow
repartitioner/
  META-INF/com/google/android/
    update-binary                    edify interpreter (add this, see below)
    updater-script
  extreme/
    repartition.sh
    jq                               static arm64 binary (add this)
    parted                           static arm64 binary (add this)
cleaner/
  META-INF/com/google/android/
    update-binary                    edify interpreter (add this, see below)
    updater-script
  extreme/
    cleanup.sh
```

### Binaries you need to add

The workflow stops with a clear error if any of these are missing:

- `repartitioner/extreme/jq` and `repartitioner/extreme/parted`: arm64 builds that run in recovery.
- `update-binary` in both `META-INF/com/google/android/` folders: the edify interpreter that runs `updater-script` (for example, the one from any existing flashable ZIP).

## Credits

- Written by @Astrako
- Adapted to 9810 by @xxmustafacooTR and @JeyKul
- Adapted to 9820 by @ExtremeXT and @david42069

## License

Apache License 2.0. See `LICENSE`.
