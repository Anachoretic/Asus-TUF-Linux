# Arch Linux Post-Install Guide

{% stepper %}

{% step %}

### 1. Install an AUR Helper

One of the most prominent and enticing features of Arch Linux is the **Arch User Repository (AUR)**. The AUR is a community-driven repository that provides package descriptions (PKGBUILDs). These allow you to compile software from source using `makepkg` and then install it with `pacman`.

To use the AUR, you’ll need to install an **AUR helper** such as `yay` or `paru`. Most Arch-based distros (like Manjaro or EndeavourOS) already come with one preinstalled, but on pure Arch Linux you’ll need to set it up yourself.

**Yay (Yet Another Yogurt – written in Go):**
```bash
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

**Paru (a pacman wrapper with extra features):**
```bash
sudo pacman -S --needed base-devel
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

An AUR helper isrequired for installing **Asus software** on Arch, and you can also use it to install popular applications like **Visual Studio Code**, **DaVinci Resolve**, and much more.

{% hint style="warning" %}
The AUR is community-driven. Anyone can upload packages, so there’s always a small risk of malicious code. Always **read the PKGBUILD file** before installing to see exactly what commands will run on your system.
{% endhint %}

{% endstep %}

{% step %}

### 2. Enable Multilib (Required for Drivers and Wine)

Open the pacman configuration file:
```bash
sudo nano /etc/pacman.conf
```

Uncomment these lines:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

### 2.1 Driver Installation

If you're using an **Nvidia GPU**, you’ll also need to install proprietary drivers manually. **AMD users can skip this**, since Mesa drivers are included in the kernel and work out of the box.

{% hint style="info" %}
Unlike Windows, most drivers are already included in the Linux kernel. You usually don’t need to install them manually.
{% endhint %}

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

**Manual installation (other Arch-based distros):**
```bash
sudo pacman -S nvidia nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader
```

**Optional: Vulkan drivers**
- AMD:
```bash
sudo pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon libva-mesa-driver libva-utils
```
- Intel:
```bash
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver libva-utils
```

{% hint style="info" %}
After installing, wait 5–8 minutes for the kernel module to build in the background.
{% endhint %}

Enable Nvidia services:
```bash
sudo systemctl enable nvidia-hibernate.service nvidia-suspend.service nvidia-resume.service nvidia-powerd.service
```

### 2.2 Asus Software Installation

Install Asus tools:
```bash
yay -S asusctl rog-control-center supergfxctl
```

Enable GPU switching daemon:
```bash
sudo systemctl enable supergfxd.service
sudo systemctl start supergfxd.service
```

{% hint style="info" %}
Ignore the "Asus kernel isn’t loaded" message in `rog-control-center`. It’s safe.
{% endhint %}

### 2.3 Switching GPU Modes

**GNOME:** Install the `supergfxctl-gex` extension.  
**KDE:**
```bash
yay -S plasma6-applets-supergfxctl
plasmashell --replace &
```

Switch to Hybrid mode:
```bash
supergfxctl --mode Hybrid
```

{% hint style="info" %}
Changing to/from Hybrid mode requires logout. Ultimate mode requires a reboot.
{% endhint %}

{% endstep %}

{% step %}

### 3. Fixing Hotkeys

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

### 4. Update Mirrors to Use the Fastest Server

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

{% endstep %}

{% step %}

### 5. Enable Bluetooth (If it isn't enabled by default)

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

### 6. Power Management

#### 6.1 TLP

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


#### 6.2 Auto-CPUFreq

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
After installation, open the cpu-auto-freq app and verify if it’s working properly.
{% endhint %}

{% endstep %}

{% step %}

### 7. Bonus: Pacman Animation

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

{% endstep %}

{% endstepper %}
