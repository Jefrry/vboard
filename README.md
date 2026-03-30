# vboard
*A virtual keyboard for Linux with Wayland support and extensive customization options.*


<img src="https://github.com/user-attachments/assets/66e9a879-c677-429f-bd11-503d10e63c2b" width="400">

## Overview
vboard is a lightweight, customizable virtual keyboard designed for Linux systems with Wayland support. It provides an on-screen keyboard solution that's especially useful for:

- Touchscreen devices without physical keyboards
- Systems with malfunctioning physical keyboards
- Accessibility needs
- Kiosk applications

The keyboard supports customizable colors, opacity settings, and can be easily modified to support different layouts.

## Features
- **Customizable appearance**: Change background color, text color, and opacity
- **Persistent settings**: Configuration is saved between sessions
- **Modifier key support**: Use Shift, Ctrl, Alt and Super keys
- **Hold for repetitive clicks**: Keep holding the mouse button to trigger repeated clicks
- **Compact interface**: Headerbar with minimal controls to save screen space
- **Always-on-top**: Stays above other windows for easy access
- **Russian layout support**: Automatically detects the active system layout on startup and displays Cyrillic or Latin labels accordingly. Switch layouts at any time with **Super + Space** on the virtual keyboard

### **1. Install Dependencies**
Install the required packages using your package manager:

**For Debian/Ubuntu-based distributions:**
```bash
sudo apt install python3-uinput steam-devices python3-gi gir1.2-gtk-3.0 gir1.2-gio-2.0
```

**For Fedora-based distributions:**
```bash
sudo dnf install python3-uinput steam-devices python3-gobject gtk3
```

**For arch-based distributions:**
```bash
yay -Syu python-uinput steam-devices python-gobject gtk3
```

> `python3-gi` (PyGObject) and GTK 3 are required for the UI. `gir1.2-gio-2.0` is needed for automatic system layout detection on startup (reads `org.gnome.desktop.input-sources` via GSettings).


### **2. Download vboard**  
Retrieve the latest version of `vboard.py` using `wget`:  
```bash
wget https://github.com/mdev588/vboard/releases/download/v1.21/vboard.py
```



### **3. Run**  

```bash
python3 vboard.py
```

### **4. Create shortcut (optional)**  

```bash
mkdir -p ~/.local/share/applications/
cat > ~/.local/share/applications/vboard.desktop <<EOF
[Desktop Entry]
Exec=bash -c 'python3 ~/vboard.py'
Icon=preferences-desktop-keyboard
Name=Vboard
Terminal=false
Type=Application
Categories=Utility
NoDisplay=false
EOF
```
Make shortcut executable
```
chmod +x ~/.local/share/applications/vboard.desktop
```
Now you should find it in menu insdie Utility section

### Usage
When launched, vboard presents a compact keyboard with a minimal interface. The keyboard includes:
- Standard QWERTY layout keys
- Arrow keys
- Modifier keys (Shift, Ctrl, Alt, Super)

#### Interface Controls
- ☰ (menu) - Toggle visibility of other interface controls
- **EN / RU** - Indicator showing the current keyboard layout
- + - Increase opacity
- - - Decrease opacity
- **Background dropdown** - Change the keyboard background color

#### Switching Keyboard Layout
vboard supports English (QWERTY) and Russian (ЙЦУКЕН) layouts. On startup it reads the active system input source and displays the matching labels automatically.

To switch layouts while using vboard, press **Super** then **Space** on the virtual keyboard. This toggles the displayed labels between Latin and Cyrillic and sends a `Super+Space` event to the system so the OS input source switches in sync.

> Layout detection on startup requires GNOME or any desktop that uses `org.gnome.desktop.input-sources` (GSettings). The layout switch via Super+Space works on any desktop that handles `Super+Space` as an input source switcher.

### Configuration
vboard saves its settings to ~/.config/vboard/settings.conf. This configuration file stores:
- Background color
- Opacity level
- Text color
You can manually edit this file or use the built-in interface controls to customize the appearance.

### Customizing Keyboard Layout
The keyboard layout is defined in the rows list in the source code. To modify the layout:
1. Download the source code
2. Locate the rows definition (around line 175)
3. Modify the key arrangement as needed
4. The format follows a nested list structure where each inner list represents a row of keys

## Troubleshooting
### 1. Error: 'no such device'
 Make sure uinput kernel module is loded with
```bash
sudo modprobe uinput
```

to make sure it auto load on boot create file with
```bash
echo 'uinput' | sudo tee /etc/modules-load.d/module-uinput.conf
```
---
### 2. Error: 'Permission Denied'
Reload udev rules with
```bash
sudo udevadm control --reload-rules && sudo udevadm trigger
```
---
### 3. Error: 'steam-devices package not found'.
- in Fedora make sure the RPM Fusion repository is enabled. You can follow the guide here:
https://rpmfusion.org/Configuration
- Others can follow steps in here https://github.com/mdev588/vboard/issues/8
## Contributing 
Contributions to vboard are welcome! Here are some ways you can help:

- Add support for more keyboard layouts
- Improve the UI
- Fix bugs or implement new features
- Improve documentation

Please make sure to test your changes before submitting a pull request.

## License
vboard is licensed under the GNU Lesser General Public License v2.1. See LICENSE.md for the full license text.

## Note

* English and Russian layouts are supported. Other system layouts will display English labels by default.

* Currently do not work correctly on wlroots based window managers.

