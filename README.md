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

## 2. Set System Time
Set the system time to your local timezone.

## 3. Partitioning the Disk
Partition the disk according to your requirements.

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

