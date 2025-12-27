---
title: Arch
icon: linux
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
- [Endeavour OS](https://endeavouros.com)
- [Cachy OS](https://cachyos.org/)
- [Arch](https://archlinux.org/download/)

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

{% endtabs %}

{% step %}
{% endstep %}
 
# 3. Partitioning for Dual Boot:
If you are trying to dual boot Linux alongside Windows, you will need to leave some unallocated space for the installer to detect and use. A minimum of 60 GiB is recommended for the Linux partition. If you plan on playing multiple large games, you may want to allocate even more space, as games installed on the Windows (NTFS) partition generally won’t work on Linux.

## 3.1 Steps:
To create a partition, open Disk Management, then right-click on your partition or drive. If you want to share the same SSD between two operating systems, right-click on the C: partition and select Shrink Volume. In the field **Enter the amount of space to shrink in MB**, enter the size you want to allocate to Linux and click Shrink.

After shrinking, you will see a black unallocated space of the same size. Do not create a new volume, leave it unallocated. Once confirmed, you can exit Disk Management and continue to the next step.

![](/Images/Windows/Diskmgmt.png)

{% hint style="info" %}
If you are not confident with partitioning and are worried you might mess something up, then take a screenshot of the Disk Management screen so you can identify which partitions are new and can be safely removed.
{% endhint %}

{% step %}
{% endstep %}

# 4.  Installation Steps:

## 4.1 Booting from the Installation USB:

Assuming you have disabled Secure Boot, if you have not, hold the F2 key and press the Power button, keeping F2 held until you enter the BIOS screen. Inside the BIOS, go to the Security tab, turn off Secure Boot, then save the changes and exit. Once Secure Boot is disabled, plug in your USB drive, hold the Esc key, and press the Power button. When prompted to select a boot device, choose your USB drive and press Enter.


## 4.2 Installation:
 
<details>
<summary><strong>EndeavourOS/CachyOs Installation:</strong> </summary>


**CachyOS and EndeavourOS use the same Calamares installer. While CachyOS is included in the title, it isn’t covered specifically in the installation steps. There are no images showing the CachyOS installation process, since the steps are largely the same, with only a few appearing in a different order. Following the EndeavourOS steps will work for CachyOS as well.**


Once you’re in the live session, connect to the internet using Wi-Fi or Ethernet. Then launch the installer from the taskbar and choose the Online Installation method.

{% hint style="info" %} An internet connection is required for updating the system, and some options, like selecting a desktop environment or bootloader, are not available in the offline installer. {% endhint %}



![Installer](/Images/EndeavourOS/Installer.png)


![Language](/Images/EndeavourOS/Language.png)

Choose your language.


![Region and Timezone](/Images/EndeavourOS/Region.png)

Choose your region and your timezone.


![Keyboard Layout](/Images/EndeavourOS/Keyboard-Layout.png)   

Choose your keyboard layout. If you have the US layout, simply press Next.


![Desktop Environment](/Images/EndeavourOS/DE.png)

Now choose your desktop environment. I would recommend sticking to either GNOME or KDE. It will show a preview image of what the desktop environment looks like, but in short: KDE is more Windows-like and GNOME is more macOS-like. Once you've decided, simply double click on the option and choose Next.

![Additional Packages](/Images/EndeavourOS/Packages.png)

I’d recommend getting an LTS kernel that you can use as a backup, and if you have a printer, you can check the printing support box. Other than that, I’d leave the rest as is.

![Bootloader](/Images/EndeavourOS/Bootloader.png)

Select GRUB as the bootloader.

![Disk Setup](/Images/EndeavourOS/Partitions.png)

For a single-boot setup, select Erase Disk and enable Swap with Hibernate if you plan on using suspend to disk instead of swap on zram. For a dual-boot setup, select Replace a Partition and choose the unallocated space.

![An example of dual boot.](/Images/EndeavourOS/Partitions-DB.png)

This is an example of a dual-boot setup. The disk size is irrelevant because it is running in a virtual machine.

![User Setup](/Images/EndeavourOS/Username.png)

Now enter your username, hostname, and password, then click Next.

![Summary](/Images/EndeavourOS/Summary.png)

Review all settings. If everything looks correct, click **Install**. Then wait for the installation to complete, and reboot.

</details>

<details><summary>Arch:</summary>

For a complete manual installation, please refer to the [installation steps](https://wiki.archlinux.org/title/Installation_guide) found on the Arch Wiki. 

</details>

{% step %}
{% endstep %}

# 5. Uninstalling Linux

## 5.1 For Dual Boot Users

<details>
 <summary>EndeavourOS/CachyOS</summary>
Open Disk Management in Windows, delete the Linux partitions, and then extend the partition from which the space was taken.

When Linux is installed alongside Windows, its bootloader files are copied to the Windows EFI system partition. To fully remove Linux, you’ll need to delete those files. If you skip this step, you might get an error on boot from the Linux bootloader saying it can’t find the Linux system.

### Open PowerShell as administrator and run the following commands in order.

```bash
diskpart

list disk

select disk X  # Replace X with your disk number.

select partition 1  # The EFI partition on Windows is 1.

assign letter=Z
```

### Now open a new PowerShell window as administrator while keeping the old one open.

```bash
cd Z:

cd EFI

dir
```

Depending on the installed distro, `dir` will return different output. For example, it will show `endeavouros` for EndeavourOS and `cachyos` for CachyOS. Locate your distro’s directory and remove it.

```bash
rd /s /q <distroname> 
```

Replace `<distroname>` with your distro name as shown in the `dir` output.

Return to the previous window and run:

```bash
remove letter=Z
```

After that, you can close both windows and use Windows normally.

</details>

## 5.2 For Standalone Linux Installation:

If you want to return to Windows after using Linux, open DiskPart, select the Linux drive, and remove its filesystem by returning it to an uninitialized state using the `clean` command in DiskPart.

```bash

diskpart

list disk

select disk X  # Replace X with the correct drive.

clean

exit
```

{% hint style="info" %} If you are using an Intel system and the Windows installer cannot detect your drive, disable VMD (Volume Management Device) in BIOS. {% endhint %}

{% endstep %}
{% endsteppers %}
