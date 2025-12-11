---
icon: debian
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

{% step %}
{% endstep %}

# 4.  Installation Steps:

## 4.1 Booting from the Installation USB:

Assuming you have disabled Secure Boot, if you have not, hold the F2 key and press the Power button, keeping F2 held until you enter the BIOS screen. Inside the BIOS, go to the Security tab, turn off Secure Boot, then save the changes and exit. Once Secure Boot is disabled, plug in your USB drive, hold the Esc key, and press the Power button. When prompted to select a boot device, choose your USB drive and press Enter.


## 4.2 Installation:
 
 <details>
  <summary>Ubuntu </summary>

Once Ubuntu has booted, wait a few seconds and the installer should open automatically after a chime. If it doesn’t, click on the installer icon in the dock on the left side of the screen.

![](/Images/UBUNTU/Language.png)

The installer will open on the language tab. Simply choose your preferred language and click 'Next'.

![](/Images/UBUNTU/Accessibility.png)

On the second tab, you’ll be asked if you want to enable any accessibility features. Enable any you need, or simply click 'Next' if you don’t want to use any.

![](/Images/UBUNTU/Keyboard-Layout.png)

On this screen, select your keyboard layout.

![](/Images/UBUNTU/Internet.png)

The installer will ask if you want to connect to the internet. It is recommended to do so in order to download updates and additional software.

If you connect to the internet, the installer may prompt you to update. Simply update it and continue with the setup.

![](/Images/UBUNTU/Type.png)

For this step, choose 'Interactive Installation' and click 'Next'.

![](/Images/UBUNTU/Applications.png)

The default selection will install the basic set of apps, while the extended selection will include additional apps like LibreOffice and others. You can choose either option according to your preference.

![](/Images/UBUNTU/Software.png)

You will be prompted to install third-party software and multimedia codecs. Simply enable both options to have them downloaded.

![](/Images/UBUNTU/Disk-Setup.png)

If you are dual-booting Ubuntu with Windows and have already done the partitioning in Windows, you will see the option to 'Install alongside Windows.' Simply select that and continue.
For a standalone installation, select 'Erase disk and install Ubuntu,' making sure to choose the correct disk.

![](/Images/UBUNTU/User.png)
Simply enter your username, device name, set a password, and click 'Continue.

![](/Images/UBUNTU/Timezone.png)

Select a timezone and click 'Next.'

![](/Images/UBUNTU/Summary)

Finally, the installer will show a summary of the selected options and the changes it will make. Make sure everything is correct, then click 'Install' and wait for the installation to complete.

</details> 

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

### Now open a new PowerShell window as administrator while keeping the old one open.

```bash
cd Z:

cd EFI

dir

rd /s /q ubuntu  
```

**For both Mint and Ubuntu, the folder is named ubuntu, but it may be different for other distributions, so check the listed names.**

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
