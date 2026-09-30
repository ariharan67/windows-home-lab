# SSH Failed Authentication Investigation

## Incident Summary

A failed SSH authentication attempt was detected on the Windows endpoint. The attempt originated from the Kali Linux VM at 10.123.151.204 and targeted the Windows account ELCOT on the Dell endpoint at 10.123.151.112.

The authentication failure was intentionally generated as part of a controlled SOC home lab exercise.

## Date and Time

28-09-2026 08:08:27

## Source

- Host: Kali Linux VM
- IP Address: 10.123.151.204

## Destination

- Host: Dell Windows 11 endpoint
- IP Address: 10.123.151.112
- Service: OpenSSH
- Port: TCP/22

## Account

- Username: ELCOT
- Account Status: Enabled

## Detection Evidence

### Windows Security Event 4625

- Event ID: 4625
- Failure Reason: Unknown user name or bad password
- Status: 0xC000006D
- Sub Status: 0xC000006A
- Logon Type: 8
- Caller Process: C:\Windows\System32\OpenSSH\sshd.exe

### OpenSSH Operational Log

The OpenSSH Operational log recorded:

Failed password for ELCOT from 10.123.151.204

This provided the source IP address that was not present in the Windows 4625 event.

## Investigation Timeline

| Time | Event |
|------|-------|
| 08:08:27 | OpenSSH recorded a failed password for ELCOT from 10.123.151.204 |
| 08:08:27 | Windows Security generated Event ID 4625 |
| 08:09:17 | OpenSSH recorded the connection being closed |

## Analysis

The evidence shows a failed SSH authentication attempt against the enabled ELCOT account.

The Windows Security event confirms the authentication failure and identifies sshd.exe as the caller process. The OpenSSH Operational log correlates the event with the Kali VM source address 10.123.151.204.

This was a controlled test performed by the lab operator. Therefore, the event should not be classified as a real unauthorized intrusion.

## Conclusion

A controlled failed SSH authentication event was successfully detected and correlated across Windows Security and OpenSSH Operational logs.

## Evidence Files

- evidence/event-4625.txt
- evidence/openssh-operational.txt
