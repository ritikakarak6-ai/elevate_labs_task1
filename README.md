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


**Scan Findings & Analysis**

The scan discovered 3 active hosts in the network:

1. Host 192.168.29.71: All 1000 ports are filtered (protected by a firewall or network rules).

2. Host 192.168.29.119:
       **Open Port:** 80/tcp (Service: http)
       **Security Risk:** Port 80 runs unencrypted HTTP traffic. Anyone sniffing the local network could potentially                         intercept the data or credentials passing through this service. It should be upgraded to HTTPS (Port 443) or                 closed if unnecessary.

3. Host 192.168.29.187 (My Machine): All 1000 ports are closed, indicating minimal network exposure from this specific node.



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
