# 🔍 Verification & Testing Guide

After loading all configurations, run these tests to confirm the lab is working.

---

## 1. Basic Interface Check

On **every device**, run:

```
show ip interface brief
```

**Expected:** All interfaces should show `up/up` status.

---

## 2. VLAN Verification

On both access switches and L3 switches:

```
show vlan brief
```

**Expected:**
- VLAN 10 (IT), 20 (HR), 30 (Guest) on HQ switches
- VLAN 40 (Sales), 50 (Management) on Branch switches
- Ports correctly assigned to VLANs

---

## 3. Inter-VLAN Routing Test

From a PC in VLAN 10 (IT), ping:

```bash
ping 10.1.20.1        # HR Gateway
ping 10.1.30.1        # Guest Gateway
ping 10.2.40.1        # Branch Sales Gateway
ping 10.2.50.1        # Branch Management Gateway
```

**Expected:** All pings should succeed.

---

## 4. OSPF Neighbor Verification

On HQ-RTR and BR-RTR:

```
show ip ospf neighbor
```

**Expected:** Neighbor state should be `FULL`.

On any router:

```
show ip route ospf
```

**Expected:** Routes to all remote networks via OSPF.

---

## 5. VPN Tunnel Verification

On HQ-RTR:

```
show crypto isakmp sa
show crypto ipsec sa
```

**Expected:**
- ISAKMP SA shows `QM_IDLE` state
- IPSec SA shows encaps/decaps counters increasing

---

## 6. NAT / PAT Test

From any internal host:

```bash
ping 8.8.8.8
```

On HQ-RTR:

```
show ip nat translations
```

**Expected:** NAT table shows inside local IPs translated to WAN interface IP.

---

## 7. ACL / Security Test

From a Guest VLAN host (10.1.30.x), try to ping:

```bash
ping 10.1.10.50       # IT server - should FAIL
ping 10.1.20.50       # HR server - should FAIL
ping 8.8.8.8          # Internet - should SUCCEED
```

**Expected:** Guest cannot reach internal departments but can reach internet.

---

## 8. Port Security Test

On HQ-SW-ACC, check:

```
show port-security
show port-security interface GigabitEthernet0/1
```

**Expected:** Ports show `Secure-up` with sticky MAC addresses learned.

---

## 9. SNMP Test (Optional)

From a Linux host with `snmpwalk`:

```bash
snmpwalk -v3 -u admin -l authPriv -a SHA -A SNMP_AUTH_PASS -x AES -X SNMP_PRIV_PASS 10.1.99.1 1.3.6.1.2.1.1.1.0
```

**Expected:** Returns system description (hostname and IOS version).

---

## 10. Packet Capture with Wireshark

In GNS3, right-click any link → `Start Capture`.

Filter for:
- `ospf` — See hello packets and LSU updates
- `isakmp` — See VPN negotiation
- `esp` — See encrypted VPN traffic
- `icmp` — See ping packets across VLANs

---

## Troubleshooting Flowchart

```
No connectivity?
    │
    ├─→ Check interface status (up/down?)
    │   └─→ No? Check cables / no shutdown
    │
    ├─→ Check IP addressing (correct subnet?)
    │   └─→ No? Fix IP / subnet mask
    │
    ├─→ Same VLAN?
    │   ├─→ Yes: Check switchport access vlan
    │   └─→ No: Check SVI / routing / trunk
    │
    ├─→ Different site?
    │   ├─→ Check VPN tunnel (crypto isakmp sa)
    │   ├─→ Check OSPF routes (show ip route)
    │   └─→ Check NAT exemption ACL
    │
    └─→ Check ACLs (show access-lists)
        └─→ Is traffic being dropped?
```
