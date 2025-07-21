# Fedora Post-Installation Guide

This guide walks you through setting up your Fedora system with Nvidia drivers, Asus tools, and power management tweaks.



## Step 1: Post Install Configuration

{% stepper %} {% step %} If you're using an Nvidia dGPU, you'll need to install Nvidia's proprietary drivers manually.

AMD users can skip this, as Mesa drivers are built into the kernel and work out of the box.

{% hint style="info" %} Note: Unlike Windows, most drivers are included in the kernel, so you don't usually need to install them manually. {% endhint %} {% endstep %}

## Step 2: GPU Driver Installation and Asus Software Setup

{% step %}

### 2.1 Nvidia Driver Installation (Fedora)

{% hint style="warning" %} Important: Make sure Secure Boot is turned off or the Nvidia driver won’t load. {% endhint %}

Start by updating the system:

```bash
sudo dnf update
```

Enable RPM Fusion:

```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

Install Nvidia packages:

```bash
sudo dnf install akmod-nvidia
sudo dnf install xorg-x11-drv-nvidia-cuda
```

{% hint style="info" %} After installing, wait 5–8 minutes for the kernel module to build in the background. {% endhint %}

Enable Nvidia power management:

```bash
sudo systemctl enable nvidia-hibernate.service nvidia-suspend.service nvidia-resume.service nvidia-powerd.service
```

Blacklist Nouveau and enable Nvidia DRM:

```bash
sudo nano /etc/default/grub

# Add this:
GRUB_CMDLINE_LINUX="rd.driver.blacklist=nouveau modprobe.blacklist=nouveau nvidia-drm.modeset=1 rhgb quiet"
```

{% endstep %}

{% step %}

### 2.2 Asus Software Installation

Add the COPR repo:

```bash
sudo dnf copr enable lukenukem/asus-linux
```

Install tools:

```bash
sudo dnf install asusctl supergfxctl rog-control-center
```

Enable GPU switching:

```bash
sudo systemctl enable supergfxd.service
sudo systemctl start supergfxd.service
```

{% hint style="info" %} Ignore the "Asus kernel isn't loaded" message in rog-control-center. It’s safe. {% endhint %} {% endstep %}

{% step %}

### 2.3 Switching GPU Modes with a GUI

GNOME: Use `supergfxctl-gex`

KDE: Use the plasmoid extension:

```bash
sudo dnf copr enable jhyub/supergfxctl-plasmoid
sudo dnf install supergfxctl-plasmoid
```

Reload Plasma:

```bash
plasmashell --replace &
```

Set Hybrid GPU mode:

```bash
supergfxctl --mode Hybrid
```

{% hint style="info" %} Note: Switching to/from Hybrid mode needs logout. Ultimate mode requires a reboot. {% endhint %} {% endstep %}

## Step 3: Fixing Hotkeys

{% step %} Some hotkeys are BIOS-level and can’t be remapped.

{% hint style="info" %} To test remap capability: press the key while adding a shortcut. If nothing registers, it can't be reassigned. {% endhint %}

### GNOME

- Settings → Keyboard → Shortcuts → “+”

### KDE

- System Settings → Shortcuts → Custom Shortcuts → New Global Shortcut → Command/URL

**Commands:**

- `rog-control-center`: Launch GUI
- `asusctl aura -n`: Toggle Aura lighting
- `asusctl profile -n`: Change power profile {% endstep %}

## 3. Power Management
### 3.1 TLP

TLP is a feature-rich command line utility for Linux, saving laptop battery power without the need to delve deeper into technical details. TLP's default settings are already optimized for battery life and implement Powertop's recommendations out of the box, so additional configuration is not needed. Also, TLP is completely customizable, which means you can get even more power savings or meet your exact requirements.

Install TLP:

```bash
sudo dnf install tlp
```

Then enable tlp with the following commands:
```bash
sudo systemctl enable tlp
sudo systemctl start tlp
```

{% hint style="info" %}
TLP conflicts with power-profiles-daemon. Remove it or mask its services with:

```bash
systemctl mask power-profiles-daemon.service
```

{% endhint %}
### 3.2 Auto-CPUFreq

Automatic CPU speed & power optimizer for Linux. Actively monitors laptop battery state, CPU usage, CPU temperature, and system load, ultimately allowing you to improve battery life without making any compromises.

Manual Install:

```bash
git clone https://github.com/AdnanHodzic/auto-cpufreq.git && cd auto-cpufreq && sudo ./auto-cpufreq-installer
```


{% hint style="info" %}
You'll need to run the following command to disable power-profiles-daemon if it's installed, otherwise cpu-auto-freq may not function correctly:

```bash
systemctl mask power-profiles-daemon.service
```

{% endhint %}



{% hint style="info" %}
After installation, open the cpu-auto-freq app and verify if it’s working properly.
{% endhint %}
