---
title: Fedora
icon: fedora
---

# Fedora Post-Installation Guide
**You don't need to follow everything here, just complete the steps relevant to your needs.**

**If you are using a Fedora-based distribution, you may not need to complete certain steps. You can check the distro table below to see which steps apply to your particular distribution.**

<details><summary>Distro Specific Steps:</summary>

|    Steps      |    Fedora     |    Bazzite    |    Nobara     |
|---------------|---------------|---------------|---------------|
| Updates       |    ✓        |    ✓*       |   ✓*        |
| Rpm Fusion    |    ✓        |     x         |    x          |
| Gpu Drivers   |    ✓        |     x         |    x          |
| Asus Software |    ✓        |    ✓        |   ✓         |
| Multimedia    |    Opt        |     x         |    Opt        |
| Game Launcher |    ✓        |     x         |   x			 |
|  Fonts        |    Opt        |     x         |    Opt        |
|  Secure Boot  |    Opt        |     x         |   Unsupported |


**Update Bazzite and Nobara using their own system updater app.**

To install asus software on bazzite follow this instead:
```bash

sudo wget https://copr.fedorainfracloud.org/coprs/lukenukem/asus-linux/repo/fedora-38/lukenukem-asus-linux-fedora-38.repo -O /etc/yum.repos.d/_copr_lukenukem_asus-linux.repo

rpm-ostree install asusctl supergfxctl asusctl-rog-gui
systemctl reboot

sudo systemctl enable --now supergfxd.service
```
</details>


{% hint style="info" %}
If you've never used Linux before, there are a few behaviors that might confuse you while following this guide. When entering your password in the terminal, it will be completely hidden for security purposes, so it may look like your keyboard is not working. Additionally, while downloading packages or adding repositories, you may be prompted to accept a GPG key. Unless otherwise stated, review the prompt and accept the key.
{% endhint %}


{% stepper %}
{% step %}


## 1.Updates:
Start by updating all installed packages. You can do this through the App Store (Discover/GNOME Software) or via the terminal. **A reboot is required to complete the updates.**

```bash
 sudo dnf upgrade --offline && sudo dnf offline reboot -y
```
 
### DNF Configuration:

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

Fedora doesn't provide certain software due to legal restrictions, patents, or because it is proprietary. This includes the NVIDIA driver, media codecs, Steam, and similar software.
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

While it's possible to download and install NVIDIA drivers directly from NVIDIA's website, you should avoid doing so unless you have a specific reason. Drivers installed manually cannot be updated through `dnf` and are likely to break with future system updates. For this reason, you should install NVIDIA drivers only through `dnf`.

{% hint style="warning" %}Make sure Secure Boot is turned off or the Nvidia driver won't load. {% endhint %}

If you have a newer GPU (4000 series and above), it is recommended to use only the open kernel module, as the proprietary drivers won't work with these cards. To make the open driver the default, simply run the following command and continue with the setup as usual.

```bash
sudo sh -c 'echo "%_with_kmod_nvidia_open 1" > /etc/rpm/macros.nvidia-kmod'
```

Install Nvidia packages:

```bash
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
```

After installing the driver, wait about 5 minutes for the kernel module to be built. Once that's done, **reboot the system.** You can then test whether the driver is working by running `nvidia-smi` in the terminal. If it produces an output, the driver is installed and functioning correctly. If you see an error such as `NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver`, the driver isn't working properly, and you may need to rebuild it.

<details><summary>Troubleshooting:</summary>

### Rebuilding Kernel Modules  
If you end up with a black screen after installing the drivers, or if `nvidia-smi` fails with the error `NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver`, it usually means the NVIDIA kernel module didn't build properly. In such cases, you might be stuck at a black screen on boot, or the system may boot normally but the driver won't work.
If you're stuck at a black screen, press <kbd>Ctrl + Alt + F3</kbd> to switch to a TTY, log in, and try rebuilding the kernel module. If that still doesn't fix the issue, reinstall the drivers and try again.

If you can still access your desktop, you can run the required commands directly without switching to a TTY.
```bash
sudo akmods --force
```
After running this command, wait about 5 minutes for it to finish, then run `sudo dracut -f --regenerate-all` and reboot your device.

### Uninstallation:
If rebuilding the kernel module didn't work, you might need to uninstall the current drivers and reinstall them.
```bash
sudo dnf remove xorg-x11-drv-nvidia\* && sudo dracut -f --regenerate-all
```
{% hint style="info" %} 
If you get the error message "Nvidia kernel module missing, falling back to nouveau", it usually means one of three things: the NVIDIA kernel module was not built or installed properly, the dedicated GPU (dGPU) is disabled, or Secure Boot is enabled and is preventing the NVIDIA kernel module from loading. {% endhint %}

</details>

### 3.2 AMD/Intel:
The drivers are already installed as part of the kernel, no further steps are required.

{% endstep %}

{% step %}

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

**After installing everything reboot the system.**

{% hint style="info" %} The asus-armoury driver was added in kernel version 6.19. If you are using a kernel older than that, you will get a warning indicating that the asus-armoury driver is not present. {% endhint %}

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

Some hotkeys are BIOS-level and can't be remapped.

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

## 6. Games:
If all your games are on Steam, you can just install Steam and play them. However, if your games are on other launchers like Epic or GOG, you'll need to install Lutris, Bottles, or Heroic Game Launcher to play them, as they don't have native Linux clients.

```bash
sudo dnf in steam lutris
```

[You can learn about game compatibility and the steps to add a GOG game installer here.](https://asus-tuf-linux.gitbook.io/introduction/optimizations-and-daily-use/gaming-on-linux)

{% endstep %}

{% step %}

## 7. Fonts:
If you need the Microsoft fonts, you can install them using the following command. Otherwise, you can skip this step.

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

{% step %}

## 8. Secure Boot:
Secure Boot is a security feature in UEFI (BIOS) that prevents unsigned software from loading during the boot process, potentially stopping malicious programs from running when the system first starts. Secure Boot isn't mandatory on Linux and can be left disabled in most cases. In fact, some distributions require Secure Boot to be disabled because they won't boot normally with it enabled. This is because their bootloaders or kernels are not signed, or the required keys are not present in the firmware. If you're only running Linux, you can keep Secure Boot disabled, or enable it if you want the "extra security".

If you're dual booting Windows, disabling Secure Boot and switching it on/off every time you switch OS can be annoying. In that case, it's usually better to leave Secure Boot enabled so both operating systems boot normally.

<details><summary>Intel/AMD</summary>
If you don't have an NVIDIA GPU, you can simply enable Secure Boot in the UEFI(Bios) and Fedora should boot normally.
</details>

<details><summary>Nvidia</summary>
**If you have NVIDIA drivers installed (or any other third-party kernel modules), you'll need to follow the steps below to enable Secure Boot. [These steps are based on the RPM Fusion Secure Boot guide.](https://rpmfusion.org/Howto/Secure%20Boot)** 

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

{% endstep %}
{% endstepper %}
