# Wazuh SSH Brute-Force Detection

## Objective

Create and validate a custom Wazuh correlation rule to detect repeated SSH authentication failures from the same source IP within a defined time window.

## Detection Logic

The custom rule uses the built-in SSH authentication-failure rule as its parent. It correlates matching events by source IP and generates a level 12 alert when the configured frequency threshold is reached.

--------------------------------------------------
| Parameter             | Value                  |
|-----------------------|------------------------|
| Custom rule ID        | 100100                 |
| Parent rule ID        | 5760                   |
| Alert level           | 12                     |
| Frequency             | 7                      |
| Timeframe             | 60 seconds             |
| Source correlation    | Same source IP         |
| MITRE ATT&CK          | T1110 — Brute Force    |


## Investigation Findings

During testing, failed SSH authentication attempts generated the following built-in alerts:

- Rule 5760 — SSH authentication failed; level 5.
- Rule 5762 — SSH connection reset; level 4.
- Rule 5503 — PAM user login failed; level 5.

The initial custom rule referenced rule 5710. The observed failed-password events instead matched rule 5760. After correcting the parent rule reference and restarting the Wazuh manager, custom rule 100100 appeared in the dashboard at level 12.

## Validation

1. Generated controlled failed SSH login attempts from the Dell Windows endpoint.
2. Confirmed SSH authentication-failure events in Wazuh alert logs.
3. Verified the parent rule ID from the event data.
4. Updated the custom rule to reference rule 5760.
5. Validated the Wazuh analysis configuration.
6. Restarted the Wazuh manager.
7. Confirmed level 12 alerts for rule 100100 in the dashboard.

## Result

**Detection successfully generated.**

The Wazuh dashboard displayed two alerts for custom rule 100100 with the description:

"Possible SSH brute-force activity: repeated authentication failures from the same source IP within 60 seconds."

## Technical Consideration

The endpoint logs may be collected through more than one source, such as `/var/log/auth.log` and `journald`. Duplicate events can affect correlation counts. Further validation should establish whether the threshold represents seven unique authentication failures or includes duplicate event records.

## Limitations

This rule detects repeated authentication failures. It does not, by itself, prove that an account was compromised or that a brute-force attack succeeded.

## MITRE ATT&CK Mapping

- T1110 — Brute Force
- T1110.001 — Password Guessing, where applicable to the observed activity

## Evidence

Attach the actual Wazuh dashboard alert, the custom rule configuration, and representative failed SSH event records.

## Conclusion

This lab demonstrates custom detection engineering, rule correlation, log investigation, MITRE ATT&CK mapping, and validation of a high-severity Wazuh alert.
