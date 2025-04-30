# ArchLinux Documentation
by: `Adrian Noces`

## ArchLinux Installation Guide

This is a step-by-step guide to installing Arch Linux with options for both encrypted and unencrypted setups. It covers disk partitioning, LVM setup, filesystem formatting, mounting, base system installation, user creation, desktop environments, driver installation, and final system configuration.

### 📌 links
- [Encrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/encrypted%20arch.md)
- [Unencrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/unencrypted%20arch.md)

## pacman and yay flags
```txt
-S    --install
-Ss   --Search pkg
-Qs   --search installed pakage
-Sy   --sync db
-Syu  --system fill upgrade
-Rns  --wipe clean
-Rs   --remove and clean pakage
-R    --remove pakage
-Sc   --remove old unused cache
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
```txt
sudo dd if=/path/to/your.iso of=/dev/sdX bs=4M status=progress conv=fsync

-if  --your iso file
-of  --your storage device e.g. USB flashdrive
-bs  --block size
-status=progress    --print status in progress
-conv=fsync         --makes sure everything is written right in the usb
```

