# Incident Report: SSH Brute-Force Detection

## 1. Incident Summary

- **Incident ID:** SOC-LAB-SSH-001
- **Status:** Closed
- **Severity:** Medium
- **Classification:** Authorized Security Simulation
- **Detection Source:** Elastic Stack
- **Analyst:** Elfan Mahrobi
- **Source Host:** Kali Linux
- **Source IP:** `192.168.56.122`
- **Destination Host:** Ubuntu Monitored Server
- **Destination IP:** `192.168.56.123`
- **Service:** SSH / TCP 22
- **Incident Date:** October 28, 2026
- **Alert Notification Time:** October 28, 2026 at 02:44 WIB
- **Containment Status:** Completed
- **Recovery Status:** Completed

> Exact event timestamps are preserved in the Linux authentication logs,
> Kibana event records, security alert details, and project evidence
> screenshots. System clocks were synchronized using NTP before the final
> simulation and evidence collection.

---

## 2. Executive Summary

Repeated failed SSH authentication attempts were detected from the authorized
Kali Linux lab machine at `192.168.56.122` against the monitored Ubuntu Server
at `192.168.56.123`.

The events were generated as part of a controlled security simulation inside
an isolated VirtualBox Host-Only network. Filebeat collected the Linux
authentication logs from the monitored Ubuntu Server and forwarded them to
Logstash over TCP port 5044. Logstash processed the events and sent them to
Elasticsearch, where they were investigated through Kibana.

The SSH brute-force detection rule identified the repeated authentication
failures after the configured threshold was reached. A security alert was
generated, and an Elastic email connector successfully delivered an automated
notification containing the alert details.

The source was validated as the authorized Kali Linux simulation machine.
A temporary UFW rule was applied to block SSH connections from the identified
source IP. The containment was successfully verified while the SSH service
remained operational.

The temporary firewall rule was subsequently removed, SSH connectivity was
restored, the recovery procedure was validated, and the incident was closed.

---

## 3. Detection Details

- **Rule Name:** Linux SSH Brute-Force Attempts
- **Rule Type:** Threshold
- **Detection Method:** Repeated SSH authentication failures
- **Threshold:** Five or more failed authentication events
- **Detection Window:** One minute
- **Targeted User:** `socfake`
- **Source IP:** `192.168.56.122`
- **Destination IP:** `192.168.56.123`
- **Total Failed Attempts:** At least five failed authentication events
- **Alert Severity:** Medium
- **Notification Method:** Elastic email connector
- **Successful Unauthorized Login:** No
- **Detection Result:** True Positive
- **Operational Context:** Authorized security simulation
- **Production Impact:** None

The detection was classified as a true positive because the rule correctly
identified the repeated failed SSH authentication behavior it was designed to
detect.

However, the activity was intentionally generated as part of an authorized
home-lab exercise and did not represent malicious production activity.

---

## 4. Timeline

| Date and Time | Activity |
|---|---|
| October 28, 2026 | System clocks were validated and synchronized using NTP |
| October 28, 2026 | A successful SSH authentication was recorded as the normal baseline |
| October 28, 2026 | Controlled failed SSH authentication attempts were generated from `192.168.56.122` |
| October 28, 2026 | Filebeat collected the Ubuntu authentication events |
| October 28, 2026 | Logstash processed and forwarded the events to Elasticsearch |
| October 28, 2026 | Failed SSH authentication events were investigated in Kibana Discover |
| October 28, 2026 | The configured detection threshold was reached |
| October 28, 2026 | The SSH brute-force security alert was generated |
| October 28, 2026 at 02:44 WIB | The Elastic email connector delivered the automated alert notification |
| October 28, 2026 | SOC triage and investigation were completed |
| October 28, 2026 | A temporary UFW rule blocked SSH access from `192.168.56.122` |
| October 28, 2026 | Containment was verified from the Kali Linux machine |
| October 28, 2026 | The temporary UFW deny rule was removed |
| October 28, 2026 | SSH connectivity was restored and recovery was verified |
| October 28, 2026 | The incident was documented and closed |

> Exact event-level timestamps remain available in the Kibana investigation,
> Linux authentication logs, security alert details, email notification, and
> project evidence screenshots.

---

## 5. Investigation Findings

The investigation produced the following findings:

