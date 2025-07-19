# ArchLinux Installation Guide
`by: Adrian Noces`

## 📌 Quick Navigation
- [Connecting to Internet](#network-and-time-config)
- [Disk Partition](#disk-partition)
- [Unencrypted Setup Guide](#unencrypted-setup)
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
| sda2     | 100%GB or just press Enter | 

```bash
p       # View current partitions
d       # Delete a partition
n       # Create a new one
t       # Change type (e.g., to 83 for Linux)
a       # Make bootable (optional)
w       # Write changes (NO UNDO 💀)
```

### UNENCRYPTED SETUP
```bash
// physical volume
pvcreate /dev/sda2

// volume group
vgcreate vg0 /dev/sda2

// logical volume
lvcreate -L 1GB vg0 -n lv_swap
lvcreate -L 50GB vg0 -n lv_root
lvcreate -L 250GB vg0 -n lv_home
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
```

### ACTIVATE LVM
```bash
modprobe dm_mod
vgchange -ay
```

### FILESYSTEM STRUCTURE
| partition | mount | size | format |
|------|------|------|--------|
| sda1 | /boot/EFI | +512M |	vfat32 |

| partition | lvm | mount | size | format |
|-----------|-----|-------|------|--------|
| sda2 |||||
||		 lvm ||||
|||				/swap |		+1GB |		[SWAP] | 
|||				/ |		+50GB |		ext4 |
|||				/home |		+250GB |		ext4 |

### REFORMAT PARTITION
```bash
mkfs.vfat -F 32 /dev/sda1
mkfs.ext4 /dev/vg0/lv_root
mkfs.ext4 /dev/vg0/lv_home
mkswap /dev/vg0/lv_swap
swapon /dev/vg0/lv_swap
```

### MOUNT PARTITION
```bash
mount /dev/vg0/lv_root /mnt

mkdir /mnt/home
mkdir -p /mnt/boot/EFI

mount /dev/sda1 /mnt/boot/EFI
mount /dev/vg0/lv_home /mnt/home
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
//add "defaults,noexec,nosuid,nodev" into /var/tmp nect to text "ext4"
nano /mnt/etc/fstab
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
pacman -S base-devel arch-install-scripts dosfstools efibootmgr lvm2 gdisk bluez bluez-utils gamemode 
```
### MORE
```bash
pacman -S nano vim neovim git curl openssh sudo procps-ng fastfetch
```
### MUST HAVE
```bash
pacman -S alacritty thunar firefox networkmanager unzip wireless_tools iw qbittorrent
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
pacman -S bumblebee nvidia nvidia-prime nvidia-lts nvidia-utils nvidia-settings mesa
```

### FINISHING 
```bash
nano /etc/mkinitcpio.conf
// Find HOOKS and append to the text "block" this -> lvm2 
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
systemctl enable NetworkManager iwd dhcpcd bluetooth bluetooth.service bumblebeed
systemctl start bumblebeed
exit
umount -a
reboot
```
