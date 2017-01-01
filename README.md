# ArchLinux on a MacBook
A guide for installing Arch Linux on any* MacBook using a usb drive.

\* This has been tested on the following models:

### Prepare image

1. Grab latest build from `https://www.archlinux.org/download/`
2. Build installation usb stick.

    ```
    # macOS

    $ diskutil list
    $ diskutil unmountDisk /dev/diskX
    $ sudo dd if=path/to/arch.iso of=/dev/rdiskX bs=1m
    ```

### Installation

1. Boot

### Post Installation
