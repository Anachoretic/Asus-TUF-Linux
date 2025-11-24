---
title: openSUSE Tumbleweed
icon: suse
---


# Tumbleweed Post Install:
{% stepper %}

{% step %}
## 1. Update: 

After installation, perform an update to ensure you’re on the latest version of Tumbleweed.

```bash
sudo zypper refresh && sudo zypper dup
```

While you can update the system using `gnome-software` or `discover`, it’s not recommended. These tools can’t reliably handle conflicts or errors, so it’s better to perform updates through the terminal instead.

{% endstep %}

{% step %}

## 2. Zypper Config: 

Edit the Zypper config and add the following lines at the end of the file:

```bash
sudo nano /etc/zypp/zypp.conf
```

Add the following lines to the file:

```bash
download.max_concurrent_connections = 10
```

This sets the maximum number of concurrent downloads to 10.

Optional Command:
This isn’t really necessary, but you may choose to add it:

```bash
solver.onlyRequires = true
```

This skips all the recommended packages that aren’t strictly required.

{% endstep %}

{% step %}

## 3. System Snapshot: 

Before proceeding with other steps, it’s recommended to create a snapshot of the current system. If something breaks, you can quickly load the snapshot to restore the system to its previous state.

```bash
sudo su

snapper create -t pre -d "Before Changes"
```

Make sure to list all snapshots to verify that the snapshot was created:

```bash
snapper list
```

### To restore a previous snapshot:
First, list all available snapshots:

```bash
snapper list
```

Now, find the snapshot with the same description you added while creating it, then note the ID for it. The starting number is the ID of the snapshot:

Then run the following command:
```bash
snapper rollback <snapshot-number>

Example:
snapper rollback 12
```

{% endstep %}

{% step %}

## 4. Drivers:
The drivers for AMD and Intel are baked into the kernel, so you won’t need to do anything for them for the most part. However, you will need to install the drivers for NVIDIA GPUs.

First, add the repository for the NVIDIA drivers:

```bash
sudo zypper install openSUSE-repos-Tumbleweed-NVIDIA && sudo zypper ref
```

After adding the repo, you will likely get a popup asking if you want to import the GPG key. Accept it and continue.

Then, install all the drivers. There are two drivers available for Tumbleweed: the open kernel module and the proprietary ones.

For GPUs based on Turing architecture or newer, it is recommended to use the open kernel module drivers. The performance is pretty much the same, and for Blackwell and newer, you should use the open kernel module.

For Turing, Ampere, Ada Lovelace, and Hopper, it is highly recommended to use the Open Kernel Module. For Pascal, you should use only the proprietary driver, as the newer module isn’t compatible.

{% hint style="info" %} The NVIDIA Open Kernel Module and the open-source Nouveau driver are completely different; don’t mix them up. {% endhint %}Info= 

You can compare your GPU architecture in the chart below:

| **GPU Generation**          | **Architecture** |
|-----------------------------|------------------|
| RTX 50 Series Laptop GPU    | Blackwell        |
| RTX 40 Series Laptop GPU    | Ada Lovelace     |
| RTX 30 Series Laptop GPU    | Ampere           |
| RTX 20 Series Laptop GPU    | Turing           |
| GTX 16 Series               | Turing           |
| GTX 10 Series               | Pascal           |


### 4.1. Open Kernel Module:
```bash
sudo zypper in nvidia-video-G06 nvidia-gl-G06 nvidia-compute-G06 nvidia-compute-utils-G06
```

### 4.2. Proprietary Drivers:
```bash
sudo zypper in nvidia-video-G06 nvidia-gl-G06 nvidia-compute-G06 nvidia-compute-utils-G06
```

{% endstep %}

{% step %}

## 5. Asus Software:
The ASUS-specific software for Tumbleweed is hosted on the COPR repository, so you will need to add the COPR repo to download it. 

To add the repository, we need to create a .repo file. Simply create a file called asus-linux using the following command:

```bash
sudo nano /etc/zypp/repos.d/asus-linux.repo
```

Then paste the following into the asus-linux.repo file, save it with Ctrl+S, exit with Ctrl+X, and refresh Zypper using `zypper ref`.

```bash
[asus-linux]
name=asus-linux
baseurl=https://download.copr.fedorainfracloud.org/results/lukenukem/asus-linux/opensuse-tumbleweed-$basearch/
type=rpm-md
skip_if_unavailable=True
gpgcheck=1
gpgkey=https://download.copr.fedorainfracloud.org/results/lukenukem/asus-linux/pubkey.gpg
repo_gpgcheck=0
enabled=1
enabled_metadata=1
```

