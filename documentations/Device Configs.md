# Device Configurations Summary

This document provides a **clean, professional, and structured summary** of all device configurations used in the Enterprise Network Lab. It does not include full running configs (those remain in the `/configs` folder or your uploaded files), but instead provides a **simplified overview** of:

* Interface roles
* Routing configuration
* VLAN assignments
* Security features
* AAA/SSH settings
* Special configurations

This format is ideal for GitHub, LinkedIn, and resume portfolio documentation.

---

# 📘 1. HQ Router R1 — Core & Edge Router

### **Routing**

* OSPF Area 0 with MD5 authentication
* Default route to ISP
* HSRP active gateway for most VLANs

### **Interfaces**

* `G0/0.x` → VLAN subinterfaces (10, 20, 30, 99)
* `G0/1` → Link to R2
* `G0/2` → ISP connection
* `S0/2/0` → Link to Branch R3

### **Services**

* NAT overload on `G0/2`
* DHCP relay for all HQ VLANs
* AAA via TACACS+
* SSH-enabled (v2)
* Syslog + NTP configured

### **Security**

* OSPF MD5 auth
* SSH only
* TACACS+ AAA
* ACL for NAT inside networks
* VTY access control

---

# 📘 2. HQ Router R2 — Redundant Core Router

### **Routing**

* OSPF Area 0 with MD5
* HSRP standby router for VLANs
* Default route pointing to R1

### **Interfaces**

* `G0/0.x` VLAN subinterfaces like R1
* `G0/1` link to R1
* `G0/2` unused

### **Security**

* AAA via TACACS+
* SSH v2 only
* VTY access list
* Syslog + NTP
* OSPF authentication

---

# 📘 3. Branch Router R3 — Branch Edge Router

### **Routing**

* OSPF Area 0 with MD5
* Default route pointing to R1

### **Interfaces**

* `G0/0.40` → Branch MGMT VLAN
* `G0/0.50` → Branch Users VLAN
* `S0/0/0` → Link to R1 (PPP + OSPF auth)

### **Security**

* TACACS+ AAA
* SSH-only
* DHCP relay to 10.99.99.10
* Syslog + NTP
* VTY access control ACL

---

# 📘 4. ISP Router — Internet Simulation

### **Functions**

* Provides external network (8.8.8.8, 1.1.1.1)
* Static routes back to HQ
* No AAA
* No VLANs

### **Interfaces**

* `G0/0` → HQ R1 (203.0.113.0/30)
* Loopbacks for global DNS simulation

---

# 📘 5. HQ Switch SW1 — Primary Access/Distribution

### **Layer 2 Features**

* VLANs: 10, 20, 30, 99
* EtherChannel (LACP) with SW2
* Trunk to R1

### **Security**

* Port Security (sticky MAC, restrict)
* DHCP Snooping
* Dynamic ARP Inspection
* BPDU Guard on all access ports
* STP root bridge for all VLANs
* AAA via TACACS+
* SSH-only
* Syslog + NTP

---

# 📘 6. HQ Switch SW2 — Secondary Access/Distribution

### **Layer 2 Features**

* VLANs: 10, 20, 30, 99
* EtherChannel with SW1
* Trunk to R2

### **Security**

* DHCP Snooping
* Port Security
* BPDU Guard
* SSH-only
* AAA via TACACS+
* Syslog + NTP

---

# 📘 7. Branch Switch BR-SW1 — Branch Access Switch

### **Layer 2 Features**

* VLANs 40 (MGMT) and 50 (Users)
* Trunk to R3

### **Security**

* DHCP Snooping (trusted trunk)
* DAI
* Port Security (sticky, restrict)
* BPDU Guard on access ports
* AAA + SSH
* Syslog + NTP

---

# 📘 8. DHCP / TACACS / Syslog+NTP Servers

### **DHCP Server**

Scopes:

* VLAN10 → 10.10.10.0/24
* VLAN20 → 10.20.20.0/24
* VLAN30 → 10.30.30.0/24
* VLAN99 → 10.99.99.0/24
* VLAN40 → 10.40.40.0/24
* VLAN50 → 10.50.50.0/24

### **TACACS+**

* Users: admin (15), operator (5)
* Shared key: Cisco123

### **Syslog/NTP**

* Receives logs from all routers & switches
* NTP server for entire enterprise
