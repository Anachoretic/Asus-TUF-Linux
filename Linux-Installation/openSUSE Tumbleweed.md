---
icon: suse
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

- [Agama Installer](https://agama-project.github.io/download)
- [YaST Installer](https://get.opensuse.org/tumbleweed/)


{% hint style="info" %} 
Agama will soon replace YaST, so it is recommended to use Agama instead of the YaST installer. Currently, only the installation and uninstallation steps for the Agama installer are covered.
{% endhint %}

{% tabs %}

{% tab title="Rufus" %}

1. Download [Rufus](https://rufus.ie/).  
2. Select “ISO Image” and choose your distro’s ISO.  
3. Insert your USB drive.  
4. Select **GPT** as the Partition Scheme.
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
dd if=/home/user/Downloads/agama-installer.x86_64-18.pre.0.0-openSUSE-Build10.2.iso of=/dev/sda bs=4M status=progress oflag=sync
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

![](/Images/Windows/Diskmgmt.png)

{% hint style="info" %}
If you are not confident with partitioning and are worried you might mess something up, then take a screenshot of the Disk Management screen so you can identify which partitions are new and can be safely removed.
{% endhint %}

{% step %}
{% endstep %}

# 4.  Installation Steps:

## 4.1 Booting from the Installation USB:

Assuming you have disabled Secure Boot, if you have not, hold the <kbd>F2</kbd> key and press the Power button, keeping <kbd>F2</kbd> held until you enter the BIOS screen. Inside the BIOS, go to the Security tab, turn off Secure Boot, then save the changes and exit. Once Secure Boot is disabled, plug in your USB drive, hold the <kbd>Esc</kbd> key, and press the Power button. When prompted to select a boot device, choose your USB drive and press Enter.


## 4.2 Installation:
 
<details>
<summary><strong> Agama Installer</strong> </summary>

![](/Images/openSUSE/Product-Selection.png)

First, select openSUSE Tumbleweed as the operating system you want to install. Then select it and wait for the system to detect everything. This may take a few seconds, so be patient.

![](/Images/openSUSE/Network.png)

By default, it will open in the Overview tab. Since the installer requires network access to download necessary files, you need to connect to a network. If you don’t have a network, you can skip this step. However, if you plan on using Wi-Fi, go to the Network tab. On the right side, it will list all available networks, simply select and connect to your network.

![Hostname](/Images/openSUSE/Hostname.png)

To give your device a name, go to the Hostname tab. This isn’t your username, but a name to identify your device on the network. You can also skip this step and set your hostname later using `hostnamectl`.

![Locales](/Images/openSUSE/Locales.png)

Use the Localization section to select your timezone, language, and keyboard layout.

![User account](/Images/openSUSE/User-account.png)

On the Authentication tab, add a user by entering a username and password. Also, set a password for the root account.

![Partitioning](/Images/openSUSE/Partitioning.png)


**By default, openSUSE will delete all existing partitions and create new ones. If you have Windows or another OS installed, make sure to select “Use Available Space”.** If you don’t have anything installed and want to use the entire disk, select “Delete Current Content”. Agama defaults to a 2 GB swap, but if you want to enable suspend-to-disk, choose Custom Partitioning and set the swap size to 1.5× your installed RAM. If you plan to use ZRAM exclusively, you may want to remove the swap partition.

![Software Selection](/Images/openSUSE/Software-Selection.png)

![](/Images/openSUSE/Software-Selection-2.png)

Agama will default to a minimal install, so you should select a DE before installing it. Simply click on "Change Selection" and select either KDE or GNOME. KDE looks similar to Windows, while GNOME looks like macOS. Either is a great choice. I've added an image of what each desktop looks like by default. For someone switching from Windows, KDE may look more familiar.


![KDE](/Images/KDE/KDE.png)

![GNOME](/Images/GNOME/Gnome.png)

</details>

<details>
<summary><strong> Troubleshooting: Black Screen</strong> </summary>

If you use the YaST installer and have an NVIDIA GPU, you may encounter a black screen. You can fix this by adding `nomodeset` to GRUB before booting.

Here’s how you do it:

![](/Images/openSUSE/Troubleshoot.png)


Select the installation option by pressing the down arrow key once, then press `E` to edit the GRUB entry.


![](/Images/openSUSE/Troubleshoot2.png)


Go to the line that begins with linux, then at the end where it says splash=silent press space once and type nomodeset. Press F10 to boot, and it should start normally.

Do note that the `nomodeset` parameter might carry over to the installed system, so during the installation summary click on Edit GRUB and remove it.
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

### Now open a new `cmd` window as administrator while keeping the old one open.

```bash
cd Z:

cd EFI

dir

rd /s /q opensuse
```

Return to the previous window and run:

```bash
remove letter=Z
```

After that, close both powershell and cmd windows and reboot.


## 5.2 For Standalone Linux Users

1. Boot from a Windows installation USB.  
2. Press `Shift + F10` to open Command Prompt.  
3. Run:

```bash
diskpart
select disk X  # Replace X with the correct drive
clean
exit
```

4. Continue with the Windows installation process.

{% hint style="info" %} If you are using an Intel system and the Windows installer cannot detect your drive, disable VMD (Volume Management Device) in BIOS. {% endhint %}


{% endstep %}
{% endsteppers %}
