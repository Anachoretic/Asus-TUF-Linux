---
title: Performance Monitoring
icon: monitor-waveform
---

# Performance Monitoring:

Performance monitors are tools designed to track and analyze your system’s performance in real-time. They provide detailed insights into CPU usage, memory consumption, disk activity, network traffic, and more. With customizable options and clear graphical displays, performance monitors help you identify bottlenecks, troubleshoot issues, and optimize system performance effectively. Whether you’re a casual user or a system administrator, these tools are great for keeping your system running smoothly.

## 1. System Monitoring:

This is used to monitor normal system and resource usage, similar to the Task Manager on Windows. It lets you see all resource usage at a glance and ensures everything is working as it should.

### Mission Center

Mission Center is the closest thing to a 1:1 replacement for Task Manager on Linux, but with more features. For example, it can show your fan speed alongside all the usual stats.

Installation:
```bash
flatpak install flathub io.missioncenter.MissionCenter
```

Usage:   
Just open the app from your applications menu or run:
 ```bash
 flatpak run io.missioncenter.MissionCenter
 ```
 
### Btop
Btop is a terminal-based resource monitor showing usage and stats for processor, memory, disks, network, and running processes. Install the `btop` package using your distro's package manager.

Usage:   
Type btop in the terminal to launch the monitor or use the app.

## 2. Performance Overlay:
Overlay monitors show system stats directly on top of games or GPU-heavy apps, so you can track FPS, temperatures, CPU/GPU load, and more without leaving your game. This helps you spot performance issues right away.


### Mangohud
MangoHud is an overlay for Vulkan and OpenGL games that displays FPS, temps, CPU/GPU load, VRAM usage, and other useful stats. Since RivaTuner isn’t available on Linux, MangoHud is the go-to alternative , it looks and works similarly. To install MangoHud, install the `mangohud` package along with the `goverlay` package to customize what MangoHud displays.

### Usage:   

For Steam:
1. Right-click the game in your library.
2. Select Properties.
3. Go to Launch Options.
4. Add this command: `mangohud %command%`.

For Lutris:
1. Right-click the game.
2. Select Configure.
3. Go to the System options tab.
4. Check the box for Enable MangoHud.

For Herioic game laucher:
1. Select the game.
2. Go to the Game settings.
3. Toggle Enable MangoHud on.

Customizing the Overlay:   
Open Goverlay, it has a simple, intuitive UI organized into sections. You can easily navigate and enable the stats you want.
