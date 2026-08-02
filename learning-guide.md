# Study Guide: Key Networking Concepts Demonstrated

## 1. VLANs (Virtual LANs)
**What:** Logical segmentation of a physical network into separate broadcast domains.
**Why it matters:** Without VLANs, all devices share one broadcast domain. A single misbehaving device floods the entire network. VLANs isolate departments, improve security, and reduce broadcast traffic.
**In this lab:** IT (VLAN 10), HR (VLAN 20), and Guest (VLAN 30) cannot see each other's broadcast traffic.

## 2. 802.1Q Trunking
**What:** A method to carry multiple VLANs over a single physical link between switches.
**Why it matters:** Without trunking, you'd need a separate cable for every VLAN between switches. Trunking tags frames with VLAN IDs so switches know which VLAN each frame belongs to.
**In this lab:** The link between HQ-SW-L3 and HQ-SW-ACC is a trunk carrying VLANs 10, 20, 30, and 99.

## 3. Inter-VLAN Routing (SVIs)
**What:** Switch Virtual Interfaces act as the default gateway for each VLAN, enabling communication between VLANs.
**Why it matters:** VLANs are isolated by design. To communicate between VLANs, you need a Layer 3 device (router or L3 switch) to route traffic.
**In this lab:** HQ-SW-L3 has SVIs at 10.1.10.1, 10.1.20.1, and 10.1.30.1. These are the gateways for each VLAN.

## 4. OSPF (Open Shortest Path First)
**What:** A link-state routing protocol that automatically learns and advertises network paths.
**Why it matters:** In a large network, manually configuring static routes is error-prone and doesn't adapt to failures. OSPF dynamically finds the best path and reroutes around failures.
**In this lab:** HQ-RTR and BR-RTR exchange OSPF updates. If the VPN tunnel fails, OSPF removes the route. When it comes back, OSPF reinstalls it.

## 5. IPSec VPN
**What:** Internet Protocol Security encrypts traffic between two sites over an untrusted network (the internet).
**Why it matters:** Branch offices need secure access to HQ resources. Sending sensitive traffic over the public internet unencrypted is a security breach waiting to happen.
**In this lab:** The tunnel between HQ and Branch encrypts all inter-site traffic using AES-256 and SHA-256.

## 6. NAT / PAT
**What:** Network Address Translation hides private IP addresses behind a public IP. PAT (Port Address Translation) allows many internal hosts to share one public IP by using different port numbers.
**Why it matters:** IPv4 addresses are scarce. NAT lets organizations use private IP ranges internally while presenting a single public IP to the internet.
**In this lab:** All internal hosts share the router's WAN IP (203.0.113.2 or 198.51.100.2) when accessing the internet.

## 7. Access Control Lists (ACLs)
**What:** Packet filters that permit or deny traffic based on source/destination IP, port, or protocol.
**Why it matters:** Firewalls and routers use ACLs to enforce security policies. "Guests shouldn't access HR servers" is implemented as an ACL.
**In this lab:** The Guest VLAN ACL denies traffic to IT and HR subnets while allowing internet access.

## 8. Port Security
**What:** A switch feature that restricts which MAC addresses can connect to a port.
**Why it matters:** Prevents unauthorized devices from connecting to the network. If someone plugs in a rogue access point or laptop, the switch can shut down the port.
**In this lab:** Access ports allow only 1-2 MAC addresses. Violations trigger port shutdown or restriction.

## 9. DHCP
**What:** Dynamic Host Configuration Protocol automatically assigns IP addresses, subnet masks, gateways, and DNS servers to devices.
**Why it matters:** Manually configuring IP addresses on hundreds of devices is impractical. DHCP automates this and prevents address conflicts.
**In this lab:** Each VLAN has a DHCP scope. Devices plug in and automatically receive an IP in the correct subnet.

## 10. SNMP & Syslog
**What:** SNMP (Simple Network Management Protocol) collects performance metrics. Syslog collects log messages from network devices.
**Why it matters:** You can't manage what you can't see. Monitoring tools alert you to high CPU, interface errors, or security events before they become outages.
**In this lab:** Both routers send SNMP data and Syslog messages to a central monitoring server.

---

## Exam Relevance

These concepts map directly to CompTIA Network+ exam objectives:

| N10-008 Objective | Covered In Lab |
|-------------------|----------------|
| 1.1 - OSI Model | All layers demonstrated |
| 1.2 - Network Topologies | Hub-and-spoke with redundancy |
| 1.4 - IP Addressing | Subnetting across 5 VLANs |
| 2.1 - Switching | VLANs, trunking, port security |
| 2.2 - Routing | OSPF, static routes, default routes |
| 2.3 - Network Ports & Protocols | DHCP, DNS, SNMP, Syslog |
| 3.1 - Network Security | ACLs, VPN, port security |
| 3.2 - Network Hardening | SSH, password encryption |
| 4.1 - Network Troubleshooting | Structured troubleshooting log |
| 4.3 - Infrastructure | Cabling, device placement |
