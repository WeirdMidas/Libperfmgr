# Libperfmgr
**- A user-space libperfmgr in Magisk module format that aims to be as close as possible to Google's libperfmgr.**

## Main Functions

- Reads Android touchscreen input signals from the Linux layer, recognizing clicks and swipes.
- Monitors cpuset group update operations to recognize app switching.
- It only supports Android 10 or higher.
- It only supports ARM64.
- It currently only supports Snapdragon SoCs.
- SELinux maintains `enforcing` in most cases unless it's in heavily modified ROMs.
- Does not depend on any Android application layer framework or third-party kernel.
- Provides parameter-tuned configuration hints for most popular hardware platforms. Which currently supports three hints: INTERACTION and LAUNCH.
  - In the future, we will try to add the hints for EXPANSIVE_RENDERING and SUSTAINED_PERFORMANCE, to bring it even closer to Google's libperfmgr.
- It doesn't consume background resources, it was built to have zero overhead, and it's only capable of triggering hints in the overall user experience.

## Requirements

- A non-unstable ROM or custom kernel
- Install via Magisk (or another root manager, but compatibility is not guaranteed)
- Have busybox installed (Optional)

## Credits
```
@MattYang
Some methods from Uperf were taken from here to implement in libperfmgr.
```