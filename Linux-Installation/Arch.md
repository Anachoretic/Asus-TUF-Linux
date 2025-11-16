{% stepper %}

{% step %}

# 1. Distro Selection

If you’re interested in trying an Arch-based system but want something easier to get started with, I’d suggest EndeavourOS. It gives you the Arch experience with just a few helpful extras , like a graphical installer and a helpful community.

{% hint style="warning" %} If you’ve never used Linux before and aren’t willing to read documentation or troubleshoot issues, Arch or Arch-based distributions may not be the best starting point. {% endhint %} {% endstep %}

{% step %}

# 2. Prerequisites

{% hint style="info" %} Before starting the installation, make sure you have:

1. A USB drive with at least 8 GB of capacity.
2. A bootable media tool like Rufus, Ventoy, or Balena Etcher.
3. Secure Boot disabled in BIOS.
4. BitLocker disabled in Windows (for dual boot setups).
5. Fast Boot disabled (also for dual boot setups).
6. GPU mode set to "Ultimate" or "Standard" in Linux or Windows. {% endhint %}

{% hint style="warning" %} Secure Boot may prevent the system from booting. Be sure to disable it in BIOS. {% endhint %}

{% hint style="warning" %} If BitLocker is not turned off, it can lead to data loss or prevent access to your drive during installation. {% endhint %} {% endstep %}

{% step %}

# 3. Installation Process

## 3.1 Download the ISO

Download the latest EndeavourOS ISO from the official website:\
[https://endeavouros.com](https://endeavouros.com)

## 3.2 Create Bootable USB

{% tabs %} {% tab title="Rufus" %}

1. Download and open [Rufus](https://rufus.ie).
2. Select "ISO Image" and choose the EndeavourOS ISO.
3. Insert your USB drive and select it in Rufus.
4. Set the Partition Scheme to GPT.
5. Click **Start** and wait for the process to complete.
6. Safely eject the USB drive after completion. {% endtab %}

{% tab title="Ventoy" %}

1. Download [Ventoy](https://github.com/ventoy/ventoy/releases) and extract it.
2. Run `Ventoy2Disk.exe`.
3. Open **Options** and select **Partition Style → GPT**.
4. (Optional) Enable Secure Boot support — you'll need to import MOK keys on first boot.
5. Select your USB drive and click **Install**.
6. After setup, copy the ISO file to the new "Ventoy" USB partition. {% endtab %} {% endtabs %}

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

{% hint style="info" %} If the installer doesn't appear immediately, wait 10–20 seconds or search for “Install...” in the menu. {% endhint %}

## 3.5 Begin Installation

<details>
<summary><strong>EndeavourOS Installation:</strong> </summary>

Once you're in the live session:
1. Connect to the internet using Wi-Fi or Ethernet.
2. Launch the installer from the taskbar.
3. Choose the **Online Installation** method.
### Guided Installer Walkthrough (For the Calamares installer found in EndeavourOS and CachyOS.)

![Installer](https://github.com/user-attachments/assets/d58d6f62-b0ba-4c96-86e1-5396fef4ce18)


![Language](https://github.com/user-attachments/assets/662cab30-5614-44cb-b302-47b000b22041)
Choose your language.


![Region and Timezone](https://github.com/user-attachments/assets/955937b3-34ac-4a4d-ac2d-49f7cca28f34)
Choose your region and your timezone.


![Keyboard Layout](https://github.com/user-attachments/assets/8a5de1ce-ce43-4bfc-b15c-6ce4d40ac181)   
Choose your keyboard layout. If you have the US layout, simply press Next.


![Desktop Environment](https://github.com/user-attachments/assets/4291fd8f-b9fa-465b-9838-fc8e22fb6021)
Now choose your desktop environment. I would recommend sticking to either GNOME or KDE. It will show a preview image of what the desktop environment looks like, but in short: KDE is more Windows-like and GNOME is more macOS-like. Once you've decided, simply double click on the option and choose Next.


![Additional Packages](https://github.com/user-attachments/assets/6dd59b59-fa23-40a3-957e-feddd24a755d)
I’d recommend getting an LTS kernel that you can use as a backup, and if you have a printer, you can check the printing support box. Other than that, I’d leave the rest as is.


![Bootloader](https://github.com/user-attachments/assets/a77b2c68-a3f8-4b09-9abf-74573d03f4f4)
Select GRUB as the bootloader.


![Disk Setup](https://github.com/user-attachments/assets/10b78333-e5f4-4038-9b06-7cce694a5d11)
For a single-boot setup, select Erase Disk and enable Swap with Hibernate, for a dual-boot setup, select Replace a Partition and choose the unallocated space.


![An example of dual boot.](https://github.com/user-attachments/assets/5f2ff832-49ce-4b5f-806d-756e84e3b3f9)
An example of dual boot


![User Setup](https://github.com/user-attachments/assets/e4b1b96a-b405-4d9e-9a0f-e3d19c9cff18)
Now enter your username, hostname, and password, then click Next.


![Summary](https://github.com/user-attachments/assets/344647c1-f90f-4cd0-9f4a-a24ecdac90d0)
Review all settings. If everything looks correct, click **Install**. Then wait for the installation to complete, and reboot.

</details>

### Arch Installation:
For a complete manual installation, please refer to the [installation steps](https://wiki.archlinux.org/title/Installation_guide) found on the Arch Wiki. 


## 3.6 Complete Setup

1. Exit the Live Environment.
2. Remove the USB drive.
3. Your system should now boot into Linux or display a bootloader menu if dual booting. {% endstep %}

{% step %}

# 4. Uninstalling Linux

## 4.1 For Dual Boot Systems

1. Open Disk Management and delete the Linux partitions.
2. Open **Command Prompt as Administrator**, then run:

```bash
diskpart
select disk X         # Replace X with your actual disk number
list partition
select partition 1    # EFI partition is usually number 1.
assign letter=Z
```

3. In a new Command Prompt window:

```bash
cd /d Z:\EFI
dir
rd /s /q endeavouros  # Replace with your distro`s name
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

{% hint style="info" %} If the Windows installer doesn’t detect your disk on Intel systems, disable **VMD (Volume Management Device)** in BIOS. {% endhint %} {% endstep %}

{% endstepper %}