- The source was identified as the authorized Kali Linux virtual machine at
  `192.168.56.122`.
- The destination was identified as the monitored Ubuntu Server at
  `192.168.56.123`.
- The affected service was OpenSSH on TCP port 22.
- The failed authentication attempts targeted the test username `socfake`.
- The test username did not represent a legitimate production account.
- At least five failed authentication events occurred within the configured
  detection window.
- The failed authentication events were recorded in the Linux authentication
  logs on the monitored Ubuntu Server.
- Filebeat successfully collected the Linux authentication logs.
- Filebeat successfully forwarded the events to Logstash over TCP port 5044.
- Logstash successfully processed and forwarded the events to Elasticsearch.
- The authentication failures were visible and searchable in Kibana Discover.
- The configured detection rule successfully identified the repeated failed
  authentication activity.
- A corresponding security alert was successfully generated.
- The Elastic email connector successfully delivered an automated alert
  notification.
- No unauthorized successful authentication from `192.168.56.122` was
  identified during the reviewed simulation window.
- The successful SSH authentication recorded before the simulation was an
  authorized baseline event.
- The successful connection recorded after containment was an authorized
  recovery validation.
- No activity occurred outside the isolated VirtualBox laboratory.
- No public or third-party systems were targeted.
- No production service disruption or data loss occurred.

---

## 6. Alert Triage

The alert was reviewed using the following SOC triage process:

1. Confirmed the detection rule name and alert severity.
2. Reviewed the alert generation timestamp.
3. Validated the source IP as `192.168.56.122`.
4. Validated the destination IP as `192.168.56.123`.
5. Confirmed that the affected service was SSH on TCP port 22.
6. Reviewed the failed authentication events in Kibana Discover.
7. Confirmed that the targeted username was `socfake`.
8. Reviewed the number of failed authentication attempts.
9. Reviewed the detection window and event sequence.
10. Checked for successful SSH authentication after the failed attempts.
11. Confirmed that no unauthorized successful login occurred.
12. Confirmed that the source was the authorized Kali Linux lab machine.
13. Confirmed that all activity remained inside the isolated laboratory.
14. Classified the alert as a true positive generated by an authorized
    security simulation.
15. Initiated temporary source-IP containment for incident-response validation.

### Triage Conclusion

The alert accurately represented repeated SSH authentication failures from the
authorized Kali Linux simulation machine. No evidence of unauthorized account
access, service compromise, lateral movement, or activity outside the lab was
identified.

---

## 7. Alert Notification

An Elastic email connector was configured as part of the alerting workflow.

After the SSH brute-force detection rule triggered, an automated email
notification was successfully delivered.

The notification contained the following information:

- Alert subject
- Detection rule information
- Monitored agent
- Source IP address
- Alert severity or level
- Detection message
- Notification timestamp

The successful notification confirmed that the alerting workflow operated
end-to-end:

```text
Failed SSH Authentication Events
                |
                v
Elastic Detection Rule
                |
                v
Security Alert
                |
                v
Email Connector
                |
                v
Automated Notification Received
```

The email notification provided sufficient initial context to begin SOC alert
triage and investigation.

---

## 8. Containment

A temporary UFW rule was applied on the monitored Ubuntu Server to block SSH
connections from the identified source IP.

The containment scope was:

```text
Source IP: 192.168.56.122
Destination IP: 192.168.56.123
Destination Port: TCP 22
Action: Deny
```

Before activating the firewall policy, SSH access was allowed to prevent
accidental loss of management access.

The UFW firewall was enabled, and the temporary source-specific deny rule was
inserted above the existing SSH allow rules.

This rule order ensured that SSH connections from `192.168.56.122` were blocked
before the broader allow rules were evaluated.

The temporary containment command used was:

```bash
sudo ufw insert 1 deny from 192.168.56.122 to any port 22 proto tcp
```

The active firewall policy was verified using:

```bash
sudo ufw status numbered
```

The containment action was limited to SSH traffic from the identified source
address. The SSH service itself remained active, and unrelated network traffic
was not intentionally blocked.

---

## 9. Containment Verification

Containment was tested from the Kali Linux source machine after the UFW deny
rule was applied.

The SSH port was tested using:

```bash
nc -zv -w 5 192.168.56.123 22
```

An SSH connection attempt was also performed using:

