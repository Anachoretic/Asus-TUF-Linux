---
title: Troubleshooting
icon: wrench
---


# Troubleshooting Guide
This guide contains some issues that one might face while using Linux, along with the solutions and resources to get help.

{% stepper %}

{% step %}

## 1. GRUB is Broken

**Problem:** GRUB stopped working due to a Windows update or another issue.

**Fix:** Reinstall GRUB using a live ISO.

### Steps

1. Boot from the same ISO you used to install your OS.
2. Open a terminal and identify your **root** (`/`) and **EFI** (`/boot`) partitions:
   ```bash
   lsblk
   ```
- Lists all block devices and partitions. Identify the root and EFI partitions.
- Example: `/dev/nvme0n1p2` (root) and `/dev/nvme0n1p1` (EFI)

4. Follow the commands for your distro:

#### Arch
```bash
mount /dev/X /mnt               # Mount root partition to /mnt
mount /dev/Y /mnt/boot          # Mount EFI partition to /mnt/boot
arch-chroot /mnt                # Change root into the mounted system
pacman -S grub efibootmgr os-prober  # Install GRUB and related tools
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=grub  # Install GRUB to EFI
grub-mkconfig -o /boot/grub/grub.cfg  # Generate GRUB configuration
```

#### Ubuntu
```bash
sudo su                         # Become root
mount /dev/X /mnt               # Mount root partition
mount /dev/Y /mnt/boot          # Mount EFI partition
for i in /dev /dev/pts /proc /sys /run; do mount -B $i /mnt$i; done  # Bind system dirs to chroot
chroot /mnt                     # Enter mounted system

# Install GRUB to the disk
grub-install /dev/nvme0n1

# If EFI variables cannot be set, mount efivarfs and retry
mount -t efivarfs none /sys/firmware/efi/efivars
grub-install /dev/nvme0n1
```
```bash
update-grub                     # Generate GRUB configuration
```

#### Fedora
```bash
mount /dev/X /mnt               # Mount root partition
mount /dev/Y /mnt/boot          # Mount EFI partition
sudo systemd-nspawn -D /mnt     # Enter mounted system via systemd-nspawn
grub2-install /dev/sda          # Install GRUB to the disk
grub2-mkconfig -o /boot/grub2/grub.cfg  # Generate GRUB configuration
exit                             # Leave systemd-nspawn
sudo umount /mnt/boot /mnt      # Unmount partitions
```
Reboot without the live media.

{% endstep %}

{% step %}

## 2. NTFS Drive Can't Be Opened
**Problem:** NTFS is proprietary, some distros lack built-in support.

**Fix:** Install `ntfs-3g` (adds NTFS read/write support).

- **Arch:**
```bash
sudo pacman -S ntfs-3g
```
- **Debian/Ubuntu:**
```bash
sudo apt install ntfs-3g
```
- **Fedora:**
```bash
sudo dnf install ntfs-3g
```
{% hint style="warning" %} Running Steam or other games from an NTFS partition will not work, you will need to reinstall them on a Linux filesystem. {% endhint %}
{% endstep %}

{% step %}

## 3. Brightness Doesn’t Work in Hybrid Mode

**Fix:** Add kernel parameters.
```bash
sudo nano /etc/default/grub     # Edit GRUB configuration file
```
Replace:
```
GRUB_CMDLINE_LINUX="rhgb quiet"
```
With:
```
GRUB_CMDLINE_LINUX_DEFAULT="rhgb quiet pcie_aspm=force acpi_backlight=native"
```
Update GRUB:
- Arch:
```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
- Fedora:
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```
- Ubuntu:
```bash
sudo update-grub
```
Reboot.

{% endstep %}

{% step %}

## 4. Apps Not Using dGPU in Hybrid Mode
**Fix:** Install and enable `switcheroo-control` (GPU switching service).
```bash
sudo pacman -S switcheroo-control                
sudo systemctl enable --now switcheroo-control
```
{% hint style="info" %} Most distributions like Fedora, Ubuntu, and many others include a GPU switching service by default, but many Arch Linux-based distributions do not. This guide is mainly for Arch-based distros. {% endhint %}

{% endstep %}

{% step %}

## 5. Fn Keys Not Working
Certain keys like **Fn+F5**, **Fn+F4**, or the Armoury Crate key may not work. Check the post-install guide for steps to enable them.

{% endstep %}

{% step %}


## 6. Black Screen After Installing NVIDIA Drivers
**Possible Fix:**

1. **Enable Secure Boot in BIOS temporarily**: This will block unsigned NVIDIA drivers from loading, allowing the system to boot so you can fix the drivers.
2. Once booted, either:
   - **Reinstall the NVIDIA drivers**
   - **Force rebuild the NVIDIA kernel module**:
     - Arch:
     ```bash
     sudo mkinitcpio -P   # Regenerate initramfs for all kernels
     ```
     - Fedora:
     ```bash
     sudo dracut --force  # Rebuild initramfs
     ```

- After that, **disable Secure Boot** in BIOS so the signed/working drivers can load normally.


{% endstep %}

{% step %}

## 7. Live ISO Freezes Before Installer Loads
**Possible Cause:** GPU driver conflict.
**Workaround:** Disable the dGPU in Windows, then re-enable it after completing the setup and installing supergfxctl.

{% endstep %}

{% step %}

## 8. Help Resources
I can’t include every possible problem and solution here. If you encounter an issue not listed, it’s best to search online, many issues have already been solved on Reddit, distro-specific forums, or other Linux communities. Here are some recommended resources.

- [r/linux4noobs](https://www.reddit.com/r/linux4noobs)
- [r/linux_gaming](https://www.reddit.com/r/linux_gaming)
- [LinuxQuestions](https://www.linuxquestions.org/)
- Distro-specific subreddits (`r/arch`, `r/fedora`, `r/ubuntu`)
- [Asus Linux Discord Server](https://discord.com/invite/B8GftRW2Hd)

{% endstep %}

{% endstepper %}
