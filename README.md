## 📘 Network Lab: Optimized VLAN Design with SVI, DHCP Server, and VTP Core

## 🔍 Description
#### This lab demonstrates a scalable and efficient Layer 2/3 network design that reduces router load and improves performance. It uses:
- SVIs for inter-VLAN routing on the Core Switch.
- A dedicated DHCP server in VLAN 40 to assign IP addresses.
- VTP to distribute VLAN configurations from the Core Switch to access switches.

# 🧠 Key Benefits
- Reduced Router Traffic: Inter-VLAN routing is handled locally on the Core Switch.
- Centralized IP Management: DHCP server assigns IPs to all VLANs via ip helper-address.
- Simplified VLAN Management: VTP ensures consistent VLAN configuration across switches.

# 🖥️ Topology Overview
- 1 Router
- 1 Core Switch (VTP Server + SVI Routing)
- 2 Layer 2 Access Switches (VTP Clients)
- 1 DHCP Server (in VLAN 40)
- 6 PCs
- VLANs: (10 Sales) (20 HR) (30 Finance) (40 Server)

# ⚙️ Technologies Used
- SVI (Switched Virtual Interface)
- VTP (VLAN Trunking Protocol)
- DHCP Relay (ip helper-address)
- Trunk Ports
- Access Ports
---
# 🧾 Configuration Summary
## Core Switch (Core-Sw1)
```bash
hostname Core-Sw1
vtp mode server
vtp domain Nour
vtp password 2000

vlan 10
 name Sales
vlan 20
 name Finance
vlan 30
 name HR
vlan 40
 name SRV-1

interface gi1/0/1
 switchport mode trunk
interface gi1/0/2
 switchport mode trunk
interface gi1/0/4
 switchport access vlan 40

interface Vlan10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.40.200
 no shutdown

interface Vlan20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.40.200
 no shutdown

interface Vlan30
 ip address 192.168.30.1 255.255.255.0
 ip helper-address 192.168.40.200
 no shutdown

interface Vlan40
 ip address 192.168.40.1 255.255.255.0
 no shutdown
```
## 🔹 Access Switch 1 (SW-1)
```bash
hostname SW-1
vtp mode client
vtp domain Nour
vtp password 2000

interface fa0/1
 switchport mode trunk

interface fa0/2
 switchport access vlan 20
interface fa0/3
 switchport access vlan 30
interface fa0/4
 switchport access vlan 10
```
## 🔹 Access Switch 2 (SW-2)
```bash
hostname SW-2
vtp mode client
vtp domain Nour
vtp password 2000

interface fa0/2
 switchport access vlan 10
interface fa0/3
 switchport access vlan 20
interface fa0/4
 switchport access vlan 30
```
# DHCP configuration at a Server
<img width="941" height="992" alt="image" src="https://github.com/user-attachments/assets/14ede7f8-0b21-4fad-931a-50c0df05fe6e" />

# 💻 Lap
<img width="1221" height="658" alt="image" src="https://github.com/user-attachments/assets/7efe9f1a-f2bb-44da-aa01-6aeffceb82e8" />

---
# 🧪 Notes
- Make sure the DHCP server is configured with scopes for each VLAN.
- Ensure trunk links are properly connected between Core and Access switches.
- Use show ip interface brief and show vlan brief to verify configurations.

