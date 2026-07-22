# Libperfmgr
A version of libperfmgr that runs via daemon on any SOC that has it installed, allowing devices to replace their Boost Framework solutions with this AOSP solution.

## Overview
A user-space libperfmgr project, allowing Snapdragon, MediaTek, and other SoCs that don't natively have libperfmgr to manually add it. Since it's a "user-space" version, there are still complications, so check out more about the project in the repository.

## Main Features
- The ability to insert any sysfs into powerhint.json, allowing you to tailor the performance and efficiency needs of your individual SOC based on your own sysfs
- Addition of hints, allowing the user to freely customize the hints they want, with the initial addition of two: INTERACTION and LAUNCH
- Disabling the Boost Framework in the ROM to make way for libperfmgr in user space

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