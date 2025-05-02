# ArchLinux Documentation
by: `Adrian Noces`

## Navigation
- [Arch Installation](#archlinux-installation-guide)
- [pacman and yay flags](#pacman-and-yay-flags)
- [Enable yay](#enable-yay)
- [Wireless Network issue](#unable-to-connect-to-network-after-booting-installed-arch)
- [Flash an iso file](#flash-iso-file-using-dd)

## ArchLinux Installation Guide

This is a step-by-step guide to installing Arch Linux with options for both encrypted and unencrypted setups. It covers disk partitioning, LVM setup, filesystem formatting, mounting, base system installation, user creation, desktop environments, driver installation, and final system configuration.

### links
- [Encrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/encrypted%20arch.md)
- [Unencrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/unencrypted%20arch.md)

## pacman and yay flags
```txt
Usage: pacman or yay [OPTIONS...] <package>...

Options:
  -S,   --install                Install a package
  -Ss,  --search                 Search for packages in the repo
  -Qs,  --query-installed        Search installed packages
  -Sy,  --sync-db                Refresh the package database
  -Syu, --upgrade-system         Full system upgrade
  -R,   --remove                 Remove a package
  -Rs,  --remove-deps            Remove a package and unused dependencies
  -Rns, --remove-clean           Deep clean (remove config files too)
  -Sc,  --clean-cache            Remove old/unused cache
```

## Enable `yay`
```bash
sudo pacman -S --needed base-devel git
sudo git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

## Unable to connect to network after booting installed arch
```bash
wpa_passphrase "SSID" "PASSWD" > /etc/wpa_supplicant/wpa.conf
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa.conf
dhcpcd wlan0
sudo systemctl enable dhcpcd@wlan0.service
```

## Flash iso file using `dd`
```bash
sudo dd if=/path/to/your.iso of=/dev/sdX bs=4M status=progress conv=fsync
```

### Options Breakdown:
| Flag              | Meaning                                                               |
| ----------------- | --------------------------------------------------------------------- |
| `if=`             | **Input file** – the ISO you want to burn (e.g. `if=linux.iso`)       |
| `of=`             | **Output file** – your target drive (e.g. `of=/dev/sdX`, like USB)    |
| `bs=4M`           | **Block size** – read/write 4MB at a time (faster & safer)            |
| `status=progress` | Show real-time progress info while writing                            |
| `conv=fsync`      | Force write buffer to disk before finishing – ensures full data write |
