# 📝 LinkedIn Post Templates

## Template 1: Project Launch Post

---

🚀 Just completed my **Enterprise Network Security Lab** — a hands-on project simulating a multi-site corporate network from scratch.

Here's what I built:
✅ 5 VLANs across HQ + Branch office
✅ Inter-VLAN routing with Layer 3 switches
✅ OSPF dynamic routing protocol
✅ Site-to-Site IPSec VPN tunnel
✅ NAT/PAT for internet access
✅ ACLs for network segmentation
✅ Port Security on access switches
✅ SNMP v3 + Syslog monitoring

All done in GNS3 with real Cisco IOS configurations. No simulators — real CLI, real troubleshooting, real learning.

The best part? I documented every failure in the troubleshooting log. The VPN didn't come up on the first try. OSPF neighbors got stuck in INIT. Guest VLAN could reach HR servers because of a misplaced ACL line.

Every broken config taught me more than a chapter in a textbook.

📂 Full project on GitHub: [LINK]

#CompTIA #NetworkPlus #Cisco #Networking #ITLab #DevOps #CareerChange #HandsOnLearning

---

## Template 2: "What I Learned" Post

---

5 things building a network lab taught me that books couldn't:

1️⃣ **VLANs are useless without proper trunking.** I spent an hour wondering why VLAN 30 couldn't reach the internet. The trunk port wasn't allowing VLAN 30. Theory says "trunk carries all VLANs." Practice says "not if you don't configure it."

2️⃣ **OSPF is fast, but only if everything matches.** MTU mismatch = no adjacency. Area mismatch = no adjacency. Authentication mismatch = no adjacency. Routing protocols are picky.

3️⃣ **VPNs are 20% configuration, 80% troubleshooting.** ISAKMP policy, transform set, crypto ACL, NAT exemption — one mismatch and the tunnel stays down. The debug logs are your only friend.

4️⃣ **ACL order matters.** `permit` before `deny` isn't just a suggestion. One line out of place and your security policy is a suggestion, not a rule.

5️⃣ **Monitoring isn't optional.** SNMP and Syslog felt like "nice to have" until I had to figure out why a port went err-disabled. Without logs, you're guessing.

Building things breaks your assumptions. That's the point.

Check out the full lab on my GitHub: [LINK]

#NetworkEngineering #CompTIA #Cisco #ITSkills #LabEveryday

---

## Template 3: Short Update Post

---

Weekend project: Built a simulated enterprise network with Cisco routers, Layer 3 switches, VLANs, OSPF, and a site-to-site VPN.

Troubleshooting log is 6 entries long. Every single one made me better.

GitHub repo in bio 🔗

#NetworkPlus #ITLab #AlwaysLearning

---

## Tips for Posting

- Add screenshots from YOUR actual lab to make it authentic
- Tag people in the networking community
- Post on Tuesday-Thursday for best engagement
- Respond to every comment — engagement boosts reach
- Pin the project launch post to your profile
