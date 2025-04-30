# ArchLinux Documentation
by: `Adrian Noces`

## ArchLinux Installation Guide

This is a step-by-step guide to installing Arch Linux with options for both encrypted and unencrypted setups. It covers disk partitioning, LVM setup, filesystem formatting, mounting, base system installation, user creation, desktop environments, driver installation, and final system configuration.

### 📌 links
- [Encrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/encrypted%20arch.md)
- [Unencrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/unencrypted%20arch.md)

## Enable `yay`
```bash
sudo pacman -S --needed base-devel git
sudo git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
