# ArchLinux on a MacBook
My personal guide for installing Arch Linux on a MacBook using a usb drive.

\* *tested on the following models:*
- MacBook Pro (Retina, 13-inch, Mid 2014)

##### Reminders
- This guide will setup a single boot Arch Linux setup
- Use at your own risk

### Prepare image

1. Grab latest build from `https://www.archlinux.org/download/`
2. Build installation usb stick.

    ```
    # macOS

    $ diskutil list # in order to get disk number (diskX)
    $ diskutil unmountDisk /dev/diskX
    $ sudo dd if=path/to/arch.iso of=/dev/rdiskX bs=1m
    ```

### Installation

1. Boot into laptop while holding **alt**.
2. Check if we are in EFI mode.

    ```
    $ efivar -l
    ```

3. Partition drive into three.

    ```
    $ parted /dev/sda
    (parted) mklabel gpt
    (parted) mkpart ESP fat32 1049kB 538MB
    (parted) set 1 boot on
    (parted) mkpart primary ext4 538MB 113GB
    (parted) mkpart primary linux-swap 113GB 100%
    (parted) quit
    $ mkfs.vfat -F32 /dev/sda1
    $ mkfs.ext4 /dev/sda2
    $ mkswap /dev/sda3
    $ swapon /dev/sda3
    ```

4. Install Arch Linux files

    ```
    $ mount /dev/sda2 /mnt
    $ mkdir -p /mnt/boot
    $ mount /dev/sda1 /mnt/boot
    $ pacstrap /mnt base base-devel
    ```

5. Generate fstab

    ```
    $ genfstab -U -p /mnt >> /mnt/etc/fstab

    # swap ends with 0
    # boot ends with 1
    # ext4 ends with 2 and has options rw,relatime,data=ordered,discarded
    $ sudo vim /mnt/etc/fstab
    ```

6. Configuration

    ```
    $ arch-chroot /mnt

    # set locale
    # uncomment en_US.UTF-8 UTF-8
    $ vim /etc/locale.gen

    # generate locale
    $ locale-gen
    $ echo LANG=en_US.UTF-8 > /etc/locale.conf
    $ export LANG=en_US.UTF-8

    # set preferred timezone
    $ ln -s /usr/share/zoneinfo/Asia/Hong_Kong /etc/localtime
    $ hwclock --systohc --utc
    ```

7. Enable some kernel modules specifically for macbook

    ```
    # add coretemp and applesmc
    $ vim /etc/modules
    $ echo archlinux-on-mbp > /etc/hostname

    $ append hostname to end of lines
    $ vim /etc/hosts
    ```

8. Temporary connection

    ```
    $ pacman -S dhcpcd
    $ sudo systemctl enable dhcpcd
    ```

8. Set admin password

    ```
    $ passwd
    ```


### Post Installation
