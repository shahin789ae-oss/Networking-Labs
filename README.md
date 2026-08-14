# Networking Labs 🌐

Welcome to my networking portfolio repository! This space documents my hands-on practice with network administration, infrastructure design, and command-line utilities.

## Topics Covered
* Local Area Network (LAN) design and host address configurations
* Subnetting and IP addressing strategies
* Cisco Packet Tracer routing and switch configurations

## Tools Used
* Cisco Packet Tracer
* Command Line Utilities


# Cisco Packet Tracer: Multi-Subnet Network Lab

## 🌐 Lab Overview
This repository contains a comprehensive multi-subnet network topology designed and configured in **Cisco Packet Tracer**. The lab demonstrates core networking concepts, including multi-area subnetting, centralized DHCP server deployment, static IP provisioning, global DNS integration, core router hardware expansion, enterprise-grade router security hardening, and remote Telnet verification.

---

## 🏗️ Network Topology & Architecture
The network is structured around a central core router (`R1` - Cisco 2811) interconnecting three distinct subnets:

1. **Green Subnet (Static Corporate & Service Zone)**
2. **Pink Subnet (Dynamic Client Zone)**
3. **Yellow Subnet (Lightweight Client Zone)**

---

## 🟩 Green Subnet (Static Zone & Core Services)
The Green subnet hosts core corporate workstations, internal web services, and a centralized DNS server accessible across the entire topology.

* **Switch:** `Switch0` (Cisco 2960-24TT)
* **Default Gateway:** `192.168.1.1`

### Static IP Allocations:
* **Boss Workstation (`PC-Boss`):** `192.168.1.2`
* **HR Workstation (`PC-HR`):** `192.168.1.3`
* **DNS Server (`Server-PT DNS`):** `192.168.1.10`
* **Facebook Web Server (`facebook`):** `192.168.1.20`
* **Instagram Web Server (`instagram`):** `192.168.1.30`

### Cross-Network Services:
* **Global DNS Integration:** The DNS server IP (`192.168.1.10`) was configured on devices across all other network segments to allow clients outside the Green zone to resolve names and access the web servers seamlessly.

---

## 🩷 Pink Subnet (Dynamic DHCP Zone)
The Pink subnet utilizes an automated DHCP server to distribute configuration parameters dynamically to client workstations.

* **Switch:** `Switch1` (Cisco 2960-24TT)
* **Default Gateway:** `172.16.0.1`
* **DHCP Server (`Server1`):** `172.16.0.10`
* **DHCP Pool Start IP:** `172.16.0.20` *(configured to exclude server/gateway addresses and prevent conflicts)*
* **Maximum Users:** 20
* **Workstations:** `PC5`, `PC6`, `PC7` (configured successfully via DHCP)

---

## 🟨 Yellow Subnet (Client Zone)
A secondary client segment connected via a dedicated local switch to the core router.

* **Switch:** `Switch2` (Cisco 2960-24TT)
* **Default Gateway:** `192.168.2.1`
* **Laptop 0 (`Laptop0`):** `192.168.2.2`
* **Laptop 1 (`Laptop1`):** `192.168.2.3`

---

## 🔀 Core Router Hardware & Configuration (`R1` - Cisco 2811)
Since the default Cisco 2811 router layout features a limited number of onboard interfaces, a **WIC-1ENET** expansion port module was added to the hardware slot. This enabled the physical capacity needed to connect the third network segment (Yellow Subnet).

### Interface IP Assignments:
* **FastEthernet0/0 (Green):** `192.168.1.1` / `255.255.255.0`
* **FastEthernet0/1 (Pink):** `172.16.0.1` / `255.255.255.0`
* **FastEthernet1/0 (Yellow - via WIC-1ENET):** `192.168.2.1` / `255.255.255.0`

### Sample CLI Implementation:
```text
Router> enable
Router# configure terminal
Router(config)# hostname R1

! --- Green Interface ---
R1(config)# interface FastEthernet0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! --- Pink Interface ---
R1(config)# interface FastEthernet0/1
R1(config-if)# ip address 172.16.0.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! --- Yellow Interface (WIC-1ENET Expansion Port) ---
R1(config)# interface FastEthernet1/0
R1(config-if)# ip address 192.168.2.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

🔒 Router Security, Hardening & Remote Management
To secure administrative control and test remote management over the network:

! --- 1. Secure Privileged EXEC Mode ---
R1(config)# enable password ccna
R1(config)# enable secret ccnp

! --- 2. Secure Console Access ---
R1(config)# line console 0
R1(config-line)# password ConsolePass123
R1(config-line)# login
R1(config-line)# exit

! --- 3. Secure Virtual Terminal (VTY) Access for Remote Management ---
R1(config)# line vty 0 4
R1(config-line)# password ccie
R1(config-line)# login
R1(config-line)# exit

! --- 4. Encrypt Plaintext Passwords ---
R1(config)# service password-encryption

