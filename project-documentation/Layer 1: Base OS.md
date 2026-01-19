# Layer 1: Base OS

In this document, I am writing about the Operating System selection. This involves learning about different operating system choices and learning which one is suitable based on the business requirments & hardware selection. 

## Hardware Specification

The current specifications of the HP ProLiant Gen8 Microserver is the following:

* CPU - Intel Xeon E3-1280
* RAM - 16GB DDR3
* Storage - 4TB HHD (Hard Disk Drive) & 500GB SSD (Solid State Drive)


## Operating System Selection

When choosing an operating system. I wanted to choose an operating system that helps optimize the usage of my current server - HP ProLiant Microserver Gen8 and an OS that helps build the cloud-based photo storage. In an addition, an operating system that allows me to learn how to troubleshoot new issues with different technologies hands-on. I also want it to give me flexibility to choose the technologies that work based on the needs. 

### True NAS Scale

True NAS Scale is a Linux-based open source operating system for network-attached storage (NAS) which is built on ZFS filesystem. It offers advanced storage management alongside integrations for modern applicatons such as Docker containers and Virtual Machines (VM). It offers features such as snapshots, data replication, self-healing storage and unified file/block/object access. The main benefit of using True NAS Scale is it gives an easy UI interface for you to administrate on. The main downside of using TrueNAS Scale is that it doesn't give enough flexibility as a general purpose system. 

### Proxmox VE

Proxmox Virtual Environment is a open source server management platform that manages two tech items which are virtual machines and containters from single web interface. Proxmox also integrates will with other tools such as software-defined storage, networking, high availability and backups. The main advantage of using Proxmox VE is that it enables you to run multiple virtual machines at once. However, the main downside of using Proxmox VE is that low end systems may not have enough resources to run virtual machines. 


### Ubuntu Server

Ubuntu Server is an Linux based operating system that is specifically built for servers. This operating system focuses on stability, security and scaleability for hosting applications. It's a very lightweight operating system since it only has the command line interface and not a graphical user interface (GUI). The important benefit of using Ubuntu Server OS is that it would give me ability to choose every component myself. However, the downside is that there's no UI which makes it a steeper learning curve & intergrate the storage tools myself. 

### Comparison

| OS                | Storage safety | Learning value | Hardware utilisation (16 GB RAM) | Flexibility   | Complexity | Stress level |
| ----------------- | -------------- | -------------- | -------------------------------- | ------------- | ---------- | ------------ |
| **TrueNAS SCALE** | **High**       | Medium         | Medium–Low                       | Medium        | Medium     | **Low**      |
| **Proxmox**       | Medium         | **Very High**  | Medium                           | **High**      | **High**   | **High**     |
| **Ubuntu Server** | Medium         | **Very High**  | **High**                         | **Very High** | Medium     | Medium       |


## Overall

After researching and comparing TrueNAS SCALE, Proxmox VE, and Ubuntu Server, I concluded that Ubuntu Server is the most suitable operating system for my HP ProLiant Gen8 Microserver.

I chose Ubuntu Server because it provides the flexibility needed to design and configure a system tailored to building the MediaSafe cloud photo storage solution. Unlike more appliance-style platforms, Ubuntu Server allows full control over storage, services, and security, enabling a custom and scalable setup.

Additionally, using Ubuntu Server gives me the opportunity to develop and master core Linux administration skills through hands-on configuration, troubleshooting, and system management. Finally, Ubuntu Server offers efficient resource utilisation, allowing me to maximise the performance and capabilities of the HP ProLiant Microserver Gen8 hardware within its 16 GB RAM limitation.
