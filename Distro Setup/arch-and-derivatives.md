---
title: Arch Linux
icon: linux
---

{% stepper %}
{% step %}

# 1. AUR Helper

One of the most prominent and enticing features of Arch Linux is the **Arch User Repository (AUR)**. The AUR is a community-driven repository that provides package descriptions (PKGBUILDs). These allow you to compile software from source using `makepkg` and then install it with `pacman`.

An AUR helper is recommended for installing packages from the AUR, as it simplifies the process. You can install packages without an AUR helper, but using one makes it much easier. Distros like EndeavourOS and other Arch-based distros usually include an AUR helper by default, but on vanilla Arch Linux, you need to install it yourself.

You can choose between Yay or Paru, if you’re unsure, just go with Yay.

### **Yay (Yet Another Yogurt):**
```bash
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

### **Paru :**
```bash
sudo pacman -S --needed base-devel && git clone https://aur.archlinux.org/paru.git && cd paru && makepkg -si
```


{% hint style="warning" %}
The AUR is community-driven. Anyone can upload packages, so there’s always a small risk of malicious code. Always **read the PKGBUILD file** before installing to see exactly what commands will run on your system.
{% endhint %}

{% endstep %}

{% step %}

# 2. Pacman Configuration: 

## 2.1 Enable Multilib repository (Required for Drivers and Wine)

Open the pacman configuration file:
```bash
sudo nano /etc/pacman.conf
```

Uncomment these lines:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

## 2.2. Update Mirrors to Use the Fastest Server

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

<details><summary>Pacman Animation</summary> 

This replaces the progress bar in Arch with a Pacman.

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
</details>

{% endstep %}

{% step %}

# 3. Driver Installation

If you're using an **Nvidia GPU**, you’ll also need to install proprietary drivers manually. AMD users can skip this, since Mesa drivers are included in the kernel and work out of the box. However, if you want to install all the 32-bit libraries and Vulkan support, see the instructions below.

Unlike Windows, most drivers are already included in the Linux kernel and the rest of the drivers is usually included in the `linux-firmware` package. You usually don’t need to install them manually.

## 3.1 Nvidia Drivers:
The NVIDIA proprietary driver has been removed from the repositories and replaced by the newer `nvidia-open` driver. For GPUs newer than Pascal, you should install the `nvidia-open` driver.`nvidia-open` does not support the GTX 10series (Pascal) or older cards. 

For Turing and newer GPUs, the `nvidia-open` driver functions largely the same as the older proprietary driver.


If your GPU isn’t listed, please refer to the [Arch Wiki page for NVIDIA drivers](https://wiki.archlinux.org/title/NVIDIA) to determine which driver version is supported for your GPU.


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
You must install the headers package for your specific kernel before installing the DKMS drivers. The package name varies depending on the kernel. For example, for the Zen kernel it is called linux-zen-headers, and for the Hardened kernel it is called linux-hardened-headers.

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

### 3.2. AMD Drivers:
The drivers for AMD are usually installed out of the box since they’re part of the Linux kernel. If you want to make sure you have everything, including the 32-bit libraries and Vulkan, simply run the command below.

```bash
sudo pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon libva-mesa-driver libva-utils
```

### 3.3 Intel:
The drivers for Intel are usually installed out of the box since they’re part of the Linux kernel. If you want to make sure you have everything, including the 32-bit libraries and Vulkan, simply run the command below.

```bash
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver libva-intel-driver libva-utils
```

{% endstep %}
{% step %}

# 4. Asus Software Installation:

Install Asus tools:

```bash
yay -S asusctl rog-control-center supergfxctl
```

Enable GPU switching daemon:
```bash
sudo systemctl enable supergfxd.service --now
```

{% hint style="info" %}
Ignore the "Asus kernel isn’t loaded" message in `rog-control-center`.
{% endhint %}

### 4.1 Switching GPU Modes (via Terminal or GUI)

`Rog-control-center` Center is the GUI for asusctl. You can also use asusctl without the GUI if you prefer. The same applies to supergfxctl , it’s a terminal only tool by default, but you can install the GNOME extension or KDE applet to get a graphical interface.

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

### Fixing Hotkeys

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

# 5. Flatpak:
Flatpak is a Linux tool for installing and managing software. It runs applications in a sandboxed environment, keeping them partially separated from the main system. It is a widely used platform that allows software to work across various Linux distributions.

Most Arch-based distributions come with Flatpak, but if it isn’t installed, you can use the following command to install it and add the Flatpak repository.

```bash
sudo pacman -S flatpak && flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

