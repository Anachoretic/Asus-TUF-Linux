# Gaming on Linux

Gaming on Linux has come a long way, mostly thanks to tools like **Wine** and **Proton**. It’s much better than it used to be, but it’s still not flawless. Some games won’t run at all, and others might need extra tweaks to get working.

If most of your games are on Steam, you’re in a good spot. **Valve’s Proton** lets you run a large number of Windows games on Linux without much hassle. For games from other platforms (like Epic or Ubisoft), support can be inconsistent. That’s where tools like **Lutris** and **Heroic Launcher** come in , they make things easier, though not everything will work perfectly.

Multiplayer games that use kernel-level anti-cheat (like **Easy Anti-Cheat** or **BattlEye**) often don’t run on Linux. This is by design, as anti-cheat programs need deep system access and are built specifically for Windows, so unfortunately many competitive titles won’t launch or let you connect online on Linux.
Anti cheat makers could add Linux support, but most don’t , mainly because the Linux player base is small, security is tricky to handle properly, and honestly, it’s not a priority for them.

### Check Compatibility First
Before diving in, check whether the game works on Linux using these sites:

- [**ProtonDB**](https://www.protondb.com) – Real user reports on how games run with Proton.
- [**Are We Anti-Cheat Yet?**](https://areweanticheatyet.com) – Lists anti-cheats and whether they work on Linux or Steam Deck.

If your favorite games aren’t compatible, you have two options:
- **Dual-boot Linux with Windows**
- **Stick with Windows** if gaming is a major priority

{% hint style="tip" %}
Check ProtonDB for useful launch options or configuration tweaks. The community often shares helpful info for getting specific games working.
{% endhint %}

This guide is split into two sections:

1. **Steam Games**
2. **Non-Steam Games**

{% hint style="warning" %}
Make sure your GPU drivers are properly installed before continuing. NVIDIA users should install the proprietary driver. For AMD, the open-source Mesa drivers are usually sufficient.
{% endhint %}

## 1. Steam Games

### Installing Steam
Steam is easy to install and available through both Flatpak and package managers.

**Flatpak (Recommended for most users):**
```bash
flatpak install flathub com.valvesoftware.Steam
```

**Package Manager Install:**
- **Arch (multilib must be enabled):**
```bash
sudo pacman -S steam
```
- **Debian/Ubuntu:**
```bash
sudo apt install steam
```
- **Fedora (Enable RPM Fusion):**
```bash
sudo dnf install steam
```

{% hint style="tip" %}
Flatpak offers better isolation and avoids system dependency issues. However, native installs might offer slightly better integration with your GPU drivers.
{% endhint %}

### Enabling Proton Support
Once Steam is installed and launched:

1. Open **Steam > Settings > Compatibility**
2. Enable **"Steam Play for all other titles"**
3. Set it to use **Proton Experimental**

This lets you run many Windows-only games directly through Steam.

{% hint style="tip" %}
Always prefer native Linux versions when available , they usually offer better performance and fewer bugs.
{% endhint %}

### Adding Non-Steam Games
To manage other games through Steam:
1. Go to **Games > Add a Non-Steam Game to My Library**
2. Browse to the game’s executable and add it

This enables features like controller support and Steam Overlay.

{% hint style="tip" %}
You can also enable Proton for these non-Steam games. Just right-click the game > Properties > Compatibility.
{% endhint %}

   

## 2. Non-Steam Games

### Lutris
**Lutris** is an open-source game launcher that supports a wide range of platforms, including:
- GOG
- Epic Games Store
- Ubisoft Connect
- Battle.net
- Emulators (RetroArch, Dolphin, etc.)

It helps organize and launch both native and Windows-based games from one place.

### Installing Lutris
**Via Flatpak:**
```bash
flatpak install flathub net.lutris.Lutris
```

**Via Package Manager:**
- **Arch:**
```bash
sudo pacman -S lutris
```
- **Debian/Ubuntu:**
```bash
sudo apt install lutris
```
- **Fedora:**
```bash
sudo dnf install lutris
```


### Running Games in Lutris
After installing:
- Log into your accounts (Epic, GOG, etc.) directly in Lutris
- Your game libraries should appear if supported

To manually install a Windows game:
1. Click **+** > **Install a Windows game from an executable**
2. Enter a name for the game
3. If needed, Lutris will prompt to install Wine
4. Browse to the `.exe` file and continue the setup

{% hint style="tip" %}
Not every game is listed in the app, but many have community installers available at [lutris.net](https://lutris.net).
{% endhint %}

### Adding Emulator Games
For emulators:

1. Go to **Preferences > Runners**, and enable the emulator you want
2. Add a game manually:
   - Click **+ > Add a Game**
   - Set the emulator executable
   - Point it to the ROM file

{% hint style="warning" %}
Just make sure you only use ROMs you legally own. I don’t condone piracy in any form.
{% endhint %}

   

That’s the basics of gaming on Linux. It won’t be perfect for every setup or game, but it’s come a long way. Steam and Lutris together can cover a large chunk of your library.

{% hint style="tip" %}
If you need help with gaming on Linux, I’d recommend joining communities like [r/linux_gaming](https://www.reddit.com/r/linux_gaming/)  and  [GamingOnLinux](https://www.gamingonlinux.com/). They’re great places to get advice and troubleshooting steps.
{% endhint %}
