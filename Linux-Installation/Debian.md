
# Important Note

I highly recommend using **Fedora** or another up-to-date distro instead of Debian-based ones. They generally offer better hardware and software compatibility with Asus laptops in general. However, if you still want to go with something like Ubuntu or Mint, that’s fine too,just follow this guide. Keep in mind that some Asus-specific features and software might not work as expected. Also, a quick side note: this guide includes links to video tutorials in case you prefer a visual walkthrough.

{% hint style="info" %} This guide includes links to video tutorials for visual learners. {% endhint %}


{% stepper %}

{% step %}

# 1. Distro Selection

Fedora is the preferred distribution for ASUS laptops. However, for those who prefer to stick with Debian-based systems:

Ubuntu and Linux Mint are among the most user-friendly distributions. I recommend using one of these, as the rest of this guide is based on these two specific distros,though it is also applicable to other Debian-based distributions.

{% endstep %}

{% step %}

# 2. Prerequisites

{% hint style="info" %}

### Required Before You Begin

• USB drive with at least 8 GB of storage.  
• Bootable media tool: **Rufus**, **Ventoy**, or **Balena Etcher**.  
• **Secure Boot** must be disabled in BIOS.  
• **BitLocker** must be disabled in Windows.  
• **Fast Boot** should be disabled (for dual-boot users).  
• Set **GPU mode to Ultimate or Standard** in Windows.

{% endhint %}

{% hint style="warning" %} **Secure Boot can prevent some distros from booting. Disable it in BIOS.** {% endhint %}

{% hint style="warning" %} **If BitLocker is not disabled, it can cause data loss or access issues during installation.** {% endhint %}

{% endstep %}

{% step %}

# 3. Installation Process

## Video Tutorials

- [Standalone Linux Installation](https://youtu.be/WiW4KN2rNZY?si)  
- [Dual Boot Setup with Windows](https://youtu.be/mXyN1aJYefc?si)


## Step 3.1: Download ISO

Download the latest version of your chosen Linux distribution:

[Ubuntu](https://ubuntu.com/download)  
[Linux Mint](https://linuxmint.com/download.php)


## Step 3.2: Create Bootable USB

{% tabs %} {% tab title="Rufus" %}

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

{% endtab %} {% endtabs %}


## Step 3.3: Partitioning (Dual Boot Only)

1. Open **Disk Management** in Windows.  
2. Right-click on the **C: drive** and select **Shrink Volume**.  
3. Enter the desired size for Linux (at least 50 GB recommended).  
4. Do **not** format the new space; leave it unallocated.

{% endstep %}

{% step %}

## Step 4: BIOS Setup

1. Restart your PC and press `F2` (or your BIOS key).  
2. In BIOS settings:  
   • Disable **Secure Boot**.  
   • Set the USB drive as the first boot device.  
3. Save changes and exit BIOS.  
4. Boot from the USB.

{% hint style="info" %} If the installer doesn't appear right away, wait 10–20 seconds or search for “Install...” in the menu. {% endhint %}

{% endstep %}

{% step %}

## Step 5: Begin Installation

Each distro’s setup may differ slightly, but the basics are the same:

- Enable **third-party software** (e.g., codecs, drivers).  
- Choose:  
  - **Erase disk and install** (for Linux-only users).  
  - **Install alongside Windows** (for dual boot).

{% hint style="warning" %} If your system has multiple drives, ensure the correct one is selected to avoid data loss. {% endhint %}

- **Disk encryption**:  
  - Optional for standalone Linux.  
  - Not recommended for dual-boot systems, as it may cause bootloader issues.

Once confirmed, begin the installation process.

{% endstep %}

{% step %}

## Step 6: Complete Setup

1. After installation, **exit the Live Environment**.  
2. **Remove the USB drive**.  
3. Reboot your system. You should now boot directly into Linux or see a bootloader menu.

{% endstep %}

{% endstepper %}

{% endstep %}

{% step %}

# 7. Uninstalling Linux

## 7.1 For Dual Boot Users

1. Open **Disk Management** in Windows and delete the Linux partitions.  
2. Launch **Command Prompt as Administrator**, then run:

```bash
diskpart
select disk X  # Replace X with your disk number
list partition
select partition 1  # The EFI partition on Windows is usually 1
assign letter=Z
```

3. Open a new Command Prompt window:

```bash
cd Z:
cd EFI
dir
rd /s /q ubuntu  # Replace "ubuntu" with your distro’s folder name
```

4. Return to the previous terminal and run:

```bash
remove letter=Z
```

5. Restart your system. Windows should boot normally.

## 7.2 For Standalone Linux Users

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
