---
title: Arch Linux
icon: linux
---


# Arch Linux Post-Install Guide

{% stepper %}

{% step %}

This post-install guide aims to simplify running Arch Linux on TUF laptops. While it covers most topics and simplifies the process, it is by no means a replacement for the Arch Wiki. You should still rely primarily on the Arch Wiki for the quality and depth of information it contains. This guide cannot possibly match the Arch Wiki, so if you haven’t gotten used to reading the Wiki yet, now is the time to start.

## 1. AUR Helper

One of the most prominent and enticing features of Arch Linux is the **Arch User Repository (AUR)**. The AUR is a community-driven repository that provides package descriptions (PKGBUILDs). These allow you to compile software from source using `makepkg` and then install it with `pacman`.

An AUR helper is recommended for installing packages from the AUR, as it simplifies the process. You can install packages without an AUR helper, but using one makes it much easier. Distros like EndeavourOS and other Arch-based distros usually include an AUR helper by default, but on vanilla Arch Linux, you need to install it yourself.

You can choose between Yay or Paru, if you’re unsure, just go with Yay.

### **Yay (Yet Another Yogurt):**
```bash
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

### **Paru :**
```bash
sudo pacman -S --needed base-devel && git clone https://aur.archlinux.org/paru.git && cd paru && makepkg -si
```


{% hint style="warning" %}
The AUR is community-driven. Anyone can upload packages, so there’s always a small risk of malicious code. Always **read the PKGBUILD file** before installing to see exactly what commands will run on your system.
{% endhint %}

{% endstep %}

{% step %}

## 2. Pacman Configuration: 

### 2.1 Enable Multilib repository (Required for Drivers and Wine)

Open the pacman configuration file:
```bash
sudo nano /etc/pacman.conf
```

Uncomment these lines:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

### 2.2. Update Mirrors to Use the Fastest Server

Backup current mirrorlist:
```bash
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
```

Enable parallel downloads:
```bash
sudo nano /etc/pacman.conf
```
Set `ParallelDownloads = 10`

Install reflector:
```bash
sudo pacman -S reflector
```

Update mirrors:
```bash
sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

Restore backup if needed:
```bash
sudo cp /etc/pacman.d/mirrorlist.bak /etc/pacman.d/mirrorlist
```



### Bonus: Pacman Animation

This replaces the progress bar in Arch with a Pacman animation.

Edit pacman config:
```bash
sudo nano /etc/pacman.conf
```

Uncomment:
```
Color
```

Add:
```
ILoveCandy
```

Update system:
```bash
sudo pacman -Syu
```

## 3. Driver Installation

If you're using an **Nvidia GPU**, you’ll also need to install proprietary drivers manually. AMD users can skip this, since Mesa drivers are included in the kernel and work out of the box. However, if you want to install all the 32-bit libraries and Vulkan support, see the instructions below.

Unlike Windows, most drivers are already included in the Linux kernel and the rest of the drivers is usually included in the `linux-firmware` package. You usually don’t need to install them manually.

## 3.1 Nvidia Drivers:
There are currently two drivers available for NVIDIA GPUs: the proprietary one and the open-source GPU kernel modules. To determine which one you need, please read below.

NVIDIA released open-source Linux GPU kernel modules (not to be confused with the fully open-source drivers Nouveau). These are still the proprietary drivers but semi open-source and are recommended for NVIDIA Blackwell and higher, as proprietary drivers won’t work for them. However, if you have anything older than Pascal, then it is recommended to use the proprietary drivers, as the open-source ones aren’t compatible. The performance is the same for other architectures, so you may choose to use whichever one you want.

You can compare your gpu architecture in the chart below:

| **GPU Generation**          | **Architecture** |
|-----------------------------|------------------|
| RTX 50 Series Laptop GPU    | Blackwell        |
| RTX 40 Series Laptop GPU    | Ada Lovelace     |
| RTX 30 Series Laptop GPU    | Ampere           |
| RTX 20 Series Laptop GPU    | Turing           |
| GTX 16 Series               | Turing           |
| GTX 10 Series               | Pascal           |


{% hint style="warning" %}
Make sure **Secure Boot is disabled**, or the Nvidia driver won’t load.
{% endhint %}

Update system:
```bash
sudo pacman -Syu
```

Check GPU detection:
```bash
lspci | grep -i nvidia
```

If it doesn’t show, try switching to Hybrid GPU mode using `supergfxctl` and run the command again.

**For EndeavourOS users:**
```bash
yay -S nvidia-inst
nvidia-inst
```

**Manual installation (Arch and Arch-based distros):**
 
### **Proprietary driver:**

