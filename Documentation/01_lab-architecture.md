LAB ARCHITECTURE OVERVIEW

This document outlines the architecture and purpose of each machine in the offensive security lab.


VIRTUALIZATION PLATFORM
- **VirtualBox** on Windows Host
- **All VMs** stored on external/internal disk


MACHINES & ROLES
| Machine | Role | OS |
|--------|------|----|
| Kali | Attacker | Kali Linux 2024.x |
| Metasploitable3 | Vulnerable target | Ubuntu/Windows |
| BWAPP | Vulnerable web app | Ubuntu (Apache, PHP, MySQL) |
| Commando VM | Pentesting tools (Windows) | Windows 10 |
| DC01 | Domain Controller | Windows Server 2019 |
| pfSense | Firewall / Router | pfSense 2.x |

NETWORKING
-NAT networks because:
  -VM gets private IP addresses.
  -VM can access internet with host machine IP address
  -Outside internet can't directly access VM

Also, we won't be using DHCP as we would need static IP addressing. In the intenal zone however, DHCP is managed by Domain Controller
