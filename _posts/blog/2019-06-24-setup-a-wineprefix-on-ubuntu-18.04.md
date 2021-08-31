---
layout: post
categories: [Setup Guide]
tags: [Wine, Wineprefix, Ubuntu 18.04, Linux]
permalink: /blog/:year/:month/:title
featured-img: /images/features/shell-feature.png
---

Wine is great, fill a glass, sit back, and find another reason to not boot into Windows. Wine lets us run built-for-windows applications on Linux. Wineprefix's lets us setup different Wine configurations, do you need 32 or 64-bit Wine, or specific runtime libraries, or do you just want to separate Windows applications. There's dozens of different ways to get to the same goal, and this applies to Wine in Linux too. Here's one way to setup Wineprefix(s), install an application and create a shortcut for it on Ubuntu 18.04.

- Install Wine and Winetricks. You may or may not need Winetricks, Winetricks is the preferred way for installing redistributable runtime libraries like dotnet, vcrun, corefonts, rather than using `wine winecfg` GUI to load those extra windows things.

```bash
sudo apt install wine-stable winetricks
```

When working with long path names make things easier. Specify a bash variable for the working path.

```bash
prefix=$HOME/.wine/wineprefixes
```

If the path doesn't exist you may need to create it first. Tell mkdir to make directories and sub directories with -p. `-p, --parents     no error if existing, make parent directories as needed`  
```bash
mkdir -p $HOME/.wine/wineprefixes/
```

- Set the WINEPREFIX environment variable.
- Use `export` so child processes have the same WINEPREFIX path.
- Wine and Winetricks will use this specified Wineprefix for this session.
- The wine prefix name you choose should not already exist. Ill create a wineprefix named 'tools64'.
- Create a 64-bit Wineprefix and "start/restart" it.

```bash
export WINEPREFIX=$prefix/tools64
wine wineboot
```
- Or create a 32-bit wineprefix.
  - From [winehq](https://wiki.winehq.org/FAQ#How_do_I_create_a_32_bit_wineprefix_on_a_64_bit_system.3F) "...there are some significant bugs that prevent many 32 bit applications from working in a 64 bit wineprefix."

```bash
export WINEPREFIX=$prefix/tools32
WINEARCH=win32 wine wineboot
```
- Optional extra steps. On Ubuntu 18.04 and wine-stable is version 3, I get error messages for missing gecko and mono. If missing mono (recommended instead of dotnet) or text/ fonts are messed up.
  - For text/fonts try
  ```bash
  winetricks corefonts
  ```
  - For installing mono manually. Download [https://wiki.winehq.org/Mono](https://wiki.winehq.org/Mono) depending on your download location.
  ```bash
  wine msiexec /i $HOME/Downloads/wine-mono-4.9.0.msi
  ```

---

For the rest of the commands I'll be using the 64-bit Wineprefix to install [LTspice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html) for this example.

- Install the application. Depending where your application installer is.

```bash
wine $HOME/Downloads/LTspiceXVII.exe
```

- Create a desktop shortcut. LTspice creates a desktop shortcut for us, but not all applications do. If you need a desktop shortcut, here's an example.

```bash
nano ~/Desktop/shortcut.desktop
```
- Paste and edit. Changing application paths and names to your exe's path.

```
[Desktop Entry]
Name=LTspice XVII
Exec=env WINEPREFIX=/home/username/.wine/wineprefixes/tools64 wine "/home/username/.wine/wineprefixes/tools64/drive_c/Program Files/LTC/LTspiceXVII/XVIIx64.exe"
Type=Application
StartupNotify=true
Comment=Analog Devices's LTspice XVII
Path=/home/username/.wine/wineprefixes/tools64/dosdevices/c:/Program Files/LTC/LTspiceXVII
Icon=DB38_XVIIx64.0
StartupWMClass=xviix64.exe
```

- Make it executable

```bash
chmod +x ~/Desktop/shortcut.desktop
```

- After the application has been run once, an icon can be found in `.local/share/icons` or in a sub folder.
