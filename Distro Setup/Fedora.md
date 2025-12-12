---
title: Fedora
icon: fedora
---

# Fedora Post-Installation Guide

This guide walks you through setting up your Fedora system with Nvidia drivers, Asus tools, power management tweaks, backup solutions, multimedia codecs, DNF configuration, fonts, Steam installation, and more.

{% stepper %}

{% step %}

## 1. DNF Configuration

Configure DNS to use the fastest available mirrors and increase the number of parallel downloads.

**Edit the config file for DNF:**

```bash
sudo nano /etc/dnf/dnf.conf
```

**Add these lines at the end:**

```bash
max_parallel_downloads=10
fastestmirror=true
```
{% endstep %}

{% step %}

## 2. Enable RPM Fusion

Fedora doesn't ship certain stuff like proprietary drivers and codecs out of the box. RPM Fusion adds that in. You’ll need both **free** and **nonfree** versions.

### Enable RPM Fusion Free and Non-Free repositories

```bash

sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

{% endstep %}

{% step %}

## 3. GPU Driver Installation:

### 3.1 NVIDIA Driver Setup

{% hint style="warning" %}Make sure Secure Boot is turned off or the Nvidia driver won’t load. {% endhint %}


Start by updating the system:

```bash
sudo dnf update
```
If you have a newer GPU (4000 series and above), it is recommended to use only the open kernel module, as the proprietary drivers won’t work with these cards. To make the open driver the default, simply run the following command and continue with the setup as usual.

```bash
sudo sh -c 'echo "%_with_kmod_nvidia_open 1" > /etc/rpm/macros.nvidia-kmod'
```

Install Nvidia packages:

```bash
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
```

{% hint style="info" %} After installing, wait 3–6 minutes for the kernel module to build in the background. {% endhint %}

Enable Nvidia power management:

```bash
sudo systemctl enable nvidia-hibernate.service nvidia-suspend.service nvidia-resume.service nvidia-powerd.service
```


{% endstep %}

{% step %}


## 4. Asus Software Setup

### 4.1 Asus Linux Tools

These give you access to GPU modes, fan profiles, and Aura lighting control.


First, add the repository.
```bash
sudo dnf copr enable lukenukem/asus-linux
```

Then install the following packages:

```bash
sudo dnf install asusctl supergfxctl rog-control-center
```
Finally, enable the supergfxd service.
```bash
sudo systemctl enable --now supergfxd.service
```

{% hint style="info" %} Ignore the "Asus kernel isn't loaded" message in rog-control-center. It’s safe. {% endhint %}

### 4.2 GPU Switching (Hybrid/Integrated/Discrete)

- GNOME users: [supergfxctl-gex](https://extensions.gnome.org/extension/5344/supergfxctl-gex/)

- KDE users:Install the supergfxctl-plasmoid:

```bash
sudo dnf copr enable jhyub/supergfxctl-plasmoid
sudo dnf install supergfxctl-plasmoid
```

Reload Plasma:

Reboot for the changes to take effect.

Set Hybrid GPU mode:

```bash
supergfxctl --mode Hybrid
```
{% hint style="info" %} Switching to/from Hybrid mode needs logout. Ultimate mode requires a reboot. {% endhint %}

## 4.3. Fix Hotkeys (Asus Only)

Some hotkeys are BIOS-level and can’t be remapped.

{% hint style="info" %} To test remap capability: press the key while adding a shortcut. If nothing registers, it can't be reassigned. {% endhint %}

<details><summary>GNOME</summary>
Go the following:
Settings > Keyboard > View and Customize Shortcuts > Custom Shortcuts

![](/Images/GNOME/KBD-1.png)

Then, click Add a Shortcut.

![](/Images/GNOME/KBD-2.png)

![](/Images/GNOME/KBD-3.png)

Next, enter the command in the Command field. For the shortcut, click Set Shortcut and press the hotkey you want to assign. After that, give your shortcut a name, and finally, click Add. The shortcut should now work normally.
</details>

<details><summary>KDE</summary>

Go to Settings > Keyboard > Shortcuts and click Add New (Command or Script).

![](/Images/KDE/KBD-1.png)

Enter the command and assign a name for the hotkey.

![](/Images/KDE/KBD-2.png)

Locate the command you just added in the Command section, then click Add under Custom Shortcuts.

![](/Images/KDE/KBD-3.png)

Assign the key combination you want, then click Apply.

![](/Images/KDE/KBD-4.png)
</details>

Commands:

- `rog-control-center`: Launch GUI
- `asusctl aura -n`: Toggle Aura lighting
- `asusctl profile -n`: Change power profile


{% endstep %}

{% step %}

## 5. Multimedia and Hardware Codecs
Add full multimedia support by installing codecs and tools for playing all common audio and video formats.

### 5.1 Get the Basics
```bash
sudo dnf install libavcodec-freeworld
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf group install -y multimedia sound-and-video
```

### 5.2 Enable Hardware Acceleration

**Intel:**
```bash
sudo dnf install intel-media-driver
```

**AMD:** 
Since Fedora 37 and later, hardware acceleration is disabled by default. Use the `*-freeworld` versions.

```bash
# For 64-bit
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld

