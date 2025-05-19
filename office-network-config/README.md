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

Repeat with incremental IPs for PC1, PC2, PC3 in their respective subnets.

---




