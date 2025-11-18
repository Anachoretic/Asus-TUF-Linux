

# Fedora Installation Guide for ASUS Laptops

Fedora has excellent compatibility with ASUS laptops in terms of both hardware and software. The ASUS software does not need to be manually compiled from source, making Fedora one of the best distros for newcomers starting out with Linux.

{% stepper %}
{% step %}

## Prerequisites

{% hint style="info" %}

### Pre-Installation Requirements

• A USB drive with at least 8 GB of storage capacity.\
• One of the following tools to create bootable media: **Rufus**, **Ventoy**, **Balena Etcher**, or **Fedora Media Writer**.\
• **Secure Boot** must be disabled in the BIOS.\
• **BitLocker** must be turned off in Windows.\
• **Fast Boot** should be disabled (required only if you are planning to dual boot).\
• Set **GPU mode to Ultimate or Standard** in Windows. {% endhint %}

{% hint style="warning" %} Secure Boot can prevent Fedora from booting if not disabled.{% endhint %}

{% hint style="warning" %} Failure to disable BitLocker may result in data loss or drive access issues.{% endhint %}

{% endstep %}
{% step %}

## Installation Steps

## Fedora ISO download
Download the latest Fedora Workstation ISO from the official website:

-[Fedora Workstation GNOME ](https://fedoraproject.org/en/workstation/download)

-[Fedora KDE Plasma Desktop ](https://www.fedoraproject.org/kde/download)


{% endstep %}
{% step %}

##  Bootable media creation:

{% tabs %} {% tab title="Using Rufus" %}

1. Download [Rufus](https://rufus.ie/).
2. Select “ISO Image” and choose the Fedora ISO.
3. Insert your USB drive.
4. Select the correct **Partition Scheme** (GPT for modern systems).
5. Click **Start** and wait for it to finish.
6. Safely eject the USB. {% endtab %}

{% tab title="Ventoy" %}

1. [Download Ventoy](https://github.com/ventoy/ventoy/releases).
2. Extract and run `Ventoy2Disk.exe`.
3. Go to **Options > Partition Style > GPT**.\
   (Secure Boot support is optional; MOK keys are required on first boot if enabled.)
4. Select your USB drive and click **Install**.
5. After setup, copy the Fedora ISO file to the **Ventoy** partition. {% endtab %}

{% tabs %}

{% tab title="dd" %} 
The dd command is a simple utility that comes with GNU and is available on every Linux distro. It lets you copy data block by block and can be used to create a bootable live ISO on Linux without needing to install any additional tools.

1. Identify your USB drive using `lsblk`.
2. Open the terminal and run the following command:
```bash 
dd if=/path/to/iso of=/dev/sdX bs=4M status=progress oflag=sync

# Example:
dd if=/home/user/Downloads/Fedora-Workstation-Live-43-1.6.x86_64.iso of=/dev/sda bs=4M status=progress oflag=sync
```


{% endtab %}

{% endtabs %}

{% endstep %}
{% step %}

## Partitioning (Dual Boot Only)

1. Open **Disk Management** in Windows.
2. Right-click on the **C: drive** and select **Shrink Volume**.
3. Enter the desired size for Linux (at least 50 GB recommended).
4. Leave the space unallocated; do **not** format it. {% endstep %}

{% endstep %}
{% step %}

## BIOS Setup

1. Restart your PC and press `F2` (or the correct key for your system) to enter BIOS.
2. In BIOS:
   - Disable **Secure Boot**.
   - Set the USB drive as the first boot device.
3. Save changes and exit BIOS.
4. Boot from the USB. {% endstep %}

{% endstep %}
{% step %}

## Begin Installation

![](https://github.com/user-attachments/assets/24b75533-fb63-41ec-9104-fea52f66c126)

{% hint style="info" %} If the installer doesn't appear immediately, wait 10–20 seconds. {% endhint %}

![](https://github.com/user-attachments/assets/eecaab46-e385-47d8-9aca-1d6fdae49dd6)

On the Welcome screen, select your preferred language and click Next.

![](https://github.com/user-attachments/assets/c84315cf-bc5b-44b2-812d-b6d9ab800720)

On the **Installation Destination** screen:
   - **Dual Boot:** Ensure unallocated space is available. Select "Share disk with other operating system".
   - **Standalone:** Choose "Use entire disk".

{% hint style="warning" %} **Ensure you select the correct target drive if your system has multiple disks to avoid data loss.** {% endhint %}

![](https://github.com/user-attachments/assets/dfbe6e24-bd76-4ed8-a45f-6e8c647493fc)

Drive encryption is optional; you can choose to encrypt the drive or skip it. If you’re trying to dual-boot, do not enable drive encryption, as it can cause issues.

![](https://github.com/user-attachments/assets/212e2c66-cc26-4b28-a987-e9f43aa8b288)


Review your configuration on the Summary screen. Click **Begin Installation** to start.

After installation, exit the live environment by shutting down the laptop. Then remove the USB and power it on; you should see the GRUB menu. Simply select Fedora and boot into it.

{% endstep %}
{% step %}


## Fedora uninstallation:

### For Dual Boot Setups

1. Open **Disk Management** in Windows and delete the Linux partitions. Then merge the unallocated space into your C: drive or any other drive.

2. Launch **Command Prompt as Administrator**, then run:

```bash
diskpart

select disk X  # Replace X with your disk where windows is installed.

list partition

select partition 1

assign letter=Z
```

3. Open a new Command Prompt window (As administrator):

```bash
cd Z:

dir

rd /s System

del mach_kernel

cd EFI

rd /s fedora
```

4. Return to the previous terminal and run:

```bash
remove letter=Z
```

### For Standalone Fedora Installations

1. Boot from Windows installation media.
2. Press `Shift + F10` to open Command Prompt.
3. Run the following commands:

```batch
diskpart
select disk X  # Replace X with the Fedora disk
clean
```

4. Proceed with the Windows installation.

{% hint style="info" %} If you're using an Intel system and the Windows installer doesn't detect your disk, disable VMD (Volume Management Device) in BIOS. {% endhint %}

{% endstep %}

{% endstepper %}
