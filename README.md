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
  - WAN (External): Connected to the VirtualBox NAT network for internet access.
  - LAN (Internal): An isolated **internal virtual network** named "LabNet" with the `192.168.1.0/24` block to manage internal network traffic.
  - Gateway Configuration: Configured pfSense as the single exit point (Gateway) for the entire internal network, providing DHCP, DNS (`8.8.8.8`), and the local domain name `soclab.lan`.
- Installed **Kali Linux** and configured it as the SOC analysis platform
- Installed **Splunk Enterprise** on Kali Linux (4 GB of RAM)
  - Splunk `.deb` package was downloaded and installed using `dpkg`, and configured with `enable boot-start` to ensure it starts automatically upon system boot.
  - Data Ingestion Setup: TCP port `9997` (Indexing Port) was configured via the Splunk web interface (`localhost:8000`) to listen for incoming logs from external systems (Windows DC01).
- Deployed Target System & Domain Environment (**Windows Server DC01**)
  - **Windows Server 2022** was installed in **"Desktop Experience"** mode using an ISO and connected to the pfSense gateway with a static IP of `192.168.1.10`.
  - Active Directory Configuration: A new forest was created with the root domain name `soclab.lan`, and DC01 was promoted to the Domain Controller role. This enabled the generation of SYSVOL, NTDS, and Active Directory security logs.
- Installed and configured **Sysmon** (System Monitor) on DC01 for Enhanced Monitoring.
  - Advanced Endpoint Telemetry: Provides deep visibility (process command lines, etc.) in cases where standard Windows logs are insufficient.
  - Noise Reduction: To prevent complexity during analysis, SwiftOnSecurity's optimized `config.xml` file was used to ensure only critical security events are monitored.
- Configured **Splunk Universal Forwarder** on DC01 to send logs to Kali
  - Log Forwarding Agent Deployment: Defined the Kali Linux machine (`192.168.1.100:9997`) as the `"Receiving Indexer"`.
  - Log Configuration (`Inputs.conf`): The `inputs.conf` file was manually edited to ensure that Application, Security, System, and specifically Sysmon/Operational logs are sent to the `main` index.
  - Persistence: The `SplunkForwarder` service was restarted, and the log pipeline was verified using the `netstat` command.
- Validated end-to-end detection by generating events and analyzing them in Splunk
- SOC Dashboard panel was created in Splunk to categorize attack types for real-time monitoring.

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
