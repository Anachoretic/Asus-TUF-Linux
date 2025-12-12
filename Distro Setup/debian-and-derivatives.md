---
icon: debian
---


# Setup (Debian)

Officially, tools like asusctl and supergfxctl aren’t supported on Debian. While this won’t stop you from running Debian on your laptop, you might need to use other software that can partially do the same job.

The main problem is updates. If you have a newer laptop, there’s a good chance some of your hardware might not be fully supported on Debian, things like your keyboard, trackpad, or GPU might not work properly, or at all, because of older packages and kernels. For that reason, it’s recommended to use a distro that’s updated more frequently, such as Fedora, Tumbleweed, or Arch.

{% stepper %}

{% step %}

## 1. Updates:

After installation, it’s crucial to update the system to ensure all packages are up-to-date.

```bash
sudo apt update && sudo apt upgrade
```

{% endstep %}

{% step %}

## 2. Driver's:

By default, most drivers (e.g., for Intel, AMD, and other hardware) are included in the Linux kernel. However, **Nvidia dGPU drivers must be installed separately.**

<details>
<summary><strong>Debian(Trixie):</strong></summary>

To install the NVIDIA driver on Debian, you’ll need to add the non-free  sources.

First, create a backup of the sources file in case something goes wrong:

First create a backup of the sources file in case something goes wrong:
```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

Then, edit the sources file:
```bash
sudo nano /etc/apt/sources.list
```

-  deb http://deb.debian.org/debian/ trixie <span style="color:red">main contrib non-free non-free-firmware</span>

- deb-src http://deb.debian.org/debian/ trixie <span style="color:red">main contrib non-free non-free-firmware</span>

- deb http://deb.debian.org/debian-security/ trixie-security <span style="color:red">main contrib non-free non-free-firmware</span> 

- deb-src http://deb.debian.org/debian-security/ trixie-security <span style="color:red">main contrib non-free non-free-firmware</span>

- deb http://deb.debian.org/debian/ trixie-updates <span style="color:red">main contrib non-free non-free-firmware</span> 

- deb-src http://deb.debian.org/debian/ trixie-updates <span style="color:red">main contrib non-free non-free-firmware</span>

 

**The following links already exist in the sources section, but components like `non-free` and `non-free-firmware` may be missing. Simply compare the existing entries with the ones highlighted in red and add any missing components, nothing else needs to be changed. If that’s unclear, you can instead comment out the existing sources and copy these updated ones to the end of the file.**


 After editing the file update your system with
```bash
sudo apt update
```

Then, if anything breaks after adding the new entries, you can restore the original file with:
```bash
sudo cp /etc/apt/sources.list.bak /etc/apt/sources.list
```

Now install the `nvidia-detect` package along with the dependencies for the driver: the driver:
```bash
sudo apt install nvidia-detect linux-headers-amd64
```

After that, simply launch `nvidia-detect` from the terminal and install the recommended NVIDIA driver.

```bash
sudo apt install nvidia-driver-full
```

Also, install and enable the GPU switching wrapper for Debian.
```bash
sudo apt install switcheroo-control && systemctl enable switcheroo-control --now
```

</details>

<details>

<summary><strong>Ubuntu/Mint</strong></summary>

Use the built-in **Driver Manager** or **Software & Updates > Additional Drivers**. Ensure that proprietary drivers (not Nouveau) are selected.

</details>


{% hint style="warning" %}
**Important:** Set GPU mode to *Hybrid* or *Ultimate* in the Windows before installation or through supergfxctl, or the Driver installer might not detect the GPU.
{% endhint %}


{% endstep %}

{% step %}

## 3. Flatpak:

If your distro doesn’t come with Flatpak by default, you can add Flatpak with the following command. If your distro already has Flatpak enabled, you can skip this step.

```bash
sudo apt install flatpak
sudo apt install gnome-software-plugin-flatpak #For Gnome
sudo apt install plasma-discover-backend-flatpak #For KDE
```

Then, add the Flathub repository:

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
{% endstep %}

{% step %}

## 4. Asus Software:

{% hint style="warning" %}
**Note:** These tools need to be built from source. Follow steps carefully.
{% endhint %}

### 4.1 Build Dependencies**

Install required development tools and libraries:

```bash
sudo apt install -y build-essential git cmake pkg-config libpci-dev libsysfs-dev libudev-dev \
libboost-dev libgtk-3-dev libglib2.0-dev libseat-dev
```

Install Rust (Cargo):

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Configure the environment:

```bash
. "$HOME/.cargo/env"
```

### 4.2 Installing `asusctl`

1. Clone the repository or download the latest release.
2. Extract it and open a terminal inside the `asusctl` folder.
3. Run:

```bash
make && sudo make install
```

{% hint style="info" %}
This software requires **Wayland.** It will not work under X11.

Icons may appear missing for the first few reboots, this is expected.
{% endhint %}

### 4.3 Installing `supergfxctl`

1. Clone the repository:

```bash
git clone https://gitlab.com/asus-linux/supergfxctl.git
cd supergfxctl
```

2. Build and install:

```bash
make && sudo make install
```

3. Enable and start the service:

```bash
sudo systemctl enable supergfxd
sudo systemctl start supergfxd
```

### 4.4 GUI Support

* **GNOME:** Install the [`supergfxctl-gex`](https://extensions.gnome.org/extension/5344/supergfxctl-gex) extension.

* **Other DEs (e.g., Cinnamon, MATE,XFCE):** Use CLI commands.

**4.5 Switching GPU Modes via CLI**

```bash
supergfxctl --mode Integrated   # Use integrated GPU only
supergfxctl --mode Hybrid       # Use both integrated and discrete GPU
supergfxctl --mode AsusMuxDgpu  # Use discrete GPU only
```

To check available options:

```bash
supergfxctl --help
```
{% endstep %}

{% step %}

### 4.5. Hotkeys:

{% hint style="info" %}
Some hotkey functions are controlled at the BIOS level and cannot be remapped. To check if a hotkey can be remapped, try creating a shortcut. If the input is detected, remapping is possible.
{% endhint %}

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


### Commands:

* Armoury Crate: `rog-control-center`
* Aura Mode: `asusctl aura -n`
* Performance Mode: `asusctl profile -n`

{% endstep %}

{% step %}

## 5. Backup's:

### 1. System Settings Backup:

Timeshift is a powerful Linux backup tool that functions similarly to System Restore on Windows or Time Machine on macOS. It protects your system by creating incremental snapshots of your file system at regular intervals. These snapshots allow you to restore your system to a previous state, undoing any system changes or issues.

Installation:
```bash
sudo apt install timeshift
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

### 2.Backup Personal Files with Pika Backup:

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

## 6. Multimedia Support

To enable playback for formats like MP3, MPEG4, AVI, and more, you’ll need to install the necessary media codecs. Ubuntu doesn’t include them out of the box due to licensing restrictions.

```bash
sudo apt install ubuntu-restricted-extras
```

{% endstep %}

{% step %}

## 7. Firewall

{% endstep %}

{% endstepper %}
