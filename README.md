# 🛡️ pfSense High Availability (HA) Lab

A comprehensive step-by-step guide and documentation for configuring **High Availability (HA)** on pfSense using **CARP**, **XMLRPC Configuration Synchronization**, and **Advanced Outbound NAT**.

---

## 📌 Project Overview

This lab demonstrates how to build a redundant, highly available firewall infrastructure. In a production environment, this setup ensures zero downtime by seamlessly failing over network traffic from a Master node to a Backup node if a hardware or network failure occurs.

---

## 🛠️ Infrastructure & Topology

- **Hypervisor:** VMware Workstation
- **Firewall Nodes:** 2x pfSense Virtual Machines (Master & Backup)
- **VIP (Virtual IPs):** Managed via CARP for WAN, LAN, and DMZ interfaces
- **Synchronization:** XMLRPC over a dedicated Sync interface (pfsync)

### Network Interfaces & Addressing Scheme

| Interface | Master IP | Backup IP | Virtual IP (CARP) | Subnet Mask |
| :--- | :--- | :--- | :--- | :--- |
| **WAN** | `192.168.253.10` | `192.168.253.20` | `192.168.253.200` | `/24` |
| **LAN** | `192.168.1.2` | `192.168.1.3` | `192.168.1.1` | `/24` |
| **DMZ** | `192.168.2.2` | `192.168.2.3` | `192.168.2.1` | `/24` |
| **SYNC** | `10.10.10.1` | `10.10.10.2` | — | `/30` |

---

## 🚀 Key Features Configured

1. **CARP (Common Address Redundancy Protocol):**
   - Configured Virtual IPs for WAN, LAN, and DMZ.
   - Assigned appropriate skew values (`0` for Master, `100` for Backup) to control mastership.

2. **State & Config Synchronization (XMLRPC & pfsync):**
   - Configured high-availability sync over a dedicated point-to-point interface.
   - Synchronized firewall rules, NAT configurations, aliases, and system settings automatically from Master to Backup.

3. **Outbound NAT Redundancy:**
   - Switched from Automatic NAT to **Hybrid / Manual Outbound NAT**.
   - Mapped internal traffic (LAN/DMZ) to the **WAN CARP VIP** (`192.168.253.200`) instead of individual interface IPs to maintain seamless outbound connectivity during failover.

---

## 🔍 Troubleshooting & Key Learnings

During the implementation, an XMLRPC sync error occurred due to protocol mismatch on port 443:
- **Issue:** Master node was trying to communicate via `HTTP` while the Backup node was listening on `HTTPS`.
- **Root Cause:** Inconsistent webConfigurator protocol settings between nodes.
- **Resolution:** Explicitly configured HTTPS protocol and port synchronization across both nodes, successfully resolving the XMLRPC authentication error.

---

## ✅ Failover & Validation Tests

- **CARP Status Check:** Verified that Master holds `MASTER` status on all CARP interfaces while Backup stays in `BACKUP` mode.
- **Config Sync Verification:** Created rules/aliases on Master and confirmed instant replication to Backup.
- **Simulated Host Failure:** Powered down Master node; VIPs instantly transitioned to Backup node with zero dropped connections.

---

## 📄 Documentation PDF

You can find the full detailed lab report with screenshots in the repository:
➡️ [`pfSense_HA_Lab_Report.pdf`](./pfSense_HA_Lab_Report.pdf)

---

## 👤 Author

**[اسمك ولقبك]**
- **Email:** your.email@domain.com
- **LinkedIn:** [linkedin.com/in/yourprofile](https://linkedin.com/in/yourprofile)
- **GitHub:** [github.com/yourusername](https://github.com/yourusername)
