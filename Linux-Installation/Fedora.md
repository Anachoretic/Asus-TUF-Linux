---
title: Fedora
icon: fedora
---


{% stepper %}
{% step %}

# 1. Prerequisites:
Before installation, you’ll need the following:
- [x] A USB drive with at least 8 GB of storage.
- [x] **Secure Boot** should be disabled.
- [x] BitLocker disabled to avoid potential data loss during installation.
- [x] dGPU mode should be set to Standard (Hybrid) or Ultimate in Windows.

{% step %}
{% endstep %}

# 2. Installation Media Creation:
After downloading the ISO of your preferred distro, you will need to flash it to a USB drive to boot from it and install the OS. You can use tools such as Balena Etcher, Rufus, or other similar software to burn the ISO.

If you haven’t downloaded the ISOs yet, here’s where you can get them.
- [Fedora GNOME](https://www.fedoraproject.org/workstation/download)
- [Fedora KDE](https://www.fedoraproject.org/kde/download)

{% tabs %}

{% tab title="Rufus" %}

1. Download [Rufus](https://rufus.ie/).  
2. Select “ISO Image” and choose your distro’s ISO.  
3. Insert your USB drive.  
4. Select the correct **Partition Scheme** (GPT for modern systems).  
5. Click **Start** and wait for it to finish.  
6. Safely eject the USB.

{% endtab %}

{% tab title="Ventoy" %} 

Ventoy allows multiple ISO files on one USB. It’s perfect for testing or switching between OSes.

1. [Download Ventoy](https://github.com/ventoy/ventoy/releases).  
2. Extract and run `Ventoy2Disk.exe`.  
3. Go to **Options > Partition Style > GPT**   
4. Select your USB drive and click **Install**.  
5. After installation, copy the ISO file directly to the **Ventoy** partition.

{% endtab %}

{% tab title="dd" %} 
The dd command is a simple utility that comes with GNU and is available on every Linux distro. It lets you copy data block by block and can be used to create a bootable live ISO on Linux without needing to install any additional tools.

1. Identify your USB drive using `lsblk`.
2. Open the terminal and run the following command:
```bash 
dd if=/path/to/iso of=/dev/sdX bs=4M status=progress oflag=sync

# Example:
dd if=~/Downloads/Fedora-Workstation-Live-43-1.6.x86_64.iso of=/dev/sda bs=4M status=progress oflag=sync
```


{% endtab %}
{% endtabs %}

{% step %}
{% endstep %}
 
# 3. Partitioning for Dual Boot:
If you are trying to dual boot Linux alongside Windows, you will need to leave some unallocated space for the installer to detect and use. A minimum of 60 GiB is recommended for the Linux partition. If you plan on playing multiple large games, you may want to allocate even more space, as games installed on the Windows (NTFS) partition generally won’t work on Linux.

## 3.1 Steps:
To create a partition, open Disk Management, then right-click on your partition or drive. If you want to share the same SSD between two operating systems, right-click on the C: partition and select Shrink Volume. In the field **Enter the amount of space to shrink in MB**, enter the size you want to allocate to Linux and click Shrink.

After shrinking, you will see a black unallocated space of the same size. Do not create a new volume, leave it unallocated. Once confirmed, you can exit Disk Management and continue to the next step.

{% step %}
{% endstep %}

# 4.  Installation Steps:

## 4.1 Booting from the Installation USB:

Assuming you have disabled Secure Boot, if you have not, hold the F2 key and press the Power button, keeping F2 held until you enter the BIOS screen. Inside the BIOS, go to the Security tab, turn off Secure Boot, then save the changes and exit. Once Secure Boot is disabled, plug in your USB drive, hold the Esc key, and press the Power button. When prompted to select a boot device, choose your USB drive and press Enter.


## 4.2 Installation:
 
![](/Images/Fedora/Installer.png)

After booting into Fedora, wait a few seconds. The installer should launch automatically. If it doesn’t, you can start it manually from the dock in GNOME, the application menu, or by clicking the desktop icon in KDE.

Once it launches, click <kbd>Install Fedora Linux</kbd>.

![](/Images/Fedora/Language.png)


In the first step, choose your language and keyboard layout.

![](/Images/Fedora/Disks.png)

First, make sure the installer has chosen the correct disk. If not, click <kbd> Change Destination </kbd> and select the correct disk. If you have a single drive, you don’t need to worry about this.

Next, if you are planning to dual boot with Windows and have completed the previous partitioning steps, you should see an option called <kbd> Share disk with other operating system </kbd>. Select it and continue to the next step.


![](/Images/Fedora/Encryption.png)

Encryption is optional for a standalone installation, but it is not recommended for a dual boot setup.

![](/Images/Fedora/User.png)

In the `KDE` version of Fedora Workstation, you will be required to add a user and set a password during installation. On `GNOME`, however, this step is available after the installation is complete, when you first boot into the OS.

Simply enter your name, username, and the password you want to use, then continue to the next step. Enabling the root account is not recommended, leave it disabled unless you specifically need it.

![](/Images/Fedora/Summary.png)
Finally, verify that the choices you made are correct and click <kbd>Install</kbd>.

After that, wait for the installation to complete, then shut down the laptop, remove the USB drive, and boot into Fedora.

{% step %}
{% endstep %}

# 5. Uninstalling Linux

## 5.1 For Dual Boot Users

Open Disk Management in Windows, delete the Linux partitions, and then extend the partition from which the space was taken.

When Linux is installed alongside Windows, its bootloader files are copied to the Windows EFI system partition. To fully remove Linux, you’ll need to delete those files. If you skip this step, you might get an error on boot from the Linux bootloader saying it can’t find the Linux system.

### Open PowerShell as administrator and run the following commands in order.

```bash
diskpart

select disk X  # Replace X with your disk number

list partition

select partition 1  # The EFI partition on Windows is usually 1

assign letter=Z
```

### Now open a new `CMD` window as administrator while keeping the old one open.

```bash
Z:
```

```bash
dir
```

You should see an `EFI` folder inside `Z:`. If you see anything like `System` or `mach_kernel`, simply remove it with the following commands:

```
rd /s /q System

rd /s /q mach_kernel
```

Now, cd into the `EFI` folder and delete the Fedora entry present inside it.

```
cd EFI

rd /s /q fedora
```

Do not remove anything else, as it may break your Windows bootloader installation.

Finally, verify that the Fedora folder has been removed with:

```bash
dir
```

Return to the previous window and run:

```bash
remove letter=Z
```

After that, you can close both windows and use Windows normally.


## 5.2 For Standalone Linux Installation:

If you want to return to Windows after using Linux, open DiskPart, select the Linux drive, and remove its filesystem by returning it to an uninitialized state using the `clean` command in DiskPart.

```bash

diskpart

select disk X  # Replace X with the correct drive

clean

exit
```

{% hint style="info" %} If you are using an Intel system and the Windows installer cannot detect your drive, disable VMD (Volume Management Device) in BIOS. {% endhint %}


{% endstep %}
{% endsteppers %}
