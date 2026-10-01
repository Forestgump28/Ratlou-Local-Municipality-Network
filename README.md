# Ratlou Local Municipality Network Infrastructure Project

**Module Code:** CMPG325  
**Student Name:** MZOBE, BAYA  
**Student Number:** 44027516  
**Institution:** North-West University  
**Submission Date:** 02 October 2026  
**Repository:** [Ratlou Local Municipality Network Project](https://github.com/Forestgump28/Semester-project)

---

## Executive Summary
This project delivers a resilient, scalable, 3-tier hierarchical Cisco network infrastructure designed for the Ratlou Local Municipality administrative offices in Setlagole. The design supports administrative services, secure public guest Wi-Fi, inter-departmental isolation via IEEE 802.1Q sub-interfaces, dynamic addressing via dedicated DHCP pools, and enterprise wireless security.

---

## Network Architecture & Design Strategy

### 1. Topology & Hierarchical Design
The network is structured following Cisco's 3-tier design principles:
* **Core Layer (`SW1-Core` & `R1-Ratlou`):** Handles high-speed packet switching, default gateway routing, sub-interface encapsulation, and central DHCP service allocation.
* **Distribution/Access Layer (`SW2-Admin`, `SW3-Public`, `SW4-Floor2-CR2`):** Delivers port-level security, VLAN segmentation, end-user device connectivity, and wireless access point bridging.

### 2. VLSM Subnetting Scheme (`192.168.44.0/24` Base)

| Subnet / VLAN | Function | Network Address | Subnet Mask | Usable Host Range | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | Admin & Staff Workstations | `192.168.44.0/28` | `255.255.255.240` | `192.168.44.1 - .14` | `192.168.44.1` |
| **VLAN 20** | Public Area & Guest Wi-Fi | `192.168.44.64/28` | `255.255.255.240` | `192.168.44.65 - .78` | `192.168.44.65` |
| **VLAN 30** | CR2 Expansion Floor | `192.168.44.96/27` | `255.255.255.224` | `192.168.44.97 - .126` | `192.168.44.97` |
| **VLAN 99** | Native & Management | `192.168.44.128/28` | `255.255.255.240` | `192.168.44.129 - .142` | N/A |

---

## Milestone 2 Implementation: Change Request 2 (CR2)

### 1. Scope of Implementation
Change Request 2 expands the physical infrastructure to accommodate a second operational floor (`Floor 2 Expansion`):
* **Hardware Integrated:** `SW4-Floor2-CR2` access switch, `AP-Floor2` wireless access point, wired workstations (`floor2 pc1`), and mobile units (`Laptop8`).
* **Trunk Alignment:** IEEE 802.1Q trunking on port `Fa0/24` between `SW4-Floor2-CR2` and `SW1-Core` with explicit **Native VLAN 99** alignment for administrative security.
* **Services Activated:** Router sub-interface `GigabitEthernet0/0.30` and DHCP pool `CR2Pool` on `R1-Ratlou`. Wireless security configured using **WPA2-PSK** encryption.

### 2. Key Device Configurations (CLI Snippets)

#### Router `R1-Ratlou` Sub-Interface & DHCP Setup
```text
interface GigabitEthernet0/0.30
 description CR2-New-Floor-Gateway
 encapsulation dot1Q 30
 ip address 192.168.44.97 255.255.255.224
!
ip dhcp pool CR2Pool
 network 192.168.44.96 255.255.255.224
 default-router 192.168.44.97
 dns-server 8.8.8.8
