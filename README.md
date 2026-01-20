# Multi-VLAN Campus Network Architecture 
### Core Routing with Raspberry Pi & Layer 2 Redundancy

This project demonstrates the design and implementation of a secure, segmented, and high-availability campus network. It utilizes a **Raspberry Pi** as a Core Router (Router-on-a-Stick model) and a matrix of four **Cisco 2960** switches with physical link redundancy.

##  Network Engineering Highlights

* **Core Routing:** Implemented a **Router-on-a-Stick** model using a Raspberry Pi with **OpenWrt/LuCi**, managing VLAN termination and inter-VLAN traffic.
* **Layer 2 Redundancy:** Configured **EtherChannel** across the switching matrix using both **PAgP** (Port Aggregation Protocol) and **LACP** (Link Aggregation Control Protocol) for fault tolerance.
* **Logical Segmentation:** Applied **VLSM** (Variable Length Subnet Masking) for efficient IP allocation across 4 distinct VLANs:
    * **VLAN 10 (Microservices):** FastAPI infrastructure.
    * **VLAN 20 (Streaming):** Multimedia delivery (VLC).
    * **VLAN 30 (Mail Server):** SMTP services (aiosmtpd + SQLite).
    * **VLAN 40 (Gaming):** Low-latency UDP traffic (Counter-Strike).
* **Security Architecture:** Deployed strict **Firewall policies** (Principle of Least Privilege) and **NAT Masquerading** for secure WAN access.

## Addressing Plan (VLSM)
| VLAN | Service | Hosts | Subnet | Gateway |
| :--- | :--- | :--- | :--- | :--- |
| 30 | Mail Server | 50 | 10.10.0.0/26 | 10.10.0.1 |
| 20 | Streaming | 40 | 10.10.0.64/26 | 10.10.0.65 |
| 10 | Microservices | 30 | 10.10.0.128/27 | 10.10.0.129 |
| 40 | Gaming | 15 | 10.10.0.160/27 | 10.10.0.161 |

## Application Layer Validation
The architecture was validated by deploying cross-VLAN services:
* **SMTP Flow:** Notification service in VLAN 10 sending emails to VLAN 30 authorized by Netfilter rules.
* **REST API:** Auth and Notification microservices handling secure communication.
* **Real-time Traffic:** Verified bi-directional UDP traffic for gaming and TCP streams for video.

## 📸 Topology Overview
<img width="1393" height="975" alt="Captura de pantalla 2026-01-20 122432" src="https://github.com/user-attachments/assets/239023f3-98a2-4ff2-b1d8-9888b87cd9dc" />
<img width="921" height="435" alt="image-b2c2d27c-9dfd-488c-bf8f-1ce9db4e160d" src="https://github.com/user-attachments/assets/39d7d1ad-16b3-4c1b-9174-23d9f5794b3b" />

