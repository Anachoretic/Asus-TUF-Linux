---
icon: check
---

# Choosing a Distro:

Due to Linux being open source, there are many different distributions (distros) available. At the time of writing, roughly 405 distros are actively maintained (according to DistroWatch). Because of this huge number of options, it’s easy to get confused about which distro to choose.

All you really need to know is that there are four major mainstream base distros: Debian, Fedora, openSUSE, and Arch. Most Linux distros are based on one of these (there are some unique distros, but they’re beyond the scope of this guide).

Most distros you’ll come across are built on top of these four, with only minor or important changes. The most noticeable difference is usually the desktop environment (DE). On Linux, the user interface is largely handled by the desktop environment, and different distros use different DEs. For now, you should focus on just two major desktop environments: [KDE](https://kde.org/), which looks similar to Windows, and [GNOME](https://www.gnome.org/), which looks similar to macOS. You can visit their respective websites to see how they look.

If you’re not sure which desktop environment is right for you, you can try them out using [DistroSea](https://distrosea.com/), which lets you test Linux distros without installing them. Keep in mind that performance won’t be as good as running Linux directly on your hardware. Alternatively, you can try using a virtual machine.

## **Things not to do while choosing a distro:**

1. **Choose a distro based on looks:**
   Don’t pick a distro just because it looks pretty. Customization is one of Linux’s biggest strengths, with a few tweaks, you can make any distro look like another. Choose based on functionality, not form.

2. **Choose a niche distro with no community support:**
   Avoid distros that have little to no community. When something breaks (and it will at some point), the community is where you’ll go for help, guides, and fixes.

3. **Choose a “hacking distro”:**
   Distros like Kali or Parrot OS aren’t meant for daily use, even their developers say so. They’re designed for quick testing and security research in a virtual machine. With a huge number of preinstalled tools comes a huge attack surface. And please, don’t install Kali just because you saw it in "Mr. Robot."

4. **Choose something you don’t understand:**
   Just don’t jump into Arch just because you heard it’s popular. If you’re willing to read the wiki and learn as you go, Arch can be a great experience. But if you’re not ready to troubleshoot or spend time setting things up, it’ll quickly become annoying and more trouble than it’s worth.

5. **Distro hop:**
   There’s nothing wrong with trying different distros, but if you’re just hopping around to “find one for you” without knowing what you actually need, you’ll just waste time jumping from one to another.

## Recommended Linux Distributions:
This is just a list, not a ranking.

<pre>
1. <a href="https://www.fedoraproject.org/">Fedora</a>
   |
   |-- <a href="https://bazzite.gg/">Bazzite</a>
   |
   |__ <a href="https://nobaraproject.org/download.html">Nobara</a>

2. Arch
   |
   |-- <a href="https://endeavouros.com/">EndeavourOS</a>
   |
   |__ <a href="https://cachyos.org/">CachyOS</a>

3. <a href="https://get.opensuse.org/tumbleweed/">openSUSE Tumbleweed</a>

4. Debian
   |
   |-- <a href="https://ubuntu.com/desktop">Ubuntu</a>
   |
   |-- <a href="https://linuxmint.com/">Linux Mint</a>
</pre>

{% hint style="warning" %}
For Debian-based distributions, there isn’t any Armoury Crate alternative readily available. Asusctl and supergfxctl are considered unsupported on Debian-based distros. While they can be installed by compiling them manually, they may not work properly or may not be usable at all. Alternatively, you can install different applications to control different functions, but this can quickly become annoying.
{% endhint %}
