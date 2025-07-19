# ArchLinux Documentation
by: `Adrian Noces`

A simple documentation of ArchLinux by Adrian Noces — crafted for personal learning, experimentation, and mastery of the system. This documentation serves as a personal lab notebook, capturing commands, configs, and concepts. While made for his own use, those who understand are welcome to explore, learn, and build from it.

#### Navigation
- [Arch Installation Guide](#archlinux-installation-guide)
- [User management](#user-management)
- [Linux](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/LinuxSystem.md ) 
- [pacman and yay flags](#pacman-and-yay-flags)
- [Enable yay](#enable-yay)
- [Wireless Network issue](#unable-to-connect-to-network-after-booting-installed-arch)
- [Flash an iso file](#flash-iso-file-using-dd)
- [Useful files to configure](#useful-files-to-configure)
- [Generate keyring](#generate-keyring)
- [Creates a Linux command](#linux-cmd-creation)
- [icon and theme](#icon-and-theme) 

---

## ArchLinux Installation Guide

This is a step-by-step guide to installing Arch Linux with options for both encrypted and unencrypted setups. It covers disk partitioning, LVM setup, filesystem formatting, mounting, base system installation, user creation, desktop environments, driver installation, and final system configuration.

#### links
- [Encrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/encrypted%20arch.md)
- [Unencrypted Arch Install](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/unencrypted%20arch.md)

---

## Pacman and Yay flags
```bash
Usage: pacman or yay [OPTIONS...] <package>...

[OPTIONS]
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

---

## Enable `yay`
```bash
sudo pacman -S --needed base-devel git
sudo git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

---

## Useful files to configure
```bash
Usage: nano /Path/to/file

⚙️ System Permissions & Users

/etc/sudoers → Who can use sudo and how.
/etc/passwd → All user account info.
/etc/shadow → Encrypted user passwords.
/etc/group → User groups and their members.
/etc/login.defs → Default settings for user logins.

🌐 Networking & DNS

/etc/resolv.conf → DNS settings.
/etc/hosts → Maps hostnames to IP addresses (local DNS).
/etc/hostname → Your systems name on the network.
/etc/network/interfaces → Manual network config (Debian-based).
/etc/netplan/*.yaml → Network settings (newer Ubuntu).
/etc/ssh/sshd_config → SSH server config.
/etc/hosts.allow & /etc/hosts.deny → TCP wrappers for access control.

🧠 System Brain & Boot

/etc/fstab → Defines how drives mount at boot.
/etc/init.d/ → Old-style startup scripts.
/etc/systemd/ → Systemd service files (modern boot management).
/etc/default/grub → Bootloader (GRUB) config.
/boot/grub/grub.cfg → Auto-generated GRUB config.

🧠🧠 Package & Service Managers

/etc/pacman.conf → Pacman config (Arch-based distros).
/etc/apt/sources.list → APT repos (Debian/Ubuntu).
/etc/systemd/system/ → Custom systemd services.

🧠🔐 Security & Firewalls

/etc/ufw/ufw.conf → UFW firewall settings.
/etc/fail2ban/ → Intrusion prevention settings.
/etc/audit/auditd.conf → Linux audit framework.

💻 Desktop & Shell

/etc/profile → System-wide shell settings.
/etc/bash.bashrc → System-wide bashrc.
/etc/environment → Global environment variables.
/etc/X11/xorg.conf → X11 (display server) config.

dotfiles
~/.config

~/.bashrc
~/.zshrc

~/.xinitrc
~/.xsession

~/.profile
~/.bash_profile

~/.vimrc
~/.config/nvim/init.vim
~/.config/nano/nanorc

~/.gitconfig

🧠🕳️ Bonus: Black Hole of Power

/proc/ → Virtual files that show real-time kernel data (like /proc/cpuinfo, /proc/meminfo).
/sys/ → Kernel interface to hardware devices.
```

---

## Flash iso file using `dd`
```bash
sudo dd if=/path/to/your.iso of=/dev/sdX bs=4M status=progress conv=fsync
```

#### Options Breakdown:
| Flag              | Meaning                                                               |
| ----------------- | --------------------------------------------------------------------- |
| `if=`             | **Input file** – the ISO you want to burn (e.g. `if=linux.iso`)       |
| `of=`             | **Output file** – your target drive (e.g. `of=/dev/sdX`, like USB)    |
| `bs=4M`           | **Block size** – read/write 4MB at a time (faster & safer)            |
| `status=progress` | Show real-time progress info while writing                            |
| `conv=fsync`      | Force write buffer to disk before finishing – ensures full data write |

---

## User Management
#### user Creation
```bash
// add user only
useradd -m -g users username

// add secondary admin
useradd -m -g users -G sudo adminName

// add super admin
useradd -m -g users -G sudo,wheel adminName
```
#### user Deletion
```bash
// kill user processes
sudo pkill -u username,adminName

// delete user
sudo userdel -r username,adminName
```
#### Configure a user
```bash
Usage: sudo usermod [OPTIONS] username,adminName

[OPTIONS]
-aG,  --add-user-to-group  append the user to a specified group
Usage: sudo usermod -aG sudo <username>

-L,   --lock-user
Usage: sudo usermod -L <username>

-U,   --unlock-user
Usage: sudo usermod -U <username>

-l,   --rename-user
Usage: sudo usermod -l <prevname> <newname>

-d,   --remove-to-suplemmentary-group
Usage: sudo gpasswd -d <username> <group>
```
#### change user or admin passwd
```bash
[OPTIONS]
--change-user-passwd,  change user or admin password
Usage: passwd <username>

--change-root-password
Usage: passwd
```

---

## Unable to connect to network after booting installed arch
```bash
wpa_passphrase "SSID" "PASSWD" > /etc/wpa_supplicant/wpa.conf

wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa.conf

dhcpcd wlan0

sudo systemctl enable dhcpcd@wlan0.service
```

---

## Generate Keyring
```bash
pacman-key --init
pacman-key --populate archlinux
pacman -Sy archlinux-keyring
```

---

## Linux CMD CREATION
```bash
Creates cmd with c or any
compile it
move it to the /usr/local/bin
now you have working global cmd
```

---

## icon and theme
```bash
yay -S tela-icon-theme papirus-icon-theme
yay -S qogir-gtk-theme nordic-theme arc-gtk-theme
```
