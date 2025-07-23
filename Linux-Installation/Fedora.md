

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

{% hint style="warning" %} **Secure Boot can prevent Fedora from booting if not disabled.** {% endhint %}

{% hint style="warning" %} **Failure to disable BitLocker may result in data loss or drive access issues.** {% endhint %}

{% endstep %}
{% step %}

## Installation Steps

## Step 1: Download Fedora ISO

Download the latest Fedora Workstation ISO from the official website:

[https://fedoraproject.org/en/workstation/download](https://fedoraproject.org/en/workstation/download)

- Recommended: Fedora Workstation (GNOME)
- Alternative: Fedora Workstation (KDE)

{% endstep %}
{% step %}

## Step 2: Create Bootable Media

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
5. After setup, copy the Fedora ISO file to the **Ventoy** partition. {% endtab %} {% endtabs %} {% endstep %}

{% endstep %}
{% step %}

## Step 3: Partitioning (Dual Boot Only)

1. Open **Disk Management** in Windows.
2. Right-click on the **C: drive** and select **Shrink Volume**.
3. Enter the desired size for Linux (at least 50 GB recommended).
4. Leave the space unallocated; do **not** format it. {% endstep %}

{% endstep %}
{% step %}

## Step 4: BIOS Setup

1. Restart your PC and press `F2` (or the correct key for your system) to enter BIOS.
2. In BIOS:
   - Disable **Secure Boot**.
   - Set the USB drive as the first boot device.
3. Save changes and exit BIOS.
4. Boot from the USB. {% endstep %}

{% endstep %}
{% step %}

## Step 5: Begin Installation

Video guide: [https://www.youtube.com/watch?v=eHQJMy8Q7Zk](https://www.youtube.com/watch?v=eHQJMy8Q7Zk)

{% hint style="info" %} If the installer doesn't appear immediately, wait 10–20 seconds. {% endhint %}

1. On the Welcome screen, select your preferred language and click Next.
2. On the **Installation Destination** screen:
   - **Dual Boot:** Ensure unallocated space is available. Select "Share disk with other operating system".
   - **Standalone:** Choose "Use entire disk".

{% hint style="warning" %} **Ensure you select the correct target drive if your system has multiple disks to avoid data loss.** {% endhint %}

3. (Optional) Configure disk encryption on the Storage Configuration screen. Unless required, it's recommended to skip encryption for simplicity.
4. Review your configuration on the Summary screen. Click **Begin Installation** to start.
5. After installation:
   - Exit the Live Environment.
   - Remove the USB drive.
   - Reboot into Fedora.

{% endstep %}
{% step %}


## Uninstalling Fedora

### For Dual Boot Setups

Refer to the [same video guide](https://www.youtube.com/watch?v=eHQJMy8Q7Zk) for safe removal of Fedora.

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

{% hint style="info" %} If you're using an Intel system and the Windows installer doesn't detect your disk, disable **VMD (Volume Management Device)** in BIOS. {% endhint %}

{% endstep %}
