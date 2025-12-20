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


## Lutris

Lutris is a game manager that lets you install and manage games from different stores and platforms all in one place. It works with Steam, GOG, Epic Games Store, emulators, and even standalone Windows and Linux games, so you can keep all your games organized in a single library. You can also manage and launch your Steam games through Lutris, making everything much easier to access.

### Installation:
You can install it either through Flatpak or your distribution's package manager; it is available on all distributions.

```flatpak install net.lutris.Lutris```

The package name is `lutris` on all distributions, so you can install it using your distro's package manager. Make sure to also install `wine` to run Windows apps.

## Games:
Before proceeding, make sure to create a folder with any name you like inside your home directory to install the apps there. This isn’t mandatory, but it makes managing your apps and games much easier.

```bash
Desktop    Downloads  Music     Public     Videos      yay
Documents  **Games**      Pictures  Templates  Wallpapers
```
Here, the folder that will be used is called **Games**. You can name it anything, the name doesn’t matter, but make sure to remember it so you can select it later.

<details>
  <summary>Installing Games via Lutris:</summary>

First, open Lutris and let it update and download all the required packages. After installation, download `Wine-GE` from Settings > Updates within Lutris. Lutris will download `Wine-GE` along with `Proton-GE`, which will be required to run games.

![](/Images/Lutris/Lutris-Wine.png)

Now click on the `+` icon in the top left and select `Add locally installed game`. The installation steps for most games should be the same. In this case, I’m using a game I got from GOG as an offline installer as an example.


![](/Images/Lutris/Runner.png)

Begin by entering the name of the game and selecting the runner. For a Windows game, you should select wine. The other details aren’t important and can usually be left blank if you want to.

![](/Images/Lutris/Executable.png)

Select the executable installer for the game. In this case, it’s `setup_fallout_1.2_(27130).exe`. Choose the path to the installer and continue. Other parameters are usually not necessary, but you can enter them if needed.

![](/Images/Lutris/Wine-Version.png)

On the Runner tab, select `Proton-GE` as the runner. If it doesn’t work, you can choose a different runner such as `Wine-GE` or the system-installed `wine` version.

On the System Options tab, you can enable `MangoHud`, `Gamescope`, or other functions, but these aren’t really needed for the installer, so you can skip this step for the most part.

After entering all the details, click Save and the game should appear in Lutris. Double-click it to launch the game, and the installer should start afterward.

The installation steps might vary depending on the installer but the steps are largely the same.

![](/Images/Lutris/Installation-1.png)

Select the language for the installer.

![](/Images/Lutris/Installation-2.png)

You can leave the rest of the settings as they are, but for the installation location, browse and choose the folder you created earlier since this will be important for the next step. You can use the default location, but make sure you know where it is. Using a folder inside your home directory will be much easier to manage.

After selecting everything, just click Install to continue.

![](/Images/Lutris/Installation-3.png)

Wait for the installation to finish.

![](/Images/Lutris/Installation-4.png)

After the installation completes, exit the installer and open Lutris. Right-click on the game and select `Configure`.

![](/Images/Lutris/Rebind.png)

Now, in the game options, reselect the executable at the new location where the game was installed. It will usually be called something like `gamename.exe` or an abbreviation of it ending in `.exe`. Simply select the new executable and click Save.

That’s pretty much it. You can now just launch the game and play it normally through Lutris.

</details>
 
{% hint style="info" %}
If you need a specific version of `Proton-GE` or `Wine-GE`, you can install `ProtonUp-Qt` from Flatpak to manage different versions.
```bash
flatpak install flathub net.davidotek.pupgui2
```
{% endhint %}
