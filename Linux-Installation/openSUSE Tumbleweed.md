{% stepper %}

{% step %}

# openSUSE Tumbleweed:
Tumbleweed is a rolling release distro, kind of like Arch. You get updates faster than Debian or Fedora, but slower than Arch, since updates are more thoroughly tested.

Currently, there are two installers available for Tumbleweed: the YaST installer and the updated Agama installer. Agama isn’t officially out yet, but you can still use it, it’s more updated and faster than YaST. You can use whichever one you want until YaST gets fully replaced by Agama.

If you have an NVIDIA GPU, you should use the Agama installer. The YaST installer might get stuck on a black screen because of driver conflicts, but you can fix it by adding `nomodeset` to GRUB.

When installing Tumbleweed, you can choose your desktop environment. Pick either GNOME (Mac-like) or KDE (Windows-like). Both have excellent customization support. If you’re transitioning from Windows, I’d recommend KDE since it mostly feels like Windows.

{% endstep %}

{% step %}

## Prerequisites:

## Required Before You Begin

• USB drive with at least 8 GB of storage.  
• Bootable media tool: **Rufus**, **Ventoy**, or **Balena Etcher**.  
• **Secure Boot** must be disabled in BIOS.  
• **BitLocker** must be disabled in Windows.  
• **Fast Boot** should be disabled (for dual-boot users).  
• Set **GPU mode to Ultimate or Standard** in Windows.

{% hint style="warning" %} Secure Boot can prevent some distros from booting. Disable it in BIOS. {% endhint %}

{% hint style="warning" %} If BitLocker is not disabled, it can cause data loss or access issues during installation. {% endhint %}

{% endstep %}

{% step %}

## Bootable USB Creation and Partitioning

### Downloading the ISO:

Download the installer:


- [Agama Installer](https://agama-project.github.io/download)

- [YaST Installer](https://get.opensuse.org/tumbleweed/)



### Create a bootable USB:

{% tabs %}

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
   (Secure Boot support is optional; MOK keys are required on first boot if enabled).  
4. Select your USB drive and click **Install**.  
5. After setup, copy ISO files directly to the **Ventoy** partition.

{% endtab %}

{% endtabs %}


## Partitioning (Dual Boot Only)

1. Open **Disk Management** in Windows.  
2. Right-click on the **C: drive** and select **Shrink Volume**.  
3. Enter the desired size for Linux (at least 50 GB recommended).  
4. Do **not** format the new space; leave it unallocated.

{% endstep %}

{% step %}

## BIOS Setup

1. Restart your PC and press `F2` (or your BIOS key).  
2. In BIOS settings:  
   • Disable **Secure Boot**.  
   • Set the USB drive as the first boot device.  
3. Save changes and exit BIOS.  
4. Boot from the USB.

<details>
<summary><strong> Troubleshooting: Black Screen</strong> </summary>
If you use the YaST installer and have an NVIDIA GPU, you may encounter a black screen as previously mentioned. You can fix this by adding `nomodeset` to GRUB before booting. Here’s how you do it.

<img src="Images/openSUSE/Troubleshoot.png">

Select the installation option by pressing the down arrow key once, then press `E` to edit the GRUB entry.

![](https://github.com/user-attachments/assets/ba43eb8a-0bb0-4866-a617-2d02d50fe4de)

Go to the line that begins with linux, then at the end where it says splash=silent press space once and type nomodeset. Press F10 to boot, and it should start normally.

Do note that the `nomodeset` parameter might carry over to the installed system, so during the installation summary click on Edit GRUB and remove it.


</details>
{% endstep %}

{% step %}

## Installation

The installation and uninstallation steps are currently specifically made for the Agama installer.

<details>
<summary><strong>Agama Setup:</strong> </summary>

![Product Selection](https://github.com/user-attachments/assets/2670af41-e155-452e-95b8-6d330c61d8d4)

First, select openSUSE Tumbleweed, then click Select and wait for it to detect everything. This may take a few seconds, so be patient.

![Network Connection](https://github.com/user-attachments/assets/288c8c4f-20d7-43ed-bb2d-cef57dbaac47)


It will place you in the Overview section. Then, click on the Network tab to connect to a network, as the installer will need to download packages for the setup.

![Hostname](https://github.com/user-attachments/assets/884a2fe2-568c-43ad-9dbd-4940f7f9dfd1)

Now, give your device a hostname, which is the name for the laptop itself, not the username.

![Locales](https://github.com/user-attachments/assets/1117c27a-8d8b-4a01-bb2a-d6dd5551d64b)

Now head over to the localization tab and pick your language,keybord layout and your timezone.

![User account](https://github.com/user-attachments/assets/30be9569-db43-4a6b-a411-7a1a0a1f4912)

Now, add a username and password, and make sure to set a password for the root account by clicking Edit under First User to add a user and under Root User to set the root password.


![Partitioning](https://github.com/user-attachments/assets/34ec97c8-7d27-43f5-965e-c6c198a33703)


**By default, openSUSE will delete all existing partitions and create new ones. If you have Windows or another OS installed, make sure to select “Use Available Space”.** If you don’t have anything installed and want to use the entire disk, select “Delete Current Content”. Agama defaults to a 2 GB swap, but if you want to enable suspend-to-disk, choose Custom Partitioning and set the swap size to 1.5× your installed RAM. 
A typical custom partition scheme with seperate home partition should be as follows:

-1 GiB for /boot/efi 

-60–80 GiB for / (root), 

-the remaining space minus swap for /home

-the rest as swap


![Software Selection](https://github.com/user-attachments/assets/6ace82da-7e1f-47ef-9ff7-65fb0dcdc039)

![osT-8.png](https://github.com/user-attachments/assets/a6c1d9dd-6e7e-44f1-9dac-1c3486b2631f)

Agama will default to a minimal install, so you should select a DE before installing it. Simply click on "Change Selection" and select either KDE or GNOME. KDE looks similar to Windows, while GNOME looks like macOS. Either is a great choice. I've added an image of what each desktop looks like by default. For someone switching from Windows, KDE may look more familiar.


![KDE](https://github.com/user-attachments/assets/7883750f-eaa0-469d-8191-5811fc2477a2)

![GNOME](https://github.com/user-attachments/assets/5d652677-53e1-4fca-8f63-5eb71a75a50d)
</details>

After installation, exit the Live Environment, remove the USB drive, and reboot your system; you should now see the GRUB menu, select openSUSE Tumbleweed and boot into it.

{% endstep %}

{% step %}

## Uninstalling Linux:

### For Dual Boot Users:

1. Open **Disk Management** in Windows and delete the Linux partitions.  
2. Launch **Command Prompt as Administrator**, then run:

```bash
diskpart
list disk
select disk X
select partiton 1
```

Disk X is where Windows and Linux are installed. If you have only one drive, it should be Drive 0, if you have multiple drives, select the one where Windows is installed.

```bash
  
list partition
select partition 1
assign letter=X
```

Now, open a new terminal window while keeping the old one open, and type:

```bash
cd x:
cd EFI
dir
```

If you see an entry for openSUSE, simply remove it with:

```bash
rd /s /q "Name"
```

Return to the previous terminal where you ran DiskPart, and run:

```bash
remove letter=Z
```



### For Standalone Linux Users

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

{% hint style="info" %} If you are using an Intel system and the Windows installer cannot detect your drive, disable **VMD** (Volume Management Device) in BIOS. {% endhint %}

{% endstep %}

{% endstepper %}
