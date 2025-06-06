
## 📑 Table of Contents

- [🧠 Cisco Packet Tracer Network Setup](#-cisco-packet-tracer-network-setup)
- [🖼️ Topology Overview](#️-topology-overview)
- [🔗 Network Segments](#-network-segments)
- [🧰 Devices Used](#-devices-used)
- [💻 PC Configuration – Static IP Addressing](#-pc-configuration--static-ip-addressing)
- [🖧 Switch Configuration – VLAN 1 Setup](#-switch-configuration--vlan-1-setup)
- [🚀 Router Configuration – Step-by-Step Setup](#-router-configuration--step-by-step-setup)
- [🔌 Cabling – Physical Connections](#-cabling--physical-connections)
- [🧪 Testing – Verify Connectivity](#-testing--verify-connectivity)

# 🧠 Cisco Packet Tracer Network Setup

This project involves the design and configuration of a small office network using Cisco networking equipment within Cisco Packet Tracer. The objective is to simulate a functional, secure, and remotely manageable network using a Cisco 2911 router, two Cisco 2960 switches, and four generic PCs.

The network is logically divided into two subnets:

- **Subnet A (192.168.1.0/24)** connects to GigabitEthernet0/0 on the router, linking to Switch0 and PCs PC0 & PC1.
- **Subnet B (192.168.2.0/24)** connects to GigabitEthernet0/1, linking to Switch1 and PCs PC2 & PC3.
- A third interface, **GigabitEthernet0/2**, is configured with the IP address 192.168.3.1 and reserved for potential WAN/cloud connectivity simulation.

---

![Network Topo Figure](https://github.com/tadiusfrank2001/packetracertopology/blob/main/office-network-config/office_net_topology.png)

---

## 🖼️ Topology Overview

- **Router:** 1 Cisco 2911  
- **Switches:** 2 Cisco 2960  
- **PCs:** 4 Generic PCs  
- **Cloud/WAN:** 1 Simulated WAN (via Cloud object)  
- **Cabling:** Copper Straight-Through

---

### 🔗 Network Segments:

| Subnet        | IP Range           | Devices                              |
|---------------|--------------------|---------------------------------------|
| Subnet A      | 192.168.1.0/24     | PC0, PC1, Switch0, Router G0/0        |
| Subnet B      | 192.168.2.0/24     | PC2, PC3, Switch1, Router G0/1        |
| WAN Segment   | 192.168.3.0/24     | Router G0/2, Cloud (simulated WAN)    |

---

## 🧰 Devices Used

| Device Type | Model       | Quantity |
|-------------|-------------|----------|
| Router      | Cisco 2911  | 1        |
| Switch      | Cisco 2960  | 2        |
| PC          | Generic PC  | 4        |
| Cloud       | Generic WAN | 1        |
| Cables      | Copper Straight-Through | 6        |

---


## 💻 PC Configuration – Static IP Addressing

Each PC was manually configured with an IP address in its respective subnet and the default gateway pointing to the router interface on that subnet.

### Example (PC0 in Subnet A):

| Field          | Value             |
|----------------|------------------|
| IP Address     | 192.168.1.10     |
| Subnet Mask    | 255.255.255.0    |
| Default Gateway| 192.168.1.1      |

Repeat with incremental IPs for PC1.

### Example (PC2 in Subnet B):

| Field          | Value             |
|----------------|------------------|
| IP Address     | 192.168.2.10     |
| Subnet Mask    | 255.255.255.0    |
| Default Gateway| 192.168.1.1      |


Repeat with incremental IP for PC3.

---

## 🖧 Switch Configuration – VLAN 1 Setup

Both Switch0 and Switch1 were configured to allow remote management via VLAN 1 and secured access:

### Switch0 Example:

```bash
enable
configure terminal
hostname Switch0
banner motd # Unauthorized access is prohibited. #
# you can use your own secret password to secure the CLI
enable secret Sw1tch0Secret! 
service password-encryption
# you can use your domain name
ip domain-name tadiuslab.local
interface vlan 1
# the host ID of the router can be anything within 2-9 and 12-254 ranges
ip address 192.168.1.100 255.255.255.0
# now we can implement our config
no shutdown
exit


ip default-gateway 192.168.1.1

# Set up console security
line console 0
password c0nsol3Sw0
login
logging synchronous
exit
# Set up VTY
line vty 0 4
password vtySw0Pass
login
transport input all
exit
end
# startup our config so our switch operates with our configs
copy running-config startup-config
```

Apply a similar configuration for Switch1 using the `192.168.2.x` range, I utilized `192.168.2.100` to keep it similar.

---
## 🚀 Router Configuration – Step-by-Step Setup

### 1. Enter Privileged EXEC Mode

```bash
enable
```

### 2. Enter Global Configuration Mode

```bash
configure terminal
```

### 3. Set Hostname and Security

```bash
hostname R1
banner motd # Unauthorized access is prohibited. #
# You can choose your secret pass to secure the CLI
enable secret ClassC0nfig!
service password-encryption
# Remember to use your selected domain name
ip domain-name tadiuslab.local
# Set local dns, and time
ip name-server 8.8.8.8
ip domain-lookup
clock timezone EST -5
```

### 4. Configure Router Interfaces

Notice how all Router interfaces in each subnet is the first device on the network!

1. Connect Subnet A to the router via interface g0/0.

```bash
interface g0/0
 description Connected to Switch0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit
```
2. Connect Subnet B to the router via interface g0/1.
```bash
interface g0/1
 description Connected to Switch1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit
```
3. Connect router to WAN Cloud via interface g0/2.
```bash
interface g0/2
 description Connected to WAN/Cloud
 ip address 192.168.3.1 255.255.255.0
 no shutdown
exit
```

### 5. Secure Access Lines

```bash
line console 0
 password c0nsol3Pass
 login
 logging synchronous
exit

line vty 0 4
 password vtyPa55word
 login
 transport input all
exit
```

### 6. Save the Configuration

```bash
end
copy running-config startup-config
```

---
## 🔌 Cabling – Physical Connections

Use **Copper Straight-Through** cables for all device connections:

- **PCs to Switches:** PC FastEthernet → Switch FastEthernet (e.g., PC0 → Switch0 Fa0/1)
- **Switches to Router:** 
  - Switch0 → Router G0/0  
  - Switch1 → Router G0/1
- **Router to WAN (Cloud):** Router G0/2 → Cloud Ethernet

Ensure ports are properly selected and status indicators confirm successful links (green triangles that indicate traffic between devices).

---
## 🧪 Testing – Verify Connectivity

Use the following from each PC terminal:

```bash
ping 192.168.1.1   // PC0/PC1 → Router G0/0
ping 192.168.2.1   // PC2/PC3 → Router G0/1
ping <Other PC IP> // Cross-subnet ping test
```
![PC0toRouter](https://github.com/tadiusfrank2001/packetracertopology/blob/main/office-network-config/PC0toRouter.png)

![PC2toRouter](https://github.com/tadiusfrank2001/packetracertopology/blob/main/office-network-config/PC2toRouter.png)

![PC1toPC2](https://github.com/tadiusfrank2001/packetracertopology/blob/main/office-network-config/PC1toPC2.png)

Notice some loss, from PC1 to PC2 because the system had perform ARP resolution and MAC address registration to then forward a packet from PC1 to the router interface g0/0 so the router can look at its routing table (after finding the network ID for the destination IP by bitwise AND the subnet mask and destination IP) and forward it to G0/1 where PC2 is a part of.

From Router CLI:

```bash
show ip interface brief
show running-config
```
![ipinterfacebrief](https://github.com/tadiusfrank2001/packetracertopology/blob/main/office-network-config/RouterInterfaces.png)

---

