# Libperfmgr
![8887e2e87f38b5a9b0abe99fad5f045a](https://github.com/user-attachments/assets/160ac75a-03b5-49b3-ad7e-fb8a7e592026)

# 设计目标
The previous [Project WIPE](https://github.com/yc9559/cpufreq-interactive-opt), automatically adjust the `interactive` parameters via simulation and heuristic optimization algorithms, and working on all mainstream devices which use `interactive` as default governor. The recent [WIPE v2](https://github.com/yc9559/wipe-v2), improved simulation supports more features of the kernel and focuses on rendering performance requirements, automatically adjusting the `interactive`+`HMP`+`input boost` parameters. However, after the EAS is merged into the mainline, the simulation difficulty of auto-tuning depends on raise. It is difficult to simulate the logic of the EAS scheduler. In addition, EAS is designed to avoid parameterization at the beginning of design, so for example, the adjustment of schedutil has no obvious effect.  

[WIPE v2](https://github.com/yc9559/wipe-v2) focuses on meeting performance requirements when interacting with APP, while reducing non-interactive lag weights, pushing the trade-off between fluency and power saving even further. `QTI Boost Framework`, which must be disabled before applying optimization, is able to dynamically override parameters based on perf hint. This project utilizes `QTI Boost Framework` and extends the ability of override custom parameters. When launching APPs or scrolling the screen, applying more aggressive parameters to improve response at an acceptable power penalty. When there is no interaction, use conservative parameters, use small core clusters as much as possible, and run at a higher energy efficiency OPP under heavy load.  

Details see [the lead project](https://github.com/yc9559/sdm855-tune/commits/master) & [perfd-opt commits](https://github.com/yc9559/perfd-opt/commits/master)    

# How to help...
- If your processor is not listed in the list, please, first: make an issue containing the nickname [integration of (name of your processor)]

And then, answer this form:

1. What is the frequency your processor uses to open these applications: Devcheck, RAR and any game that you consider heavy. And the available frequencies of your processor (which it transitions to, can be found in /sys/devices/system/cpu/cpufreq/policy*/scaling_available_frequencies and do this in ALL CLUSTERS, to help as much as possible). And show what is your DDR max frequency.

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
- Do not have a messy kernel (like many Xiaomi).
- Have a UFS storage. (Optional, it is just to ensure better I/O performance so that the module can be fully effective).
- Follow the compatibility list, do not force the module on devices with incompatible SOCs.
- Do not have too high expectations of the performance you will get, remember that this is a module that needs to adapt to many different users and socs, so some socs may have initial or full support.

# Compability List
- For now, the project only supports snapdragon. I want to master the efficiency of snapdragon to avoid future problems with mediatek and derivatives.

```
    sdm865
    sdm855/sdm855+
    sdm845
    sdm765/sdm765g
    sdm730/sdm730g
    sdm680
    sdm675
    sdm710/sdm712
```

# Features
- Cleanup script, which applies these cleanups every 7-15 days:
    - Fstrim on cache, system, data and persist every 7 days.
    - Compile, sync and cleanup of ART every 7 days.
    - Cleans cache and junk from apps every 7 days.
    - Applies Vacuum, Reindex and Analyze on the database every 15 days.
    - Applies Zipalign every 15 days.
- Cgroup script, for better optimization of cgroups, threads, etc. without having to add or remove things.
- Added adjguard, for users who want to prevent LMK from killing their most beloved apps.
- Added memperfd, a better and adapted variant for current Androids of Matt Yang's qti-mem-opt. The focus is to reduce CPU usage of memory management while improving memory management. With the focus on stability and power consumption above all.
- All the features and scripts from Matt Yang's original Perfd opt, with improvements and updates to work with current and updated rooting solutions.
- Focuses on compatibility, stability and energy efficiency above all. With each update, an optimization for at least one platform is guaranteed, or a new SOC is added (but this depends on the users).

# Profiles
- powersave: based on balance mode, but with lower max frequency
- balance: smoother than the stock config with lower power consumption
- performance: dynamic stune boost = 30 with no frequency limitation
- fast: providing stable performance capacity considering the TDP limitation of device chassis

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
Until I know how to update all the fork files, I will update you via releases.
