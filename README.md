# Arch Linux Documentation

By: Adrian Noces

# Linux Installation Guide

Follow the steps below to install and configure your Linux system.

## Steps

1. [Network Connection Setup](#1-network-connection-setup)
2. [Set System Time](#2-set-system-time)
3. [Partitioning the Disk](#3-partitioning-the-disk)
4. [Reformat the Partitions](#4-reformat-the-partitions)
5. [Mount Partitions (Skip Boot Partition)](#5-mount-partitions-skip-boot-partition)
6. [Initialize PGP Keyring (For Legacy Devices)](#6-initialize-pgp-keyring-for-legacy-devices)
7. [Install Base System](#7-install-base-system)
8. [Generate fstab (Filesystem Table)](#8-generate-fstab-filesystem-table)
9. [Enter chroot Environment](#9-enter-chroot-environment)
10. [Set Root Password](#10-set-root-password)
11. [Set System Time (Again)](#11-set-system-time-again)
12. [Create User and Set Password](#12-create-user-and-set-password)
13. [Initialize PGP Keyring in chroot (For Legacy Devices)](#13-initialize-pgp-keyring-in-chroot-for-legacy-devices)
14. [Install Essential Packages](#14-install-essential-packages)
15. [Allow User Sudo Access](#15-allow-user-sudo-access)
16. [Install Desktop Environment (Optional)](#16-install-desktop-environment-optional)
17. [Install Linux Kernel and Drivers](#17-install-linux-kernel-and-drivers)
18. [Configure /etc/mkinitcpio.conf for Kernel](#18-configure-etc-mkinitcpioconf-for-kernel)
19. [Set Locale (Uncomment Country, Run locale-gen)](#19-set-locale-uncomment-country-run-locale-gen)
20. [Configure GRUB Bootloader (Add Hard Disk)](#20-configure-grub-bootloader-add-hard-disk)
21. [Mount Boot Partition](#21-mount-boot-partition)
22. [Install Bootloader](#22-install-bootloader)
23. [Final Configurations & Tweaks](#23-final-configurations-tweaks)
24. [Enable Necessary System Services](#24-enable-necessary-system-services)
25. [Unmount Partitions and Reboot](#25-unmount-partitions-and-reboot)

## 1. Network Connection Setup
Set up the network connection to proceed with downloading necessary packages.
## Connect to Network

To connect to the network, follow these steps:

1. Display available network interfaces:
    ```sh
    ip link
    ```

2. Enter the command line network manager:
    ```sh
    iwctl
    ```

3. Connect to the wireless network (replace `SSID` with your network name and `interface` to your wireless network interface name):
    ```sh
    station interface connect "SSID"
    ```

4. For Ethernet, the connection is automatic.

## Troubleshooting

If you face any issues with the network, you can use the following commands:

1. See available wireless networks:
    ```sh
    iw dev interface scan | grep SSID
    ```

2. Display the network block status:
    ```sh
    rfkill list
    ```

3. Unblock WiFi if it's blocked by `rfkill`:
    ```sh
    rfkill unblock wifi
    ```

4. Set the network interface up if it's down:
    ```sh
    ip link set interface up
    ```

5. Check the IP address and see if you are connected:
    ```sh
    ip addr
    ```

6. Verify if you are connected to the internet:
    ```sh
    ping 8.8.8.8
    ```

    Or check connectivity by pinging Google:
    ```sh
    ping google.com
    ```

7. Display network interface details, including MAC address and interface status:
    ```sh
    ip a
    ```


## 2. Set System Time
Set the system time to your local timezone.

1. Check the current time:

You can use the following command to check the current system time:
```sh
timedatectl
```

2. Set the correct timezone:

Arch uses timedatectl to manage the system time. First, set your timezone:
```sh
timedatectl set-timezone <Your/Timezone>
```
Replace <Your/Timezone> with your desired timezone (e.g., `Asia/Manila`).

3. Verify the timezone:

After setting the timezone, you can verify it by running:
```sh
timedatectl
```

4. Synchronize the system clock with NTP (Network Time Protocol):

Ensure that the system clock is synchronized. You can enable the NTP service:
```sh
timedatectl set-ntp true
```
5. verify:
```sh
timedatectl
```
## 3. Partitioning the Disk
Partition the disk according to your requirements.

### Check Storage Devices
```sh
lsblk  # Display storage devices (look for /sda, /nvme, etc.)
```
### Delete Existing Partitions (if needed)
```sh
parted /dev/sda
```
```sh
print #View partitions
```
```sh
rm 1 #to rm 4 Remove partitions
```
```sh
quit    #to quit parted
```
### Create Partitions with fdisk
```sh
fdisk /dev/sda
```
Inside fdisk:

`p` — Display existing partitions

`n` — Create new partitions:

sda1 → /efi → `+512M`

sda2 → /boot → `+512M`

sda3 → swap → `+6G` (or adjust)

sda4 → for LVM → press Enter for default

Set Partition Types
```sh
t  # Set type
3  # sda3
82 # Linux swap

t
4  # sda4
44 # LVM
w  # Write and exit
```

### Encrypt sda4 Before LVM Setup
```sh
cryptsetup luksFormat /dev/sda4
```
Type `YES`

Set a strong password

Open Encrypted Partition
```sh
cryptsetup open --type luks /dev/sda4 lvm
```
### Configuring sda4
```sh
pvcreate /dev/mapper/lvm
```
🔹 *Initializes the encrypted partition as a Physical Volume (PV) that LVM can use.*

---

```sh
vgcreate volgroup0 /dev/mapper/lvm
```
🔹 *Creates a Volume Group (VG) named `volgroup0` using the physical volume.*

---

```sh
lvcreate -L 30GB volgroup0 -n lv_root
```
🔹 *Creates a Logical Volume (LV) called `lv_root` with 30GB size inside `volgroup0` — this will be your root (`/`) filesystem.*

---

```sh
lvcreate -L 250GB volgroup0 -n lv_home
```
🔹 *Creates a Logical Volume for your `/home` directory with 250GB space (adjust size as needed).*

---

## Summary of LVM Roles:
| Volume      | Purpose     | Example Size |
|-------------|-------------|--------------|
| lv_root     | System root | 30GB+        |
| lv_home     | User files  | 250GB+        |

## 4. Reformat the Partitions
Reformat the partitions if necessary.

## 5. Mount Partitions (Skip Boot Partition)
Mount the partitions, but skip mounting the boot partition for now.

## 6. Initialize PGP Keyring (For Legacy Devices)
Initialize the PGP keyring, especially important for older devices.

## 7. Install Base System
Install the essential base system.

## 8. Generate fstab (Filesystem Table)
Generate the fstab file for proper mounting.

## 9. Enter chroot Environment
Use `arch-chroot` to change root to the newly installed system.

## 10. Set Root Password
Set a strong root password for the system.

## 11. Set System Time (Again)
Ensure the system time is correctly set in the chroot environment.

## 12. Create User and Set Password
Create a non-root user and assign a password.

## 13. Initialize PGP Keyring in chroot (For Legacy Devices)
Re-initialize the PGP keyring within the chroot environment if needed.

## 14. Install Essential Packages
Install the necessary packages required for your system.

## 15. Allow User Sudo Access
Grant sudo access to the user created earlier.

## 16. Install Desktop Environment (Optional)
Install a desktop environment if you want a GUI.

## 17. Install Linux Kernel and Drivers
Install the Linux kernel and any necessary drivers for your system.

## 18. Configure /etc/mkinitcpio.conf for Kernel
Edit and configure the `/etc/mkinitcpio.conf` file for your kernel.

## 19. Set Locale (Uncomment Country, Run locale-gen)
Configure the system's locale by uncommenting your country in the locale settings and running `locale-gen`.

## 20. Configure GRUB Bootloader (Add Hard Disk)
Configure the GRUB bootloader by adding your hard disk to the configuration.

## 21. Mount Boot Partition
Mount the boot partition to ensure it's properly set up.

## 22. Install Bootloader
Install the bootloader to ensure the system can boot properly.

## 23. Final Configurations & Tweaks
Apply any final tweaks and configurations as necessary.

## 24. Enable Necessary System Services
Enable the system services required for the system to function.

## 25. Unmount Partitions and Reboot
Unmount all partitions and reboot the system to start using it.

