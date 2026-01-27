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

When I was using Rufus to create a bootable USB. I have set MBR as the partition scheme, File system to be FAT32, Target system BIOS or UEFI and Cluster size was 4096KB

Why #4:
Why #5:

Root cause: 
Preventive actions:
