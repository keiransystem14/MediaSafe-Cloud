# USB Boot Failure

Symptoms:

- A bootable USB was created using Rufus containing Ubuntu Server 24.04 LTS.
- The USB was inserted into the HP ProLiant MicroServer Gen8 USB port.
- Upon powering on the server and selecting the USB as the boot device, the system displayed the message:

<img width="1256" height="384" alt="image" src="https://github.com/user-attachments/assets/130725dd-64c9-4282-b683-b4c37e53f722" />


- The boot process stalled at this screen for an extended period (more than 10 minutes) and did not proceed to the Ubuntu installer.
- No additional error messages appeared during this time.

# Feedback 

Why #1: Why is grub stuck on loading screen?

Answer: 

GRUB becoming stuck on the loading screen suggests there is a compatibility issue between the bootable USB drive and the BIOS configuration of the HP ProLiant MicroServer Gen8.

Why #2: Why is there a mismatch between USB and BIOS Configuration of HP ProLiant Gen8 Microserver. 

Answer: 

The issue is likely related to the age of the HP ProLiant MicroServer Gen8. Older systems often rely on strict Legacy BIOS boot requirements.

Although the BIOS is able to detect the USB drive and begin the boot process, GRUB becomes stuck during the loading stage. This indicates that the problem is not with USB detection, but with bootloader compatibility.

This leads to the next question: why was the USB created in a format that is incompatible with the system?

Why #3: why was the USB formatted in a way that is incompatible with this system?

The USB may still be incompatible because Rufus, even when configured with common settings (MBR partition scheme, FAT32 filesystem, and BIOS or UEFI target system), may not fully satisfy the strict Legacy BIOS requirements of the HP ProLiant MicroServer Gen8.

Multiple USB drives were tested and produced the same result, which indicates that the issue is not the USB hardware itself. Instead, the problem likely relates to how the bootloader is written to the USB by Rufus.

This suggests that Rufus may not be the most suitable tool for creating bootable media for this older hardware.

Why #4: Why does Rufus fail to create a fully compatible bootloader for the HP ProLiant MicroServer Gen8?

Rufus typically creates bootable media using modern GRUB implementations and hybrid BIOS/UEFI boot methods.

However, the HP ProLiant MicroServer Gen8 relies on older Legacy BIOS behavior, which may not fully support these newer bootloader configurations. As a result, the bootloader begins loading but fails during initialization, causing GRUB to freeze on the loading screen.

Why #5: Why use other tools such as Syslinux or Grub4Dos instead of Rufus?

Tools such as Syslinux and Grub4Dos are designed specifically for Legacy BIOS environments. They use simpler bootloader structures and avoid hybrid BIOS/UEFI boot methods and newer GRUB features that may not be supported by older hardware.

Using these tools increases the likelihood of creating a bootable USB that the HP ProLiant MicroServer Gen8 can load successfully without GRUB becoming stuck during the boot process.

Root cause: 

The root cause of the issue is that the bootable Ubuntu USB created using Rufus is incompatible with the Legacy BIOS requirements of the HP ProLiant MicroServer Gen8.

Using alternative methods, such as Syslinux or the dd (disk dump) method, may provide better compatibility. The dd method writes the ISO image directly to the USB drive, including the boot sector, which bypasses many BIOS compatibility issues that can occur when tools attempt to modify the bootloader.

Preventive actions:

I have a test laptop currently running Ubuntu Server 24.03 LTS. Once I determine how to create a bootable USB using the Syslinux tool on this system, I will document the full procedure and add the steps to this section for future reference.
