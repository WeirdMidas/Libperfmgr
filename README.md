# Libperfmgr
A version of libperfmgr that runs via daemon on any SOC that has it installed, allowing devices to replace their Boost Framework solutions with this AOSP solution.

## Overview
A user-space libperfmgr project, allowing Snapdragon, MediaTek, and other SoCs that don't natively have libperfmgr to manually add it. Since it's a "user-space" version, there are still complications, so check out more about the project in the repository.

## Main Features
- Ability to add any sysfs to hints/nodes equal to the original libperfmgr
- Hints similar to Google's Libperfmgr. But, for now: only TOUCH_SLOP, INTERACTION and LAUNCH are supported
- Replaces the ROM's default boost framework with our user-space libperfmgr

## Requirements

- Android 8 or higher
- A non-unstable ROM or custom kernel
- Magisk (or another root manager, but be aware that it may not be as compatible)
- Only Qualcomm SOCs are currently supported
- Have busybox installed (Optional)

## Credits
```
@MattYang
Some methods from Uperf were taken from here to implement in libperfmgr.
```