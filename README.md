# Insider Threat's Lateral Movement Simulation, Detection & Identification using NetworkMiner3.0 & Zeek

##  Project Overview

This project simulates a **realistic insider threat scenario** in which an employee, “Alex”, begins with normal file access and escalates into a **multi-stage attack chain**, including port scanning, SSH brute-forcing, and log clearing.

All network activity was captured, analyzed, and reconstructed using **Zeek** and **NetworkMiner** to demonstrate how security analysts can **detect, investigate, and document** such incidents.

##  Objectives

- Simulate **insider threat behavior** from reconnaissance to lateral movement.
- Capture **full packet data** for post-incident forensic analysis.
- Detect malicious patterns using **Zeek logs**.
- Extract and visualize evidence with **NetworkMiner**.
- Map activity to **MITRE ATT&CK techniques**.

##  Lab Environment

| Component            | Role / Purpose                                  | Example IP       |
| -------------------- | ----------------------------------------------- | ---------------- |
| **Windows 10 Host**  | Victim machine with HR file share & SSH enabled | `192.168.56.101` |
| **Kali Linux VM**    | Attacker machine simulating insider workstation | `192.168.56.102` |
| **NetworkMiner 3.0** | Passive network forensics tool                  | N/A              |
| **Zeek**             | Network traffic analysis & logging              | N/A              |


##  Tools & Technologies

- **Zeek** – Network security monitoring and log generation
- **NetworkMiner** – File & credential extraction, session reconstruction
- **tcpdump** – Packet capture
- **nmap** – Port scanning
- **hydra** – Brute-force attack simulation
- **SMBclient** – Accessing network shares

## Attack Chain Simulation

- **Initial Access** – Insider “Alex” connects to HR file share and downloads sensitive file.
- **Discovery – Nmap** scan to enumerate open services on victim host.
- **Credential Access** – Hydra brute-force attack on SSH service.
- **Lateral Movement** – Successful SSH login into victim host.
- **Defense Evasion** – Security event logs cleared (inside encrypted SSH session).

##  Detection & Analysis Workflow

- **Zeek:**

    - `notice.log` → Alert for SSH::Password_Guessing
    - `ssh.log` → Multiple failed logins followed by a success
    - `smb_files.log` → File retrieval activity from HR share
    - `conn.log` → Port scan activity and connection patterns

- **NetworkMiner:**

    - *Files Tab* → Extracted Employee_Salaries_Q3.txt
    - *Hosts Tab* → Mapped attacker/victim IPs & open ports
    - *Credentials Tab* → Captured SMB login credentials
    - *Sessions Tab* → Timeline of SMB & SSH connections

##  MITRE ATT&CK Mapping
 

| Stage                | Technique                                   | ID            |
| -------------------- | ------------------------------------------- | ------------- |
| File Share Access    | Data from Information Repositories          | **T1213**     |
| Port Scan            | Network Service Scanning                    | **T1046**     |
| SSH Brute Force      | Brute Force: Password Guessing              | **T1110.001** |
| Successful SSH Login | Remote Services: SSH                        | **T1021.004** |
| Clear Logs           | Indicator Removal: Clear Windows Event Logs | **T1070.001** |

##  Challenges & Solutions

- **Encrypted Traffic Limitation** – Could not see post-login commands in SSH; solved by correlating events before/after login.
- **Detection Gaps** – Inferred suspicious behavior from missing logs and verified with host telemetry.


##  Key Learnings

- How to **trace attack chains** by linking seemingly separate events.
- Using Zeek to **transform raw PCAPs into searchable evidence**.
- Leveraging NetworkMiner for **file extraction & OS fingerprinting**.
- Importance of **pre-attack context** in detecting insider threats early.

  

All details are documented in the Incident Report and PCAP/Zeek outputs inside this repo.
