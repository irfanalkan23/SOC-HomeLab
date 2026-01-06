# SOC Homelab Project

This project documents the design and implementation of a **Security Operations Center (SOC) Home Lab** built to simulate real-world enterprise security monitoring, attack detection, and log analysis workflows.

The lab focuses on **network segmentation, firewalling, endpoint logging, and centralized SIEM analysis**, following SOC and Blue Team best practices.

---

## 🧭 Network Topology

![SOC Homelab Network Topology](images/soc-homelab-network-topology.png)

**Internal Network:** `192.168.1.0/24`  
All systems communicate through a pfSense firewall acting as the gateway.

---

## 🧩 Lab Components

### pfSense – Gateway & Firewall
- Acts as the **single entry/exit point** between WAN and the internal network
- WAN: `10.0.2.x` (VirtualBox NAT)
- LAN: `192.168.1.1`
- Provides network isolation, routing, and traffic control

### Kali Linux – Attacker & SOC Console
- IP Address: `192.168.1.100`
- Used for:
  - SOC analysis and investigation
  - Hosting **Splunk Enterprise** (SIEM)
- Receives and analyzes logs from internal systems

### Windows DC01 – Target & Log Generator
- IP Address: `192.168.1.10`
- Roles and services:
  - Active Directory Domain Controller
  - Sysmon for detailed endpoint telemetry
  - Splunk Universal Forwarder for log forwarding

---

## 🔄 Log & Traffic Flow

- Windows DC01 generates security telemetry using **Sysmon**
- Logs are forwarded via **Splunk Universal Forwarder** to **Splunk Enterprise on Kali Linux**
- Bidirectional communication allows:
  - Attack simulation
  - Log ingestion
  - SOC-style investigation and correlation
- pfSense enforces routing and network boundaries between WAN and LAN

---

## 🛠️ Implementation Steps

- Installed **Oracle VirtualBox** to host all virtual machines
- Deployed **pfSense** with dual interfaces (WAN/LAN) to enforce network segmentation
- Created an isolated **internal network (192.168.1.0/24)**
- Installed **Kali Linux** and configured it as the SOC analysis platform
- Installed **Splunk Enterprise** on Kali Linux
- Deployed **Windows Server DC01** with Active Directory
- Installed and configured **Sysmon** on DC01
- Configured **Splunk Universal Forwarder** to send logs to Kali
- Validated end-to-end detection by generating events and analyzing them in Splunk

---

## 🎯 Skills Demonstrated

- SOC lab design and architecture
- Firewall and network segmentation (pfSense)
- Windows endpoint telemetry (Sysmon)
- Centralized log collection and analysis (Splunk)
- Attack simulation and detection workflow
- Blue Team / SOC investigation mindset

---

## 📌 Purpose

This homelab is designed for:
- SOC Analyst skill development
- Hands-on Blue Team practice
- Detection engineering fundamentals
- Interview and portfolio demonstration

---

## 📄 Notes

This environment is fully isolated and used strictly for **educational and defensive security purposes**.
