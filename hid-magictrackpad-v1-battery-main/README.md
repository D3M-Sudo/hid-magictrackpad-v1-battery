# Apple Magic Trackpad v1 Battery Support for Linux Kernel  
   
This repository contains a kernel patch to enable battery reporting for the **Apple Magic Trackpad v1 (A1339)**.  
   
## Development Transparency
In compliance with evolving community standards regarding AI-assisted development:
- **AI Assistance:** Claude AI was used for initial code boilerplate and HID descriptor analysis.
- **Human Verification:** All logic, Report IDs (0x47), and power_supply structures have been manually verified via `debugfs` and `hidraw` traces.
- **Maintainer:** [Damiano Gragnaniello]

## The Problem  
While newer Magic Trackpad 2 models have native battery support in the `hid-magicmouse` driver, the original v1 (Bluetooth, AA batteries) was excluded. Users couldn't see the battery percentage in GNOME/KDE settings or `upower`.  
   
## The Solution  
The patch adds:  
- Detection of Device ID `0x030e`.  
- Parsing of **Report ID 0x47**, which carries the battery level.  
- Registration of a `power_supply` interface.  
   
## Hardware Validation (Real Data)  
Validated on Linux Mint with Kernel 6.17. Raw HID Descriptor analysis:  
- **Feature Report (71)**: `GenericDeviceControls.BatteryStrength`  
- **Logical Range**: 0 - 100  
- **Report Size**: 8 bits  
   
```bash  
# Capture from /sys/kernel/debug/hid/*/rdesc  
FEATURE(71)[FEATURE]  
  Usage(1) GenericDeviceControls.BatteryStrength  
  Logical Minimum(0)  
  Logical Maximum(100)

## How to Test and Apply

### 1. Manual Compilation (The "Quick" Way)
If you want to test the module without recompiling the entire kernel:
1. Ensure you have kernel headers installed: `sudo apt install linux-headers-$(uname -r)`
2. Download `hid-magicmouse.c` from this repo.
3. Use a standard `Makefile` to build the `.ko` (kernel object) file.
4. Unload the current driver and load yours:
   ```bash
   sudo rmmod hid_magicmouse
   sudo insmod hid-magicmouse.ko

### 2. Official Patching (Recommended)
If you are a developer or want to include this in a custom build:
1. Clone the Linux kernel source.
2. Apply the patch: git am 0001-HID-magicmouse-add-battery-reporting-for-Magic-Track.patch
3. Rebuild the hid-magicmouse module.

### Stability & Safety
Stability Test: This code has been stress-tested for 7 days on Linux Mint (Cinnamon) without system freezes or kernel panics.

Disclaimer: Always back up your data before loading experimental kernel modules.
Use dmesg -w to monitor system logs during the first load.
