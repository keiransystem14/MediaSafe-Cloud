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

# Step 2 - Download Ubuntu Sever LTS ISO

```bash
wget https://releases.ubuntu.com/24.03/ubuntu-ubuntu-24.04-live-server-amd.iso
```

## Why is this step required?
- `wget` downloads the files directly from internet via terminal.
- The URL points to the official Ubuntu 24.03 LTS Server ISO.
- The file will be saved in your current directory. 

---

# Step 3 - Identify the USB

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

# Step 4 - Wipe and Format the USB Drive

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

### Why is this step required?
Removes existing:
- Filesystem signatures
- RAID metadata
- Old partition table informaton.

This ensures the disk starts at a complete clean state. 

---

## Create an MBR Partition Table

```bash
sudo parted -s /dev/sda mklabel msdos
```

### Why is this step required?
Legacy BIOS systems such as HP ProLiant Microserver Gen8 requires MBR (Master Boot Record) partition table. This command removes any existing partitions and creates a new empty MBR layout. 

---

## Mark the Partition as Bootable

```bash
sudo parted -s /dev/sda set 1 boot on
```

### Why is this step required?
Sets the boot flag in the Master Boot Record (MBR) partition table. Legacy BIOS checks this flag to determine which partition should be used for booting. 

---

## Format the partition as FAT32

```bash
sudo mkfs.vfat -F32 -n UBUNTU2404 /dev/sda1
```

### Why is this step required?
Syslinux requires a FAT filesystem. 

This command:
- Creates a FAT32 filesystem.
- Writes the filesystem structures.
- Creates the FAT boot record.

The label `UBUNTU2404` helps identify the USB mounted. 

---

# Step 5 – Mount the USB and Ubuntu ISO

Both the USB partition and Ubuntu ISO image must be mounted before copying the files. 

---

## Create Mount Directories

```bash
mkdir -p /tmp/iso /tmp/usb
```

### Why is this step required?
These directeries act as **mount points** where Linux attaches filesystems.

---

## Mount the USB partition

```bash
sudo mount /dev/sda1 /tmp/usb
```

### Why is this step required?
Makes the USB filesystem accessible at `/tmp/usb`. Anything written into this directory is directly written to the USB drive. 

---

## Mount the Ubuntu ISO

```bash
sudo mount -o loop ubuntu-24.04-live-server-amd.iso /tmp/iso
```

### Why is this step required?
The `-o loop` allows Linux to treat the ISO file like a virtual disk. This lets you browse the ISO contents like a DVD. 

---

# Step 6 - Copy Ubuntu Files to USB

```bash
sudo cp -a /tmp/iso/. /tmp/usb/
```

## Why is this step required?
This copies all the files from the Ubuntu ISO to the USB drive. 

The `-a` flag preserves:
- Permissions
- Symbolic links
- Directory structure

This effectively replicates the Ubuntu installer media. 

---

## Flush Filesystem Buffers

```bash
sync
```

## Why is this step required?

Linux buffers disk writes in memory for performance. Running `sync` forces all the buffer data to be written to disk. 

This ensures:
- Files are fully copied.
- The USB can be safely removed after unmounting.

---

# Step 7 - Create the Syslinux Configuration File

First open the syslinux.cfg file using nano text editor by entering the command below:

```bash
sudo nano /tmp/usb/syslinux.cfg
```
Add the following configuration:

```bash
DEFAULT ubuntu
PROMPT 0
TIMEOUT 50

LABEL ubuntu
  KERNEL /casper/vmlinuz
  APPEND initrd=/casper/initrd quiet ---
```
## Why is this step required?
The syslinux.cfg file tells the bootloader:
- Which kernel to load
- Which parameters to use

### Kernel Entry

```bash
KERNEL /casper/vmlinuz
```
`vmlinuz` is the Linux kernel. 

The kernel is responsible for:
- Hardware management
- Memory management
- Process control

Syslinux loads this kernel into memory during boot. 

---

### Initial RAM Disk

```bash
APPEND initrd=/casper/initrd quiet ---
```
This tells the kernel to load the initial RAM disk (initrd).

The initrd:
- Contains temporary boot scripts
- Loads essential drivers
- Prepares the system to mount the real filesystem

Without it, the kernel would not know how to access the Ubuntu live environment. 

---

### Why the Casper Directory is used?

The `/casper` directory is part of Ubuntu's live boot system. 

It contains the components required to run Ubuntu from installation media. 

Casper is responsible for:
- Detecting boot media
- Mounting the compressed filesystem
- Creating a temporary RAM-based environment.
- Launching the Ubuntu installer or live session.

---

### Ubuntu Live Boot Process
1. BIOS loads the Syslinux bootloader.
2. Syslinux loads the Linux kernel (vmlinuz)
3. The kernel loads the initrd
4. initrd runs Casper scripts
5. Casper mounts `filesystem.squashfs`
6. Ubuntu installer starts

---

# Step 8 - Copy required Syslinux Runtime Modules

```bash
sudo cp /usr/lib/syslinux/modules/bios/ldlinux.c32 /tmp/usb/
sudo cp /usr/lib/syslinux/modules/bios/libcom32.c32 /tmp/usb/
sudo cp /usr/lib/syslinux/modules/bios/libutil.c32 /tmp/usb/
```

## Why are these files required?

Syslinux requires runtime modules to be run correctly. 

During boot:

1. BIOS loads the syslinux boot sector
2. The boot sector loads `ldlinux.c32`
3. `ldlinux.c32` loads supporting libraries
4. Syslinux reads syslinux.cfg
5. Syslinux loads the Linux kernel.

Without these modules, Syslinux cannot complete the boot process. 

---

# Final Notes

After copying all files:

1. Unmount the drives:
```bash
sudo umount /tmp/usb
sudo umount /tmp/iso
```

2. Safely remove the USB.
The USB drive should now boot Ubuntu on Legacy BIOS system like HP ProLiant Microserver Gen8 using Syslinux.
