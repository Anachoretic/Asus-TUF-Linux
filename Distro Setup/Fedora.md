---
title: Fedora
icon: fedora
---

# Fedora Post-Installation Guide

This guide walks you through setting up your Fedora system with Nvidia drivers, Asus tools, power management tweaks, backup solutions, multimedia codecs, DNF configuration, fonts, Steam installation, and more.

{% stepper %}

{% step %}

## 1. DNF Configuration:

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

## 2. RPM Fusion:

Fedora doesn’t provide certain software due to legal restrictions, patents, or because it is proprietary. This includes the NVIDIA driver, media codecs, Steam, and similar software.
RPM Fusion provides this software through its repositories: **Free (software that is restricted in Fedora due to legal reasons or patents)** and **Nonfree (proprietary software).**

```bash

sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

{% endstep %}

{% step %}

## 3. GPU Driver:

### 3.1 NVIDIA Driver Setup

While it’s possible to download and install NVIDIA drivers directly from NVIDIA’s website, you should avoid doing so unless you have a specific reason. Drivers installed manually cannot be updated through `dnf` and are likely to break with future system updates. For this reason, you should install NVIDIA drivers only through `dnf`.

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

After installing the driver, wait about 5 minutes for the kernel module to be built. Once that’s done, reboot the system. You can then test whether the driver is working by running `nvidia-smi` in the terminal. If it produces an output, the driver is installed and functioning correctly. If you see an error such as `NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver`, the driver isn’t working properly, and you may need to rebuild it.

<details><summary>Troubleshooting:</summary>

### Rebuilding Kernel Modules  
If you end up with a black screen after installing the drivers, or if `nvidia-smi` fails with the error `NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver`, it usually means the NVIDIA kernel module didn’t build properly. In such cases, you might be stuck at a black screen on boot, or the system may boot normally but the driver won’t work.
If you’re stuck at a black screen, press <kbd>Ctrl + Alt + F3</kbd> to switch to a TTY, log in, and try rebuilding the kernel module. If that still doesn’t fix the issue, reinstall the drivers and try again.

If you can still access your desktop, you can run the required commands directly without switching to a TTY.
```bash
sudo akmods --force
```
After running this command, wait about 5 minutes for it to finish, then run `sudo dracut -f --regenerate-all` and reboot your device.

### Uninstallation:
If rebuilding the kernel module didn’t work, you might need to uninstall the current drivers and reinstall them.
```bash
sudo dnf remove xorg-x11-drv-nvidia\* && sudo dracut -f --regenerate-all
```
{% hint style="info" %} 
If you get the error message "Nvidia kernel module missing, falling back to nouveau", it usually means one of three things: the NVIDIA kernel module was not built or installed properly, the dedicated GPU (dGPU) is disabled, or Secure Boot is enabled and is preventing the NVIDIA kernel module from loading. {% endhint %}


</details>

{% endstep %}

{% step %}

### 3.2 AMD/Intel:
The drivers are already installed as part of the kernel, no further steps are required.

<details><summary>Secure Boot:</summary>
Secure Boot is a security feature in UEFI (BIOS) that prevents unsigned software from loading during the boot process, potentially stopping malicious programs from running when the system first starts. Secure Boot isn’t mandatory on Linux and can be left disabled in most cases. In fact, some distributions require Secure Boot to be disabled because they won’t boot normally with it enabled. This is because their bootloaders or kernels are not signed, or the required keys are not present in the firmware. If you’re only running Linux, you can keep Secure Boot disabled, or enable it if you want the "extra security".

If you’re dual booting Windows, disabling Secure Boot and switching it on/off every time you switch OS can be annoying. In that case, it’s usually better to leave Secure Boot enabled so both operating systems boot normally.

If you don’t have an NVIDIA GPU, you can simply enable Secure Boot in the firmware and Fedora should boot normally. **However, if you have NVIDIA drivers installed (or any other third-party kernel modules), you’ll need to follow the steps below to enable Secure Boot. [These steps are based on the RPM Fusion Secure Boot guide.](https://rpmfusion.org/Howto/Secure%20Boot)** 

**Before proceeding, make sure Secure Boot is currently disabled and that the NVIDIA drivers are already installed.**

Install the following packages in order to generate and import the keys:
```bash
sudo dnf in sudo dnf install kmodtool akmods mokutil openssl
```

First, generate a key:
```bash
sudo kmodgenca -a
```
Then, import it into the firmware:
```bash
sudo mokutil --import /etc/pki/akmods/certs/public_key.der
```
{% hint style="info" %}
After a BIOS update, you will need to reimport the key, which can be done by running the command above.
{% endhint %}

You will be asked to create a password, which will be required later to complete the key enrollment, so make sure to remember it. After creating the password, reboot the system using `systemctl reboot`.

During reboot, the **MOK management** screen will appear; press any key to start **MOK management**, then select <kbd>Enroll MOK</kbd> and enter the password you created. After that, you will be prompted to reboot; press <kbd>Enter</kbd>, and while the system restarts, hold the <kbd>F2</kbd> key to enter the Bios, enable Secure Boot, save the changes, and exit.

{% hint style="warning" %}
The keyboard layout on the MOK management screen defaults to QWERTY regardless of your physical keyboard layout.
{% endhint %}

On the next boot, you may see the message `Nvidia kernel module missing, falling back to nouveau`, which is normal because Secure Boot prevents the unsigned NVIDIA module from loading. Once Fedora boots, open a terminal and run the command to regenerate the kernel module, wait for it to complete, then reboot again. After this, the NVIDIA driver should load and work properly.

```bash
sudo akmods --force --rebuild
```
**You can check the Secure Boot status by running `mokutil --sb-state` If it says `SecureBoot enabled`, then Secure Boot is active.**

</details>

## 4. Asus Software:

### 4.1 Asus Linux Tools:

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

### 4.2 GPU Switching:

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

## 4.3. Hotkeys:

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

## 5. Multimedia:
Add full multimedia support by installing codecs and tools for playing all common audio and video formats.

### 5.1 Get the Basics:

```bash
sudo dnf install libavcodec-freeworld
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf group install -y multimedia sound-and-video
```

### 5.2 Hardware Acceleration:

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


## 6. Backup:
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

## 7. Fonts:

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