```bash
ssh -o ConnectTimeout=5 azadmin@192.168.56.123
```

The connection attempt from `192.168.56.122` was blocked as expected.

The SSH service was separately verified on the monitored Ubuntu Server using:

```bash
sudo systemctl is-active ssh
```

The SSH service remained active during containment.

This confirmed that:

- The source-specific UFW rule was functioning correctly.
- SSH access from `192.168.56.122` was blocked.
- The SSH service remained operational.
- The failed connection was caused by the firewall restriction.
- The failed connection was not caused by an SSH service outage.

Containment was therefore considered successful.

---

## 10. Recovery

After containment validation was completed, the temporary UFW deny rule was
removed.

The active UFW rules were reviewed using:

```bash
sudo ufw status numbered
```

The temporary deny rule was identified by its current rule number.

The rule was then removed using:

```bash
sudo ufw delete <rule-number>
```

The firewall rules were reviewed again to confirm that the temporary deny rule
was no longer active:

```bash
sudo ufw status numbered
```

After the rule was removed, SSH connectivity was tested again from Kali Linux:

```bash
nc -zv 192.168.56.123 22
```

The connection to TCP port 22 succeeded, confirming that SSH connectivity had
been restored.

A normal authorized SSH connection was also used to verify that the monitored
Ubuntu Server had returned to its expected operating state:

```bash
ssh azadmin@192.168.56.123
```

Filebeat, Logstash, Elasticsearch, and Kibana remained operational throughout
the containment and recovery activities.

Recovery was successfully completed.

---

## 11. Root Cause and Classification

### Root Cause

The security alert was generated by intentionally repeated failed SSH
authentication attempts performed from an authorized Kali Linux virtual
machine.

The activity was designed to validate:

- Linux authentication log collection
- Filebeat log forwarding
- Logstash processing
- Elasticsearch indexing
- Kibana investigation
- SSH brute-force detection logic
- Security alert generation
- Email notification
- SOC triage
- UFW containment
- Containment verification
- Recovery verification

### Incident Classification

```text
Incident Type: SSH Brute-Force Detection
Detection Result: True Positive
Operational Context: Authorized Security Simulation
Production Impact: None
Account Compromise: Not Observed
Unauthorized Access: Not Observed
Data Loss: None
Service Disruption: None
Containment Required: Yes, for validation
Recovery Required: Yes
Final Status: Closed
```

The event was classified as a true positive because the detection rule
correctly identified the activity it was designed to detect.

The event was not classified as a malicious production incident because all
activity was intentionally generated inside an isolated and authorized
laboratory environment.

---

## 12. MITRE ATT&CK Mapping

### T1110 – Brute Force

Repeated attempts were made to authenticate to the SSH service.

### T1110.001 – Password Guessing

Multiple failed password attempts were generated against the test username
`socfake`.

### T1078 – Valid Accounts

Successful authentication following repeated failed attempts was reviewed as a
possible follow-on condition.

No unauthorized use of valid credentials was identified during the
investigation.

---

## 13. Evidence

The following evidence was collected and stored in the repository.

### Lab and Service Validation

- ../screenshots/01-virtualbox-lab.png
- ../screenshots/02-elk-services.png
- ../screenshots/03-filebeat-validation.png

### Authentication Activity

- ../screenshots/04-successful-ssh-baseline.png
- ../screenshots/05-failed-ssh-events.png

### Investigation and Detection

- ../screenshots/06-kibana-discover.png
- ../screenshots/07-detection-rule.png
- ../screenshots/08-security-alert.png
- ../screenshots/09-email-notification.png

### Containment and Recovery

- ../screenshots/10-ufw-containment.png
- ../screenshots/11-containment-verification.png
- ../screenshots/12-recovery-verification.png

---

## 14. Lessons Learned

- System clocks must be synchronized before collecting and correlating security
  evidence.
- Time synchronization should be validated across the ELK Server, monitored
  endpoint, and simulation machine.
- A successful authentication baseline should be recorded before generating
  suspicious authentication activity.
- Structured event fields should be validated before enabling a
  production-style detection rule.
- Detection rules should group events by `source.ip` when the field is
  consistently available.
- Successful authentication following repeated failures must be reviewed to
  identify potential account compromise.
- Alert notifications should contain enough information to support initial SOC
  triage.
