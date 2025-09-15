# Lab Architecture Overview

This document outlines the architecture and purpose of each machine in the offensive security lab.


## Virtualization Platform

- **VirtualBox** on Windows Host
- **All VMs** stored on external/internal disk
- Host-only and NAT network adapters used


##  Machines & Roles

| Machine | Role | OS |
|--------|------|----|
| Kali | Attacker | Kali Linux 2024.x |
| Metasploitable3 | Vulnerable target | Ubuntu/Windows |
| BWAPP | Vulnerable web app | Ubuntu (Apache, PHP, MySQL) |
| Commando VM | Pentesting tools (Windows) | Windows 10 |
| DC01 | Domain Controller | Windows Server 2019 |
| pfSense | Firewall / Router | pfSense 2.x |


##  Networking

- **Host-only adapter:** Internal lab communication
- **Bridged adapter;** Accessing network and IP from host machine
- **NAT adapter:** Internet access for updates

