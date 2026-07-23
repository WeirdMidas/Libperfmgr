# Libperfmgr
**A Boost Framework for Android that allows you to replace the ROM's Boost Framework and implement a solution similar to Libperfmgr on Google Pixel devices.**

## Main Functions

- Dynamically set parameters to control performance release based on the identified scenario type, supporting all `sysfs` nodes. For now, there are only three supported scenarios: INTERACTION, TOUCH_SLOP, and LAUNCH.
- Reads Android touchscreen input signals from the Linux layer, recognizing clicks and swipes.
- Monitors cpuset group update operations to recognize app switching.
- Supports Android 10 or higher.
- It only supports ARM64.
- Supports one-click installation via Magisk, version 28.4 and above.
- Does not depend on any Android application layer framework or third-party kernel.

## WARNING! 

Do not use the module without knowing this:
- Download the module's zip file and extract the powerhint.json file (located in the module's /bin folder). From there, make the modifications you want, preferably modifying the cluster frequencies to your desired level, changing the paths to the correct ones, etc.
- This module is not a "install and forget" module; this manual intervention is necessary to avoid using parameters incompatible with your SOC.

## Credits
```
@MattYang
Some methods from Uperf were taken from here to implement in libperfmgr.
```