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

{% hint style="info" %}
Check ProtonDB for useful launch options or configuration tweaks. The community often shares helpful info for getting specific games working.
{% endhint %}

## Steam:
If you have all your games on Steam, you won’t have to do much to install and play them. You can simply enable Proton for most games, and they will be perfectly playable. As mentioned earlier, not all games will work though, especially those with anti cheat. Some are supported, while others are not.

Steam is easy to install and available through both Flatpak and package managers.

**Flatpak:**
```bash
flatpak install flathub com.valvesoftware.Steam
```

**Package Manager :**

The package name for Steam is `steam`; it’s the same across all distributions, so you can simply install it using your package manager.

{% hint style="info" %}
On Arch, you will need to enable the `multilib` repository to install Steam, while on Fedora, you will need to enable the `RPM Fusion` repository.
{% endhint %}

### Proton Support:
Once Steam is installed and updated, you will need to enable Proton for all games in order to play them. Go to Steam > Settings > Compatibility, enable Steam Play for all other titles, and set it to use Proton Experimental.

![](/Images/Steam/Compatibility.png)

{% hint style="info" %}
If a game has a native Linux port, you should use that instead of running it through Proton, as the native version usually performs better. If the native version doesn’t work well for you, then you can try running it through Proton.
{% endhint %}


### Adding Non-Steam Games to Steam:
Go to Games > Add a Non-Steam Game to My Library and browse to the game’s executable to add it. If the game is installed using Lutris or other launchers, you can usually play it through Steam without doing anything extra.

However, if you are trying to add an executable directly through Steam, you will also need to enable Proton for that specific game by going to Properties > Compatibility.

![](/Images/Steam/Compatibility(Non-Steam).png)

{% hint style="info" %}
You can enable Steam to start in Big Picture mode for a Steam Deck like experience, but you will occasionally need to exit to the desktop to perform updates.
{% endhint %}