```bash
sudo pacman -S dkms nvidia-dkms nvidia-utils lib32-nvidia-utils nvidia-settings libva-nvidia-driver vulkan-icd-loader lib32-vulkan-icd-loader
```

### **Nvidia open kernel module driver:**

```bash
sudo pacman -S dkms nvidia-open-dkms nvidia-utils lib32-nvidia-utils nvidia-settings libva-nvidia-driver vulkan-icd-loader lib32-vulkan-icd-loader
```

### 3.2. AMD Drivers

The drivers for AMD are usually installed out of the box since they’re part of the Linux kernel. If you want to make sure you have everything, including the 32-bit libraries and Vulkan, simply run the command below.

```bash
sudo pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon libva-mesa-driver libva-utils
```

### 3.1 Intel:
Same for intel as well.

```bash
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver libva-intel-driver libva-utils
```

{% hint style="info" %}
After installation, wait for the initramfs to be regenerated.
{% endhint %}

Enable Nvidia services:
```bash
sudo systemctl enable nvidia-hibernate.service nvidia-suspend.service nvidia-resume.service nvidia-powerd.service
```

## 4. Asus Software Installation:

Install Asus tools:

```bash
yay -S asusctl rog-control-center supergfxctl
```

Enable GPU switching daemon:
```bash
sudo systemctl enable supergfxd.service --now
```

{% hint style="info" %}
Ignore the "Asus kernel isn’t loaded" message in `rog-control-center`.
{% endhint %}

### 4.1 Switching GPU Modes (via Terminal or GUI)

`Rog-control-center` Center is the GUI for asusctl. You can also use asusctl without the GUI if you prefer. The same applies to supergfxctl , it’s a terminal only tool by default, but you can install the GNOME extension or KDE applet to get a graphical interface.

**GNOME:** Install the `supergfxctl-gex` extension.  

**KDE:**
```bash
yay -S plasma6-applets-supergfxctl
```
Log out and log back in for the applet to appear on the taskbar.

Switch to Hybrid mode:
```bash
supergfxctl --mode Hybrid
```

{% hint style="info" %}
Changing to/from Hybrid mode requires logout. Ultimate mode requires a reboot.
{% endhint %}

{% endstep %}

{% step %}

### 5. Fixing Hotkeys

{% hint style="info" %}
Some hotkeys are handled by the BIOS directly and can’t be remapped. Test by creating a shortcut and see if it registers.
{% endhint %}

#### GNOME
- Go to **Settings > Keyboard > Shortcuts**
- Click “+” to add a new shortcut

#### KDE
- Go to **System Settings > Shortcuts > Custom Shortcuts**
- Create a new **Global Shortcut → Command/URL**

#### Add the following Commands with their own hotkey:
- Open Armoury Crate: `rog-control-center`
- Toggle Aura lighting: `asusctl aura -n`
- Change performance profile: `asusctl profile -n`

{% endstep %}

{% step %}

### 
{% endstep %}

{% step %}

### 6. Enable Bluetooth (If it isn't enabled by default)
Install:
```bash
sudo pacman -S bluez bluez-utils
```

Enable service:
```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

{% endstep %}

{% step %} 


### 7.Flatpak
Flatpak is a Linux tool for installing and managing software. It runs applications in a sandboxed environment, keeping them partially separated from the main system. It is a widely used platform that allows software to work across various Linux distributions.

Most Arch-based distributions come with Flatpak, but if it isn’t installed, you can use the following command to install it and add the Flatpak repository.

```bash
sudo pacman -S flatpak && flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

{% endstep %}

{% step %} 


### 8.Backups:
Timeshift is a powerful Linux backup tool that functions similarly to System Restore on Windows or Time Machine on macOS. It protects your system by creating incremental snapshots of your file system at regular intervals. These snapshots allow you to restore your system to a previous state, undoing any system changes or issues.

Installation:
```bash
sudo pacman -S  timeshift
```

How to Use Timeshift:

1. Select Snapshot Type: Choose between **RSYNC** and **BTRFS** based on your file system.

{% hint style="info" %} If your system is using the BTRFS file system, it is recommended to use the BTRFS snapshot option for better performance. If not, select RSYNC. {% endhint %}
  
2. Choose Snapshot Location: Select the disk or partition where snapshots will be saved.

3. Configure Snapshot Schedule: Enable periodic snapshots if desired and select a snapshot frequency (daily, weekly, or on boot).

4. Create a Snapshot: Click Create to manually create a snapshot at any time.

5. Restore a Snapshot: To undo system changes, select a previous snapshot and click Restore.

#### Restoring a Broken System Using Timeshift:

1. Boot from a Linux ISO with Timeshift installed.

2. Select the same snapshot type (BTRFS or RSYNC) as used before.

3. Choose the location where your backup is stored.

4. Select the desired backup from the list shown.

5. Click Restore to revert your system to the previous working state.

{% hint style="warning" %} Timeshift does not back up personal user files such as documents, pictures, or downloads. It focuses exclusively on system files and settings. {% endhint %}

{% endstep %}

{% step %} 


### 9. Power Management
If you aren’t satisfied with your battery life on Linux while using `power-profiles-daemon`, you may want to try alternative power management software. However, keep in mind that other software like tlp doesn’t really work well with asusctl, and battery life on Linux might be the same as on Windows no matter what you try.

#### 9.1 TLP

TLP is a feature-rich command line utility for Linux, saving laptop battery power without the need to delve deeper into technical details.
TLP's default settings are already optimized for battery life and implement Powertop's recommendations out of the box, so additional configuration is not needed. Also, TLP is completely customizable, which means you can get even more power savings or meet your exact requirements. 

Install TLP:

```bash
sudo pacman -S tlp
sudo systemctl enable tlp
sudo systemctl start tlp
```

{% hint style="info" %}
TLP conflicts with power-profiles-daemon. Remove it or mask its services with:

```bash
systemctl mask power-profiles-daemon.service
```
{% endhint %}

{% endstep %}

{% step %}


#### 9.2 Auto-CPUFreq

Automatic CPU speed & power optimizer for Linux. Actively monitors laptop battery state, CPU usage, CPU temperature, and system load, ultimately allowing you to improve battery life without making any compromises.

Using AUR:

```bash
yay -S auto-cpufreq
```

{% hint style="info" %}
Installation from the AUR requires a bit of manual intervention. You'll need to run the following command to disable power-profiles-daemon if it's installed, otherwise cpu-auto-freq may not function correctly:

```bash
systemctl mask power-profiles-daemon.service
```

{% endhint %}

Manual Install:

```bash
git clone https://github.com/AdnanHodzic/auto-cpufreq.git && cd auto-cpufreq && sudo ./auto-cpufreq-installer
```

{% hint style="info" %}
After installation, open the auto-cpufreq app and verify if it’s working properly.
{% endhint %}

{% endstep %}

{% step %}

### 10. Firewall
A firewall is a security system that monitors, filters, and controls incoming and outgoing network traffic according to predefined security rules. While it isn't mandatory to have a firewall for a workstation, it is highly recommended to set up some form of firewall. There are two firewall options depending on the netfilter installed:

This covers only the basic things about firewalls. If you want advanced configuration or documentation, please visit the respective page for the firewalls on the Arch Wiki.

To check which netfilter is installed, run the following command:
```bash
sudo pacman -Q | grep -E 'nftables|iptables'
```

## 10.1. Ufw:

If you have `iptables(legacy)` installed, then it is recommended to use Uncomplicated Firewall (ufw).

Install ufw and its GUI with:
```bash
sudo pacman -S ufw gufw
```

Then enable it with:
```bash
sudo ufw enable
```

The default configuration denies all incoming connections while allowing outgoing connections. The default should be enough for most users, but if you use Docker, SSH, or any torrent software, you will need to give it access through your firewall.

{% hint style="info" %}
The gufw package provides a GUI for ufw, which can be used to configure ufw. For your home network, set the profile as "home," and for a private network, set the profile as "public."
{% endhint %}

**To allow/deny access to a specific port, run the following command:**
```bash
sudo ufw allow <port>/<optional: protocol>
```

## 10.2. Firewalld:
Firewalld is recommended for systems that have `iptables-nft` installed, which includes most modern distros.

Install the firewalld package:
```bash
sudo pacman -S firewalld python-pyqt6
```

Then enable it with the following command:
```bash
sudo systemctl enable firewalld --now
```

Again, if you need to open a port or a service, you can either use the GUI or the terminal for it. The `firewalld` package contains a GUI by default, while the Python package adds applet support for firewalld.

To open/close a port or service, you can run the following command:
```bash
sudo firewall-cmd --zone=public --remove-port=<port>/<optional: protocol>
```

 **For Services:**
```bash
sudo firewall-cmd --zone=public --remove-service=<service>
```

By default, firewalld only saves these changes temporarily until a reboot. If you want to make the changes persistent, use the `--permanent` flag as such:

```bash
sudo firewall-cmd --permanent --zone=public --remove-service=<service>
```

Then reload firewalld to integrate changes into the current runtime:
```bash
sudo firewall-cmd --reload
```

{% endstep %}

{% endstepper %}