- Console or management access should be preserved before applying host
  firewall containment.
- SSH access should be allowed before activating UFW to prevent accidental
  management lockout.
- Source-specific deny rules must be positioned above broader allow rules.
- Containment must be verified while confirming that the affected service
  remains operational.
- Temporary containment rules must be removed after validation.
- Recovery must be independently verified after containment is reversed.
- Detection evidence should include logs, investigation results, rule
  configuration, security alerts, notification delivery, containment, and
  recovery.
- Screenshots and documentation must be reviewed before publication to prevent
  the exposure of passwords, tokens, API keys, private keys, encryption keys,
  email addresses, and other sensitive information.

---

## 15. Recommendations

The following improvements are recommended for future implementations:

1. Enable TLS between Filebeat and Logstash.
2. Validate Elasticsearch certificates instead of disabling certificate
   verification.
3. Use the Filebeat System module or a standardized ECS-compatible ingest
   pipeline for Linux authentication events.
4. Ensure that `source.ip`, `source.port`, `user.name`, `event.outcome`, and
   `event.action` are consistently populated.
5. Add automated source-IP enrichment where appropriate.
6. Create an additional rule for successful authentication following multiple
   failed attempts.
7. Create an additional rule for one source IP targeting multiple usernames.
8. Add alert suppression to reduce duplicate notifications.
9. Tune the threshold and detection window before adapting the rule to a
   production environment.
10. Apply least-privilege access to Elastic Security rules and connectors.
11. Store Elasticsearch and connector credentials in secure keystores.
12. Implement log-retention and index-lifecycle policies.
13. Enable secure certificate verification between Logstash and Elasticsearch.
14. Document detection rule ownership and review schedules.
15. Periodically validate notification connectors and incident-response
    procedures.

---

## 16. Final Outcome

The project successfully demonstrated an end-to-end defensive security
workflow:

```text
Authorized SSH Authentication Simulation
                |
                v
Linux Authentication Logs
                |
                v
Filebeat
                |
                | TCP/5044
                v
Logstash
                |
                v
Elasticsearch
                |
                v
Kibana Investigation
                |
                v
SSH Brute-Force Detection Rule
                |
                v
Security Alert
                |
                v
Automated Email Notification
                |
                v
SOC Triage
                |
                v
UFW Source-IP Containment
                |
                v
Containment Verification
                |
                v
Temporary Firewall Rule Removal
                |
                v
Recovery Verification
                |
                v
Incident Closure
```

The following objectives were successfully completed:

- ELK Stack service validation
- Filebeat configuration validation
- Filebeat-to-Logstash connectivity validation
- Linux authentication log collection
- Successful SSH baseline validation
- Controlled failed SSH authentication simulation
- Kibana investigation
- SSH brute-force detection rule creation
- Security alert generation
- Automated email notification
- SOC alert triage
- Source-IP containment using UFW
- Containment verification
- Firewall rule removal
- SSH recovery verification
- Incident documentation
- Evidence collection

No unauthorized access, production impact, data loss, service disruption, or
activity outside the isolated laboratory environment was identified.

---

## 17. Incident Closure

The SSH brute-force detection, investigation, notification, containment, and
recovery objectives were successfully completed.

The security alert was confirmed as a true positive generated by an authorized
security simulation.

The detection rule correctly identified repeated SSH authentication failures
from `192.168.56.122` against the monitored Ubuntu Server at
`192.168.56.123`.

The Elastic email connector successfully delivered the automated security
notification. The source IP was temporarily contained using UFW, containment
was validated, and the temporary firewall rule was removed after testing.

SSH connectivity was successfully restored, and all monitoring components
remained operational.

### Closure Summary

- **Final Status:** Closed
- **Detection Validated:** Yes
- **Kibana Investigation Completed:** Yes
- **Security Alert Generated:** Yes
- **Email Notification Validated:** Yes
- **SOC Triage Completed:** Yes
- **Containment Applied:** Yes
- **Containment Validated:** Yes
- **Recovery Completed:** Yes
- **Recovery Validated:** Yes
- **Unauthorized Access Identified:** No
- **Account Compromise Identified:** No
- **Production Impact:** None
- **Data Loss:** None
- **Service Disruption:** None
- **Further Immediate Action Required:** No

The incident is formally closed.