# For 32-bit (Steam, Wine, etc.)
sudo dnf swap mesa-va-drivers.i686 mesa-va-drivers-freeworld.i686
sudo dnf swap mesa-vdpau-drivers.i686 mesa-vdpau-drivers-freeworld.i686
```

**NVIDIA:** 
NVIDIA doesn't support VAAPI natively, but there's a wrapper available.

```bash
sudo dnf install libva-nvidia-driver.{i686,x86_64}
```

{% endstep %}

{% step %}


## 6. Create a Backup
### 6.1. System Settings Backup with Timeshift

Timeshift is a powerful Linux backup tool that functions similarly to System Restore on Windows or Time Machine on macOS. It protects your system by creating incremental snapshots of your file system at regular intervals. These snapshots allow you to restore your system to a previous state, undoing any system changes or issues.

Installation:
```bash
sudo dnf install timeshift
```
How to Use Timeshift:
1. Select Snapshot Type: Choose between **RSYNC** and **BTRFS** based on your file system.

{% hint style="info" %} If your system is using the BTRFS file system, it is recommended to use the BTRFS snapshot option for better performance. If not, select RSYNC. {% endhint %}
  
2. Choose Snapshot Location: Select the disk or partition where snapshots will be saved.

3. Configure Snapshot Schedule: Enable periodic snapshots if desired and select a snapshot frequency (daily, weekly, or on boot).

4. Create a Snapshot: Click Create to manually create a snapshot at any time.

5. Restore a Snapshot: To undo system changes, select a previous snapshot and click Restore.

### Restoring a Broken System Using Timeshift:

1. Boot from a Linux ISO with Timeshift installed.

2. Select the same snapshot type (BTRFS or RSYNC) as used before.

3. Choose the location where your backup is stored.

4. Select the desired backup from the list shown.

5. Click Restore to revert your system to the previous working state.

{% hint style="warning" %} Timeshift does not back up personal user files such as documents, pictures, or downloads. It focuses exclusively on system files and settings. {% endhint %}

### 6.2. Backup Personal Files with Pika Backup

Pika Backup is a user-friendly tool designed for personal data backup. It leverages the BorgBackup engine for secure and efficient backups. Note that Pika Backup does **not** support full system restoration.

### Installation

Install Pika Backup via Flatpak:

```bash
flatpak install flathub org.gnome.World.PikaBackup
```
#### Creating a Backup

1. Open Pika Backup
2. Select Storage Location: Choose a **USB drive or external disk** for storing backups. **Using a USB drive is recommended.**
3. Enable Encryption: Choose to encrypt your backups if you want added security.
4. Create the Backup:  Click on **" Backup Now"** to create a backup.


### Restoring Files from a Backup
1. Go to the **Archives Tab** in Pika Backup.
2. Select the Preferred Backup you want to restore.
3. Click the **Drop-down Arrow** next to the archive entry.
4. Choose **"Browse Saved Files"**.
5. A file browser will open showing the backed-up contents.
6. Manually Copy the desired files or folders to your main directory or another location.

{% endstep %}

{% step %}

## 7. Install Fonts (Fix weird web text):

Some websites or apps might look broken without these:

```bash
# Microsoft and emoji fonts
sudo dnf install msttcore-fonts-installer
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
sudo dnf install google-noto-emoji-color-fonts
```
Rebuild font cache
```bash
fc-cache -f
```

{% endstep %}

{% endstepper %}
