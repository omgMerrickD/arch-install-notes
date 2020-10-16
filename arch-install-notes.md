# Installing Arch Linux on a Lenovo X1 Carbon (6th Gen)

## Download and create bootable ISO
Arch website > Download ISO > Burn with ``Rufus`` or `dd`

## Setup wifi and test connectivity
```bash
$ iwctl

[iwd]: station wlan0 connect DNET       # Connect to SSID
[iwd]: exit                             # Exit

$ ping 1.1.1.1                          # Test connectivity
```

## Create partitions
```bash
$ cfdisk /dev/nvme0n1

# create 3 partitions as follows
/dev/nvme0n1p1 EFI System 512M from Start
/dev/nvme0n1p2 Linux Swap 8G from end of EFI
/dev/nvme0n1p3 Linux filesystem 100%FS from end of swap (root, home all in one)
```

## Format partitions
```bash
# format partitions
$ mkfs.vfat /dev/nvme0n1p1          # EFI must be formatted as vfat
$ mkfs.ext4 /dev/nvme0n1p3          # Use EXT4 for root/home
$ mkswap /dev/nvme0n1p2             # Create swap
$ swapon /dev/nvme0np2              # Enable swap
```

## Mount partitions
```bash
# mount disk partitions
$ mount /dev/nvme0n1p3 /mnt         # Mount root so /mnt becomes /
$ mkdir /mnt/boot                   # Create boot/EFI in root
$ mount /dev/nvme0n1p1 /mnt/boot    # Mount boot/EFI

```
## Install base system
```bash
# install base system
$ pacstrap /mnt base base-devel linux linux-firmware intel-ucode nano vim sudo emacs \
                netctl iwd dialog tldr man-db man-pages dhcpcd dhclient grub
```

## Generate partition table
```bash
# generate partition table
$ genfstab -U /mnt > /mnt/etc/fstab
```

## Login to the new install
```bash
# chroot into new arch install
$ arch-chroot /mnt
```

## Set timezone and sync hardware clock
```bash
# set timezone and sync clock
$ ln -sf /usr/share/zoneinfo/America/Los_Angeles /etc/localtime
$ hwclock --systohc
```

## Update locales
```bash
# Update locales
$ vim /etc/locale.gen
# Uncomment en_US.UTF-8 UTF-8
```
## Generate locales
```bash
# Generate locales
$ locale-gen
```

## Set default locale
```bash
# set default locale
$ echo "LANG=en_US.UTF-8 LC_COLLATE=C" > /etc/locale.conf
```

## Set hostname
```bash
# set hostname
$ echo "madarch" > /etc/hostname
```

## Set hosts files
```bash
# setup hosts file
$ vim /etc/hosts

# add the following
127.0.0.1    localhost
::1          localhost
127.0.1.1    madarch.localdomain     madarch # Change 127.0.1.1 to static if you want
```
## Set root password
```bash
# set root password
$ passwd
```

## TODO: Create user

## TODO: Configure sudo

## TODO: Install display manager/desktop environment/window manager

## TODO: Misc setup and configuration
