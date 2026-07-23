# Libperfmgr
**A libperfmgr in user space, allowing the user to customize their own hints to the scheduler.**

## Main Functions

- Reads Android touchscreen input signals from the Linux layer, recognizing clicks and swipes.
- Monitors cpuset group update operations to recognize app switching.
- It only supports Android 10 or higher.
- It only supports ARM64.
- It currently only supports Snapdragon SoCs.
- SELinux maintains `enforcing` in most cases unless it's in heavily modified ROMs.
- Does not depend on any Android application layer framework or third-party kernel.
- Provides parameter-tuned configuration files for most popular hardware platforms. Currently supported are three hints: INTERACTION, TOUCH_SLOP, and LAUNCH.

## Requirements

- A non-unstable ROM or custom kernel
- Install via Magisk (or another root manager, but compatibility is not guaranteed)
- Have busybox installed (Optional)

## Credits
```
@MattYang
Some methods from Uperf were taken from here to implement in libperfmgr.
```