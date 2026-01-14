# Welcome to the ASUS TUF Linux guide!

The idea behind this guide is to simplify the process of running Linux on ASUS TUF laptops for beginners. It covers most of the basic steps you would normally follow, such as updating your system, installing drivers, and setting up specific software. Everything is condensed and kept in one place so you do not get overwhelmed or lost along the way.

This guide is by no means perfect. It is intended as a basic introduction to help people get familiar with the essentials without the experience feeling annoying or like a hassle. The main goal is to make users more comfortable with Linux.

There are several limitations. Some topics are not explored as deeply as they probably should be, especially in areas where more explanation would be useful. Since a single person is writing everything, it is easy to run out of ways to present certain topics clearly, which means some sections are very straightforward and lack detailed explanations. Despite these shortcomings, this guide is a starting point, and it will continue to improve over time through updates and suggestions from others who use it.

Whether you find it helpful or annoying, or incomplete, thank you for taking the time to visit this guide. :)

# Compatibility:
The components used in TUF laptops are fairly standard and similar to those found in other laptops from different manufacturers. There are no proprietary parts known to cause major issues on Linux. Older models should have no compatibility problems since they have been around for a while and support has matured over the years. Any distribution that does not ship with an extremely old kernel should work fine on these laptops.

For recently released models from 2025, compatibility might be an issue if your distribution does not support the newest hardware. In that case, using a distro with more frequent updates or a newer kernel may improve hardware support.

If you run into issues with Wi-Fi on Linux or Windows, particularly with MediaTek or Realtek cards, consider replacing them with an Intel AX210 or a similar model. Drivers for MediaTek and Realtek cards are often less reliable on Linux.

There is a [list of some specific TUF models and their Linux compatibility in the Arch Wiki](https://wiki.archlinux.org/title/Laptop/ASUS), but detailed documentation for every model is limited. The best approach is to test hardware compatibility yourself. You can check basic functions like Wi-Fi, Bluetooth, Ethernet, trackpad, and keyboard directly from a live installer before installation.

# Limitations:
Not everything may work on every TUF model. Some features might require extra steps or kernel parameters, and broken firmware can prevent things like suspend-to-RAM from working.

There is no official Armory Crate software for Linux. The closest options with a GUI are [asusctl](https://gitlab.com/asus-linux/asusctl) and [supergfxctl](https://gitlab.com/asus-linux/supergfxctl). They support most basic functions such as fan control, RGB, and battery or power limits, but features like keyboard state (during sleep, awake, boot, or shutdown), and GameVisual are not available.

These tools are officially supported on Fedora, Arch, and openSUSE Tumbleweed. On Debian-based systems, you will need to build and install them manually. You can also try alternatives or use no software at all.

# License & Credits
THe current iteration of the guide is licensed under CC by SA 4.0. The lisence can be viewed [here](https://creativecommons.org/licenses/by-sa/4.0/).

Almost all the resources and information used to create this guide are freely accessible on the web. Each section for a specific distribution follows the recommended approach according to that distro’s official wiki. Many topics covered here can also be found on the Arch Wiki, especially for material that is not specific to a particular distribution.

All images used in this guide were self-captured in a virtual machine(GNOME Boxes + QEMU/KVM).
