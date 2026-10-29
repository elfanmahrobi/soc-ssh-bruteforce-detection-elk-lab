# Project Evidence

This directory contains the sanitized evidence collected during the SSH
brute-force detection and incident response exercise.

All testing was performed inside an isolated VirtualBox Host-Only network
using personally controlled virtual machines.

## Evidence Files

### `01-virtualbox-lab.png`

Displays the VirtualBox laboratory environment containing the ELK Server,
Kali Linux attack-simulation machine, and monitored Ubuntu Server.

### `02-elk-services.png`

Confirms that Elasticsearch, Logstash, and Kibana are active and that the
required service ports are listening.

### `03-filebeat-validation.png`

Shows the Filebeat configuration validation, Logstash output test, and
Filebeat service status on the monitored Ubuntu Server.

### `04-successful-ssh-baseline.png`

Records the successful SSH authentication event used as the normal
authentication baseline.

### `05-failed-ssh-events.png`

Shows repeated failed SSH authentication events generated from the
authorized source machine.

### `06-kibana-discover.png`

Displays the centralized investigation of failed SSH authentication
events using Kibana Discover.

### `07-detection-rule.png`

Shows the SSH brute-force detection rule configuration and enabled status.

### `08-security-alert.png`

Displays the security alert generated after the detection conditions were
met.

### `09-email-notification.png`

Confirms that the Elastic email connector successfully delivered the
automated SSH brute-force alert notification.

### `10-ufw-containment.png`

Shows the temporary UFW deny rule applied to contain SSH access from the
identified source address.

### `11-containment-verification.png`

Confirms that SSH access from the contained source was blocked while the
SSH service remained operational.

### `12-recovery-verification.png`

Confirms that SSH connectivity was restored after the temporary UFW deny
rule was removed.

## Evidence Handling

Before being added to this repository, the evidence was reviewed to avoid
publishing sensitive information.

The following information must not be committed:

- Passwords
- Elasticsearch credentials
- API keys
- Service account tokens
- Encryption keys
- SSH private keys
- Browser cookies
- Session tokens
- Unrelated personal information

## Incident Workflow

```text
Baseline Validation
        |
        v
Failed SSH Authentication Simulation
        |
        v
Centralized Log Collection
        |
        v
Kibana Investigation
        |
        v
Detection Rule Triggered
        |
        v
Security Alert Generated
        |
        v
Email Notification Delivered
        |
        v
UFW Containment
        |
        v
Containment Verification
        |
        v
Recovery Verification
        |
        v
Incident Closure
