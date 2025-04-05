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
- Android 11-14. If you have a lower Android, it is not recommended to use the module because it is adapted for current Androids (where resource management has changed abruptly). Android 15 has not been tested yet because I am on an Android 14 ROM, so if you want to test on Android 15, feel free.
- 2GB of RAM or more (Because I use a specific parallelism for cell phones with 2GB and higher. Cell phones with lower memory cannot benefit from memory adjustments, so it is not recommended to use the module due to these problems).
- Don't have a messy or too "spoiled" kernel (like many custom rom or custom kernel that tend to hide, modify and trap certain optimizations for reasons of "it's to specialize in my kernel")
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
- Unadapted And Adapted: SoCs that still use the old Matt Yang’s Perfd-opt settings and have not been optimized for a better balance between performance and power consumption. With adapted, are SoCs that have already undergone an initial adaptation to improve efficiency and performance, but can still be optimized further.

- Initial: SoCs that have basic compatibility with the design and have received initial optimizations. However, they may still have incomplete or not well-organized settings.

- Complete: All major optimizations have been applied, ensuring greater consistency and improved performance. The performance profiles are already balanced.

Extras and Advanced Levels:

- Extra: For SoCs that already come with native perfboost settings (performance acceleration).

- Boost Fun: An extra tweak to improve the SoC's responsiveness in moments that require immediate performance, such as opening applications or loading games quickly.

- Specialized: The maximum level of optimization. Here, the SoC strikes the ideal balance between performance, power consumption, longevity and compatibility, taking full advantage of the available optimizations.

# Features
- Cleanup script, which applies these cleanups every 7-15 days:
    - Fstrim on cache, system, data, persist, vendor, product and system_ext every 7 days.
    - Cleans cache and junk from apps every 7 days.
    - Compiles all user apps into speed-profile and XML images every 7 days. Along with this, at the end of each month (specifically every 30 days) the final ART synchronization and cleaning is applied, to keep everything stable and taking up less space in the end.
    - Applies Vacuum, Reindex, Analyze and Optimize on the database every 15 days.
    - Applies Zipalign every 15 days.
- Cgroup script, for better optimization of cgroups, threads, etc. without having to add or remove things.
- Added adjguard, for users who want to prevent LMK from killing their most beloved apps.
- Added memperfd, a better and adapted variant for current Androids of Matt Yang's qti-mem-opt. The focus is to reduce CPU usage of memory management while improving memory management. With the focus on stability and power consumption above all. This includes a attempts to make the compression rate reach 3.8x (At least on my device, on others it may be lower or higher). Also, memperfd comes with selectable options that are applied after boot, such as:
  - ZRAM size: If you are not satisfied with the amount of ZRAM, you can change it to the value you want to reduce or increase. Remember: The amount of ZRAM in the presets is based on maximizing swapcache while avoiding an overly large disk size. So a 4gb zram would be equivalent to 1440mb, a 6gb would be 2160mb and so on.
  - ZRAM algorithm: If you want to change the ZRAM algorithm from lz4 to another, feel free.
- ART optimizations coming from AOSP Master. The focus is to deliver better app execution, and in turn improve responsiveness across all profiles.
- The module contains the activation and configuration of the UFFD garbage collection made for kernel 4.4 and which exists until today, where some custom ROMs use it to better manage memory with an addition of overhead, a good trade-off for devices with less memory. It is better than generational concurrent in terms of freeing memory, as both cannot work together, due to the overhead issues that they would cause, focus on meeting management needs and reduce the complexity of UFFD with simple flags.
- Use kernel, system and build prop optimizations that are used in custom ROMs that are extremely optimized for user experience and battery management (like zygote preforking). The focus is to bring a bit of the experience of a custom ROM optimization to cell phones with stock ROMs (or ROMs very close to stock).
- Disable and silence tons of logging/debugging. This will save a lot of space, as well as save small CPU cycles for your tasks.
- Added schemes to make the foreground prefer small cores, but allow scaling to big cores IF necessary. (This saves a small amount of power in light usage situations, such as scrolling, using browser, using messengers like whatsapp). The focus of top-app is exclusively on the big cores, avoiding early allocation and overload on these cores that are naturally focused on the user.
- Important system binaries will be updated with the module, this means that binaries like SQLITE will receive updates, avoiding users having to install updates from external modules.
- All the features and scripts from Matt Yang's original Perfd opt, with improvements and updates to work with current and updated rooting solutions.
- Focuses on compatibility, stability and energy efficiency above all. With each update, an optimization for at least one platform is guaranteed, or a new SOC is added (but this depends on the users).
- The module avoids using storage for very demanding functions, such as SWAP. Because of this, we will minimize writing to the internal storage, and in turn: extend the lifespan and maintain storage performance. We will try to make the device last as long as possible. So much so that the goal of the module is to make devices last longer and to be a temporary alternative for users who do not have the money to buy a powerful cell phone, or who want a module that controls consumption, performance, etc. through profiles.

# Profiles
- powersave: based on balance mode, but with lower max frequency
- balance: smoother than the stock config with lower power consumption
- performance: no frequency limitation
- fast: providing stable performance capacity considering the TDP limitation of device chassis

```plain
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
- It's still in the early stages, so there are no changes in frequencies across profiles

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
yinwanxi - For the uperf libcgroup, I am very grateful, and you were one of my inspirations at the time that uperf worked on my cell phone (before it suffered from data corruption nowadays) it was one of the famous "game changes" that I had and that some pages talked a lot about.
Pedrozz, Tytydraco, korom42, yinwanxi and all the other contributors to their projects, I want to thank you all for helping with this project. I will try to extract as much as I can for each SOC because of all of you.
```

# In case of bootloop
If the bootloop occurs without allowing you to boot (i.e. enter the system), delete the system.prop inside the file and test. If you can, identify the prop that caused the bootloop and let me know so I can remove it.

# Exclaimer
The module will receive a rework for optimization reasons and personal motivations. The previous versions were deleted. I will notify and leave the download of the tested and stable version when I get it.
