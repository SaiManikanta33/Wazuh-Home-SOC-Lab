# Wazuh-Home-SOC-Lab
# Wazuh Home SOC Lab

A hands-on cybersecurity project to learn Security Information and Event Management (SIEM), endpoint log monitoring, authentication-event detection, and security alert investigation using Wazuh.

## Project Overview

This project demonstrates a basic Home Security Operations Center (SOC) environment using Wazuh deployed with Docker Compose on Ubuntu. A Kali Linux endpoint sends system events to the Wazuh manager, where security rules analyze the events and generate alerts.

The goal is to understand how a SOC analyst collects logs, investigates authentication events, and interprets security alerts.

## Objectives

- Deploy Wazuh using Docker Compose.
- Connect a Linux endpoint to the Wazuh manager.
- Collect Linux authentication and system events.
- Detect successful and failed authentication activity.
- Investigate alerts using rule IDs, severity levels, and MITRE ATT&CK mappings.
- Practise basic SOC alert triage.

## Lab Environment

| Component | Details |
|---|---|
| Host OS | Windows 11 |
| Virtualization | VMware |
| SIEM Platform | Wazuh 4.14.2 |
| Deployment | Docker Compose |
| SIEM Server | Ubuntu Desktop VM |
| Monitored Endpoint | Kali Linux VM |
| Endpoint Agent | Wazuh Agent 4.14.2 |

## Architecture

Windows 11 Host  
└── VMware  
&nbsp;&nbsp;&nbsp;&nbsp;├── Ubuntu Desktop — Wazuh Manager, Indexer and Dashboard  
&nbsp;&nbsp;&nbsp;&nbsp;└── Kali Linux — Wazuh Agent  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── System and authentication logs  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓  
Wazuh Manager → Detection Rules → Security Alerts → Dashboard

## Tools and Technologies

- Wazuh SIEM
- Docker and Docker Compose
- Ubuntu Linux
- Kali Linux
- VMware
- Linux journald and PAM authentication logs
- MITRE ATT&CK

## Practical Detection Results

### 1. Successful Authentication Activity

Observed PAM session events generated during legitimate user activity and privilege escalation using `sudo`.

### 2. Failed Login Detection

Performed a controlled failed authentication attempt against a dedicated lab account named `soc_test_user`.

Wazuh generated the following alerts:

| Rule ID | Detection | Severity |
|---|---|---|
| 5503 | PAM: User login failed | Level 5 |
| 5557 | unix_chkpwd: Password check failed | Level 5 |
| 5301 | Failed `su` attempt | Level 5 |

These alerts were associated with the MITRE ATT&CK technique **T1110.001 — Password Guessing**.

This was an intentionally generated lab event, not a real-world attack.

## What I Learned

- Deploying a SIEM using Docker Compose.
- Connecting and verifying a Wazuh endpoint agent.
- Understanding Linux authentication logs and PAM events.
- Investigating Wazuh alerts using rule IDs and severity levels.
- Relating authentication detections to MITRE ATT&CK.
- Following a basic SOC investigation workflow.

## Current Scope

The initial lab focuses on Wazuh deployment, Linux endpoint monitoring, and authentication-event detection. Additional endpoints, detection scenarios, and investigation exercises can be added as the lab develops.

## Disclaimer

This project is for educational purposes and authorized lab testing only. All simulations were performed in my own virtualized lab environment.

## Author

Cybersecurity student | Interested in SOC operations, Blue Team security, SIEM, and incident investigation.
