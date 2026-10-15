# Exynos 9820/9825 Repartitioner & Partition Cleaner

Open-source, recovery-flashable ZIPs for Samsung Exynos 9820/9825 devices (Galaxy S10 / Note10 series). Everything is built from the source in this repo by GitHub Actions, so you can read exactly what runs on your phone.

> [!CAUTION]
> **This COMPLETELY DESTROYS ALL DATA on the device.** Back up everything first (including internal storage). You flash this at your own risk.

## Downloads

Grab the latest ZIPs from the [Releases](../../releases) page:

| Release | Contents |
| --- | --- |
| `Exynos-9820-Repartitioner` | Default sizes |
| `Exynos-9820-Repartitioner-Custom` | Custom System / Vendor / Product sizes |

## What's in it

| ZIP | What it does |
| --- | --- |
| `Exynos9820_Repartitioner.zip` | Resizes `system` and recreates `vendor`, `product`, `odm`, `prism`, `optics`, `cache`, `omr`, `cp_debug`, `cp2_debug`, `spu` and `userdata`. |
| `Exynos9820_Repartitioner_Custom.zip` | Same as above, built with your own `system`, `vendor` and `product` sizes. |
| `Exynos9820_cleaner.zip` | Formats the new partitions (ext4, and f2fs for `userdata`). |

## Installation

1. Boot to recovery.
2. Flash the **Repartitioner** ZIP (default or custom). You get a 15 second window to abort by hard rebooting (hold **Volume Down + Power**) before anything is touched. The phone reboots to recovery when it finishes.
3. Flash **`Exynos9820_cleaner.zip`**. The phone reboots to recovery again.
4. Flash your ROM as usual.

> [!IMPORTANT]
> Always flash the Repartitioner first, then the cleaner. Do not skip the cleaner.

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

There are two workflows. Go to the **Actions** tab, pick one, then **Run workflow**.

| Workflow | Inputs | Output |
| --- | --- | --- |
| **Build flashable ZIPs** | Publish release (optional) | `Exynos9820_Repartitioner.zip`, `Exynos9820_cleaner.zip` |
| **Build flashable ZIPs (Custom sizes)** | System, Vendor, Product (MiB), Publish release (optional) | `Exynos9820_Repartitioner_Custom.zip`, `Exynos9820_cleaner.zip` |

- Sizes must be whole numbers in MiB and greater than 0. Make sure the total fits on your device.
- Only `system`, `vendor` and `product` can be set from the workflow. To change any other partition, edit `repartitioner/extreme/repartition.sh` directly.
- Tick **Publish a GitHub release** to publish the ZIPs and a `SHA256SUMS.txt`. Each release is updated in place.
- A push to `main` runs the default workflow and publishes it automatically.
- Every run also uploads the ZIPs as a build artifact, and the chosen sizes are shown in the run summary.

## Repo layout

```
.github/workflows/
  build.yml                          default build + release
  build-custom.yml                   custom sizes build + release
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

Both workflows stop with a clear error if any of these are missing:

- `repartitioner/extreme/jq` and `repartitioner/extreme/parted`: arm64 builds that run in recovery.
- `update-binary` in both `META-INF/com/google/android/` folders: the edify interpreter that runs `updater-script` (for example, the one from any existing flashable ZIP).

## Credits

- Written by @Astrako
- Adapted to 9810 by @xxmustafacooTR and @JeyKul
- Adapted to 9820 by @ExtremeXT and @david42069

## License

Apache License 2.0. See `LICENSE`.
