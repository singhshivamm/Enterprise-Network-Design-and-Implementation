# Security Features Documentation

This document outlines all security mechanisms implemented across the **Enterprise HQ + Branch Network Infrastructure Project**, following Cisco best practices and industry standards. This includes switch security, AAA, routing security, management-plane protections, and network service hardening.

---

# 🔐 1. AAA Security – TACACS+ & Local Fallback

AAA is implemented across **all routers and switches** using TACACS+ with local fallback.

### **Features Implemented:**

* `aaa new-model`
* Centralized authentication using **TACACS+ server (10.99.99.11)**
* Privilege level control (admin = 15, operator = 5)
* Exec authorization via TACACS+
* Local fallback for resilience

### **Benefits:**

* Centralizes user management
* Supports accounting & auditing
* Ensures secure administrative access

---

# 🔒 2. Secure Remote Access (SSH Only)

Telnet is disabled across all devices.

### **Configured:**

* SSH v2
* RSA key generation
* Domain name configured for SSH
* VTY lines restricted to SSH-only
* ACL restricting management access

### **Benefits:**

* Encrypted management traffic
* Granular access control
* Eliminates insecure protocols

---

# 📘 3. Switchport Security (Port-Security)

Implemented on **all access ports** on HQ and Branch switches.

### **Configured:**

* Maximum MACs per port: `2`
* Sticky MAC learning
* Violation mode: `restrict`
* Disabled unused ports

### **Benefits:**

* Prevents MAC flooding attacks
* Prevents unauthorized device connections
* Limits lateral movement

---

# 🔍 4. DHCP Snooping (Layer 2 Security)

Enabled globally on SW1, SW2, and BR-SW1.

### **Configured:**

* DHCP Snooping on user VLANs
* Trunk ports to routers marked as *trusted*
* Rate limiting on access ports

### **Benefits:**

* Blocks rogue DHCP servers
* Protects users from man-in-the-middle attacks

---

# 🛡️ 5. Dynamic ARP Inspection (DAI)

Enabled on VLANs where DHCP Snooping is active.

### **Configured:**

* DAI on VLANs (HQ: 10,20,30,99 / Branch: 40,50)
* Trunk ports marked *trusted*

### **Benefits:**

* Protects against ARP spoofing
* Prevents ARP poisoning attacks

---

# 🧯 6. BPDU Guard

Enabled on all **access ports** across all switches.

### **Benefits:**

* Protects STP topology from rogue switches
* Auto-shutdown on BPDU reception

---

# 🧪 7. STP Hardening

HQ SW1 & SW2 configured with:

* STP root priority tuning
* Rapid convergence using PortFast
* BPDU Guard

### **Benefits:**

* Stable L2 topology
* Fast recovery for end hosts

---

# 🔑 8. Routing Security (OSPF Authentication)

All router interfaces participating in OSPF have:

* **MD5 authentication**
* Area-level MD5 authentication
* PPP encapsulation on serial WAN link for R3

### **Benefits:**

* Prevents rogue neighbors
* Ensures route integrity

---

# 🌐 9. NAT Security

Implemented on R1:

* `ip nat inside` and `ip nat outside` defined
* ACL for inside networks
* Overload PAT for safe Internet access

**Benefits:**

* Hides internal addressing
* Prevents direct inbound traffic

---

# 📡 10. Syslog + NTP Security

All routers & switches send logs to:

* **Syslog server (10.99.99.12)**
* Use NTP for timestamp accuracy

### **Benefits:**

* Central log correlation
* Supports security audits
* Accurate time for event troubleshooting

---

# 📍 11. Access Control (VTY Security)

VTY lines across all routers/switches:

* Use access-class ACL (`VTY_LOGIN`)
* Allow only management & trusted VLANs

### **Benefits:**

* Prevents unauthorized remote access
* Restricts management to internal subnets

---

# 🧱 12. Device Hardening

### Applied on all routers & switches:

* Disable CDP on external interfaces
* Disable unused ports
* Service password encryption
* MOTD banner
* Exec-timeout
* Logging synchronous
* No IP domain-lookup
* SSH keys generated

---

# ⭐ Summary of Security Layers

| Layer         | Security Feature                              |
| ------------- | --------------------------------------------- |
| L2            | Port Security, DHCP Snooping, DAI, BPDU Guard |
| L3            | OSPF MD5 Authentication, HSRP Security        |
| Mgmt Plane    | SSH, AAA/TACACS+, VTY ACL, Syslog, NTP        |
| Control Plane | STP Hardening                                 |
| Data Plane    | NAT, VLAN segmentation                        |

---
