# Task 1: Scan Your Local Network for Open Ports

## Objective
The goal of this task is to discover open ports on devices within the local network to understand network service exposure and analyze potential security risks.

## Tools & Methods Used
- **Tool:** Nmap
- **Scan Type:** TCP SYN Scan (`-sS`)
- **Target IP Range:** 192.168.29.0/24

## Command Executed
sudo nmap -sS -oN intern_scan.txt 192.168.29.0/24




-----------------------------------------------------------------------------------------------------------------------------




## Scan Findings & Analysis

The network reconnaissance discovered **3 active hosts** within the targeted range `192.168.29.0/24`. Below is an in-depth technical analysis of each host, its security posture, and associated risks:

### 1. Host `192.168.29.71`
- **Status:** Active
- **Scan Result:** All 1000 ports are **Filtered**.
- **Technical Analysis:** When Nmap sent TCP SYN packets to this host, it received no response (neither a SYN-ACK nor a RST). This implies that a packet filter or a firewall (such as Windows Defender or network rules) is actively monitoring the traffic and dropping the packets before they reach the target ports. 
- **Security Posture:** Highly secure attack surface. Since the ports are filtered, external attackers cannot easily enumerate the services or OS running on this machine, minimizing the risk of targeted exploits.

### 2. Host `192.168.29.119`
- **Status:** Active
- **Scan Result:** **Port 80/tcp** is **Open** (Service: `http`).
- **Technical Analysis:** Port 80 is hosting an active web server utilizing the HTTP protocol. 
- **Security Risk & Vulnerability (Critical):** HTTP is an unencrypted, clear-text protocol. It does not use any cryptographic wrapping (like SSL/TLS). If a user transmits confidential data (such as login credentials or session tokens) to this server, the data travels in plain text over the local network. An attacker conducting a Man-in-the-Middle (MITM) attack using packet sniffing tools like Wireshark can easily intercept and view this data without needing to crack any encryption.
- **Remediation Plan:** 
  1. If the web service is mandatory, it must be migrated to **HTTPS (Port 443)** by implementing an SSL/TLS certificate to secure data in transit.
  2. If the service is redundant, the port should be closed or restricted using firewall rules.

### 3. Host `192.168.29.187` (My Machine)
- **Status:** Active
- **Scan Result:** All 1000 ports are **Closed**.
- **Technical Analysis:** For every SYN packet sent by Nmap, this machine responded immediately with a **RST (Reset)** packet. This confirms that while the host is alive, there are no active network services or applications listening for incoming connections on these 1000 standard ports.
- **Security Posture:** Safe and minimal attack surface. With zero open services exposed to the local network, the host is well-protected against remote execution vulnerabilities or unauthorized entry points through network ports.



----------------------------------------------------------------------------------------------------------------------------



## Interview Questions & Answers

### 1. What is an open port?
An open port is a network port that is actively accepting incoming packets. It indicates that an application or service is actively listening on that port for connections.

### 2. How does Nmap perform a TCP SYN scan?
Nmap sends a SYN packet to the target port. If it receives a SYN-ACK, the port is open, and Nmap immediately sends a RST packet to close the connection before the full 3-way handshake completes (hence called a half-open scan). If it receives a RST, the port is closed.

### 3. What risks are associated with open ports?
Open ports can expose vulnerable or outdated services. If a service running on an open port has flaws or misconfigurations, attackers can exploit it to compromise the system or gain unauthorized access.

### 4. Explain the difference between TCP and UDP scanning.
TCP scanning is connection-oriented, utilizing flags like SYN and ACK, making it fast and reliable. UDP scanning is connectionless, relies on waiting for timeouts or ICMP port unreachable messages, making it significantly slower and harder to enumerate accurately.

### 5. How can open ports be secured?
Open ports can be secured by disabling unnecessary services, implementing firewall rules to restrict access, enforcing strong authentication mechanisms, and regularly patching or updating the software running on those ports.

### 6. What is a firewall's role regarding ports?
 A firewall filters incoming and outgoing network traffic based on security rules. It can block or filter access to specific ports, preventing unauthorized external entities from interacting with internal open ports.

### 7. What is a port scan and why do attackers perform it?
An attack surface exploration technique used to discover active hosts and open ports on a network. Attackers perform it to map the network structure and find potential entry points or vulnerabilities in the target infrastructure.

### 8. How does Wireshark complement port scanning?
Wireshark captures raw network packets, allowing analysts to visually inspect the exact packets (like SYN, SYN-ACK, RST) generated during an Nmap scan. This helps verify scan techniques, analyze responses, and debug network behaviors.
