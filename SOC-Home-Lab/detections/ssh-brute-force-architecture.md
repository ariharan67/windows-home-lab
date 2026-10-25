# SSH Brute-Force Detection Architecture

## Event Flow

Dell Windows endpoint  
↓  
SSH client generates failed authentication attempts  
↓  
Kali Linux SSH server records authentication events  
↓  
Wazuh agent collects the relevant logs  
↓  
Wazuh manager analyzes the events  
↓  
Built-in rule 5760 identifies SSH authentication failure  
↓  
Custom rule 100100 correlates repeated failures from the same source IP  
↓  
Level 12 alert is generated  
↓  
SOC analyst investigates the alert in the Wazuh dashboard

## Detection Components

- **Log source:** SSH authentication logs
- **Event classification:** Built-in rule 5760
- **Correlation:** Seven matching events within 60 seconds from the same source IP
- **Custom detection:** Rule 100100
- **Severity:** Level 12
- **ATT&CK mapping:** T1110 — Brute Force

## Analyst Responsibilities

1. Confirm the source IP and affected account.
2. Review the timestamps and frequency of failures.
3. Check for a subsequent successful login.
4. Identify duplicate records or legitimate causes.
5. Document the investigation and escalation decision.
