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

Possible reasons:

Potentially because when creating the bootable Ubuntu Server 24.04 LTS, it wasn't written correctly to the USB. 

Why #2: 
Why #3:
Why #4:
Why #5:

Root cause: 
Preventive actions:
