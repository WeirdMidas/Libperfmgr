# Libperfmgr
![742b6dbaa02122922bf7b891265fde0c_1](https://github.com/user-attachments/assets/7581dec5-d7c5-4123-beba-e67c584dedfd)

A Magisk/KSU module made especially for Qualcomm devices with Adreno GPUs. The intention is to mimic the power-libperfmgr of Pixel phones and generate a module that imitates the performance, power consumption and adaptability of the Pixel engine but for different SOCs, taking advantage of Perfhal and the old QTI Boost Framework and making it adapt to the custom parameters of general optimizations, and the parameters specific to each SOC.

# 设计目标
The previous [Project WIPE](https://github.com/yc9559/cpufreq-interactive-opt), automatically adjust the `interactive` parameters via simulation and heuristic optimization algorithms, and working on all mainstream devices which use `interactive` as default governor. The recent [WIPE v2](https://github.com/yc9559/wipe-v2), improved simulation supports more features of the kernel and focuses on rendering performance requirements, automatically adjusting the `interactive`+`HMP`+`input boost` parameters. However, after the EAS is merged into the mainline, the simulation difficulty of auto-tuning depends on raise. It is difficult to simulate the logic of the EAS scheduler. In addition, EAS is designed to avoid parameterization at the beginning of design, so for example, the adjustment of schedutil has no obvious effect.  

[WIPE v2](https://github.com/yc9559/wipe-v2) focuses on meeting performance requirements when interacting with APP, while reducing non-interactive lag weights, pushing the trade-off between fluency and power saving even further. `QTI Boost Framework`, which must be disabled before applying optimization, is able to dynamically override parameters based on perf hint. This project utilizes `QTI Boost Framework` and extends the ability of override custom parameters. When launching APPs or scrolling the screen, applying more aggressive parameters to improve response at an acceptable power penalty. When there is no interaction, use conservative parameters, use small core clusters as much as possible, and run at a higher energy efficiency OPP under heavy load.  

Details see [the lead project](https://github.com/yc9559/sdm855-tune/commits/master) & [perfd-opt commits](https://github.com/yc9559/perfd-opt/commits/master)  

Overall, this is the explanation of Matt Yang's Perfd-opt. Full credit to him. Now, with the adaptation to current Androids, I was able to revive the module to work today. And now the module has a new look, it is basically to give the same feeling as the Google Pixel, but on other devices.

Libperfmgr, based on the same function as the Pixel. It is a module that tries to bring fluidity and energy saving limits and push them beyond what would otherwise be possible, providing performance and profiles for different users, and especially for specific Soc's. What does it mean? That the module has specific optimizations for each processor! In addition to the general optimizations, it will also have the profile optimizations that are specified for each processor.

Don't be alarmed, if you don't find your processor in the compatibility list, you can help to integrate it into the list.

# How to help...
- If your processor is not listed in the list, please, first: make an issue containing the nickname [integration of (name of your processor)]

And then, answer this form:

1. What is the frequency your processor uses to open these applications: Devcheck, RAR and any game that you consider heavy. And the available frequencies of your processor (which it transitions to, can be found in /sys/devices/system/cpu/cpufreq/policy*/scaling_available_frequencies and do this in ALL CLUSTERS, to help as much as possible). And show what is your DDR path and bw_hwmon max frequency.

2. What are its governor functions (which adjustable ones it has) and which governor does your kernel use as the main one (schedutil or interactive).

3. Name of your processor. And what is the energy efficiency cost it has when moving up and down (tell the minimum and maximum).

4. How many hours you last moving without turning off the screen.

5. What tunables do you have in /proc/sys/kernel?

6. What FPS do you achieve in your favorite game without having an interference module, be pure.

7. And finally, the name of your platform (which can be obtained via this command: getprop ro.board.platform).

# Additional help
If you want to help in another way, for example, recommending adjustments/tweaks, you can make an issue with the nickname [adjustment suggestion] and then show what the adjustment was, ALONG with any benchmark you have done, to test if you really got any improvement with the adjustment.

# Prerequisites
- Android 11-15. If you have a lower Android, it is not recommended to use the module because it is adapted for current Androids (where resource management has changed abruptly).
- Don't have a messy or too "mimic" kernel (like many custom rom or custom kernel that tend to hide, modify and trap certain optimizations for reasons of "it's to specialize in my kernel")
- Have a UFS storage. (Optional, it is just to ensure better I/O performance so that the module can be fully effective).
- Follow the compatibility list, do not force the module on devices with incompatible SOCs.
- Do not have too high expectations of the performance you will get, remember that this is a module that needs to adapt to many different users and socs, so some socs may have initial or full support.

# Compability List
- For now, the project only supports snapdragon. I want to master the efficiency of snapdragon to avoid future problems with mediatek and derivatives.

```
    sdm865 (Complete + Extra)
    sdm855/sdm855+ (Complete + Extra)
    sdm845 (Complete + Extra)
    sdm765/sdm765g (Complete + Extra)
    sdm730/sdm730g (Complete + Extra)
    sdm680 (Initial + Extra)
    sdm675 (Complete + Extra)
    sdm710/sdm712 (Complete + Extra)
```

# Explanation of each "Compatibility Stage":
- Initial: Does not have complete optimizations, is still in EXTREMELY EXPERIMENTAL phases! Needs extra feedback.
- Complete: In the profiles section, optimizations are complete, allowing the user to use the adjustments as they see fit.
- Initial + Extra: It is still in the testing phase but contains extra optimizations like Perfboost.
- Complete + Extra: In addition to the complete traditional optimizations, it comes with perfboost optimization. To ensure better response and agility.
- Specialized: Fully optimized, a SOC containing this indication is said to be fully optimized for maximum possible energy savings, compatibility and performance.

# Features
- Cleanup script, which applies these cleanups every 7-15 days:
    - Fstrim on cache, system, data and persist every 7 days.
    - Compile, sync and cleanup of ART every 7 days.
    - Cleans cache and junk from apps every 7 days.
    - Applies Vacuum, Reindex, Analyze and Optimize on the database every 15 days.
    - Applies Zipalign every 15 days.
- Cgroup script, for better optimization of cgroups, threads, etc. without having to add or remove things.
- Added adjguard, for users who want to prevent LMK from killing their most beloved apps.
- Added memperfd, a better and adapted variant for current Androids of Matt Yang's qti-mem-opt. The focus is to reduce CPU usage of memory management while improving memory management. With the focus on stability and power consumption above all.
- All the features and scripts from Matt Yang's original Perfd opt, with improvements and updates to work with current and updated rooting solutions.
- Focuses on compatibility, stability and energy efficiency above all. With each update, an optimization for at least one platform is guaranteed, or a new SOC is added (but this depends on the users).

# Profiles
- powersave: focused on saving as much energy as possible, without drastically impacting the performance and fluidity of the device (but it will still suffer in situations of load fluctuation).
- balance: smoother than stock, and consumes less energy overall. Recommended for most users who want to save energy, but want to maintain performance in everyday use.
- performance: without frequency limitation, focused on bringing the original performance that the device has but improved with general optimizations.
- fast: brings stable performance considering the TDP of the device, focused on being 100% stable in most situations by being fast and quickly lowering frequencies.

SOC's profiles:
Perhaps not all SOCs will have the signaling of how much the "boost" is.
```SOC's
sdm865 (Schedutil)
- powersave:    1.8+1.6+2.4g, boost 1.8+2.0+2.6g, min 0.3+0.7+1.1
- balance:      1.8+2.0+2.6g, boost 1.8+2.4+2.7g, min 0.7+0.7+1.1
- performance:  1.8+2.4+2.8g, boost 1.8+2.4+2.8g, min 0.7+0.7+1.1
- fast:         1.8+2.0+2.7g, boost 1.8+2.4+2.8g, min 0.7+1.2+1.2

sdm855/sdm855+ (Schedutil)
- powersave:    1.7+1.6+2.4g, boost 1.7+2.0+2.6g, min 0.3+0.7+0.8
- balance:      1.7+2.0+2.6g, boost 1.7+2.4+2.7g, min 0.5+0.7+0.8
- performance:  1.7+2.4+2.8g, boost 1.7+2.4+2.8/2.9g, min 0.5+0.7+0.8
- fast:         1.7+2.0+2.7g, boost 1.7+2.4+2.8/2.9g, min 0.5+1.2+1.2

sdm845 (Schedutil)
- powersave:    1.7+2.0g, boost 1.7+2.4g, min 0.3+0.3
- balance:      1.7+2.4g, boost 1.7+2.7g, min 0.5+0.8
- performance:  1.7+2.8g, boost 1.7+2.8g, min 0.5+0.8
- fast:         1.7+2.4g, boost 1.7+2.8g, min 0.5+1.6

sdm765/sdm765g (Schedutil)
- powersave:    1.8+1.7+2.0g, boost 1.8+2.0+2.2g, min 0.3+0.6+0.8
- balance:      1.8+2.0+2.2g, boost 1.8+2.2+2.3/2.4g, min 0.5+0.6+0.6
- performance:  1.8+2.2+2.3g, boost 1.8+2.2+2.3/2.4g, min 0.5+0.6+0.8
- fast:         1.8+2.0+2.2g, boost 1.8+2.2+2.3/2.4g, min 0.5+1.1+1.4

sdm730/sdm730g (Schedutil)
- powersave:    1.7+1.5g, boost 1.7+1.9g, min 0.3+0.3
- balance:      1.7+1.9g, boost 1.7+2.1g, min 0.5+0.6
- performance:  1.8+2.2g, boost 1.8+2.2g, min 0.5+0.6
- fast:         1.8+1.9g, boost 1.8+2.2g, min 0.5+1.2

sdm680 (Schedutil)
- powersave:    1.8+2.2g, boost 1.9+2.4g, min 0.3+0.3
- balance:      1.8+2.2g, boost 1.9+2.4g, min 0.6+1.0
- performance:  1.9+2.4g, boost 1.9+2.4g, min 0.6+1.0
- fast:         1.9+2.4g, boost 1.9+2.4g, min 0.6+1.3

sdm675 (Schedutil)
- powersave:    1.7+1.5g, boost 1.7+1.7g, min 0.3+0.3
- balance:      1.7+1.7g, boost 1.7+1.9g, min 0.5+0.6
- performance:  1.8+2.0g, boost 1.8+2.0g, min 0.5+0.6
- fast:         1.8+1.7g, boost 1.8+2.0g, min 0.5+1.2

sdm710/sdm712 (Schedutil)
- powersave:    1.7+1.8g, boost 1.7+2.0g, min 0.3+0.3
- balance:      1.7+2.0g, boost 1.7+2.2/2.3g, min 0.5+0.6
- performance:  1.7+2.2g, boost 1.7+2.2/2.3g, min 0.5+0.6
- fast:         1.7+2.0g, boost 1.7+2.2/2.3g, min 0.5+1.5
```

# Installation
- Download zip in Release Page
- Flash in Magisk manager, Kernelsu or Other Root Solution
- Reboot
- Check whether /sdcard/Android/panel_powercfg.txt exists

# Switch modes
# Switching on boot

- Open /sdcard/Android/panel_powercfg.txt
- Edit line default_mode=balance, where balance is the default mode applied at boot
- Reboot

# Switching after boot

Option 1:
Exec sh /data/powercfg.sh balance, where balance is the mode you want to switch.

Option 2:
Install vtools and bind APPs to power mode.

## Credit

```plain
Matt Yang — For the Original Module and Some Functions from qti-mem-opt and my inspiration for create this.
Pedrozz, Tytydraco, korom42, yinwanxi and all the other contributors to their projects, I want to thank you all for helping with this project. I will try to extract as much as I can for each SOC because of all of you.
```

# Exclaimer
The module will receive a rework for optimization reasons and personal motivations. The previous versions were deleted. I will notify and leave the download of the tested and stable version when I get it.
