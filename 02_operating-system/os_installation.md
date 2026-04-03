# How to install Ubuntu 24.04 LTS Operating System

1. Insert the bootable Ubuntu Server 24.04 LTS USB drive into the HP ProLiant MicroServer Gen8 and power on the server.
2. Ensure the server boots from the USB drive by selecting the USB device from the boot menu if necessary, which will load the Syslinux bootloader.

<img width="2322" height="1266" alt="image" src="https://github.com/user-attachments/assets/14caa51f-95a8-4238-9c4b-ba0733bb4878" />

3. When the Ubuntu Server installer screen appears, select English (UK) as the installation language and continue.

<img width="2136" height="1112" alt="image" src="https://github.com/user-attachments/assets/02355a11-78a6-4965-99fc-4bfb8064f960" />

4.  Confirm the keyboard layout as English (UK) and select Done.
5.  Leave Ubuntu Server selected as the installation type and select Done.
6.  Select the USB Wi-Fi adapter from the network list and choose Edit Wi-Fi.
7.  Enter the Wi-Fi network details including the SSID and password, then select Save.
8.  Confirm the network configuration once the connection is established and select Done.
9.  Leave the proxy configuration blank since it is not required and select Done.
10. Accept the automatically detected Ubuntu mirror configuration and select Done.
11. Choose to update the installer when prompted so the installation process uses the latest available installer version.
12. Select Use an entire disk and choose the 500GB SSD installed in the optical drive bay of the HP ProLiant MicroServer Gen8.
13. Leave the option Set up this disk as an LVM group enabled and select Done.
14. Review the storage configuration, keep the default layout, and select Done to confirm the changes.
15. Enter the required details including your name, server name, username, and password to create the user account.
16. Select Skip for now when prompted to upgrade to Ubuntu Pro.
17. Enable the option Install OpenSSH server and select Done.
18. Ignore the Featured Server Snaps section by leaving everything unselected and selecting Done.
19. Wait while Ubuntu Server 24.04 LTS installs on the system and allow the server to reboot when the installation is complete after removing the USB drive when prompted.
20. After the server restarts, log in using the username and password created during installation.
