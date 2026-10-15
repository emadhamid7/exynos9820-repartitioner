# Exynos 9820/9825 Repartitioner & Partition Cleaner

Recovery-flashable ZIPs that resize and recreate the partitions on Samsung Exynos 9820/9825 devices, so modern **One UI 5 and newer** port ROMs or GSIs fit.

> [!CAUTION]
> **This entirely wipes all your data on the device, including internal storage.** Back up everything first. Flash this at your own risk.

---

## Supported ROMs

Made for One UI 5 and up **port ROMs** for the S10 / Note10 family, such as:

- ExtremeROM
- ApricityROM
- ArtisanROM
- AzROM
- ...and any other One UI 5+ port that needs more room than the stock partition layout provides

Stock partitions are too small for some newer, larger system images. Repartitioning gives them the space they need.

## What else is it useful for

- Expanding partitions to flash **GSIs**
- Giving `system`, `vendor` and `product` more room for large or heavily modified ROMs
- Starting from a clean, freshly formatted partition layout before installing a new ROM

## Supported devices

Samsung **Exynos 9820 / 9825** devices:

- Galaxy S10 series (Exynos 9820)
- Galaxy Note10 series (Exynos 9825)

**Exynos variants only. Do not flash this on Snapdragon models.**

---

## Prerequisites

- **ExtremeXT's version of TWRP.** These ZIPs are designed to be flashed with it, not stock recovery or other TWRP builds. [Download here](https://github.com/emadhamid7/android_device_samsung_exynos9820/releases)
- An unlocked bootloader
- A full backup of everything you care about
- A charged battery (50% or more recommended)
- A PC with Odin, plus your device's stock firmware and matching PIT file, in case you need to restore the stock partition layout

## Downloads

Get the ZIPs from the [Releases](../../releases) page:

| Release | Contents |
| --- | --- |
| `Exynos-9820-Repartitioner` | Default sizes |
| `Exynos-9820-Repartitioner-Custom` | Custom System / Vendor / Product sizes |

| ZIP | What it does |
| --- | --- |
| `Exynos9820_Repartitioner.zip` | Resizes `system` and recreates `vendor`, `product`, `odm`, `prism`, `optics`, `cache`, `omr`, `cp_debug`, `cp2_debug`, `spu` and `userdata` |
| `Exynos9820_Repartitioner_Custom.zip` | Same, built with custom `system`, `vendor` and `product` sizes |
| `Exynos9820_cleaner.zip` | Formats the new partitions (`ext4`, with `f2fs` for `userdata`) |

---

## How to flash

**Flash in this exact order. Do not boot into system between steps.**

1. Boot into **ExtremeXT's TWRP**.
2. Flash **`Exynos9820_Repartitioner.zip`** (or the Custom one).
   - You get a **15 second window** to abort by hard rebooting (**Volume Down + Power**) before anything is touched.
   - The phone reboots to recovery when it finishes.
3. Flash **`Exynos9820_cleaner.zip`**.
   - The phone reboots to recovery again.
4. Flash your **ROM** (or GSI) as usual.

| When | What to flash |
| --- | --- |
| First | Repartitioner |
| Second | Cleaner |
| Last | Your ROM / GSI |

> [!IMPORTANT]
> Always flash the Repartitioner first, then the Cleaner, then your ROM. The Cleaner is required because the new partitions are unformatted until it runs.

## Default partition sizes (MiB)

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

---

## Undoing the repartition

The stock layout can't be restored from recovery. To go back:

1. Download the **stock firmware** for your exact model and region, and the matching **PIT file** for your device.
2. Boot into Download Mode and open **Odin** on your PC.
3. Load the stock firmware files (**BL / AP / CP / CSC**) as normal.
4. Load the **PIT file** in the PIT tab and enable **Re-Partition**.
5. Flash. This restores the original partition table.

> [!CAUTION]
> **Use the PIT that matches your exact device. A wrong PIT can brick it.**

---

## Tips & troubleshooting

- **Flashed the Repartitioner, then rebooted to system and it won't boot.** Expected. Go back to recovery and flash the Cleaner, then your ROM.
- **Recovery looks wrong or the ZIP errors out.** Make sure you're using ExtremeXT's TWRP from the link above.
- **Not enough space for your ROM or GSI.** Use the Custom build with larger `system`, `vendor` or `product` values. Keep the total within your device's storage capacity.
- **Stuck or bootlooping after everything.** Reflash the Repartitioner and Cleaner in order, then your ROM. If that fails, restore stock with Odin and the matching PIT.
- **Changed your mind after step 2?** Hard reboot during the 15 second window. After the partition table has been modified, you'll need Odin and the correct PIT to undo it.
- Keep a copy of your stock firmware and PIT file somewhere safe before you start.

---

## Credits

- Written by @Astrako
- Adapted to 9810 by @xxmustafacooTR and @JeyKul
- Adapted to 9820 by @ExtremeXT and @david42069
