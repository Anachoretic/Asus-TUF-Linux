# Setup (Debian)

## Post-Installation Configuration for Debian-Based Systems

{% hint style="danger" %}
**Note:** Debian and its derivatives (Ubuntu, Mint, Pop!\\\_OS, etc.) are considered **unsupported** on the official \[Asus Linux]\(https://asus-linux.org/) site. Software such as \`asusctl\` and \`supergfxctl\` must be compiled from source. Fedora is the recommended distribution for full support. If you still wish to proceed with Debian, follow this guide carefully. Some features may not function as expected.
{% endhint %}

{% stepper %}
{% step %}
#### 1. System Update

After installation, it’s crucial to update the system to ensure all packages are up-to-date.

```bash
sudo apt update && sudo apt upgrade
```

This command updates the package list and upgrades all installed packages.
{% endstep %}

{% step %}
#### 2. Driver Installation

By default, most drivers (e.g., for Intel, AMD, and other hardware) are included in the Linux kernel. However, **Nvidia dGPU drivers must be installed separately.**

* **Debian users:** Follow the [Debian Nvidia Guide](https://wiki.debian.org/NvidiaGraphicsDrivers).
* **Ubuntu/Mint/Pop!\_OS users:** Use the built-in **Driver Manager** or **Software & Updates > Additional Drivers**. Ensure that proprietary drivers (not Nouveau) are selected.

{% hint style="warning" %}
**Important:** If using hybrid graphics, set GPU mode to \*Hybrid\* in the BIOS or through tools, or the Nvidia driver installer might not detect the GPU.
{% endhint %}

* **Pop!\_OS:** Offers a dedicated Nvidia ISO. If you have an Nvidia GPU, this is the recommended option.
{% endstep %}

{% step %}
#### 3. Flatpak Installation (Ubuntu Only)

Most Debian-based distributions ship with Flatpak, but Ubuntu defaults to Snap. To install Flatpak on Ubuntu:

```bash
sudo apt install flatpak
sudo apt install gnome-software-plugin-flatpak
```

Then, add the Flathub repository:

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
{% endstep %}

{% step %}
#### 4. Asus Software Installation (Manual Compilation)

{% hint style="warning" %}
**Note:** These tools need to be built from source. Follow steps carefully.
{% endhint %}

**4.1 Install Build Dependencies**

Install required development tools and libraries:

```bash
sudo apt install -y build-essential git cmake pkg-config libpci-dev libsysfs-dev libudev-dev \
libboost-dev libgtk-3-dev libglib2.0-dev libseat-dev
```

Install Rust (Cargo):

<pre class="language-bash"><code class="lang-bash"><strong>curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
</strong></code></pre>

Configure the environment:

```bash
. "$HOME/.cargo/env"
```

**4.2 Installing `asusctl`**

1. Clone the repository or download the latest release.
2. Extract it and open a terminal inside the `asusctl` folder.
3. Run:

<pre class="language-bash"><code class="lang-bash"><strong>make &#x26;&#x26; sudo make install
</strong></code></pre>

{% hint style="info" %}
This software requires **Wayland.** It may not work under X11.

Icons may appear missing for the first few reboots, this is expected.
{% endhint %}

**4.3 Installing `supergfxctl`**

1. Clone the repository:

```bash
git clone https://gitlab.com/asus-linux/supergfxctl.git
cd supergfxctl
```

2. Build and install:

```bash
make && sudo make install
```

3. Enable and start the service:

```bash
sudo systemctl enable supergfxd
sudo systemctl start supergfxd
```

**4.4 Optional GUI Support**

* **GNOME:** Install the [`supergfxctl-gex`](https://extensions.gnome.org/extension/5344/supergfxctl-gex) extension.
* **Other DEs (e.g., Cinnamon, MATE,XFCE):** Use CLI commands.

**4.5 Switching GPU Modes via CLI**

```bash
supergfxctl --mode Integrated   # Use integrated GPU only
supergfxctl --mode Hybrid       # Use both integrated and discrete GPU
supergfxctl --mode AsusMuxDgpu  # Use discrete GPU only
```

To check available options:

```bash
supergfxctl --help
```
{% endstep %}

{% step %}
#### 5. Fixing Hotkeys

{% hint style="info" %}
Some hotkey functions are controlled at the BIOS level and cannot be remapped. To check if a hotkey can be remapped, try creating a shortcut. If the input is detected, remapping is possible.
{% endhint %}

**5.1 GNOME**

* Go to **Settings > Keyboard > Keyboard Shortcuts**
* Click “+” to add a new shortcut

**5.2 KDE**

* Go to **System Settings > Shortcuts > Custom Shortcuts**
* Create a new **Global Shortcut → Command/URL**

Enter a name, press the desired key (e.g., Fn+F4), and set one of the following commands:

* Armoury Crate: `rog-control-center`
* Aura Mode: `asusctl aura -n`
* Performance Mode: `asusctl profile -n`
{% endstep %}
{% endstepper %}
