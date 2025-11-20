# Fedora Post-Installation Guide

This guide walks you through setting up your Fedora system with Nvidia drivers, Asus tools, power management tweaks, backup solutions, multimedia codecs, DNF configuration, fonts, Steam installation, and more.

{% stepper %}

{% step %}

## Step 1: Enable RPM Fusion

Fedora doesn't ship certain stuff like proprietary drivers and codecs out of the box. RPM Fusion adds that in. You’ll need both **free** and **nonfree** versions.

### Enable RPM Fusion Free and Non-Free repositories

```bash

sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

{% endstep %}

{% step %}

## Step 2: GPU Driver Installation and Asus Software Setup

### 2.1 NVIDIA Driver Setup

{% hint style="warning" %}Make sure Secure Boot is turned off or the Nvidia driver won’t load. {% endhint %}


Start by updating the system:

```bash
sudo dnf update
```

Install Nvidia packages:

```bash
sudo dnf install akmod-nvidia
sudo dnf install xorg-x11-drv-nvidia-cuda
```

{% hint style="info" %} After installing, wait 3–6 minutes for the kernel module to build in the background. {% endhint %}

Enable Nvidia power management:

```bash
sudo systemctl enable nvidia-hibernate.service nvidia-suspend.service nvidia-resume.service nvidia-powerd.service
```

### 2.2 Asus Linux Tools

These give you access to GPU modes, fan profiles, and Aura lighting control.

```bash
# Enable the ASUS tool repository
sudo dnf copr enable lukenukem/asus-linux

# Install the tools
sudo dnf install asusctl supergfxctl rog-control-center

# Enable the service for GPU switching
sudo systemctl enable --now supergfxd.service
```

{% hint style="info" %} Ignore the "Asus kernel isn't loaded" message in rog-control-center. It’s safe. {% endhint %}

### 2.3 GPU Switching (Hybrid/Integrated/Discrete)

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

{% endstep %}

{% step %}

## Step 3: Fix Hotkeys (Asus Only)

Some hotkeys are BIOS-level and can’t be remapped.

{% hint style="info" %} To test remap capability: press the key while adding a shortcut. If nothing registers, it can't be reassigned. {% endhint %}

### For GNOME
- Settings → Keyboard → Shortcuts → “+”

### For KDE
- System Settings → Shortcuts → Custom Shortcuts → New Global Shortcut

Add the following Commands:

- `rog-control-center`: Launch GUI
- `asusctl aura -n`: Toggle Aura lighting
- `asusctl profile -n`: Change power profile


{% endstep %}

{% step %}

## Step 4: Power Management

If you notice that your battery life on Linux is significantly shorter compared to Windows, you may benefit from additional power management tools. Two of the most commonly recommended options are **TLP** and **CPU AutoFreq**. These tools help optimize power usage, particularly on laptops, by dynamically adjusting CPU frequencies and managing various power-related settings.

{% hint style="warning" %} **Important**: Only install **one** of these tools. Running both simultaneously can cause conflicts and lead to unexpected behavior. {% endhint %}

### 4.1 TLP

TLP is a feature-rich command-line utility for Linux that helps extend battery life without requiring manual tuning. Its default configuration is already optimized and implements many of Powertop’s recommendations.

**Install TLP:**

```bash
sudo dnf install tlp
```

**Enable TLP:**

```bash
sudo systemctl enable tlp
sudo systemctl start tlp
```

{% hint style="info" %} TLP conflicts with power-profiles-daemon. Remove it or mask its service:

```bash
systemctl mask power-profiles-daemon.service
```

{% endhint %}

### 4.2 CPU AutoFreq

CPU AutoFreq is a real-time CPU frequency and power optimizer. It monitors your system load, battery level, and temperature to dynamically manage CPU scaling for better battery life.

**Manual Install:**

```bash
git clone https://github.com/AdnanHodzic/auto-cpufreq.git && cd auto-cpufreq && sudo ./auto-cpufreq-installer
```

{% hint style="info" %} If power-profiles-daemon is installed, disable it:

```bash
systemctl mask power-profiles-daemon.service
```

{% endhint %}

{% hint style="info" %} After installation, open the cpu-auto-freq app and verify if it’s working properly. {% endhint %}

{% endstep %}

{% step %}



## Step 5: Enable Flatpak Support

By default, Fedora restricts the set of available Flatpak apps. You can unlock full Flatpak support by enabling third-party repos during the initial setup, or manually with the following command:

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

{% endstep %}

{% step %}


## Step 6: Multimedia and Hardware Codecs
Add full multimedia support by installing codecs and tools for playing all common audio and video formats.

### 6.1 Get the Basics
```bash
sudo dnf install libavcodec-freeworld
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf group install -y multimedia sound-and-video
```

### 6.2 Enable Hardware Acceleration

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

## Step 7: DNF Configuration

You can configure DNF to increase download speeds by enabling parallel downloads and selecting the fastest mirror.

**Edit the config file:**

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

## Step 8: Create a Backup
### 1. System Settings Backup with Timeshift

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

### 2.Backup Personal Files with Pika Backup

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

## Step 9: Install Steam

Make sure RPM Fusion is enabled first.
```bash
sudo dnf install steam
```

{% endstep %}

{% step %}

## Step 10: Install Fonts (Fix weird web text)

Some websites or apps might look broken without these:

```bash
# Microsoft and emoji fonts
sudo dnf install msttcore-fonts-installer
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
sudo dnf install google-noto-emoji-color-fonts

# Rebuild font cache
fc-cache -f
```

{% endstep %}

{% endstepper %}
