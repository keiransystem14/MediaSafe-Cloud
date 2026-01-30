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

Answer: Grub getting stuck on the loading screen points out that there is a mismatch between USB and the BIOS Configuration of the HP ProLiant Gen8 Microserver. 

Why #2: Why is there a mismatch between USB and BIOS Configuration of HP ProLiant Gen8 Microserver. 

Answer: This is most likely due to the age of the HP ProLiant MicroServer Gen8. Older systems often have strict Legacy BIOS boot requirements. Although the BIOS can detect the USB drive and start booting from it, GRUB becomes stuck at the loading stage. This suggests a compatibility issue rather than a detection problem. It leads to the next question: why was the USB formatted in a way that is incompatible with this system?

Why #3: why was the USB formatted in a way that is incompatible with this system?

The USB may still be incompatible because Rufus, even with standard settings, may not fully match the strict Legacy BIOS requirements of the HP ProLiant MicroServer Gen8. Although the USB was created using MBR, FAT32, and a BIOS or UEFI target system, GRUB still becomes stuck during loading. This indicates that the issue is not the USB drive itself, as multiple USB devices produced the same result, but rather how the bootloader is being written or handled by Rufus for this older hardware. This suggests that Rufus may not be the best tool for this system, and alternative tools such as Syslinux or Grub4Dos may be required to resolve the compatibility issue.

Why #4: Why does Rufus fail to create a fully compatible bootloader for the HP ProLiant MicroServer Gen8?

Because the HP ProLiant MicroServer Gen8 relies on older Legacy BIOS behavior that may not fully support modern GRUB implementations or hybrid BIOS/UEFI boot methods used by Rufus. As a result, the bootloader loads partially but fails during initialization, causing GRUB to hang on the loading screen.

Why #5: Why use other tools such as Syslinux or Grub4Dos instead of Rufus?

Because Syslinux and Grub4Dos are designed particularly for Legacy BIOS systems and use simpler bootloader structures. These tools avoid hybrid BIOS/UEFI boot methods and modern GRUB features that may not be supported by the HP ProLiant MicroServer Gen8. Using them increases the chance of creating a USB that the system can fully load without GRUB hanging on the loading screen.

Root cause: 
Preventive actions:

Create a bootable USB using SysLinux tool. I have a test laptop that currently has Arch Linux installed. Once I am able to identify how I can use SysLinux tool on my test laptop. I will document the steps under this section. 
