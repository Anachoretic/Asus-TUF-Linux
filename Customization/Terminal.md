---
title: Terminal
icon: rectangle-terminal
---

# Fonts:
Fonts are what change how text looks. Start by downloading a font of your choice from [Nerd Fonts](https://www.nerdfonts.com/), and also download the “Symbols Nerd Font” because it is required by fastfetch and some other apps to display icons. Once you have both fonts, install them on your system.

After installing the fonts, you only need to enable the main Nerd Font you downloaded in your terminal, like Meslo, Fira Code, or any other Nerd Font you chose. You do not need to enable the Nerd Symbols font separately, as it will be used automatically. Depending on the terminal you use, you may need to edit the terminal configuration file or enable the font through the terminal settings in the GUI.

# Fastfetch:
Fastfetch is a CLI application that displays system information when run. It can be customized to show images or GIFs and to display only selected information.

To install Fastfetch, install the fastfetch package using your system’s package manager, then generate its configuration file with:
```bash
fastfetch --gen-configg
```

After it is generated, the configuration file is located in `~/.config/fastfetch/config.jsonc`.

To run Fastfetch automatically when your terminal starts, you can add fastfetch to the end of your shell configuration file (such as .bashrc or .zshrc).

```bash
echo "fastfetch" >> ~/.bashrc #For zsh replace bash with zsh.
```

{% hint style="info" %}
Type this command carefully. Using `>` instead of `>>` would overwrite your `.bashrc` file.
{% endhint %}

![Example](/Images/Miscellaneous/Fastfetch.png)

You can customize Fastfetch yourself by following the [Fastfetch wiki](https://github.com/fastfetch-cli/fastfetch/wiki/Configuration), or you can use someone else’s configuration.


{% hint style="info" %}
Not every terminal emulator supports displaying images or GIFs. Make sure your terminal supports these features before enabling image or GIF output in Fastfetch.
{% endhint %} 
