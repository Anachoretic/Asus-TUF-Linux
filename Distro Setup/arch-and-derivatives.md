---
title: Arch Linux
icon: linux
---

{% stepper %}
{% step %}

# 1. Updates:
Begin by updating your system:
```bash
sudo pacman -Syu
```

{% hint style="info" %}
Some updates require manual intervention, so please check the [Arch News page](https://archlinux.org/news/) to stay up to date.
{% endhint %}

{% endstep %}
{% step %}

# 2. Pacman Configuration:

<details><summary>Multilib:</summary> 
By default on vanilla Arch, the multilib repository is disabled, and you will need to manually enable it in order to install Wine and other 32-bit software.

On Arch derivatives like EndeavourOS and CachyOS, the repository is enabled by default, so no additional steps are required. However, if your distro disables it by default, follow the steps below to enable it.

Edit the `/etc/pacman.conf` file and uncomment the following lines to enable it
```bash
#[multilib]
#Include = /etc/pacman.d/mirrorlist
```
</details>

<details><summary>Mirrors:</summary> 

You should also update the mirror list to use the fastest servers closest to you. This isn’t mandatory, but doing so can make updates and downloads via pacman faster.

First, back up your current mirror list so you can restore it in case something goes wrong:
```bash
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
```

To restore the mirror list, run the following command. You must have saved a backup in order to restore it.
```bash
sudo cp /etc/pacman.d/mirrorlist.bak /etc/pacman.d/mirrorlist
```

Now edit the pacman.conf file and increase the number of parallel downloads to a higher value, such as 10. You can set it even higher if you have a fast internet connection, but 10 is a good sweet spot for most people. You may increase or decrease it if needed.

```bash
sudo nano /etc/pacman.conf
```

Now edit the following line and set it to your desired value.

```bash
ParallelDownloads = 5
```

Now install `reflector` and run it with the following parameters:

```bash
sudo pacman -S reflector
```


```bash
sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

This will sort the fastest 10 servers and save them to your mirror list.
</details>

<details><summary>Pacman Progress Bar:</summary> 
This replaces the default progress bar in Arch with a Pacman-themed progress bar.

This will change the progreess bar from this:
```bash
[######################---------]

To this:
[-----------c o  o  o  ]
```

Edit the `pacman.conf` file, uncomment the `Color` line, and add `ILoveCandy` below it.

```bash
Color
ILoveCandy
```
</details>

{% endstep %}
{% step %}

# 3. Drivers:
If you're using an **NVIDIA GPU**, you’ll need to install the required drivers manually. **AMD** users can skip this, as the Mesa drivers are included in the kernel and work out of the box. However, if you want to install all the 32-bit libraries and Vulkan support, follow the instructions below. The same applies to **Intel** GPUs.

Unlike Windows, most drivers are already included in the Linux kernel and the rest of the drivers is usually included in the `linux-firmware` package. You usually don’t need to install them manually.

## Nvidia:
The NVIDIA proprietary driver has been removed from the repositories and replaced by the newer `nvidia-open` driver. For GPUs newer than Pascal, you should install the `nvidia-open` driver. `nvidia-open` does not support the GTX 10series (Pascal) or older cards. 

For Turing and newer GPUs, the `nvidia-open` driver functions largely the same as the older proprietary driver.


If your GPU isn’t listed (Pascal and older, or Quadro/Studio cards), please refer to the [Arch Wiki page for NVIDIA drivers](https://wiki.archlinux.org/title/NVIDIA)
to determine which driver version is supported for your GPU.


You can compare your gpu architecture in the chart below:

| **GPU Generation**          | **Architecture** |
|-----------------------------|------------------|
| RTX 50 Series Laptop GPU    | Blackwell        |
| RTX 40 Series Laptop GPU    | Ada Lovelace     |
| RTX 30 Series Laptop GPU    | Ampere           |
| RTX 20 Series Laptop GPU    | Turing           |
| GTX 16 Series               | Turing           |
| GTX 10 Series               | Pascal           |

{% hint style="warning" %}
Before installaing the driver make sure **Secure Boot is disabled**, or the Nvidia driver won’t load.
{% endhint %}

Assuming the system is up to date, begin by checking whether your NVIDIA GPU is detected and visible to the system:

```bash
lspci | grep -i nvidia
```

If it doesn’t show, try switching to Hybrid GPU mode using `supergfxctl` and run the command again.

<details>
<summary>Arch:</summary>

Begin by checking which kernel is installed by running:

```bash
uname -r
```

Now, depending on the kernel you are using, you may need to install a specific variant of the NVIDIA driver. For the default (`linux`) and LTS (`linux-lts`) kernels, you can choose between the `non-DKMS` and `DKMS` packages. Functionally, both provide the same NVIDIA driver and features.

The main difference is how kernel compatibility is handled. The `non-DKMS` package (for example `nvidia-open`) is built specifically for a particular kernel version and must be updated whenever that kernel is updated. In contrast, the `DKMS` variant builds the kernel module automatically for any installed kernel, including future updates, as long as the `kernel headers` are present.

For other kernels such as `linux-zen` or `linux-hardened`, the `DKMS` package is required, as prebuilt `non-DKMS` modules are not provided for those kernels.

### For Linux:
```bash
sudo pacman -S nvidia-open nvidia-utils lib32-nvidia-utils nvidia-settings libva-nvidia-driver vulkan-icd-loader lib32-vulkan-icd-loader
```

### For Linux-lts:
```bash
sudo pacman -S nvidia-open-lts nvidia-utils lib32-nvidia-utils nvidia-settings libva-nvidia-driver vulkan-icd-loader lib32-vulkan-icd-loader
```

### For other kernels:
You must install the headers package for your specific kernel before installing the DKMS drivers. The package name varies depending on the kernel. For example, for the Zen kernel it is called `linux-zen-headers`, and for the Hardened kernel it is called `linux-hardened-headers`.

```bash
sudo pacman -S dkms nvidia-open-dkms nvidia-utils lib32-nvidia-utils nvidia-settings libva-nvidia-driver vulkan-icd-loader lib32-vulkan-icd-loader
```

</details> 

 <details>
 <summary>EndeavourOS/CachyOS:</summary>

### CachyOS:
The drivers should already be installed by default, and no additional steps are required.

You can verify if they are installed and running with:
```bash
nvidia-smi
```

### EndeavourOS:
Install `nvidia-inst` using yay, then run it. It should automatically detect your GPU and install all required drivers, with no additional steps needed.

```bash
yay -S nvidia-inst
```

```bash
nvidia-inst
```
</details> 

{% hint style="info" %}
After installation, wait for the initramfs to be regenerated.
{% endhint %}

Finally, after installation, verify if the driver is installed and working by running `nvidia-smi`.

## AMD:
The drivers for AMD are usually installed out of the box since they’re part of the Linux kernel. If you want to make sure you have everything, including the 32-bit libraries and Vulkan, simply run the command below.

```bash
sudo pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon libva-mesa-driver libva-utils
```

## Intel:
The drivers for Intel are usually installed out of the box since they’re part of the Linux kernel. If you want to make sure you have everything, including the 32-bit libraries and Vulkan, simply run the command below.

```bash
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver libva-intel-driver libva-utils
```

{% endstep %}
{% step %}


# 4. AUR:

One of the most prominent and enticing features of Arch Linux is the **Arch User Repository (AUR)**. The AUR is a community-driven repository that provides package descriptions (PKGBUILDs). These allow you to compile software from source using `makepkg` and then install it with `pacman`.

An AUR helper is recommended for installing packages from the AUR, as it simplifies the process. You can install packages without an AUR helper, but using one makes it much easier. Distros like EndeavourOS and other Arch-based distros usually include an AUR helper by default, but on vanilla Arch Linux, you need to install it yourself.

{% hint style="info" %}
Arch Derivatives usually ship with an AUR helper by default, so make sure to check your distro’s wiki to confirm that it includes one.
{% endhint %}

Yay works well for most cases, but there are other helpers like Paru and Pikaur. If you’re not sure which to choose, just use Yay.

**Yay (Yet Another Yogurt):**
```bash
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

{% hint style="danger" %}
The AUR is community-driven. Anyone can upload packages, so there’s always a small risk of malicious code. Always **read the PKGBUILD file** before installing to see exactly what commands will run on your system.


While rare, there have been instances of [malware in the AUR](https://lists.archlinux.org/archives/list/aur-general@lists.archlinux.org/thread/7EZTJXLIAQLARQNTMEW2HBWZYE626IFJ/), so use it at your own risk.
{% endhint %}

{% endstep %}
{% step %}

# 5. Asus Software:
There are currently two ways to get asusctl and supergfxctl. You can either add the [g14 repository](https://arch.asus-linux.org/)
by following the instructions provided by the [asus-linux team](https://asus-linux.org/guides/arch-guide/), which allows you to install these tools along with other software like a custom kernel, or you can simply use the AUR. I highly recommend following the instructions from the `asus-linux` site, but the choice is yours.

**Since all the repository instructions are already covered by the asus-linux team, I’ll focus on installation via the AUR instead.**

{% hint style="info" %}
If you decide to use the AUR, you will need to compile everything from source. Compiling asusctl and supergfxctl won’t take long, but if you choose to use the custom G14 kernel from the AUR, it will take a significantly longer time to complete.
{% endhint %}

Installation:

```bash
yay -S asusctl rog-control-center supergfxctl
```

Enable GPU switching daemon:
```bash
sudo systemctl enable supergfxd.service --now
```

{% hint style="info" %}
After installation, you may see the following message in `rog-control-center` "Asus kernel isn’t loaded." You will get this warning if you don’t use the custom kernel. You can choose not to use the custom kernel, and the app will still function normally for all other features, but you won’t have access to power limits.
{% endhint %}

## GUI:
`rog-control-center` Center is the GUI for asusctl. You can also use asusctl without the GUI if you prefer. The same applies to supergfxctl , it’s a terminal only tool by default, but you can install the GNOME extension or KDE applet to get a graphical interface.

**GNOME:** Install the `supergfxctl-gex` extension.  

**KDE:**
```bash
yay -S plasma6-applets-supergfxctl
```
Log out and log back in for the applet to appear on the taskbar.

Switch to Hybrid mode:
```bash
supergfxctl --mode Hybrid
```

{% hint style="info" %}
Changing to/from Hybrid mode requires logout. Ultimate mode requires a reboot.
{% endhint %}

## Hotkeys:
{% hint style="info" %}
Some hotkeys are handled by the BIOS directly and can’t be remapped. Test by creating a shortcut and see if it registers.
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


#### Commands:
- Open Armoury Crate: `rog-control-center`
- Toggle Aura lighting: `asusctl aura -n`
- Change performance profile: `asusctl profile -n`

{% endstep %}
{% step %}
