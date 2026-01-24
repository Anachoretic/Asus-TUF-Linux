---
title: Advanced
icon: sliders
---


## 1. CPU boost:
CPU boost is a feature that temporarily raises a processor’s clock speed above its base frequency to improve performance during demanding tasks. Disabling it 
causes the CPU to stay near its base clock, which reduces temperatures but also lowers performance. Applications that rely on high CPU speeds such as games or other CPU-intensive workloads will see a 
noticeable performance drop, while lighter tasks will be mostly unaffected. You can disable boost if you don’t use such applications or if you are fine with the performance and temperature tradeoff.

```bash
echo 0 > /sys/devices/system/cpu/cpufreq/boost
```

To enable CPU Boost again, simply echo `1` instead of `0`.

## 2. GPU Undervolt:

NVIDIA’s Linux driver does not expose direct voltage control, so traditional GPU undervolting (as done on Windows) is not supported. On Linux, power limits and GPU/VRAM clock frequencies can still be adjusted using tools such as LACT.

AMD GPUs expose voltage controls through the Linux kernel and driver stack, so proper GPU undervolting on Linux is generally supported and can also be done using LACT.

The installation steps are available on [LACT’s GitHub page](https://github.com/ilya-zlobintsev/LACT).
For supported GPUs, the undervolting process is essentially the same as on Windows, start by lowering the voltage by 50 mV and test for stability. If it becomes unstable or there’s a noticeable performance drop, increase the voltage in 20 mV steps until it’s stable, then repeat as needed.


## 3. Secure Boot:
Secure Boot is a security feature in UEFI (BIOS) that prevents unsigned software from loading during the boot process, potentially stopping malicious programs from running when the system first starts. Secure Boot isn’t mandatory on Linux and can be left disabled in most cases. In fact, some distributions require Secure Boot to be disabled because they won’t boot normally with it enabled. This is because their bootloaders or kernels are not signed, or the required keys are not present in the firmware. If you’re only running Linux, you can keep Secure Boot disabled, or enable it if you want the "extra security".

If you’re dual booting Windows, disabling Secure Boot and switching it on/off every time you switch OS can be annoying. In that case, it’s usually better to leave Secure Boot enabled so both operating systems boot normally.

The steps to enable Secure Boot vary depending on your distro’s implementation, so the relevant wiki pages for enabling Secure Boot are linked below.
* [Debian](https://wiki.debian.org/SecureBoot)

* [Fedora](https://rpmfusion.org/Howto/Secure%20Boot)

* [openSUSE Tumbleweed](https://en.opensuse.org/openSUSE:UEFI#How_to_enable_or_disable_Secure_Boot_support)

* [Arch](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot)
    
    *  [CachyOS](https://wiki.cachyos.org/configuration/secure_boot_setup/)
 
    *  [EndeavourOS](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot)
