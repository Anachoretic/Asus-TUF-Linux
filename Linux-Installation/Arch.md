{% stepper %}

{% step %}
# 1. Distro Selection

If you’re interested in trying an Arch-based system but want something easier to get started with, I’d suggest EndeavourOS. It gives you the Arch experience with just a few helpful extras , like a graphical installer and a helpful community.

{% hint style="warning" %}
If you’ve never used Linux before and aren’t willing to read documentation or troubleshoot issues, Arch or Arch-based distributions may not be the best starting point.
{% endhint %}
{% endstep %}

{% step %}
# 2. Prerequisites

{% hint style="info" %}
Before starting the installation, make sure you have:

1. A USB drive with at least 8 GB of capacity.  
2. A bootable media tool like Rufus, Ventoy, or Balena Etcher.  
3. Secure Boot disabled in BIOS.  
4. BitLocker disabled in Windows (for dual boot setups).  
5. Fast Boot disabled (also for dual boot setups).  
6. GPU mode set to "Ultimate" or "Standard" in Linux or Windows.
{% endhint %}

{% hint style="warning" %}
Secure Boot may prevent the system from booting. Be sure to disable it in BIOS.
{% endhint %}

{% hint style="warning" %}
If BitLocker is not turned off, it can lead to data loss or prevent access to your drive during installation.
{% endhint %}
{% endstep %}

{% step %}
# 3. Installation Process

## 3.1 Download the ISO

Download the latest EndeavourOS ISO from the official website:  
[https://endeavouros.com](https://endeavouros.com)

## 3.2 Create Bootable USB

{% tabs %}
{% tab title="Rufus" %}
1. Download and open [Rufus](https://rufus.ie).  
2. Select "ISO Image" and choose the EndeavourOS ISO.  
3. Insert your USB drive and select it in Rufus.  
4. Set the Partition Scheme to GPT.  
5. Click **Start** and wait for the process to complete.  
6. Safely eject the USB drive after completion.
{% endtab %}

{% tab title="Ventoy" %}
1. Download [Ventoy](https://github.com/ventoy/ventoy/releases) and extract it.  
2. Run `Ventoy2Disk.exe`.  
3. Open **Options** and select **Partition Style → GPT**.  
4. (Optional) Enable Secure Boot support — you'll need to import MOK keys on first boot.  
5. Select your USB drive and click **Install**.  
6. After setup, copy the ISO file to the new "Ventoy" USB partition.
{% endtab %}
{% endtabs %}

## 3.3 Partitioning

1. Open Disk Management in Windows.  
2. Locate your C: drive, right-click, and select **Shrink Volume**.  
3. Shrink the volume by at least 50 GB (recommended).  
4. Leave the space unallocated; do not create a new volume.

## 3.4 BIOS Setup

1. Reboot your system and press `F2`.  
2. Disable **Secure Boot**.  
3. Set your USB drive as the first boot device.  
4. Save changes and exit BIOS.

{% hint style="info" %}
If the installer doesn't appear immediately, wait 10–20 seconds or search for “Install...” in the menu.
{% endhint %}

## 3.5 Begin Installation

Once you're in the live session:

1. Connect to the internet using Wi-Fi or Ethernet.  
2. Launch the installer from the taskbar.  
3. Choose the **Online Installation** method.

### Guided Installer Walkthrough

1. **Language, Location, and Keyboard**  
   Choose your preferred language, region, and keyboard layout.

2. **Desktop Environment**  
   Select a desktop environment:  
   - GNOME (macOS-like)  
   - KDE Plasma (Windows-like)  
   *(The live session uses KDE by default.)*

3. **Packages**  
   Leave the default selection unless you have specific requirements.

4. **Bootloader**  
   Select **GRUB**.

5. **Disk Setup**  
   - For single-boot: Select **Erase Disk**, then choose **Swap with Hibernate**.  
   - For dual boot: Select **Replace a Partition** and choose the unallocated space.

6. **User Setup**  
   Enter your full name, username, and password.

7. **Summary**  
   Review all settings. If everything looks correct, click **Install**.

8. Wait for the installation to complete, then reboot.

## 3.6 Complete Setup

1. Exit the Live Environment.  
2. Remove the USB drive.  
3. Your system should now boot into Linux or display a bootloader menu if dual booting.
{% endstep %}

{% step %}
# 4. Uninstalling Linux

## 4.1 For Dual Boot Systems

1. Open Disk Management and delete the Linux partitions.  
2. Open **Command Prompt as Administrator**, then run:

```bash
diskpart
select disk X         # Replace X with your actual disk number
list partition
select partition 1    # EFI partition is usually number 1
assign letter=Z
```

3. In a new Command Prompt window:

```bash
cd /d Z:\EFI
dir
rd /s /q endeavouros  # Replace with your distro’s folder name
```

4. Return to the first terminal and run:

```bash
remove letter=Z
```

5. Reboot your system. You should now boot directly into Windows.

## 4.2 For Standalone Linux Installations

1. Boot using a Windows installation USB.  
2. Press `Shift + F10` to open Command Prompt.  
3. Run the following commands:

```bash
diskpart
select disk X         # Replace X with the correct disk number
clean
exit
```

4. Proceed with installing Windows normally.

{% hint style="info" %}
If the Windows installer doesn’t detect your disk on Intel systems, disable **VMD (Volume Management Device)** in BIOS.
{% endhint %}
{% endstep %}

{% endstepper %}