Then download the following packages:
```bash
sudo zypper in asusctl rog-control-center supergfxctl switcheroo-control 
```

Then enable the services with:
```bash
systemctl enable switcheroo-control supergfxd
```
{% hint style="info" %} Ignore the "Asus kernel isn't loaded" message in rog-control-center. It’s safe. {% endhint %}

### 5.1 GPU Switching:

- GNOME users: [supergfxctl-gex](https://extensions.gnome.org/extension/5344/supergfxctl-gex/)
- KDE users: Install the supergfxctl-plasmoid:

There is no official Tumbleweed package for supergfxctl-plasmoid, but you can still compile it yourself or install it from an OBS (openSUSE Build Service ), just make sure to verify the packages before installing them.

Set Hybrid GPU mode:

```bash
supergfxctl --mode Hybrid
```


{% hint style="info" %} Switching to/from Hybrid mode needs logout. Ultimate mode requires a reboot. {% endhint %}

### 5.2 Fix Hotkeys (Asus Only)

Some hotkeys are BIOS-level and can’t be remapped.

{% hint style="info" %} To test remap capability: press the key while adding a shortcut. If nothing registers, it can't be reassigned. {% endhint %}

### For GNOME
- Settings → Keyboard → Shortcuts → “+”

### For KDE
- System Settings → Shortcuts → Custom Shortcuts → New Global Shortcut

Add the following commands:

- `rog-control-center`: Launch GUI
- `asusctl aura -n`: Toggle Aura lighting
- `asusctl profile -n`: Change power profile

### 5.3 Groups:

For asusctl and supergfxctl to work properly, you need to be a member of the wheel and video groups. Run the following command to add yourself:

```bash
sudo su
usermod -a -G wheel,video yourusername
```

{% endstep %}

{% step %}

## 6. Power Managemnt:
Tumbleweed includes tuned by default, but it doesn’t work well with asusctl. It’s recommended to use power-profiles-daemon instead, since it works better and supports the existing Fn+F5 power hotkey.

```bash
sudo zypper rm tuned && sudo zypper in power-profiles-daemon
```

Then enable it with:
```bash
sudo systemctl enable power-profiles-daemon --now
```

{% endstep %}

{% step %}

## 7. Firewall:

A firewall is a security system that monitors, filters, and controls incoming and outgoing network traffic according to predefined security rules. While it isn't mandatory to have a firewall for a workstation, it is highly recommended to set up some form of firewall. Firewalld is the recommended firewall for Tumbleweed.

By default, the firewall should already be installed. If it is not installed, you can install it with the following command:

```bash
sudo zypper in firewalld

sudo systemctl enable firewalld --now
```

Then enable it with the following command:
```bash
sudo systemctl enable firewalld
```

If you need to open a port or a service, you can either use the GUI or the terminal for it. The `firewalld` package contains a GUI by default, while the Python package adds applet support for firewalld.

### To open/close a port or service, you can run the following command:
```bash
sudo firewall-cmd --zone=public --remove-port=<port>/<optional: protocol>
```

 **For Services:**
```bash
sudo firewall-cmd --zone=public --remove-service=<service>
```

By default, firewalld only saves these changes temporarily until a reboot. If you want to make the changes persistent, use the `--permanent` flag as such:

```bash
sudo firewall-cmd --permanent --zone=public --remove-service=<service>
```

Then reload firewalld to integrate changes into the current runtime:

```bash
sudo firewall-cmd --reload
```

{% endstep %}

{% step %}

## 8. Codecs:
Due to licensing restrictions, Tumbleweed does not ship with proprietary codecs. For most use cases, the built-in codecs should be sufficient. If they are not, you can run the following command to install all proprietary codecs.

```bash
sudo zypper in opi 
sudo opi
```

{% endstep %}

{% step %}


## 9. Optional Stuff:
### 9.1 Firefox branding:
By default, Firefox comes customized with openSUSE branding, which can be helpful as it adds many resources to your bookmarks. If you prefer the “default” Firefox without these additions, follow the steps below:
```bash
sudo zypper rm firefox MozillaFirefox-branding-openSUSE
```

Then reinstall Firefox with the upstream branding:
```bash
sudo zypper in firefox MozillaFirefox-branding-upstream
```

### 9.2 Package Kit:
Sometimes when you try to use `zypper`, you might get a popup saying it is being used by `PackageKit` and asking if you want to quit PackageKit. This popup is harmless, and zypper will usually become available after a short time. However, if it’s annoying, you can remove PackageKit, but note that doing so will disable the ability to download packages from other repositories (excluding Flatpak) using software stores like GNOME Software or Discover.

```bash
sudo zypper rm PackageKit
```

{% endstep %}

{% endstepper %}
