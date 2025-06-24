# PlasmaxHyprland
This is a concise guide to setting up a dual Hyprland and KDE Plasma environment on a ThinkPad T440 with a minimal Arch Linux install. The focus is on the Wayland setup, configuration tips, and troubleshooting **not** the base Arch install.

---

## 1. Post-Arch Install Fix: TPM2 Timeout

After the first boot, a persistent error may appear during shutdown or boot:

```
A stop job is running for /dev/tpmrm0
```

This is caused by the `systemd-tpm2` service attempting to access the TPM module on the ThinkPad. To disable it:

```bash
sudo systemctl mask systemd-tpm2.service
```

This prevents long hangs during shutdown or reboot.

---

## 2. Install KDE Plasma (Minimal)

Install the essential KDE Plasma components without a display manager:

```bash
sudo pacman -S xorg plasma-desktop konsole dolphin xdg-utils xdg-user-dirs
```

Ensure the system boots to TTY:

```bash
systemctl set-default multi-user.target
```

From TTY, manually start KDE Plasma:

```bash
startplasma-x11
```

---

## 3. Boot into Plasma and Review Changes

Once in Plasma:

- Use System Settings → Appearance to apply themes
- Fix contrast issues with light themes (e.g. unreadable text)
- Install extras like `ark` for Dolphin archive integration:

```bash
sudo pacman -S ark
```

---

## 4. Install Hyprland (AUR Version)

Install an AUR helper:

```bash
sudo pacman -S base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

Install Hyprland:

```bash
yay -S hyprland-git
```

Create default config:

```bash
mkdir -p ~/.config/hypr
cp /usr/share/hyprland/hyprland.conf ~/.config/hypr/hyprland.conf
```

Start Hyprland from TTY:

```bash
Hyprland
```

---

## 5. Configure Hyprland

Edit `~/.config/hypr/hyprland.conf` for keybindings:

```ini
bind = SUPER, SPACE, exec, kitty
bind = SUPER, Q, killactive
bind = SUPER_SHIFT, left, movewindow, l
bind = SUPER_SHIFT, right, movewindow, r
bind = SUPER_SHIFT, up, movewindow, u
bind = SUPER_SHIFT, down, movewindow, d
```

---

## 6. Wofi Application Launcher (Wayland)

Install Wofi for a minimal app launcher that works well with Hyprland:

```bash
sudo pacman -S wofi
```

Add to `hyprland.conf` (Launch Wofi with Super+R):

```ini
bind = SUPER, R, exec, wofi --show drun
```

---

## 7. Wallpaper with swww

Install:

```bash
yay -S swww
```

Add to `hyprland.conf`:

```ini
exec-once = swww-daemon
exec-once = swww img ~/Pictures/wallpapers/mywall.jpg --transition-type grow
```

Including both lines prevents "socket not found" errors.

---

## 8. Kitty Terminal Transparency

Set in `~/.config/kitty/kitty.conf`:

```conf
background_opacity 0.8
```

Confirm config file in use:

```bash
kitty --debug-config
```

---

## 9. Touchpad and Input Tweaks

Disable the feature that stops the touchpad while typing:

```bash
sudo libinput disable-while-typing=false
```

---

## 10. Display Troubleshooting

If resolution or monitor detection is broken, use:

```bash
xrandr
```

Ensure the display (usually `eDP-1`) is recognized.

---


---

## Wrap-Up

This setup provides a lightweight dual desktop environment with full manual control:

- **Plasma Desktop** – Installed without a display manager, launched manually from TTY.
- **Hyprland** – A modern Wayland compositor with tiling window management and transparent Kitty terminal.
- **Wofi** – Application launcher for Wayland environments.
- **swww** – For animated wallpaper setting in Hyprland.
- **Kitty** – GPU-accelerated terminal with transparency.
- **Optional Fixes** – TPM2 service masked, touchpad behavior tweaked, and color/theming adjustments in KDE.

Switch between environments by logging in via TTY and using either:

```bash
startplasma-x11    # KDE Plasma
Hyprland           # Hyprland
```

No login manager is used, allowing full control over the session startup.

---
