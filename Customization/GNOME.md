# GNOME Customization Guide

This guide covers the basics of GNOME customization, including extensions, the shell theme, and icon and cursor themes.

{% stepper %}
{% step %}

## 1. Extensions

Extensions add extra features to GNOME and are one of the easiest ways to customize your experience.

To install and manage extensions, install the Extension Manager (recommended Flatpak version):

```bash
flatpak install flathub com.mattjakeman.ExtensionManager
```

Launch **Extension Manager** after installation and install the extension that you want.

![](https://github.com/user-attachments/assets/9258d456-413d-4eaa-8333-dffc1c9cfb3a)


### Recommended Extensions:

1. [**Places Status Indicator**](https://extensions.gnome.org/extension/8/places-status-indicator/)
 
2. [**Apps Menu**](https://extensions.gnome.org/extension/6/applications-menu/)

3. [**User Themes**](https://extensions.gnome.org/extension/19/user-themes/) 

4. [**AppIndicator and KStatusNotifierItem Support**](https://extensions.gnome.org/extension/615/appindicator-support/) 

5. [**Dash to Dock**](https://extensions.gnome.org/extension/307/dash-to-dock/) 

6. [**Blur My Shell**](https://extensions.gnome.org/extension/3193/blur-my-shell/)

7. [**Caffeine**](https://extensions.gnome.org/extension/517/caffeine/)

8. [**Clipboard Indicator**](https://extensions.gnome.org/extension/779/clipboard-indicator/) 

9. [**Vitals**](https://extensions.gnome.org/extension/1460/vitals/) 

10. [**Just Perfection**](https://extensions.gnome.org/extension/3843/just-perfection/)

{% endstep %}
{% step %}

## 2. Shell Themes

GNOME uses the GTK toolkit to draw interface elements. Shell themes change the appearance of GNOME Shell itself (top bar, overview, notifications).

If you like having titlebar button for minimize and maximize then open gnome tweaks and head over to the Windows section and Below the Titlebar Button turn on Maximize and minimize.

### Installing a Shell Theme

1. Visit: [gnome-look.org](https://www.gnome-look.org/browse?cat=135&ord=rating) and download a theme you like.

2. Extract the theme, then open the theme folder. Its contents should look like this :

![](https://github.com/user-attachments/assets/7102e86f-8ffc-428a-9559-99aef7431a52)

3. Copy the theme folder into the .themes folder in your home directory. If the `.themes` folder doesn’t exist, create it. Press Ctrl+H to show hidden files, then copy your theme into .themes. (Make sure the theme folder isn’t nested inside any subfolders, or it won’t be detected.)

4. Now open gnome-tweaks and go to the Appearance tab. Select your downloaded theme for both Shell and Legacy Applications. (If the Shell option is greyed out, make sure the User Themes extension is installed, then relaunch gnome-tweaks.)

![](https://github.com/user-attachments/assets/99a51736-e18a-4819-a31b-54e1c09e1c3e)


### Additional Step for GTK4 Themes:
GTK 4 themes require an additional step to display properly:

1. Copy the assets folder from the theme into ~/.config/. The assets folder is usually inside the theme folder, but sometimes it may be located in the theme’s gtk-4.0 directory.

2. Copy `gtk.css` and `gtk-dark.css` from the theme’s `gtk-4.0` folder into `~/.config/gtk-4.0/` (create the folder if it doesn’t exist).

{% endstep %}
{% step %}

## 3. Icon Themes

Icon themes change the look of system and app icons.

### Installing an Icon Theme:

1. Download an icon from: [https://www.gnome-look.org/browse?cat=132&ord=rating](https://www.gnome-look.org/browse?cat=132&ord=rating)

2. Extract it, then copy the folder into the .icons directory inside your home folder (if the folder doesn’t exist, create it).

3. Open `gnome-tweaks`, go to the Appearance section, and select your downloaded theme under “Icons.”

![](https://github.com/user-attachments/assets/f5be3785-b48d-47c1-ace9-b1c908ef6b07)

{% endstep %}
{% step %}

## 4. Cursor Themes

Cursor themes are installed the same way as icon themes.

### Installing a Cursor Theme:

1. Download a cursor pack from gnome-look.org.
2. Extract it and copy the cursor theme into the `.icons` folder.
3. Open GNOME Tweaks, go to the Appearance tab, and select the cursor theme to apply it.

<details>
 <summary>Examples:</summary>
![](https://github.com/user-attachments/assets/d62cf758-edd9-4a6f-bf63-4850f89efde9)

</details>

{% endstep %}
{% endsteppers %}
