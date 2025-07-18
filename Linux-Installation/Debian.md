# 1. Important Note

I highly recommend using **Fedora** or another up-to-date distro instead of Debian-based ones. They generally offer better hardware and software compatibility with Asus laptops in general.
But if you still want to go with something like Ubuntu or Mint, that’s fine too , just follow this guide. Keep in mind that some Asus-specific features and software might not work as expected.
Also, a quick side note: this guide includes links to video tutorials in case you prefer a visual walkthrough.

{% hint style="info" %}
This guide includes links to video tutorials for visual learners.
{% endhint %}

# 2. Distro Selection

Fedora is the preferred distribution for Asus laptops. However, for those familiar with Debian-based systems:

Ubuntu and Linux Mint are user-friendly alternatives, though they may require more manual setup or lack certain hardware support out of the box.

# 3. Prerequisites

{% hint style="info" %}
### Required Before You Begin

• USB drive with at least 8 GB of storage  
• Bootable media tool: **Rufus**, **Ventoy**, **Balena Etcher**, or **Fedora Media Writer**  
• **Secure Boot** must be disabled in BIOS  
• **BitLocker** must be disabled in Windows  
• **Fast Boot** should be disabled (for dual-boot users)  
• Set **GPU mode to Ultimate or Standard** in Windows  
{% endhint %}

{% hint style="warning" %}
**Secure Boot can prevent some distros from booting. Disable it in BIOS.**
{% endhint %}

{% hint style="warning" %}
**If BitLocker is not disabled, it can cause data loss or access issues during installation.**
{% endhint %}

# 4. Installation Process

## 4.1 Video Tutorials

- [Standalone Linux Installation](https://youtu.be/WiW4KN2rNZY?si)  
- [Dual Boot Setup with Windows](https://youtu.be/mXyN1aJYefc?si)

{% stepper %}
{% step %}

## 4.2 Download ISO

Download the latest version of your chosen Linux distribution:

[Ubuntu](https://ubuntu.com/download)  
[Linux Mint](https://linuxmint.com/download.php)  

{% endstep %}

{% step %}

## 4.3 Create Bootable USB

{% tabs %}
{% tab title="Rufus" %}
1. Download [Rufus](https://rufus.ie/)  
2. Select “ISO Image” and choose your distro’s ISO  
3. Insert your USB drive  
4. Select the correct **Partition Scheme** ( GPT for modern systems)  
5. Click **Start** and wait for it to finish  
6. Safely eject the USB
{% endtab %}

{% tab title="Ventoy" %}
Ventoy allows multiple ISO files on one USB. It’s perfect for testing or switching between OSes.

1. [Download Ventoy](https://github.com/ventoy/ventoy/releases)  
2. Extract and run `Ventoy2Disk.exe`  
3. Go to **Options > Partition Style > GPT**  
   (Secure Boot support is optional; MOK keys required on first boot if enabled)  
4. Select your USB drive and click **Install**  
5. After setup, copy ISO files directly to the **Ventoy** partition
{% endtab %}
{% endtabs %}

{% endstep %}

{% step %}

## 4.4 Partitioning (Dual Boot Only)

1. Open **Disk Management** in Windows  
2. Right-click on the **C: drive** and select **Shrink Volume**  
3. Enter the desired size for Linux (at least 50 GB recommended)  
4. Do **not** format the new space, leave it unallocated

{% endstep %}

{% step %}

## 4.5 BIOS Setup

1. Restart your PC and press `F2` (or your BIOS key)  
2. In BIOS settings:
   • Disable **Secure Boot**  
   • Set the USB drive as the first boot device  
3. Save changes and exit BIOS  
4. Boot from the USB

{% hint style="info" %}
If the installer doesn't appear right away, wait 10–20 seconds or search for “Install...” in the menu.
{% endhint %}

{% endstep %}

{% step %}

## 4.6 Begin Installation

Each distro’s setup may differ slightly, but the basics are the same:

- Enable **third-party software** (e.g., codecs, drivers)
- Choose:
  - **Erase disk and install** (for Linux-only users)
  - **Install alongside Windows** (for dual boot)

{% hint style="warning" %}
If your system has multiple drives, ensure the correct one is selected to avoid data loss.
{% endhint %}

- **Disk encryption**:
  - Optional for standalone Linux
  - Not recommended for dual-boot systems, as it may cause bootloader issues

Once confirmed, begin the installation process.

{% endstep %}

{% step %}

## 4.7 Complete Setup

1. After installation, **exit the Live Environment**  
2. **Remove the USB drive**  
3. Reboot your system , you should now boot directly into Linux or see a bootloader menu.

{% endstep %}
{% endstepper %}

# 5. Uninstalling Linux

## 5.1 For Dual Boot Users

1. Open **Disk Management** in Windows and delete the Linux partitions  
2. Launch **Command Prompt as Administrator**, then run:

```bash
diskpart
select disk X  ( Replace X with your disk number)
list partition
select partition 1 (The efi partition on windows is 1)
assign letter=Z
remove letter=z (Run this after completing Step 3)
```

3. Open a new Command Prompt window:

```bash
cd Z:
cd efi
dir
rd /s /q ubuntu     (Replace "ubuntu" with your distro’s folder name)
```

4. Restart your system. Windows should boot normally.

## 5.2 For Standalone Linux Users

1. Boot from a Windows installation USB  
2. Press `Shift + F10` to open Command Prompt  
3. Run:

```bash
diskpart
select disk X  (Replace X with the correct drive)
clean
exit
```

4. Continue with the Windows installation process

{% hint style="info" %}
If you are using an Intel system and the Windows installer cannot detect your drive, disable **VMD (Volume Management Device)** in BIOS.
{% endhint %}

