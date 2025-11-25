# Network Topology Documentation

This document provides a complete explanation of the **Enterprise HQ + Branch Network Topology**, including diagrams, interface mappings, VLAN layout, routing design, redundancy mechanisms, and security architecture.

# 📌 1. High-Level Topology Overview

The network consists of **Headquarters (HQ)**, a **Branch Office**, and an **ISP/Internet simulation**. All routing, switching, redundancy, and network services are modeled after real enterprise environments.

The topology includes:

* **R1 & R2** – HQ Core Routers (HSRP + OSPF + NAT)
* **SW1 & SW2** – HQ Access/Distribution Switches (LACP EtherChannel)
* **R3** – Branch Router
* **BR-SW1** – Branch Access Switch
* **ISP Router** – Internet simulation with loopbacks
* **Servers** – DHCP, TACACS+, Syslog/NTP
* **End-user PCs** – In respective VLANs

---

# 📡 2. HQ Topology Breakdown

```
               ┌────────────────────────┐
               │        Internet        │
               │       ISP Router       │
               └──────────┬────────────┘
                          │ 203.0.113.0/30
                   ┌──────┴──────┐
                   │      R1      │ (Primary)
                   └──────┬──────┘
                          │ 192.168.12.0/30
                   ┌──────┴──────┐
                   │      R2      │ (Secondary)
                   └──────┬──────┘
                          │ Trunk
                   ┌──────┴──────┐
                   │    SW1/SW2   │
                   │ LACP EtherChannel
                   └──────┬──────┘
      ┌───────────────────┼────────────────────┐
      │                   │                    │
    HR VLAN 10      Finance VLAN 20       Admin VLAN 30
```

---

# 🏢 3. Branch Topology Breakdown

```
                   ┌──────────────┐
                   │     R1       │
                   └──────┬──────┘
                          │ 192.168.13.0/30
                   ┌──────┴──────┐
                   │     R3      │
                   └──────┬──────┘
                          │ Trunk
                   ┌──────┴──────┐
                   │   BR-SW1    │
                   └──────┬──────┘
           ┌───────────────┼────────────────┐
           │               │                │
     VLAN 40 Mgmt     VLAN 50 Users     (Future VLANs)
```

---

# 🧩 4. VLAN Summary

## HQ VLANs

| VLAN ID | Name       | Subnet        | Gateway (HSRP VIP) |
| ------- | ---------- | ------------- | ------------------ |
| 10      | HR         | 10.10.10.0/24 | 10.10.10.1         |
| 20      | Finance    | 10.20.20.0/24 | 10.20.20.1         |
| 30      | Admin      | 10.30.30.0/24 | 10.30.30.1         |
| 99      | Management | 10.99.99.0/24 | 10.99.99.1         |

## Branch VLANs

| VLAN ID | Name         | Subnet        | Gateway    |
| ------- | ------------ | ------------- | ---------- |
| 40      | Branch MGMT  | 10.40.40.0/24 | 10.40.40.1 |
| 50      | Branch Users | 10.50.50.0/24 | 10.50.50.1 |

---

# 🔀 5. IP Addressing & Interface Mapping

### HQ Routers (R1 & R2)

| Device | Interface | Purpose   | IP/Subnet       |
| ------ | --------- | --------- | --------------- |
| R1     | G0/0.10   | VLAN10    | 10.10.10.2/24   |
| R2     | G0/0.10   | VLAN10    | 10.10.10.3/24   |
| R1     | G0/0.20   | VLAN20    | 10.20.20.2/24   |
| R2     | G0/0.20   | VLAN20    | 10.20.20.3/24   |
| R1     | G0/0.30   | VLAN30    | 10.30.30.2/24   |
| R2     | G0/0.30   | VLAN30    | 10.30.30.3/24   |
| R1     | G0/0.99   | MGMT      | 10.99.99.2/24   |
| R2     | G0/0.99   | MGMT      | 10.99.99.3/24   |
| R1     | G0/1      | Core Link | 192.168.12.1/30 |
| R2     | G0/1      | Core Link | 192.168.12.2/30 |
| R1     | G0/2      | ISP       | 203.0.113.2/30  |
| R1     | S0/2/0    | Branch    | 192.168.13.1/30 |
| R3     | S0/0/0    | HQ Link   | 192.168.13.2/30 |

### Branch Router R3

| Subinterface | VLAN | IP Address    |
| ------------ | ---- | ------------- |
| G0/0.40      | 40   | 10.40.40.1/24 |
| G0/0.50      | 50   | 10.50.50.1/24 |

### Switch Links

* **SW1 ↔ SW2**: LACP EtherChannel (G0/1 + G0/2)
* **SW1 ↔ R1**: Trunk
* **SW2 ↔ R2**: Trunk

---

# ♻️ 6. Redundancy Mechanisms

### **HSRP**

* VLANs 10, 20, 30, 99 use virtual gateways
* R1 = Active for most VLANs
* R2 = Standby

### **LACP EtherChannel**

* SW1-SW2 with Port-Channel1 for redundancy & load balancing

### **OSPF Area 0**

* All routers in backbone area
* MD5 authentication enabled

---

# 🌐 7. Routing Design

* **OSPF** is the primary IGP
* **Static routes** used on ISP router
* **NAT overload** configured on R1 (HQ edge) for Internet access
* Inter-VLAN routing via router-on-a-stick (R1, R2, R3)

---

# 🔒 8. Security Architecture

* AAA with TACACS+ on all routers & switches
* SSH-only management
* DHCP Snooping enabled
* DAI enabled
* BPDU Guard applied on access ports
* Port-Security enabled
* OSPF MD5 authentication everywhere
* Syslog + NTP enabled

---

# 🖼️ 9. Diagrams

(Insert exported Packet Tracer topology images here)

I can generate a **professional Visio-style diagram** if you want.

---

# 📑 10. Future Expansion

* IPsec VPN between HQ & Branch
* DMZ network for servers
* Wireless LAN controllers
* High availability DHCP/TACACS servers
