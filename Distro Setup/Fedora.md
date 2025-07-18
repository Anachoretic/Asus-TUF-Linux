# Post-Installation Configuration for Fedora-Based Systems

{% stepper %} {% step %}

## 1. Post Install Configuration

- If you're using an Nvidia dGPU, you'll need to install Nvidia's proprietary drivers manually.
- AMD users can skip this,Mesa drivers are built into the kernel and just work out of the box.

{% hint style="info" %} **Note:** Unlike Windows, most drivers are included in the kernel, so you don't usually need to install them manually. {% endhint %} {% endstep %}

{% step %}

## 2. GPU Driver Installation and Asus Software Setup

### 2.1 Nvidia Driver Installation (Fedora)

{% hint style="warning" %} **Important:** Make sure Secure Boot is turned off or the Nvidia driver won’t load. {% endhint %}

1. First, update the system. Always a good idea to start fresh:

{% code lineNumbers="true" %}

```bash
sudo dnf update
```

{% endcode %}

2. Enable RPM Fusion, which gives you access to Nvidia drivers and other non-free packages:

{% code lineNumbers="true" %}

```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

{% endcode %}

3. Install the required Nvidia packages:

- `akmod-nvidia`: Automatically builds the kernel module on new kernel updates.
- `xorg-x11-drv-nvidia-cuda`: Adds CUDA support if you're doing compute work.

{% code lineNumbers="true" %}

```bash
sudo dnf install akmod-nvidia
sudo dnf install xorg-x11-drv-nvidia-cuda
```

{% endcode %}

{% hint style="info" %} After installing, wait 5–8 minutes for the kernel module to build in the background. {% endhint %}

4. Enable power management for better battery handling and suspend/resume support:

{% code lineNumbers="true" %}

```bash
sudo systemctl enable nvidia-hibernate.service nvidia-suspend.service nvidia-resume.service nvidia-powerd.service
```

{% endcode %}

5. Modify GRUB to blacklist Nouveau (the open-source Nvidia driver) and enable Nvidia DRM:

{% code lineNumbers="true" %}

```bash
sudo nano /etc/default/grub
```

Then set this line:

```bash
GRUB_CMDLINE_LINUX="rd.driver.blacklist=nouveau modprobe.blacklist=nouveau nvidia-drm.modeset=1 rhgb quiet"
```

And regenerate the config:

```bash
sudo grub2-mkconfig -o /etc/grub2.cfg
```

{% endcode %}

### 2.2 Asus Software Installation

1. Add the COPR repo so you don’t have to compile anything manually:

{% code lineNumbers="true" %}

```bash
sudo dnf copr enable lukenukem/asus-linux
```

{% endcode %}

2. Install everything you’ll need:

- `asusctl`: Manages performance profiles, LED effects, and fan control.
- `supergfxctl`: Lets you switch between GPU modes.
- `rog-control-center`: GUI tool for controlling Asus features.

{% code lineNumbers="true" %}

```bash
sudo dnf install asusctl supergfxctl rog-control-center
```

{% endcode %}

3. Enable the GPU switching daemon:

{% code lineNumbers="true" %}

```bash
sudo systemctl enable supergfxd.service
sudo systemctl start supergfxd.service
```

{% endcode %}

{% hint style="info" %} Ignore the "Asus kernel isn't loaded" message in `rog-control-center`. It’s safe. {% endhint %}

### 2.3 Switching GPU Modes with a GUI

- **GNOME users:** Grab the [supergfxctl-gex extension](https://extensions.gnome.org/extension/5344/supergfxctl-gex).
- **KDE users:** Use the [supergfxctl-plasmoid](https://gitlab.com/Jhyub/supergfxctl-plasmoid).

1. For KDE, install the plasmoid:

{% code lineNumbers="true" %}

```bash
sudo dnf copr enable jhyub/supergfxctl-plasmoid
sudo dnf install supergfxctl-plasmoid
```

{% endcode %}

2. Reload Plasma to apply changes:

{% code lineNumbers="true" %}

```bash
plasmashell --replace &
```

{% endcode %}

3. Set GPU mode to Hybrid for daily use:

{% code lineNumbers="true" %}

```bash
supergfxctl --mode Hybrid
```

{% endcode %}

A GPU icon should now appear in your taskbar, making it easy to switch modes without using the terminal.

{% hint style="info" %} **Note:** Changing to/from *Hybrid* mode requires logging out and back in. *Ultimate* mode needs a full reboot. {% endhint %} {% endstep %}

{% step %}

## 3. Fixing Hotkeys

{% hint style="info" %} Some hotkeys are BIOS-level and can’t be changed. To check if one is remappable, try assigning it in your shortcut settings. If it registers, you’re good. {% endhint %}

### 3.1 GNOME

- Head to **Settings > Keyboard > Keyboard Shortcuts**
- Click “+” to add your own shortcut

### 3.2 KDE

- Go to **System Settings > Shortcuts > Custom Shortcuts**
- Create a new **Global Shortcut → Command/URL**

#### Commands that need to be addded:

- Open Armoury Crate: `rog-control-center`
- Toggle Aura lighting: `asusctl aura -n`
- Change performance profile: `asusctl profile -n` {% endstep %} {% endstepper %}

