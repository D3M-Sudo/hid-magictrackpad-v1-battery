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
