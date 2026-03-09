# Create an Ubuntu USB bootable using Syslinux tool

This guide demonstrates how to create a **bootable Ubuntu USB drive using the Syslinux bootloader**.  

This method is particularly useful for systems that require **Legacy BIOS booting**, such as the **HP ProLiant Microserver Gen8**.

Syslinux is a lightweight bootloader designed for Linux systems that boot from **FAT filesystems**, making it ideal for USB-based installers.

---

# Step 1 - Prepare a Linux Environment

## Action
Install **Ubuntu Server LTS** or any Linux distribution on a test machine/laptop. 

## Why is this step required?
A Linux environment is required because the tool used in the guide (such as `syslinux`, `parted`, `wipefs` and `dd`) are native linux utilities. 

These tools allowed me to:

- Prepare and partition the USB drive.
- Install the syslinux bootloader.
- Mount and copy the Ubuntu ISO contents.

---

# Step 2 - Identify the USB

## Action
Insert USB and run the following command in terminal:
```bash
lsblk
```
This command lists all the blocked devices (such as USB, Nvme, Solid State Drive, Hard Disk Drive) connected to the laptop. 

Locate the USB (for example: /dev/sda).

## Why is this step required?
Before modifying any storage device, we must correctly identify the USB. 

`lsblk` shows:
- Devices
- Disk sizes
- Partitions
- Mount points

**Warning**: Selecting the wrong disk could result in permenant data loss. 

---

# Step 3 - Wipe and Format the USB Drive

This step prepares the USB with a clean partition table and FAT32 filesystem, which syslinux requires. 

---

## Umount the USB

```bash
sudo umount /dev/sda
```

### Why is this step required?
Ensures the device is not actively being used on the system before making modifications. 

---

## Remove existing Filesystem signatures

```bash
sudo wipefs -a /dev/sda
```

## Why is this step required?

Removes existing:
- Filesystem signatures
- RAID metadata
- Old partition table informaton.

This ensures the disk starts at a complete clean state. 





- Step 3 - Format and wipe the USB drive. 
  - sudo umount /dev/sda
  - sudo wipefs -a /dev/sda (Erase file system, RAID and partition table signatures. -a means it's all)
  - sudo parted -s /dev/sda mklabel msdos (It destroys all the existing partitons and replaces it with an empty MBR partition table)
  - sudo parted -s /dev/sda mkpart primary fat32 1MiB 100% (it creates one partition on the USB drive where it starts at 1MiB and finishes at the end of the disk, it's marked as primary, the filesystem is   FAT32).  
  - sudo parted -s /dev/sda set 1 boot on (This command marks partition 1 as bootable in MBR Partition table and sets the active/boot flag known as the bootable flag). 
  - sudo mkfs.vfat -F32 -n UBUNTU2404 /dev/sda1 (It erases all the data in partition 1 and creates a FAT32 filesystem structures, file allocation table, root directory and boot sector (FAT Boot record)).

- Step 4 - Install Syslinux bootloader
  
  - Sudo syslinux -i /dev/sda1 (writes boot code into partition boot sector (PBR) and creates a file called ldlinux.sys and it marks the file as hidden. It then prepares the partition to load syslinux.cfg. The partition can now boot using sysliunux.cfg.
    
  - sudo dd if=/usr/lib/syslinux/mbr/mbr.bin of=/dev/sda bs=440 count=1 conv=notrunc - it installs syslinux-compatiable MBR code into the first 440 bytes of disk. It doesn't delete the partitions, modify the partitions table, erase data. 

Step 5 - Mount USB and ISO

mkdir -p /tmp/usb /tmp/iso - it creates empty folders as mount points.

sudo mount /dev/sda1 /tmp/usb - It makes /dev/sda1 accessible at /tmp/usb/. After running the command, files are stored at on USB partition becomes visible at /tmp/usb directory. Anything written into /tmp/usb goes to the USB. 

sudo mount -o loop ubuntu-24.04-live-server-amd.iso /tmp/iso - This command mounts an ISO disk image so you can browse contents like a normal folder. After running this command, it shows the files that you see if you inserted a Ubuntu installer DVD. 

Step 5 - Copy ISO files to USB

Sudo cp -a /tmp/iso/. /tmp/usb - It copies and pastes files and contents from /tmp/iso folder to the USB. 
Sync - forces linux to write any buffered filesystem data to the disk immediately. Why it matters is because linux normally buffers writes in memory for performance. If you unplug the USB too soon, the data might still be in the RAM. Running the sync means all the files are fully copied to the USB device and it's safe to remove after unmounting. 

Step 6 - Create syslinux config

Sudo nano /tmp/usb/syslinux.cfg - creating a syslinux config

DEFAULT ubuntu
PROMPT 0
TIMEOUT 50

LABEL ubuntu
  KERNEL /casper/vmlinuz
  APPEND initrd=/casper/initrd quiet ---

The enteries casper/vmlinuz and casper/initrd in syslinux.cfg tells the boot loader which kernel and initial RAM disk to load when booting Ubuntu installer/live environment. 

/casper/vmlinuz - vmlinuz is the linux kernel file and KERNEL tells Syslinux which kernel to load into memory. This line means load the linux kernel located at /casper/vmlinz. Kernel is the core of operating system it manages hardware, memory and processes and etc. 

APPEND initrd=/casper/initrd quiet ---

It passes the boot parameters to the kernel. This tells the kernel to load an initial RAM disk. Ignited (initial RAM disk) contains temporary root filesystem. It loads drivers and scripts needed early in boot. Without it, kernel wouldn't know how to find the real filesystem inside the ISO. 

Why Casper directory?

Casper is ubuntu's live boot system used by live cd and installers. It's responsible for detecting the boot media, mounting the compressed file system, creating live environment for RAM, starting the installer or live session. 
The boot process:
1. Syslinux loads vmlinuz (kernel)
2. Kernel loads initrd
3. Initrd runs casper scripts
4. Casper mounts filesystem.squashfs
5. Ubuntu live/installer environment starts. 


sudo cp /usr/lib/syslinux/modules/bios/ldlinux.c32 /tmp/usb/
sudo cp /usr/lib/syslinux/modules/bios/libcom32.c32 /tmp/usb/
sudo cp /usr/lib/syslinux/modules/bios/libutil.c32 /tmp/usb/

Why the syslinux modules need to be copied is because syslinux needs its runtime modules on the boot media in order to start properly. In addition, when the BIOS boots from USB:

- BIOS loads the syslinux boot sector. 
- Boot sector loads ldlinux.c32
- ldlinux.c32 loads libraries and configuration
- syslinux.cfg tells to load the Linux kernel

Sudo umount /tmp/iso
Sudo umount /tmp/usb 
