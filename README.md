# Libperfmgr
![742b6dbaa02122922bf7b891265fde0c_1](https://github.com/user-attachments/assets/7581dec5-d7c5-4123-beba-e67c584dedfd)

A Magisk/KSU module made especially for Qualcomm devices with Adreno GPUs. The intention is to mimic the power-libperfmgr of Pixel phones and generate a module that imitates the performance, power consumption and adaptability of the Pixel engine but for different SOCs, taking advantage of Perfhal and the old QTI Boost Framework and making it adapt to the custom parameters of general optimizations, and the parameters specific to each SOC.

# 设计目标
Overall, this is basically Perfd-opt of Matt Yang's Perfd-opt. Full credit to him. Now, with the adaptation to current Androids, I was able to revive the module to work today. And now the module has a new look, it is basically to give the same feeling as the Google Pixel, but on other devices.

Libperfmgr, based on the same function as the Pixel. It is a module that tries to bring fluidity and energy saving limits and push them beyond what would otherwise be possible, providing performance and profiles for different users, and especially for specific Soc's. What does it mean? That the module has specific optimizations for each processor! In addition to the general optimizations, it will also have the profile optimizations that are specified for each processor. The focus is be more economically efficient and better for the user than Matt Yang's original module, after all, this is supposed to be an evolution for today. Focus on using the small cores more efficiently, avoid using the big cores for tasks other than gaming or heavy tasks and run in the most OPP efficiency of your phone.

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
    sdm865 (Unadapted)
    sdm855/sdm855+ (Unadapted)
    sdm845 (Unadapted)
    sdm765/sdm765g (Unadapted)
    sdm730/sdm730g (Unadapted)
    sdm680 (Initial)
    sdm675 (Unadapted)
    sdm710/sdm712 (Unadapted)
```

# Explanation of each "Compatibility Stage":
- Unadapted and Adapted: Specifically for SOCs that came with Matt Yang's Perfd-opt. Those that have not been adapted to be "improved" for today's consumption and efficiency will be flagged with this.
- Initial: Does not have complete optimizations, is still in EXTREMELY EXPERIMENTAL phases! Needs extra feedback.
- Complete: In the profiles section, optimizations are complete, allowing the user to use the adjustments as they see fit.
- Initial + Extra: It is still in the testing phase but contains extra optimizations like Perfboost.
- Complete + Extra: In addition to the complete traditional optimizations, it comes with perfboost optimization. To ensure better response and agility.
- Specialized: Fully optimized, a SOC containing this indication is said to be fully optimized for maximum possible energy savings, compatibility and performance.

# Features
- Cleanup script, which applies these cleanups every 7-15 days:
    - Fstrim on cache, system, data, persist, vendor, product and system_ext every 7 days.
    - Cleans cache and junk from apps every 7 days.
    - Compiles all user apps into speed-profile and XML images every 7 days. Along with this, at the end of each month (specifically every 30 days) the final ART synchronization and cleaning is applied, to keep everything stable and taking up less space in the end.
    - Applies Vacuum, Reindex, Analyze and Optimize on the database every 15 days.
    - Applies Zipalign every 15 days.
- Cgroup script, for better optimization of cgroups, threads, etc. without having to add or remove things.
- Added adjguard, for users who want to prevent LMK from killing their most beloved apps.
- Added memperfd, a better and adapted variant for current Androids of Matt Yang's qti-mem-opt. The focus is to reduce CPU usage of memory management while improving memory management. With the focus on stability and power consumption above all. This includes dynamic configuration of LMKD's minfree parameters based on the amount of memory on the device.
- ART optimizations coming from AOSP Master. The focus is to deliver better app execution, and in turn improve responsiveness across all profiles.
- The module contains the activation and configuration of the UFFD garbage collection made for kernel 4.4 and which exists until today, where some custom ROMs use it to better manage memory with an addition of overhead, a good trade-off for devices with less memory. It is better than generational concurrent in terms of freeing memory, as both cannot work together, due to the overhead issues that they would cause, focus on meeting management needs and reduce the complexity of UFFD with simple flags.
- All the features and scripts from Matt Yang's original Perfd opt, with improvements and updates to work with current and updated rooting solutions.
- Focuses on compatibility, stability and energy efficiency above all. With each update, an optimization for at least one platform is guaranteed, or a new SOC is added (but this depends on the users).

# Profiles
- powersave: focused on saving as much energy as possible, without drastically impacting the performance and fluidity of the device (but it will still suffer in situations of load fluctuation).
- balance: smoother than stock, and consumes less energy overall. Recommended for most users who want to save energy, but want to maintain performance in everyday use.
- performance: without frequency limitation, focused on bringing the original performance that the device has but improved with general optimizations.
- fast: brings stable performance considering the TDP of the device, focused on being 100% stable in most situations by being fast and quickly lowering frequencies.

# Installation
- Download zip in Release Page
- Flash in Magisk manager, Kernelsu or Other Root Solution
- Reboot
- Wait 30 seconds after boot for the module to start. It will only take this long during boot
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
