# ArchLinux Installation Guide
`by: Adrian Noces`

## 📌 Quick Navigation
- [Connecting to Internet](#network-and-time-config)
- [Disk Partition](#disk-partition)
- [Encrypted Setup Guide](#encrypt-partition-luks-format)
- [Reformatting](#reformat-partition)
- [Mounting Filesystem](#mount-partition)
- [System Install](#initialized-keyring)
- [Generate Filesystem Table](#generate-filesystem-table)
- [User Creation](#enter-chroot-and-setup-root-passwd-and-user)
- [Install Linux](#tools-installation)
- [Desktop Environment](#desktop-environment-installation)
- [Drivers](#install-drivers)
- [Finishing Touch](#finishing)

### NETWORK AND TIME CONFIG
```bash
// unblock wifi from rfkill
rfkill unblock all

// scan wifi networks
iw dev wlan0 scan | grep SSID

// enter iwd cmd
iwctl
// connect to the network
station wlan0 connect "HUAWEI-2.4G-D3Jf"
// to exit iwd cmd
exit

// set timezone
timedatectl set-timezone Asia/Manila
// synchronized time
timedatectl set-ntp true
// see time status
timedatectl
```

### Network Troubleshooting
if unable to connect to wifi when booted to your installed arch
```bash
wpa_passphrase "SSID" "PASSWD" > /etc/wpa_supplicant/wpa.conf

wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa.conf

dhcpcd wlan0

systemctl enable dhcpcd@wlan0.service
```

### DISK PARTITION
```bash
// delete all partition
sfdisk --delete /dev/sda

// enter to freshly make partition
fdisk /dev/sda
```

| Partition| size |
| ---------|------|
| sda1     | +512M |
| sda2     | +512M |
| sda3     | 100%GB or just press Enter | 

### Encrypt partition luks format
```bash
// ENCRYPT /dev/sda3
cryptsetup luksFormat /dev/sda3
// open it
cryptsetup open --type luks /dev/sda3 lvm
```

#### Encrypted setup
```bash
// physical volume
pvcreate /dev/mapper/lvm

// volume group
vgcreate volgroup0 /dev/mapper/lvm

// logical volume
lvcreate -L 6GB volgroup0 -n lv_swap
lvcreate -L 50GB volgroup0 -n lv_root
lvcreate -L 250GB volgroup0 -n lv_home
lvcreate -L 15GB volgroup0 -n lv_var
lvcreate -L 5GB volgroup0 -n lv_tmp
```

### Troubleshooting
```bash
// check Locations
pvdisplay
vgdisplay
lvdisplay
 
// to remove if made mistake
pvremove /dev/mapper/lvm
vgremove volgroup0 
lvremove /dev/volgroup0/lv_name

// close encrypted device
cryptsetup luksClose lvm
```

### ACTIVATE LVM
```bash
modprobe dm_mod
vgchange -ay
```

### FILESYSTEM STRUCTURE
| partition | mount | size | format |
|------|------|------|--------|
| sda1 | /boot | +512M |	vfat32 |

| partition | mount | size | format |
|------|-----------|-------|--------|
| sda2 | /boot/EFI | +512M |	ext4  |

| partition | lvm | mount | size | format |
|-----------|-----|-------|------|--------|
| sda3 |||||
||		 lvm ||||
|||				/swap |		+6GB |		[SWAP] | 
|||				/ |		+50GB |		ext4 |
|||				/home |		+250GB |		ext4 |
|||				/var |		+15GB |		ext4 |
|||				/tmp |	+5GB |		ext4 |

### REFORMAT PARTITION
```bash
mkfs.vfat -F 32 /dev/sda1
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/volgroup0/lv_root
mkfs.ext4 /dev/volgroup0/lv_home
mkfs.ext4 /dev/volgroup0/lv_var
mkfs.ext4 /dev/volgroup0/lv_tmp
mkswap /dev/volgroup0/lv_swap
swapon /dev/volgroup0/lv_swap
```

### MOUNT PARTITION
```bash
mount /dev/volgroup0/lv_root /mnt

mkdir /mnt/home
mkdir /mnt/var
mkdir /mnt/var/tmp
mkdir /mnt/boot

mount /dev/sda2 /mnt/boot
mount /dev/volgroup0/lv_home /mnt/home
mount /dev/volgroup0/lv_var  /mnt/var
mount /dev/volgroup0/lv_tmp  /mnt/var/tmp
```

### INITIALIZED KEYRING
```bash
pacman-key --init
pacman-key --populate archlinux
pacman -Sy archlinux-keyring
```

### INSTALL CORE SYSTEM
```bash
pacstrap -i /mnt base
```

### GENERATE FILESYSTEM TABLE
```bash
// generate
genfstab -U -p /mnt >> /mnt/etc/fstab
// Check
cat /mnt/etc/fstab
// Remove if mistake 
rm -rf /mnt/etc/fstab
```

### ENTER CHROOT AND SETUP ROOT PASSWD AND USER
```bash
// Enter chroot
arch-chroot /mnt
// change root password
passwd
// add normal user
useradd -m -g users adrian
// add admin user
groupadd sudo
useradd -m -g users -G sudo admin
// change user password 
passwd adrian
// change admin passwd
passwd admin
```
### troubleshooting
```bash
// terminate user running process
sudo pkill -u adrian
// delete user
sudo userdel -r adrian -> 
```

### TOOLS INSTALLATION
```bash
pacman -S base-devel dosfstools grub efibootmgr lvm2 gdisk bluez bluez-utils
```
### MORE
```bash
pacman -S mtools nano vim neovim git curl openssh os-prober sudo procps-ng fastfetch
```
### MUST HAVE
```bash
pacman -S alacritty dhcpcd thunar firefox iwd networkmanager unzip wireless_tools iw qbittorrent
```

### LINUX INSTALLATION
```bash
pacman -S linux linux-lts linux-headers linux-lts-headers linux-firmware
```

### DESKTOP ENVIRONMENT INSTALLATION
```bash
// CORE
pacman -S xorg xorg-xinit
```

```bash
GNOME
pacman -S gnome gnome-tweaks gdm
systemctl enable gdm

KDE-PLASMA
pacman -S plasma kde-systemsettings sddm
systemctl enable sddm

XFCE
pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter
systemctl enable lightdm

Cinnamon
pacman -S cinnamon nemo lightdm lightdm-gtk-greeter
systemctl enable lightdm

MATE
pacman -S mate mate-control-center lightdm lightdm-gtk-greeter
systemctl enable lightdm

LXQT
pacman -S lxqt lxqt-qt5 qterminal lightdm lightdm-gtk-greeter
systemctl enable lightdm

LXDE
pacman -S lxde leafpad lightdm lightdm-gtk-greeter
systemctl enable lightdm

DEEPIN
pacman -S deepin deepin-control-center lightdm lightdm-gtk-greeter
systemctl enable lightdm

BUDGIE
pacman -S budgie-desktop gnome-control-center lightdm lightdm-gtk-greeter
systemctl enable lightdm

PANTHEON
pacman -S pantheon switchboard lightdm lightdm-gtk-greeter
systemctl enable lightdm

ENLIGHTENMENT 
pacman -S enlightenment terminology lightdm lightdm-gtk-greeter
systemctl enable lightdm
```

### i3 + feh + wal + picom + rofi(optional) 
```bash
pacman -S i3  i3status i3blocks dmenu feh wal rofi picom
nano ~/.xinitrc

"
#!/bin/sh
feh --bg-scale ~/Pictures/wall.jpg &  # Set wallpaper
wal -i ~/Pictures/wall.jpg &          # Apply wal theme
picom --config ~/.config/picom/picom.conf &  # Start picom (custom config if you want)
exec i3  # Start i3
"

chmod +x ~/.xinitrc
// Start X manually
Just type:
startx
```

### INSTALL DRIVERS
```bash
// look for VGA and 3D controller
lspci
```

```bash
INTEL
pacman -S xf86-video-intel mesa intel-media-driver

AMD
pacman -S xf86-video-amdgpu mesa vulkan-radeon libva-mesa-driver
pacman -S xf86-video-ati -> older

NVIDIA
pacman -S nvidia nvidia-lts nvidia-utils nvidia-settings mesa
```

### FINISHING 
```bash
nano /etc/mkinitcpio.conf
// Find HOOKS and append to the text "block" this two -> encrypt lvm2 
mkinitcpio -p linux
mkinitcpio -p linux-lts
```

### Generate locale
```bash
nano /etc/locale.gen
Uncomment #en_ph utf 8
locale-gen
```

### configure /etc/sudoers file
```bash
nano /etc/sudoers
Uncomment #%sudo
```

### make grub know your encrypted device
```bash
//skip this if unencrypted setup
nano /etc/default/grub
find GRUB_CMDLINE_LINUX_DEFAULT
append before "quiet" -> cryptdevice=/dev/sda3:volgroup0
```

### BOOTLOADER FINAL TOUCH
```bash

mkdir /boot/EFI
mount /dev/sda1 /boot/EFI

grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck

cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
grub-mkconfig -o /boot/grub/grub.cfg
```

### finals
```bash
systemctl enable NetworkManager iwd dhcpcd bluetooth bluetooth.service
exit
umount -a
reboot
```
