---
title: Gaming on Linux
icon: gamepad-modern
---

# Gaming on Linux:

Gaming on Linux has come a long way, mostly thanks to tools like **Wine** and **Proton**. It’s much better than it used to be, but it’s still not flawless. Some games won’t run at all, and others might need extra tweaks to get working.

If most of your games are on Steam, you’re in a good spot. **Valve’s Proton** lets you run a large number of Windows games on Linux without much hassle. For games from other platforms (like Epic or Ubisoft), support can be inconsistent. That’s where tools like **Lutris** and **Heroic Launcher** come in , they make things easier, though not everything will work perfectly.

Multiplayer games that use kernel-level anti-cheat (like **Easy Anti-Cheat** or **BattlEye**) often don’t run on Linux. This is by design, as anti-cheat programs need deep system access and are built specifically for Windows, so unfortunately many competitive titles won’t launch or let you connect online on Linux.
Anti cheat makers could add Linux support, but most don’t , mainly because the Linux player base is small, security is tricky to handle properly, and honestly, it’s not a priority for them.

{% hint style="warning" %}
Make sure your GPU drivers are installed and working before continuing.
{% endhint %}

## Compatibility:
Before diving in, check whether the game works on Linux using these sites:

- [**ProtonDB**](https://www.protondb.com):
List of Steam games and their compatibility with Proton (the compatibility layer for Windows).

- [**Are We Anti-Cheat Yet?**](https://areweanticheatyet.com):
A list of games that use anti-cheat and their playability status on Linux

If your favorite games aren’t compatible, you have two options:
- **Dual-boot Linux with Windows**
- **Stick with Windows** if gaming is a major priority

{% hint style="tip" %}
Check ProtonDB for useful launch options or configuration tweaks. The community often shares helpful info for getting specific games working.
{% endhint %}

## Steam Games:

### Steam Installation:
Steam is easy to install and available through both Flatpak and package managers.

**Flatpak (Recommended for most users):**
```bash
flatpak install flathub com.valvesoftware.Steam
```

**Package Manager Install:**
he package name for Steam is `steam`; it’s the same across all distributions, so you can simply install it using your package manager.

{% hint style="ifno" %}
On Arch, you will need to enable the `multilib` repository to install Steam, while on Fedora, you will need to enable the `RPM Fusion` repository.
{% endhint %}

### Enabling Proton Support:
Once Steam is installed and launched:
1. Open **Steam > Settings > Compatibility**
2. Enable **"Steam Play for all other titles"**
3. Set it to use **Proton Experimental**

![](/Images/Steam/Compatibility.png)

Enabling Proton allows you to run the game using Proton (the compatibility layer for Windows), which can make most games playable. Be sure to check the compatibility first.


{% hint style="info" %}
If a game has a native Linux port, you should use that instead of running it through Proton, as the native version usually performs better. If the native version doesn’t work well for you, then you can try running it through Proton.
{% endhint %}

### Adding Non-Steam Games
To manage other games through Steam:
1. Go to **Games > Add a Non-Steam Game to My Library**
2. Browse to the game’s executable and add it

This enables features like controller support and Steam Overlay.

{% hint style="tip" %}
You can also enable Proton for these non-Steam games. Just right-click the game > Properties > Compatibility.

![](/Images/Steam/Compatibility(Non-Steam).png)
{% endhint %}

## Non-Steam Games:

### Lutris:
**Lutris** is an open-source game launcher that supports a wide range of platforms, including:
- GOG
- Epic Games Store
- Ubisoft Connect
- Battle.net
- Emulators (RetroArch, Dolphin, etc.)

It helps organize and launch both native and Windows-based games from one place.

### Lutris Installation:
**Via Flatpak:**
```bash
flatpak install flathub net.lutris.Lutris
```

**Via Package Manager:**
The package name for Lutris is `lutris` on all distributions.

### Running Games in Lutris:
After installing:
- Log into your accounts (Epic, GOG, etc.) directly in Lutris
- Your game libraries should appear if supported

To manually install a Windows game:
1. Click **+** > **Install a Windows game from an executable**
2. Enter a name for the game.
4. If needed, Lutris will prompt to install Wine.
5. Browse to the `.exe` file and continue the setup.

{% hint style="tip" %}
Not every game is listed in the app, but many have community installers available at [lutris.net](https://lutris.net).
{% endhint %}
