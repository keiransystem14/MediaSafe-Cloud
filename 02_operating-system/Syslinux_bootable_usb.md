# Create an Ubuntu USB bootable using Syslinux tool

In this document, it demonstrates a step-by-step guide on how to create a bootable USB that can support Legacy BIOS requirements such as HP ProLiant Microserver Gen8 using Syslinux tool. 

Step 1 - Install Ubuntu Server LTS or any Linux flavour on a laptop that you don't even use. 
Step 2 - Plug in your USB and in terminal, type the command - lsblk. This command shows a list of information about the block devices such as Hard Drives, SSDs, USB drives and etc connected onto the hardware. I then try to locate the USB which is in /dev/sda/ directory. 
Step 3 - Format and wipe the USB drive. 
- sudo umount /dev/sda
- sudo wipefs -a /dev/sda (Erase file system, RAID and partition table signatures. -a means it's all)
- sudo parted -s /dev/sda mklabel msdos (It destroys all the existing partitons and replaces it with an empty MBR partition table)
- sudo parted -s /dev/sda mkpart primary fat32 1MiB 100% (it creates one partition on the USB drive where it starts at 1MiB and finishes at the end of the disk, it's marked as primary, the filesystem is FAT32). 
- sudo parted -s /dev/sda set 1 boot on (This command marks partition 1 as bootable in MBR Partition table and sets the active/boot flag known as the bootable flag). 
- sudo mkfs.vfat -F32 -n UBUNTU2404 /dev/sda1 (It erases all the data in partition 1 and creates a FAT32 filesystem structures, file allocation table, root directory and boot sector (FAT Boot record)).

Step 4 - Install Syslinux bootloader
