# 🌐 Enterprise Network Security Lab

> A hands-on networking project demonstrating VLAN segmentation, inter-VLAN routing, ACLs, NAT, VPN tunneling, and network monitoring — built entirely in simulation software.

[![Network+](https://img.shields.io/badge/CompTIA-Network%2B-blue)](https://www.comptia.org/certifications/network)
[![Cisco](https://img.shields.io/badge/Cisco-IOS-darkblue)](https://www.cisco.com)
[![GNS3](https://img.shields.io/badge/GNS3-Simulation-green)](https://www.gns3.com)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Network Topology](#-network-topology)
- [Technologies Used](#-technologies-used)
- [Lab Setup Guide](#-lab-setup-guide)
- [Configuration Files](#-configuration-files)
- [What I Learned](#-what-i-learned)
- [Troubleshooting Log](#-troubleshooting-log)
- [Screenshots](#-screenshots)
- [How to Replicate](#-how-to-replicate)

---

## 🎯 Project Overview

This lab simulates a **multi-site enterprise network** with:

| Component | Purpose |
|-----------|---------|
| **3 VLANs** | Segment departments (IT, HR, Guest) |
| **Inter-VLAN Routing** | Layer 3 switch enables communication between VLANs |
| **OSPF Dynamic Routing** | Automatic route propagation between sites |
| **Site-to-Site VPN (IPSec)** | Secure tunnel between HQ and Branch Office |
| **ACLs & Port Security** | Access control and threat mitigation |
| **NAT Overload (PAT)** | Hide private IPs behind public IPs |
| **Syslog & SNMP Monitoring** | Centralized logging and performance metrics |
| **DHCP Server** | Automatic IP assignment per VLAN |

**Real-world application:** This mirrors how small-to-medium enterprises structure their networks — segmentation for security, VPN for remote sites, and monitoring for visibility.

---

## 🗺️ Network Topology

```
                                    INTERNET
                                       │
                                       ▼
                              ┌─────────────────┐
                              │   ISP Router    │  203.0.113.1/30
                              │  (Simulated)    │
                              └────────┬────────┘
                                       │
                              203.0.113.2/30
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              HEADQUARTERS (HQ)                              │
│                                                                             │
│   ┌──────────────┐      ┌──────────────┐      ┌──────────────┐             │
│   │   HQ-RTR     │──────│  HQ-SW-L3    │──────│  HQ-SW-ACC   │             │
│   │  (Router)    │      │ (L3 Switch)  │      │ (Access SW)  │             │
│   └──────────────┘      └──────┬───────┘      └──────┬───────┘             │
│         │                      │                     │                      │
│    VPN Tunnel              VLAN Trunk             VLANs 10,20,30           │
│         │                      │                     │                      │
│         ▼                      ▼                     ▼                      │
│   ┌──────────┐           ┌──────────┐         ┌──────────┐                  │
│   │  Tunnel  │           │  SVIs    │         │  PCs &   │                  │
│   │  Interface│          │ 10.1.x.1 │         │  Servers │                  │
│   └──────────┘           └──────────┘         └──────────┘                  │
│                                                                             │
│   VLANs:                                                                    │
│   • VLAN 10 - IT Department     (10.1.10.0/24)                            │
│   • VLAN 20 - HR Department     (10.1.20.0/24)                            │
│   • VLAN 30 - Guest Network     (10.1.30.0/24)                            │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       │ IPSec VPN Tunnel
                                       │ 192.168.100.0/30
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            BRANCH OFFICE                                    │
│                                                                             │
│   ┌──────────────┐      ┌──────────────┐      ┌──────────────┐             │
│   │  BR-RTR      │──────│  BR-SW-L3    │──────│  BR-SW-ACC   │             │
│   │  (Router)    │      │ (L3 Switch)  │      │ (Access SW)  │             │
│   └──────────────┘      └──────┬───────┘      └──────┬───────┘             │
│                                │                     │                      │
│                           VLAN Trunk             VLANs 40,50                │
│                                                     │                      │
│                                               ┌──────────┐                  │
│                                               │  PCs &   │                  │
│                                               │  Printer │                  │
│                                               └──────────┘                  │
│                                                                             │
│   VLANs:                                                                    │
│   • VLAN 40 - Sales             (10.2.40.0/24)                            │
│   • VLAN 50 - Management        (10.2.50.0/24)                            │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### IP Addressing Scheme

| Site | VLAN | Network | Gateway | Purpose |
|------|------|---------|---------|---------|
| HQ | 10 | 10.1.10.0/24 | 10.1.10.1 | IT Department |
| HQ | 20 | 10.1.20.0/24 | 10.1.20.1 | HR Department |
| HQ | 30 | 10.1.30.0/24 | 10.1.30.1 | Guest WiFi |
| Branch | 40 | 10.2.40.0/24 | 10.2.40.1 | Sales Team |
| Branch | 50 | 10.2.50.0/24 | 10.2.50.1 | Management |
| VPN | - | 192.168.100.0/30 | - | Tunnel Network |
| WAN | - | 203.0.113.0/30 | - | Public Link |

---

## 🛠️ Technologies Used

- **Cisco IOS** – Router and switch operating system
- **GNS3** – Network simulation and emulation
- **Wireshark** – Packet capture and protocol analysis
- **OSPF** – Open Shortest Path First dynamic routing
- **IPSec** – Site-to-site VPN encryption
- **VLANs / Trunking (802.1Q)** – Layer 2 segmentation
- **ACLs** – Access Control Lists for traffic filtering
- **NAT/PAT** – Network Address Translation
- **DHCP** – Dynamic Host Configuration Protocol
- **SNMP v3** – Network monitoring
- **Syslog** – Centralized logging

---

## 🚀 Lab Setup Guide

### Prerequisites

- [GNS3](https://www.gns3.com/software/download) installed (free, Windows/Mac/Linux)
- Cisco IOSv / IOSv-L2 images (or use Cisco Packet Tracer as alternative)
- Wireshark (included with GNS3)
- Basic understanding of CLI and IP subnetting

### Step 1: Build the Topology in GNS3

1. Open GNS3 → Create new project → Name it `enterprise-network-lab`
2. Drag and drop the following devices:
   - 2x Routers (HQ-RTR, BR-RTR)
   - 2x Layer 3 Switches (HQ-SW-L3, BR-SW-L3)
   - 2x Layer 2 Switches (HQ-SW-ACC, BR-SW-ACC)
   - 1x Cloud/ NAT node (for internet simulation)
   - 6x VPCS hosts (or use real OS images for advanced testing)
3. Connect cables as shown in the topology diagram
4. Start all devices

### Step 2: Load Configurations

Configurations are stored in `/configs/` directory. Copy and paste them into each device via console.

```bash
# Example: Access HQ-RTR console in GNS3
# Right-click device → Console
# Paste the configuration from configs/hq-router.txt
```

### Step 3: Verify Connectivity

From any end host, run:

```bash
# Test inter-VLAN routing
ping 10.1.20.1        # Ping HR gateway from IT VLAN
ping 10.2.40.1        # Ping Branch Sales from HQ

# Test internet connectivity (simulated)
ping 8.8.8.8

# Test VPN tunnel
show crypto ipsec sa   # On router CLI
```

---

## 📁 Configuration Files

| File | Device | Description |
|------|--------|-------------|
| [`configs/hq-router.txt`](configs/hq-router.txt) | HQ-RTR | WAN, VPN, NAT, OSPF, ACLs |
| [`configs/hq-l3-switch.txt`](configs/hq-l3-switch.txt) | HQ-SW-L3 | VLANs, SVIs, DHCP, Trunking |
| [`configs/hq-access-switch.txt`](configs/hq-access-switch.txt) | HQ-SW-ACC | Port assignments, Port Security |
| [`configs/branch-router.txt`](configs/branch-router.txt) | BR-RTR | WAN, VPN, NAT, OSPF |
| [`configs/branch-l3-switch.txt`](configs/branch-l3-switch.txt) | BR-SW-L3 | VLANs, SVIs, DHCP |
| [`configs/branch-access-switch.txt`](configs/branch-access-switch.txt) | BR-SW-ACC | Port assignments, Port Security |

---

## 🧠 What I Learned

### 1. VLAN Segmentation Isn't Just "Best Practice" — It's Essential
Before this lab, I understood VLANs theoretically. Configuring them showed me how broadcast domains actually work and why putting Guest WiFi on the same VLAN as HR is a security nightmare.

### 2. Inter-VLAN Routing Has Multiple Methods
I implemented **Switch Virtual Interfaces (SVIs)** on the Layer 3 switch instead of router-on-a-stick. This is faster (hardware-switched) and scales better — a real enterprise approach.

### 3. OSPF Convergence Is Fast
Watching OSPF neighbor relationships form and routes propagate after a link failure taught me more than any textbook. The `show ip ospf neighbor` command became my best friend.

### 4. IPSec VPNs Are Finicky
The tunnel didn't come up on the first try. I had to debug mismatched ISAKMP policies, ACLs for interesting traffic, and NAT exemption rules. This taught me that **VPN troubleshooting is 80% of the job**.

### 5. ACLs Are Order-Sensitive
I learned the hard way that `permit` and `deny` statements are processed top-down. One misplaced line blocked legitimate VPN traffic for an hour.

### 6. Monitoring Gives You Superpowers
Setting up SNMP and Syslog meant I could see problems before users complained. In a real network, this is the difference between reactive and proactive IT.

---

## 🔧 Troubleshooting Log

| Problem | Symptom | Root Cause | Solution |
|---------|---------|------------|----------|
| VPN tunnel down | `show crypto isakmp sa` shows no SA | NAT was encrypting VPN traffic | Added NAT exemption ACL for VPN traffic |
| No inter-VLAN ping | Hosts couldn't reach other VLANs | SVI interfaces were shutdown | `no shutdown` on all SVI interfaces |
| OSPF neighbors stuck in INIT | Routers not forming adjacency | MTU mismatch between interfaces | Standardized MTU to 1500 on all links |
| DHCP not working | Clients got APIPA addresses | IP helper-address missing | Added `ip helper-address` on SVIs |
| Port security violation | Switch port err-disabled | MAC address changed on port | `errdisable recovery cause psecure-violation` |
| Guest VLAN reached HR server | ACL not filtering | ACL applied in wrong direction | Applied ACL inbound on SVI interface |

---

## 📸 Screenshots

*Add your own screenshots here when you build the lab:*

- `screenshots/gns3-topology.png` — Your built topology
- `screenshots/ping-success.png` — Successful cross-site ping
- `screenshots/wireshark-capture.png` — IPSec ESP packets captured
- `screenshots/ospf-neighbors.png` — OSPF neighbor table
- `screenshots/show-run.png` — Running config snippet

---

## 🔄 How to Replicate

```bash
# 1. Clone this repo
git clone https://github.com/YOUR_USERNAME/network-plus-lab.git
cd network-plus-lab

# 2. Open GNS3 and import the project
# File → Open Project → Select the .gns3 file

# 3. Load configs into each device
# See configs/ directory for all device configurations

# 4. Verify with the test commands in docs/verification.md
```

---

## 📜 License

This project is licensed under the MIT License — feel free to use, modify, and share.

---

## 🤝 Connect With Me

If you found this project useful or have suggestions for improvement, let's connect!

- LinkedIn: linkedin.com/in/khader-ali-adam-621397425
- Email: itdevelops.uk99@gmail.com

---

> **Note:** This lab was built as part of my CompTIA Network+ certification journey and my transition into IT/DevOps. All configurations are for educational purposes in a simulated environment.
