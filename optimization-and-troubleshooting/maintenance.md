---
title: System Maintenance
icon: screwdriver-wrench
---


# System Maintenance

Regular maintenance includes updating software, backing up data, cleaning orphaned packages and caches, and installing LTS kernels to keep Linux secure, efficient, and stable.

{% stepper %}

{% step %}
## Step 1:Updates
Updating your system is one of the most important things you can do. Updates include security patches, bug fixes, and improvements. Linux won’t force updates on you, so it’s good practice to check and update at least once a week. Keeping packages up to date protects you from vulnerabilities and helps keep your system stable.
### Commands to update your system:

Arch
```bash
sudo pacman -Syu
```

Debian
```bash
sudo apt update && sudo apt upgrade
```

Fedora
```bash
sudo dnf upgrade
```
{% endstep %}

{% step %}

## Step 2: Cleanup Orphaned packages:
Orphaned packages are leftover dependencies installed for software you no longer have. They just take up space and clutter your system.
Removing them helps free disk space and keeps your system tidy.

### Commands to clean orphaned packages:
Arch
```bash
sudo pacman -Rns $(pacman -Qdtq)
```

Debian
```bash
sudo apt autoremove
```

Fedora
```bash
sudo dnf autoremove
```
{% endstep %}

{% step %}

## Step 3: Trim Your SSD
SSDs work differently than traditional hard drives. They need to be told which blocks of data are no longer in use to maintain speed and longevity. This process is called trimming.

You can enable automatic weekly trimming so you don’t have to think about it, or run it manually if you prefer.

### Enable periodic trimming:


```bash
sudo systemctl enable --now fstrim.timer
```

Once enabled, it runs every week and trims the SSD automatically.

### Run trim manually:

```bash
sudo fstrim --all
```

{% endstep %}

{% step %}

## Step 4: Check Logs
When something’s wrong or you just want to make sure everything’s okay, check your system logs. Logs record everything from errors to warnings and important events. It’s a great habit to glance at logs regularly to catch potential issues early.


### View all logs since last boot:

```bash
journalctl -b
```

### View error messages only:
```bash
journalctl -p err -b
```
{% endstep %}

{% step %}

## Step 5: Backup
Backing up your files can save you from headaches if something breaks. Use tools like `rsync`, `timeshift`, or `Pika Backup` to backup system settings and user files.

{% endstep %}

{% step %}

## Step 6: Alternative kernel
Install the LTS kernel alongside the main one so if something goes wrong, you can quickly switch to the LTS kernel and keep using your laptop normally.

{% endstep %}

{% step %}

## Step 7: Clean cache
Package manager cache is where Linux stores downloaded installer files (packages) locally so it can reuse them later. It helps speed up installs and saves bandwidth, but over time it can take up a lot of disk space. Cleaning the cache frees up space.

Arch
```bash
sudo pacman -Scc
```

Debian
```bash
sudo apt autoclean
```

Fedora
```bash
sudo dnf clean all
```
{% endstep %}

{% step %}

## Step 8: Don't run random commands
Don’t run any commands if you don’t know what they do, especially if you found them on the internet. Also, don’t run commands with sudo unless you know exactly what they do and that they require sudo.

{% endstep %}

{% step %}

## Step 9: Make sure you have enough space
Make sure your system has enough free space to function properly. Sometimes hidden files or directories take up a lot of space without you realizing it.

```bash
sudo du -x -h -d1 /
```
The above command will list all the folders inside the root directory along with how much space they take.

{% endstep %}

{% endstepper %}
