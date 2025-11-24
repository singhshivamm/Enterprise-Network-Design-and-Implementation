# Enterprise-Network-Design-and-Implementation
Full enterprise-grade network deployment simulated in Cisco Packet Tracer, featuring OSPF routing, HSRP redundancy, VLAN segmentation, LACP etherchannels, DHCP, NAT, AAA (TACACS+), Syslog, NTP, DHCP Snooping, DAI, Port Security, BPDU Guard, and branch site integration.
Enterprise Network Design & Implementation (Cisco Packet Tracer)

📌 Overview
This project simulates a full enterprise-grade network architecture using Cisco routers, switches, AAA servers, and security best practices.
Designed to reflect real-world corporate networking, the project includes HQ and Branch sites, redundant gateways, secure Layer-2 controls, scalable routing, and centralized authentication.

🖥️ Network Features
⭐ Core Routing
OSPFv2 Area 0 (with MD5 authentication)
Default route injection
Redundant paths between routers

⭐ Gateway Redundancy
HSRP for VLANs 10, 20, 30, 99
R1 = Active
R2 = Standby
Millisecond failover timers

⭐ VLAN & Switching
VLANs: 10 (HR), 20 (IT), 30 (Servers), 99 (MGMT), 40/50 Branch
LACP EtherChannel between SW1–SW2
Trunking across switches and routers

⭐ DHCP
Central DHCP server with:
Scopes for all VLANs
IP Helper configured on R1/R2/R3
DHCP Snooping binding

⭐ Security
AAA with TACACS+ for device login
RADIUS-style centralized authentication
Port security on access ports
BPDU Guard
DHCP Snooping
Dynamic ARP Inspection
SSHv2 only (Telnet disabled)
OSPF MD5 Auth
Management ACLs
NTP + Syslog

⭐ Branch Integration
Sub-interfaces on R3
VLAN 40/50 for branch PCs and users
OSPF advertisement back to HQ
DHCP + security controls at branch switch

⭐ NAT
PAT overload to simulated ISP (203.0.113.1)
NAT inside/outside zones defined


📂 Repository Structure

/configs        → Running configs of all routers & switches
/topology       → Packet Tracer file + Diagrams
/screenshots    → Validation screenshots (pings, neighbors, NAT, HSRP)
/documentation  → Detailed technical documentation

📚 Technical Skills Demonstrated

Enterprise network design (CCNA/CCNP-level)
Layer-2 security implementation
Routing (OSPF)
Redundant gateway design
Authentication & authorization (AAA/TACACS+)
NAT & branch connectivity
Production-style hardening practices
