# Security Investigation Report: SSH Brute-Force Detection

## Summary

A controlled SSH authentication-failure simulation was performed to validate a custom Wazuh detection rule. The initial parent rule reference did not match the observed failed-password events. After correcting the reference to rule 5760, the custom level 12 detection appeared in the Wazuh dashboard.

## Evidence
___________________________________________________________________________________________________________________________
| Field                         | Observed value                                                                          |
|-------------------------------|-----------------------------------------------------------------------------------------|
| Custom rule ID                | 100100                                                                                  |
| Custom alert level            | 12                                                                                      |
| Parent rule ID                | 5760                                                                                    |
| Observed source IP            | 10.101.66.112                                                                           |
|  Agent Name                   | `cris-agent`                                                                            |
| Alert description             | Possible SSH brute-force activity: repeated authentication failures from the same source|
|                                 IP within 60 seconds                                                                    |
| Dashboard result              | Two custom alerts observed                                                              |
|_______________________________|_________________________________________________________________________________________|


## Root Cause of Initial Failure

The custom rule referenced rule 5710, while the sampled failed-password events matched rule 5760. The parent rule reference was corrected, after which the custom detection generated alerts.

## Assessment

The custom detection was successfully demonstrated in the lab. The alerts establish that the configured detection fired; they do not independently establish that seven unique failures occurred, that an attack succeeded, or that the endpoint was compromised.

## Recommended Follow-Up

- Verify the source and destination endpoints.
- Review the two alert timestamps and corresponding source IPs.
- Investigate duplicate records from multiple log sources.
- Confirm whether any successful authentication followed the failures.
- Record final evidence and any remaining limitations.

## Status

Detection validation successful; duplicate-event and authentication-outcome checks remain recommended.
