# Arch Linux Post-Install Guide

{% stepper %} {% step %}

### 1. Post Install Configuration

If you're using an Nvidia dGPU, you'll need to install Nvidia's proprietary drivers manually.

AMD users can skip this , Mesa drivers are built into the kernel and just work out of the box.

{% hint style="info" %} Note: Unlike Windows, most drivers are included in the kernel, so you usually don't need to install them manually. {% endhint %}

{% endstep %}

{% step %}

### 2. Enable Multilib (Required for Nvidia Drivers and Wine)

Open a terminal and type:

```bash
sudo nano /etc/pacman.conf
```

Look for the lines:

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

Uncomment them if they are commented out.


### GPU Driver Installation and Asus Software Setup

#### 2.1 Nvidia Driver Installation

{% hint style="warning" %} Important: Make sure Secure Boot is turned off, or the Nvidia driver won’t load. {% endhint %}

Start by updating the system:

```bash
sudo pacman -Syu
```

Check if the Nvidia GPU is detected:

```bash
lspci | grep -i nvidia
```

If it doesn't show, try switching to Hybrid GPU mode using `supergfxctl` and perform this step again.

##### For EndeavourOS users:

Install `nvidia-inst` from AUR:

```bash
yay -S nvidia-inst
```

Then run 
```bash
nvidia-inst
```

##### Manual Installation (Other Distros):

Make sure multilib is enabled (see above).

Then install:

```bash
sudo pacman -S nvidia nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader
```

##### Optional: Install AMD or Intel Vulkan drivers

For AMD:

```bash
sudo pacman -S lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader
```

For Intel:

```bash
sudo pacman -S lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader
```

{% hint style="info" %} After installing, wait 5–8 minutes for the kernel module to build in the background. {% endhint %}

Enable Nvidia power management:

```bash
sudo systemctl enable nvidia-hibernate.service nvidia-suspend.service nvidia-resume.service nvidia-powerd.service
```



#### 2.2 Asus Software Installation

Install `yay` if it's not already installed:

```bash
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

Install necessary Asus tools:

```bash
yay -S asusctl rog-control-center supergfxctl
```

Enable the GPU switching daemon:

```bash
sudo systemctl enable supergfxd.service
sudo systemctl start supergfxd.service
```

{% hint style="info" %} Ignore the "Asus kernel isn't loaded" message in rog-control-center. It’s safe. {% endhint %}


#### 2.3 Switching GPU Modes with a GUI

**GNOME**: Use the `supergfxctl-gex` extension.\
**KDE**: Install the plasmoid:

```bash
yay -S plasma6-applets-supergfxctl
```

Reload Plasma:

```bash
plasmashell --replace &
```

Set GPU mode to Hybrid:

```bash
supergfxctl --mode Hybrid
```

{% hint style="info" %} Note: Changing to/from Hybrid mode requires logout. Ultimate mode needs a reboot. {% endhint %}

{% endstep %}

{% step %}

### 3. Fixing Hotkeys

{% hint style="info" %} Some hotkeys are handled by BIOS directly and can't be remapped. To test if a key can be remapped, try creating a shortcut and see if it registers. {% endhint %}

#### 3.1 GNOME

- Go to Settings > Keyboard > Keyboard Shortcuts
- Click “+” to add a new shortcut

#### 3.2 KDE

- Go to System Settings > Shortcuts > Custom Shortcuts
- Create a new Global Shortcut → Command/URL

#### Handy Commands

- **Open Armoury Crate**: `rog-control-center`
- **Toggle Aura lighting**: `asusctl aura -n`
- **Change performance profile**: `asusctl profile -n`

{% endstep %} 

{% step %}

### 4. Update Mirrors to Use the Fastest Server

Backup current mirrorlist:

```bash
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
```

Increase parallel downloads:

```bash
sudo nano /etc/pacman.conf
```

Find `ParallelDownloads` and change the value to `10`.

Install reflector:

```bash
sudo pacman -S reflector
```

Update mirrorlist:

```bash
sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

Restore old mirrors if needed:

```bash
sudo cp /etc/pacman.d/mirrorlist.bak /etc/pacman.d/mirrorlist
```

{% endstep %}

{% step %}


### 5. Enable Bluetooth

If Bluetooth isn't working, install required packages:

```bash
sudo pacman -S bluez bluez-utils
```

Enable and start the service:

```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

{% endstep %}

{% step %}

## 6. Power Management
### 6.1 TLP

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
### 6.2 Auto-CPUFreq

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

Edit the pacman config:

```bash
sudo nano /etc/pacman.conf
```

Uncomment:

```
#Color → Color
```

Add this below:

```
ILoveCandy
```

Update system:

```bash
sudo pacman -Syu
```
{% endstep %}

{% endstepper %}
