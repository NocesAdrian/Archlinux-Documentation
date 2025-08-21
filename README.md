# ArchLinux Documentation
by: `Adrian Noces`

A simple documentation of ArchLinux by Adrian Noces — crafted for personal learning, experimentation, and mastery of the system. This documentation serves as a personal lab notebook, capturing commands, configs, and concepts. While made for his own use, those who understand are welcome to explore, learn, and build from it.

#### Navigation
- [Arch Installation Guide](#archlinux-installation-guide)
- [User management](#user-management)
- [Linux](https://github.com/NocesAdrian/Archlinux-Documentation/blob/main/LinuxSystem.md ) 
- [pacman and yay flags](#pacman-and-yay-flags)
- [Enable yay](#enable-yay)
- [Enable sound](#enable-sound)
- [tells dhcpcd not to overwrite /etc/resolv.conf](#lock-dhcpcd)
- [Flash an iso file](#flash-iso-file-using-dd)
- [Useful files to configure](#useful-files-to-configure)
- [Generate keyring](#generate-keyring)
- [Creates a Linux command](#linux-cmd-creation)
- [icon and theme](#icon-and-theme) 
- [zram-generator](#zram-generator)
- [MarkDown CLI viewer](#md-cli-viewer)
- [Manage Printer](#printer-management)
- [install swaywm](#install-swaywm)
- [Optimize battery life](#Battery-Optimization)
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
git clone https://aur.archlinux.org/yay.git
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

## lock dhcpcd
```bash
sudo pacman -S openresolv
sudo nano /etc/dhcpcd.conf
```
```bash
nohook resolv.conf
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
yay -S tela-icon-theme papirus-icon-theme whitesur-icon-theme
yay -S qogir-gtk-theme nordic-theme arc-gtk-theme
```

## zram-generator
```bash
yay -S zram-generator
sudo mkdir -p /etc/systemd/zram-generator.conf.d
sudo nano /etc/systemd/zram-generator.conf.d/zram.conf
```
```bash
[zram0]
zram-size = ram / 4
compression-algorithm = zstd
```
```bash
sudo systemctl daemon-reexec
sudo systemctl restart systemd-zram-setup@zram0.service
```
## enable sound
```bash
sudo pacman -Syu pipewire pipewire-pulse pipewire-alsa wireplumber pavucontrol cava
systemctl --user enable pipewire pipewire-pulse wireplumber
systemctl --user start pipewire pipewire-pulse wireplumber
```

## md cli viewer
```bash
sudo pacman -S glow
glow markdown.md
```

## printer management
```bash
// download and enable printer manager
sudo pacman -S cups
sudo systemctl enable --now cups.service

// install printer drivers
yay -S epson-inkjet-printer-escpr
yay -S epson-inkjet-printer-escpr2
```
## install swaywm
```bash
sudo pacman -S sway swaybg swaylock swayidle \
             xorg-xwayland xorg-xlsclients \
             grim slurp wl-clipboard foot
yay -S swayfx
mkdir -p ~/.config/sway
cp /etc/sway/config ~/.config/sway/config
```
Breakdown:

    sway – the WM itself
    swayfx - sway but more customizable
    swaybg – wallpapers
    swaylock – lock screen
    swayidle – idle handling
    xorg-xwayland – runs old X11 apps on Wayland
    grim & slurp – screenshots
    wl-clipboard – clipboard management
    foot – terminal (Wayland native, fast)

### sway login manager
```bash
sudo pacman -S greetd greetd-tuigreet
sudo nano /etc/greetd/config.toml
```
```bash
[default_session]
command = "sway"
user = "angelica"
```
```bash
sudo systemctl enable greetd --now
```

## Battery Optimization
```bash
Lower your brightness
```
```bash
sudo nano /etc/tlp.conf
```
```bash
// start charge when hit 40%
START_CHARGE_THRESH_BAT0=40

// stop charge when hit 80%
STOP_CHARGE_THRESH_BAT0=80  

// dynamic freq
CPU_SCALING_GOVERNOR_ON_BAT=schedutil 

// turn off power surge when using battery or cpu boost
CPU_BOOST_ON_BAT=0 

// SSD/HDD uses less power when idle. 
DISK_APM_LEVEL_ON_BAT="128"

// Tiny battery savings when no audio is playing.
SOUND_POWER_SAVE_ON_BAT=1

// Makes sure your schedutil + boost settings are always applied automatically. 
RESTORE_DEVICE_STATE_ON_STARTUP=1

sudo systemctl restart tlp
```