{% endstep %}

{% step %}

# 6. Backups:
Timeshift is a powerful Linux backup tool that functions similarly to System Restore on Windows or Time Machine on macOS. It protects your system by creating incremental snapshots of your file system at regular intervals. These snapshots allow you to restore your system to a previous state, undoing any system changes or issues.

Installation:
```bash
sudo pacman -S  timeshift
```

How to Use Timeshift:

1. Select Snapshot Type: Choose between **RSYNC** and **BTRFS** based on your file system.

{% hint style="info" %} If your system is using the BTRFS file system, it is recommended to use the BTRFS snapshot option for better performance. If not, select RSYNC. {% endhint %}
  
2. Choose Snapshot Location: Select the disk or partition where snapshots will be saved.

3. Configure Snapshot Schedule: Enable periodic snapshots if desired and select a snapshot frequency (daily, weekly, or on boot).

4. Create a Snapshot: Click Create to manually create a snapshot at any time.

5. Restore a Snapshot: To undo system changes, select a previous snapshot and click Restore.

## Restoring a Broken System Using Timeshift:

1. Boot from a Linux ISO with Timeshift installed.

2. Select the same snapshot type (BTRFS or RSYNC) as used before.

3. Choose the location where your backup is stored.

4. Select the desired backup from the list shown.

5. Click Restore to revert your system to the previous working state.

{% hint style="warning" %} Timeshift does not back up personal user files such as documents, pictures, or downloads. It focuses exclusively on system files and settings. {% endhint %}

{% endstep %}

{% step %} 

# 7. Firewall
A firewall is a security system that monitors, filters, and controls incoming and outgoing network traffic according to predefined security rules. While it isn't mandatory to have a firewall for a workstation, it is highly recommended to set up some form of firewall. There are two firewall options depending on the netfilter installed:

This covers only the basic things about firewalls. If you want advanced configuration or documentation, please visit the respective page for the firewalls on the Arch Wiki.

To check which netfilter is installed, run the following command:
```bash
sudo pacman -Q | grep -E 'nftables|iptables'
```
## 7.1. Ufw:

If you have `iptables(legacy)` installed, then it is recommended to use Uncomplicated Firewall (ufw).

Install ufw and its GUI with:
```bash
sudo pacman -S ufw gufw
```
Then enable it with:
```bash
sudo ufw enable
```
The default configuration denies all incoming connections while allowing outgoing connections. The default should be enough for most users, but if you use Docker, SSH, or any torrent software, you will need to give it access through your firewall.

{% hint style="info" %}
The gufw package provides a GUI for ufw, which can be used to configure ufw. For your home network, set the profile as "home," and for a private network, set the profile as "public."
{% endhint %}

**To allow/deny access to a specific port, run the following command:**
```bash
sudo ufw allow <port>/<optional: protocol>
```
## 7.2. Firewalld:
Firewalld is recommended for systems that have `iptables-nft` installed, which includes most modern distros.

Install the firewalld package:
```bash
sudo pacman -S firewalld python-pyqt6
```
Then enable it with the following command:
```bash
sudo systemctl enable firewalld --now
```
Again, if you need to open a port or a service, you can either use the GUI or the terminal for it. The `firewalld` package contains a GUI by default, while the Python package adds applet support for firewalld.

To open/close a port or service, you can run the following command:
```bash
sudo firewall-cmd --zone=public --remove-port=<port>/<optional: protocol>
```

 **For Services:**
```bash
sudo firewall-cmd --zone=public --remove-service=<service>
```
By default, firewalld only saves these changes temporarily until a reboot. If you want to make the changes persistent, use the `--permanent` flag as such:
```bash
sudo firewall-cmd --permanent --zone=public --remove-service=<service>
```
Then reload firewalld to integrate changes into the current runtime:
```bash
sudo firewall-cmd --reload
```
{% endstep %}

{% endstepper %}
