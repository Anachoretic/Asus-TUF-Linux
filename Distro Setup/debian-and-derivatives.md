# Setup (Debian)

# Post-Installation Configuration for Debian-Based Systems

{% hint style="danger" %}
**Note:** Debian and its derivatives (Ubuntu, Mint, Pop! OS, etc.) are considered **unsupported** on the official [Asus Linux](https://asus-linux.org/) site. Software such as **asusctl** and **supergfxctl** must be compiled from source. Fedora is the recommended distribution for full support. If you still wish to proceed with Debian, follow this guide carefully. Some features may not function as expected.
{% endhint %}

{% stepper %}
{% step %}
## 1. System Update

After installation, it’s crucial to update the system to ensure all packages are up-to-date.

```bash
sudo apt update && sudo apt upgrade
```

This command updates the package list and upgrades all installed packages.
{% endstep %}

{% step %}
## 2. Driver Installation

By default, most drivers (e.g., for Intel, AMD, and other hardware) are included in the Linux kernel. However, **Nvidia dGPU drivers must be installed separately.**

* **Debian users:** Follow the [Debian Nvidia Guide](https://wiki.debian.org/NvidiaGraphicsDrivers).
* **Ubuntu/Mint/Pop!\_OS users:** Use the built-in **Driver Manager** or **Software & Updates > Additional Drivers**. Ensure that proprietary drivers (not Nouveau) are selected.

{% hint style="warning" %}
**Important:** Set GPU mode to \*Hybrid\* or \*Ultimate\* in the Windows or through supergfxctl, or the Driver installer might not detect the GPU.
{% endhint %}

* **Pop!\_OS:** Offers a dedicated Nvidia ISO. If you have an Nvidia GPU, this is the recommended option.
{% endstep %}

{% step %}
## 3. Flatpak Installation (Ubuntu Only)

Most Debian-based distributions ship with Flatpak, but Ubuntu defaults to Snap. To install Flatpak on Ubuntu:

```bash
sudo apt install flatpak
sudo apt install gnome-software-plugin-flatpak
```

Then, add the Flathub repository:

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
{% endstep %}

{% step %}
## 4. Asus Software Installation (Manual Compilation)

{% hint style="warning" %}
**Note:** These tools need to be built from source. Follow steps carefully.
{% endhint %}

### 4.1 Install Build Dependencies**

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

### 4.4 Optional GUI Support

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
## 5. Fixing Hotkeys

{% hint style="info" %}
Some hotkey functions are controlled at the BIOS level and cannot be remapped. To check if a hotkey can be remapped, try creating a shortcut. If the input is detected, remapping is possible.
{% endhint %}

### 5.1 GNOME

* Go to **Settings > Keyboard > Keyboard Shortcuts**
* Click “+” to add a new shortcut

### 5.2 KDE

* Go to **System Settings > Shortcuts > Custom Shortcuts**
* Create a new **Global Shortcut → Command/URL**

Enter a name, press the desired key (e.g., Fn+F4), and set one of the following commands:

* Armoury Crate: `rog-control-center`
* Aura Mode: `asusctl aura -n`
* Performance Mode: `asusctl profile -n`
{% endstep %}

{% step %}

## Step 6: Power Management

If you notice that your battery life on Linux is significantly shorter compared to Windows, you may benefit from additional power management tools. Two of the most commonly recommended options are **TLP** and **CPU AutoFreq**. These tools help optimize power usage, particularly on laptops, by dynamically adjusting CPU frequencies and managing various power-related settings.

{% hint style="warning" %} **Important**: Only install **one** of these tools. Running both simultaneously can cause conflicts and lead to unexpected behavior. {% endhint %}

### 6.1 TLP

TLP is a feature-rich command-line utility for Linux that helps extend battery life without requiring manual tuning. Its default configuration is already optimized and implements many of Powertop’s recommendations.

**Install TLP:**

```bash
sudo apt install tlp
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

### 6.2 CPU AutoFreq

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

## Step 7: Create a Backup
### 1. System Settings Backup with Timeshift

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



## Step 8: Multimedia Support

To enable playback for formats like MP3, MPEG4, AVI, and more, you’ll need to install the necessary media codecs. Ubuntu doesn’t include them out of the box due to licensing restrictions.

```bash
sudo apt install ubuntu-restricted-extras
```

{% endstep %}

{% step %}

## Step 9: Install Essential Software (Optional)

Install the software you need for your workflow. Below is a list of apps I typically install along with a brief description of what each does. You can install these either via the terminal or using Flatpak, but I recommend using the terminal for simplicity and speed.

```bash
sudo apt install <package name> # To install a software.
sudo apt remove <package name > # To uninstall a software.
```

* gedit - Simple text editor for GNOME.
* VLC	- Plays almost all media formats.
* htop - Terminal system monitor.
* GIMP - Powerful image editor(Photoshop alternative).
* LibreOffice / OnlyOffice - Office suites for documents.
* GParted - 	Tool to manage disk partitions.
* VSCodium - VS Code without telemetry.
* Obsidian - Markdown note-taking app.
* KDE Connect - Phone integration with your PC.
* Spotify - Music streaming player.
* OBS Studio - Screen recording and streaming software.
* OpenRGB - Control RGB lighting without vendor software.
* OpenRazer - 	Linux driver for Razer device lighting control.


{% endstep %}

{% endstepper %}
