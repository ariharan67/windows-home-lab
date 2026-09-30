# Wazuh SOC Home Lab

Hands-on SOC home lab demonstrating Windows and Linux security monitoring, Wazuh SIEM detection, alert investigation, log correlation, evidence collection, and MITRE ATT&CK mapping.

## Project Overview

This project demonstrates a practical Security Operations Center (SOC) workflow using Wazuh.

The lab includes:

- Linux authentication monitoring
- Windows security event monitoring
- SSH authentication monitoring
- Wazuh SIEM alert detection
- Security log investigation
- Log correlation
- Evidence collection
- Incident documentation
- MITRE ATT&CK mapping

## Lab Environment

| Component | Details |
|---|---|
| SIEM | Wazuh 4.14.7 |
| Manager | Docker |
| Dashboard | Wazuh Dashboard |
| Endpoint 1 | Kali Linux |
| Endpoint 2 | Windows 11 |
| Windows Wazuh Agent | `dell-windows` |
| Windows Agent ID | `002` |
| Network | Private lab network |

## Architecture

```text
Kali Linux
10.123.151.204
     │
     │ SSH
     ▼
Windows 11
10.123.151.112
     │
     ├── Windows Security Logs
     └── OpenSSH Operational Logs
              │
              ▼
        Wazuh Agent
              │
              ▼
        Wazuh Manager
              │
              ▼
        Wazuh Dashboard
              │
              ▼
       SOC Investigation
