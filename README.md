Exynos 9820/9825 Repartitioner & Partition Cleaner

Recovery-flashable ZIPs for Samsung Exynos 9820/9825 devices that resize and recreate partitions to provide enough space for modern One UI 5+ port ROMs and GSIs.

«[!CAUTION]
This process completely wipes the device, including internal storage.

Back up everything before proceeding. You are responsible for any damage or data loss caused by using these ZIPs.»

---

Supported Devices

For Exynos variants only:

- Galaxy S10 series — Exynos 9820
- Galaxy Note10 series — Exynos 9825

Do not flash on Snapdragon models.

---

Supported ROMs

Primarily intended for One UI 5 and newer port ROMs whose system, vendor, or product partitions exceed the stock layout.

Examples include:

- ExtremeROM
- ApricityROM
- ArtisanROM
- AzROM
- Other One UI 5+ ports requiring additional partition space

It can also be used when additional space is required for GSIs or heavily modified ROMs.

---

Requirements

- **ExtremeXT's TWRP** for Exynos 9820  
  [Download from GitHub Releases](https://github.com/emadhamid7/android_device_samsung_exynos9820/releases)
- Unlocked bootloader
- Complete backup of your data
- Battery charged to at least 50%
- A PC with Odin
- Stock firmware for your exact model/region
- Matching device PIT file

«[!IMPORTANT]
The repartitioner is designed for ExtremeXT's TWRP. Other recovery builds may not work correctly.»

---

Downloads

ZIPs are available from the "Releases" (../../releases) page.

ZIP| Description
"Exynos9820_Repartitioner.zip"| Applies the default partition layout
"Exynos9820_Repartitioner_Custom.zip"| Applies a custom "system", "vendor", and "product" layout
"Exynos9820_cleaner.zip"| Formats the newly created partitions

The repartitioner resizes/recreates:

"system" · "vendor" · "product" · "odm" · "prism" · "optics" · "cache" · "omr" · "cp_debug" · "cp2_debug" · "spu" · "userdata"

The cleaner formats the resulting partitions using ext4, with f2fs for "userdata".

---

Default Partition Layout

Sizes are in MiB.

Partition| Size
"system"| 7000
"vendor"| 1500
"product"| 1500
"odm"| 4
"prism"| 600
"optics"| 30
"cache"| 600
"omr"| 50
"cp_debug"| 5
"cp2_debug"| 5
"spu"| 50
"userdata"| Remaining space

The Custom version allows the "system", "vendor", and "product" sizes to be changed to suit larger ROMs.

---

Flashing

Do not boot Android between these steps.

1. Boot into TWRP

Boot into ExtremeXT's TWRP.

2. Flash the Repartitioner

Flash either:

- "Exynos9820_Repartitioner.zip"
- "Exynos9820_Repartitioner_Custom.zip"

The ZIP provides a 15-second abort window before modifying the partition table.

If you need to abort during this window, hard reboot with:

Volume Down + Power

Once the operation completes, the device automatically reboots back into recovery.

3. Flash the Cleaner

Flash:

"Exynos9820_cleaner.zip"

The device will reboot to recovery again after the cleaner finishes.

4. Flash your ROM

Now flash your ROM or GSI normally.

Flash order

1. Repartitioner
2. Cleaner
3. ROM / GSI

«[!IMPORTANT]
The cleaner must be flashed before the ROM. The newly created partitions are not ready for normal ROM installation until they have been formatted.»

---

Restoring the Stock Partition Layout

The original Samsung partition layout cannot be restored from recovery.

To return to the stock layout, use Odin with the PIT from your device:

1. Download the stock firmware for your exact model and region.
2. Obtain the matching PIT file.
3. Boot the phone into Download Mode.
4. Open Odin on a PC.
5. Load the stock firmware:
   - BL
   - AP
   - CP
   - CSC
6. Load the matching PIT in Odin's PIT tab.
7. Enable Re-Partition.
8. Flash the firmware.

«[!CAUTION]
Never use a PIT intended for another device or storage configuration. A wrong PIT can cause serious problems or leave the device unable to boot.»

---

Troubleshooting

Repartitioner flashed, but Android no longer boots

This is expected if you have not yet flashed the cleaner and ROM.

Return to recovery and flash:

Repartitioner → Cleaner → ROM

Recovery looks incorrect or the ZIP fails

Verify that you are using ExtremeXT's TWRP.

The ROM does not fit

Use the Custom repartitioner and increase the required "system", "vendor", or "product" partition size.

Make sure the total allocation remains within the physical storage capacity of the device.

Device bootloops after completing the process

Try reinstalling in the correct order:

Repartitioner → Cleaner → ROM

If the problem persists, restore the stock partition layout using Odin and the correct PIT.

I changed my mind after flashing the repartitioner

If the ZIP is still within its 15-second abort window, hard reboot with:

Volume Down + Power

After the partition table has been modified, restoration requires Odin + the correct PIT.

---

Credits

- @Astrako — Original repartitioner
- @xxmustafacooTR and @JeyKul — 9810 adaptation
- @ExtremeXT and @david42069 — 9820 adaptation