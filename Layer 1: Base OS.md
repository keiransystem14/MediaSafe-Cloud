# Layer 1: Base OS

In this document, I am writing about the Operating System selection. This involves learning about different operating system choices and learning which one is suitable based on the business requirments & hardware selection. 

## Hardware Specification

The current specifications of the HP ProLiant Gen8 Microserver is the following:

* CPU - Intel Xeon E3-1280
* RAM - 16GB DDR3
* Storage - 4TB HHD (Hard Disk Drive) & 500GB SSD (Solid State Drive)


## True NAS Scale

True NAS Scale is a Linux-based open source operating system for network-attached storage (NAS) which is built on ZFS filesystem. It offers advanced storage management alongside integrations for modern applicatons such as Docker containers and Virtual Machines (VM). It offers features such as snapshots, data replication, self-healing storage and unified file/block/object access.

## Proxmox VE

Proxmox Virtual Environment is a open source server management platform that manages two tech items which are virtual machines and containters from single web interface. Proxmox also integrates will with other tools such as software-defined storage, networking, high availability and backups. 


## Ubuntu Server

## Comparison

| OS            | Storage safety | Learning value | Complexity | Stress level |
| ------------- | -------------- | -------------- | ---------- | ------------ |
| TrueNAS SCALE | High           | Medium         | Medium     | Low          |
| Proxmox       | Medium         | High           | High       | High         |
| Ubuntu Server | Medium         | High           | Medium     | Medium       |
