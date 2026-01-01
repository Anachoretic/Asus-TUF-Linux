---
title: GNOME
icon: g
---

# GNOME Customization:

{% stepper %}
{% step %}

# 1. Extensions:
Extensions add extra features to GNOME and are one of the easiest ways to customize your experience. They allow you to add functionality that isn’t present by default, such as a clipboard manager, app indicators, and more.
To install and manage extensions, install Extension Manager from Flatpak. It lets you browse, install, and manage extensions directly from the app, without needing a browser add-on like the default GNOME extension app.
```bash
flatpak install flathub com.mattjakeman.ExtensionManager
```

Launch **Extension Manager** after installation and install the extension that you want.

![](/Images/GNOME/Extension-Manager.png)

<details><summary> Recommended Extensions:</summary>

[**User Themes**](https://extensions.gnome.org/extension/19/user-themes/)
   - Adds support for custom GNOME Shell themes.

[**Dash to Dock**](https://extensions.gnome.org/extension/307/dash-to-dock/)
  - Adds a dock to the desktop.

[**Just Perfection**](https://extensions.gnome.org/extension/3843/just-perfection/)
  - Allows you to individually tweak elements of the top bar.

[**AppIndicator and KStatusNotifierItem Support**](https://extensions.gnome.org/extension/615/appindicator-support/) 
  - Adds support for AppIndicator, KStatusNotifierItem, and tray icons in GNOME Shell. 

[**Blur My Shell**](https://extensions.gnome.org/extension/3193/blur-my-shell/)
  - Adds a translucent blur effect to panels, menus, and the overview.

[**Caffeine**](https://extensions.gnome.org/extension/517/caffeine/)
  - Allows you to temporarily disable suspend or sleep for a set duration.

[**Clipboard Indicator**](https://extensions.gnome.org/extension/779/clipboard-indicator/) 
  - Adds a clipboard manager.

[**Vitals**](https://extensions.gnome.org/extension/1460/vitals/) 
  - Displays CPU, GPU, memory, and other system usage in the top bar.

[**Places Status Indicator**](https://extensions.gnome.org/extension/8/places-status-indicator/) and [**Apps Menu**](https://extensions.gnome.org/extension/6/applications-menu/)
  -  Adds a menu for Places and Applications.

[**DING**](https://extensions.gnome.org/extension/2087/desktop-icons-ng-ding/)
  - Adds support for desktop icons.
</details>

{% endstep %}
{% step %}

# 2. Shell Themes:
GNOME Shell themes let you customize the look of the shell interface itself, including the top bar, overview, notifications, and other system elements, independent of application styling.

{% hint style="info" %}
If you like having titlebar button for minimize and maximize then open gnome tweaks and head over to the Windows section and Below the Titlebar Button turn on Maximize and minimize.
{% endhint %}


## Installation:

1. Visit: [gnome-look.org](https://www.gnome-look.org/browse?cat=135&ord=rating) and download a theme you like.

2. Extract the theme, then open the theme folder. Its contents should look something like this :

![](/Images/GNOME/Index.png)

3. Copy the theme folder into the `.themes` folder in your home directory. If the `.themes` folder doesn’t exist, create it. Press <kbd>Ctrl+H</kbd> to show hidden files, then copy your theme into `.themes` folder. (Make sure the theme itself isn’t nested inside any subfolders, or it won’t be detected.)

4. Now open `gnome-tweaks` and go to the Appearance tab. Select your downloaded theme for both Shell and Legacy Applications. (If the Shell option is greyed out, make sure the User Themes extension is installed, then relaunch gnome-tweaks.)

![](/Images/GNOME/Styles.png)

{% hint style="warning" %}
GTK4 themes require an extra step to work correctly. If you skip it, some elements, such as titlebar buttons, may not display or function properly. To fix this, copy the theme’s `assets` folder, usually in the theme root or inside `gtk-4.0`, to `~/.config/`, then copy the `gtk.css` and `gtk-dark.css` files from the theme’s `gtk-4.0` folder to `~/.config/gtk-4.0/`. After this, reapply the theme and it should display correctly.
{% endhint %}

{% endstep %}
{% step %}

# 3. Icon Themes:

Icon themes change the look of system and app icons.

## Installation:

1. Download an icon from: [gnome-look.org](https://www.gnome-look.org/browse?cat=132&ord=rating)

2. Extract it, then copy the folder into the .icons directory inside your home folder (if the folder doesn’t exist, create it).

3. Open `gnome-tweaks`, go to the Appearance section, and select your downloaded theme under “Icons.”

![](https://github.com/user-attachments/assets/f5be3785-b48d-47c1-ace9-b1c908ef6b07)

{% endstep %}
{% step %}

## 4. Cursor Themes:

Cursor themes are installed the same way as icon themes.

### Installing a Cursor Theme:

1. Download a cursor pack from [gnome-look.org](https://www.gnome-look.org/browse?cat=107).

2. Extract it and copy the cursor theme into the `.icons` folder.

3. Open `gnome-tweaks`, go to the Appearance tab, and select the cursor theme to apply it.

{% endstep %}
{% step %}

<details>
<summary>Examples:</summary>

 ![](/Images/GNOME/Example-1.png)

- GTK theme: [Catppuccin](https://www.gnome-look.org/p/1715554)
- Icon theme: [Colloid](https://www.gnome-look.org/p/1356095)
- Cursor theme: [Volantes Cursor](https://www.gnome-look.org/p/1661983)

</details>

{% endstep %}
{% endsteppers %}
