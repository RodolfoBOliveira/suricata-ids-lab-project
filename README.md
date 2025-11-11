# 🔬 Laboratory Threat Detection Analysis with Suricata

This repository contains the project materials developed for the Security course unit (2024/2025) at the Coimbra Institute of Engineering (ISEC).

The primary goal was to build a lab environment to simulate network attacks and analyze the effectiveness of Suricata as an Intrusion Detection System (IDS).

---

## 🎯 Project Objective

The objective of this laboratory was to gain a practical understanding of Suricata's operation and function in IDS mode. We focused on:

* Building a controlled network testbed in GNS3.
* Simulating various common attack vectors.
* Analyzing how Suricata detects threats and generates alerts.
* Evaluating the system's performance under stress (e.g., DoS attacks).

---

## 🔬 Testbed Architecture

The topology was created in GNS3 to simulate a simplified corporate network ("green area") and a hostile external network ("red area").

<img width="1147" height="657" alt="image" src="https://github.com/user-attachments/assets/d9b8acaf-3374-4f1b-8a7a-efadbaf4e3a2" />


* **Monitoring:** A Cisco IOU L2 switch was configured as a SPAN-CentralSwitch.
* **SPAN (Port Mirroring):** The switch mirrored all traffic passing between Router1 (internal) and Router2 (external) to the IDS machine (Ubuntu with Suricata).
* **Machines:**
    * **Attacker:** Kali Linux VM.
    * **IDS:** Ubuntu VM with Suricata.
    * **Targets:** Various VPCS (Virtual PCs) and routers.

---

## ⚡ Attack Simulation and Analysis

We used the Kali Linux machine to execute various attacks and test Suricata's detection capabilities.

| Attack Type | Tool(s) | Target | Simulation Description |
| :--- | :--- | :--- | :--- |
| **DoS/DDoS** | `hping3` | Terminal-B2 (192.168.1.2) | 1. **DDoS:** SYN Flood with random source IPs (`--rand-source`).<br>2. **DoS:** SYN Flood with a single source IP (Kali). |
| **Reconnaissance** | `snmpwalk` | Router1 (10.0.0.1) | Attempt to enumerate device information using the default `"public"` community string. |
| **Port Scanning** | `nmap` | Kali (10.0.1.1) | TCP SYN Scan (`nmap -sS`) to discover open ports. |
| **Brute Force** | `Hydra` | Router1 (10.0.0.1) | Attempt to guess Telnet (port 23) credentials using common combinations (e.g., `"admin"`/`"1234"`). |

---

## 📊 Key Findings

* **Detection Efficacy:** Suricata proved to be highly effective in detecting all simulated attack types in real-time, generating immediate alerts.
* **Rule Analysis:** We were able to identify the specific signatures (SIDs) that were triggered for each attack:
    * **SNMP:** `sid: 2101411` (GPL SNMP public access udp).
    * **Telnet Brute Force:** `sid: 2060090` (ET EXPLOIT Zyxel... Default Credentials).
    * **Port Scan (Nmap):** `sid: 2200074` (SURICATA TCPv4 invalid checksum).
* **Performance Impact:** Suricata's performance can be affected by high traffic volume. During the DoS/DDoS tests, CPU usage increased from ~50% (baseline) to ~80% (under attack), which is a critical consideration for large-scale deployments.

---

## 🛠️ Technologies and Tools Used

* **IDS/IPS:** Suricata
* **Network Simulation:** GNS3
* **Virtualization:** VirtualBox (or VMware)
* **Operating Systems:** Kali Linux, Ubuntu Server/Desktop
* **Network Devices:** Cisco IOU Images (L2/L3)
* **Attack Tools:** `hping3`, `snmpwalk`, `nmap`, `Hydra`

---

## 📁 Repository Contents

* `Laboratory Report Intrusion Detection Analysis with Suricata.pdf`: The detailed final lab report, covering the configuration, experiments, and in-depth analysis.
* `Threat Detection Analysis with Suricata.pptx`: The project presentation slides.

---

## 👤 Authors

* Henrique Dias Neves Simões Ferreira
* João Pedro Vila Pomar
* Rodolfo Miguel de Sousa Belchior Brás Oliveira
