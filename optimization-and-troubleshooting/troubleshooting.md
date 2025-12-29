---
title: Troubleshooting
icon: wrench
---

{% stepper %}
{% step %}

# 1. Black Screen
<details><summary>Live Installer:</summary>
A black screen during installation is often caused by graphics driver conflicts. In such cases, the installer may get stuck before the graphical interface launches.

Adding the `nomodeset` kernel parameter disables early graphics mode setting and can allow the installer to boot successfully.

During boot, select the live ISO entry and press <kbd>e</kbd> to edit it.
Locate the line that starts with `linux` and add `nomodeset` to the end of that line, then press <kbd>F10</kbd> to boot.

![](/Images/openSUSE/Troubleshoot2.png)

</details>

<details><summary>Installed System:</summary>
This depends on what you did right before it occurred. The drivers might not be installed properly, or something in the system may have broken.

Switch to TTY3 by pressing <kbd>Ctrl + Alt + F3 </kbd>, log in, and figure it out from there.
</details>

{% endstep %}
{% step %}

# 2. Dual boot:
<details><summary>System Clock Mismatch:</summary>
Windows uses local time to track the system clock, while Linux defaults to UTC. Because of this, when you boot back into Windows, the time may appear incorrect or ahead. You can fix this by either configuring Linux to use local time or making Windows use UTC.

## Windows:
Open Settings, go to Date & Time, and make sure 'Set time automatically' is turned off. Then, open the Registry Editor and navigate to the following path:

```bash
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation
```

Then, within that folder, add a new `DWORD (32-bit)` value named `RealTimeIsUniversal`, set its value to `1`, and reboot your system. After that, correct your time, and you're done.


## Linux:
```bash
timedatectl set-local-rtc 1 --adjust-system-clock
```
After running this, run `timedatectl` and make sure it shows `RTC in local TZ: yes`.

</details>

<details><summary>NTFS</summary>
NTFS is a proprietary format, and while it’s supported on Linux, it isn’t ideal. You should generally prefer a native Linux filesystem or another well-supported format and avoid NTFS unless necessary. If you have games on an NTFS partition and plan to play them on Linux, it’s better to install them on a Linux-compatible filesystem, as you may run into several issues trying to run them from NTFS, if they run at all.

If you can’t access NTFS partitions on Linux, make sure the `ntfs-3g` package is installed and that Fast Startup is turned off in Windows.

</details>

{% endstep %}
{% step %}

# 3. Hotkeys:
<details><summary>Brightness:</summary>
On some laptops, the brightness hotkeys or the brightness slider in your desktop environment may not work, particularly on systems with hybrid graphics and a MUX switch. In these cases, the kernel’s default ACPI backlight driver might not expose the correct backlight interface. You can try adding `acpi_backlight=native` (or other variants like vendor or video) to your bootloader’s kernel command line, which may restore brightness control.

[For instructions on adding kernel parameters, refer to the Arch Wiki.](https://wiki.archlinux.org/title/Kernel_parameters)

</details>

<details><summary>Other Hotkeys:</summary>
Some hotkeys are not mapped to any action in Linux, so pressing them will have no effect. To enable these hotkeys or configure additional ones, refer to the hotkey section of your distribution’s installation guide. You can follow the same steps there to add custom hotkeys as well.
</details>

{% endstep %}
{% step %}

# 4. dGPU:
<details><summary>dGPU Not Utilized:</summary>
First, make sure the `switcheroo-control` package is installed and enabled. After that, reboot the system and try launching any game. By default, it should handle hybrid GPUs automatically by offloading games to the discrete GPU (dGPU).

```bash
systemctl enable switcheroo-control
```

If `switcheroo-control` does not work for certain games or applications, you can add `prime-run %command%` as a launch option. This will force the game or application to use the discrete GPU (dGPU). For this to work, the `nvidia-prime` package must be installed.
</details>

{% endstep %}
{% step %}

# Other:
It’s not possible to include solutions for every problem that can occur. If you run into an issue that isn’t listed here, try searching online or checking the forum for your specific distro. A similar issue may already have been solved by someone else. If not, ask for help and be sure to include your system specifications along with relevant details such as logs.

{% endstep %}
{% endstepper %}
