# SSH Brute-Force Detection — Setup Guide

## Environment

- SIEM: Wazuh
- Deployment: Docker Compose
- Manager container: `single-node-wazuh.manager-1`
- Source endpoint: Dell Windows laptop
- SSH destination: Kali Linux
- Custom rule: 100100

## Step 1 — Start the Wazuh environment

```bash
cd ~/wazuh-docker/single-node
sudo docker compose up -d
sudo docker ps
```

## Step 2 — Edit the custom rule

```bash
sudo nano /var/lib/docker/volumes/single-node_wazuh_etc/_data/rules/local_rules.xml
```

Add or update rule 100100 inside the existing custom rule group. Use parent rule 5760 based on the observed event in this lab.

## Step 3 — Validate the configuration

```bash
sudo docker exec single-node-wazuh.manager-1 /var/ossec/bin/wazuh-analysisd -t
```

No output was observed during the successful validation attempt in this lab.

## Step 4 — Restart the manager

```bash
sudo docker restart single-node-wazuh.manager-1
```

Confirm that the container is running:

```bash
sudo docker ps
```

## Step 5 — Inspect SSH alerts

```bash
sudo docker exec single-node-wazuh.manager-1 grep -a 'sshd' /var/ossec/logs/alerts/alerts.json | tail -10
```

Inspect the event's rule ID, source IP, description, and full log to identify the matching built-in rule.

## Step 6 — Verify the custom detection

```bash
sudo docker exec single-node-wazuh.manager-1 grep -a '"id":"100100"' /var/ossec/logs/alerts/alerts.json | tail -10
```

Alternatively, search for rule ID 100100 in the Wazuh dashboard's alerts view.

## Troubleshooting

### Built-in alerts appear but the custom rule does not

Check that the parent rule ID matches the actual SSH failure event. Use Wazuh log testing to inspect the event's matching rule.

### The custom rule fires during log testing but not in live alerts

Log testing confirms the rule's behavior against supplied sample events. It does not prove that the live manager is receiving the same events or that the running configuration has been reloaded.

### The frequency threshold appears to trigger unexpectedly

Inspect the log sources for duplicate events. Determine whether the same failure is being collected through both `auth.log` and `journald`.

## Safety

Perform testing only against systems you own or are authorized to assess. Use controlled failed-login attempts and avoid testing against external systems.
