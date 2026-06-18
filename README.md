# CCNA Day 3 Lab – OSI Model Protocol Observation in Cisco Packet Tracer

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=flat&logo=cisco&logoColor=white)
![CCNA](https://img.shields.io/badge/Study-CCNA-green?style=flat)
![Status](https://img.shields.io/badge/Status-Completed-success)
![Domain](https://img.shields.io/badge/Domain-Networking%20%7C%20CCNA%20%7C%20OSI%20Model%20%7C%20Protocol%20Analysis-blueviolet)

---

## Overview

This lab uses Cisco Packet Tracer's **Simulation Mode** to observe how different protocols operate across the OSI model layers in a live network topology. A small enterprise network was built with two routers, two switches, a server, and a PC across two subnets. The simulation panel captured real-time protocol events including **STP**, **OSPF**, and **DHCP** — demonstrating how each protocol functions at its respective OSI layer during normal network operation.

---

## Environment

| Tool | Purpose |
|------|---------|
| Cisco Packet Tracer | Network simulation and protocol observation |
| Cisco 2911 Routers (x2) | R1, R2 — inter-subnet routing via OSPF |
| Cisco Switches (x2) | SW1, SW2 — LAN switching with STP |
| Server (x1) | SRV1 — DHCP server |
| PC (x1) | PC1 — DHCP client endpoint |
| Simulation Mode | Real-time protocol event capture and analysis |
| GitHub | Documentation and version control |

---

## Network Topology

### Subnets

| Subnet | Network | Devices |
|--------|---------|---------|
| Left LAN | 192.168.1.0/24 | SRV1, SW1, SW2, PC1, R1 (G0/1 = .1) |
| Right WAN | 10.0.0.0/24 | R1 (G0/0 = .1), R2 (G0/0 = .2) |

### Interface Addressing

| Device | Interface | IP Address |
|--------|-----------|-----------|
| SRV1 | F0/1 | 192.168.1.100 |
| SW1 | G0/2 | Connected to R1 |
| R1 | G0/1 | 192.168.1.1 |
| R1 | G0/0 | 10.0.0.1 |
| R2 | G0/0 | 10.0.0.2 |
| PC1 | F0/1 | DHCP assigned |

---

## Build Walkthrough

---

### ✅ Step 1 — Built the Network Topology

Placed SRV1, SW1, SW2, PC1, R1, and R2 in the logical workspace. Connected all devices using correct cable types. Configured IP addresses on router interfaces for both the 192.168.1.0/24 LAN subnet and the 10.0.0.0/24 WAN link between R1 and R2. Configured SRV1 as a DHCP server for the LAN subnet and set PC1 to obtain an IP automatically.

---

### ✅ Step 2 — Enabled OSPF on Both Routers

Configured OSPF on R1 and R2 to enable dynamic routing between the two subnets. OSPF hello packets and routing updates were captured in the simulation panel confirming neighbor adjacency was established between R1 and R2.

---

### ✅ Step 3 — Ran Simulation Mode and Observed Protocol Events

Switched Packet Tracer to Simulation Mode and played the network. The Event List captured all protocol activity across the topology in real time with timestamps, source devices, destination devices, and protocol types visible.

**Protocols observed:**

| Protocol | OSI Layer | Activity Observed |
|----------|-----------|------------------|
| STP | Layer 2 | Spanning Tree Protocol frames exchanged between SW1, SW2, PC1, R1, SRV1 to prevent loops |
| OSPF | Layer 3 | OSPF hello and LSA packets exchanged between R1 and R2 to establish routing adjacency |
| DHCP | Layer 7 (App) | DHCP discover, offer, request, and acknowledgment between PC1, SW1, SW2, and SRV1 |

![OSI Model Simulation](ccna-day3-osi-simulation.png)
*Packet Tracer Simulation Mode — Event List showing STP, OSPF, and DHCP protocol events captured
across SW1, SW2, R1, R2, PC1, and SRV1 in real time*

---

## Protocol Analysis

### STP — Spanning Tree Protocol (Layer 2)

STP events appeared first in the simulation at timestamps 1.952 through 1.954 and again at 3.951 through 3.953. SW2 and SW1 exchanged STP BPDUs with PC1, R1, and SRV1 — confirming the switches were running STP to elect a root bridge and block redundant paths to prevent Layer 2 loops.

### OSPF — Open Shortest Path First (Layer 3)

OSPF events appeared at timestamps 5.661 through 5.680. R2 and R1 exchanged OSPF hello packets and link-state advertisements, confirming dynamic routing adjacency was established between the two routers. OSPF events also propagated to SW1 and SRV1 as OSPF multicast traffic crossed the LAN segment.

### DHCP — Dynamic Host Configuration Protocol (Layer 7)

DHCP events appeared at timestamps 5.678 through 5.681. PC1 sent a DHCP discover, which propagated through SW2 and SW1 to SRV1. SRV1 responded with a DHCP offer and acknowledgment, confirming PC1 received an IP address dynamically from the server.

---

## Skills Demonstrated

| Skill | How It Was Applied |
|-------|--------------------|
| OSI Model Understanding | Identified which layer each protocol operates at |
| Protocol Analysis | Observed and explained STP, OSPF, and DHCP behavior in simulation |
| OSPF Configuration | Configured dynamic routing between two routers |
| DHCP Configuration | Configured SRV1 as a DHCP server and PC1 as a client |
| Simulation Mode | Used Packet Tracer event list to capture and read live protocol traffic |
| Network Topology Design | Built a multi-subnet topology with correct IP addressing |

---

## Lessons Learned

**The OSI model is not just a theory — it is visible in live traffic.** Every protocol captured in this simulation operates at a specific layer for a specific reason. STP runs at Layer 2 because it manages MAC-addressed frames between switches. OSPF runs at Layer 3 because it makes routing decisions based on IP addresses. DHCP runs at the application layer because it provides network configuration services to hosts. Seeing all three running simultaneously in the same topology makes the model concrete rather than abstract.

**STP runs automatically whether you configure it or not.** As soon as the switches were connected, STP began exchanging BPDUs without any configuration. This is important because it means every Layer 2 network is running STP by default — and understanding its behavior is essential for troubleshooting slow convergence, blocked ports, and unexpected traffic patterns in real networks.

**OSPF adjacency is a prerequisite for routing.** Before R1 and R2 could route traffic between the two subnets, they first had to discover each other, exchange hello packets, and agree on OSPF parameters. The simulation captured that entire process. In production networks, failed OSPF adjacency is one of the most common causes of routing failures — and knowing how to read the hello exchange is the first step in diagnosing it.

---

## 💼 Real-World Application

Every network engineer and SOC analyst needs to understand how protocols behave at each OSI layer. When a host cannot get an IP address, the problem is at Layer 7 (DHCP). When two sites cannot reach each other, the problem may be at Layer 3 (routing). When a switch port is blocked unexpectedly, the problem is at Layer 2 (STP). Being able to map a symptom to a layer — and then to a specific protocol — is the foundation of every network troubleshooting methodology used in enterprise environments today.

---

## References

- [Jeremy's IT Lab — CCNA Day 3](https://www.youtube.com/watch?v=t-ai8JzhHuY)
- [Jeremy's IT Lab — Full CCNA Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
- [Cisco Packet Tracer Download](https://www.netacad.com/courses/packet-tracer)
