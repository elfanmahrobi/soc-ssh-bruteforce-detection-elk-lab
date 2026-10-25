# Incident Report: SSH Brute-Force Detection

## 1. Incident Summary

- **Incident ID:** SOC-LAB-SSH-001
- **Status:** Closed / To be completed
- **Severity:** Medium
- **Detection source:** Elastic Stack
- **Analyst:** Elfan Mahrobi
- **Source IP:** `192.168.56.122`
- **Destination IP:** `192.168.56.123`
- **Service:** SSH / TCP 22
- **First observed:** `<UTC timestamp>`
- **Last observed:** `<UTC timestamp>`

## 2. Executive Summary

Repeated failed SSH authentication attempts were detected from the authorized Kali Linux lab machine against the monitored Ubuntu Server. The events were collected by Filebeat, processed by Logstash, indexed in Elasticsearch, and reviewed in Kibana. The activity was validated as an authorized simulation. Temporary containment was applied with UFW, verified, and removed after testing.

## 3. Detection Details

- **Rule name:** Linux SSH Brute-Force Attempts
- **Rule type:** Threshold
- **Threshold:** 5 failures from the same source IP
- **Time window:** 1 minute
- **Targeted user(s):** `<username>`
- **Total failed attempts:** `<count>`
- **Successful login after failures:** Yes / No

## 4. Timeline

| Timestamp (UTC) | Activity |
|---|---|
| `<time>` | First failed SSH authentication |
| `<time>` | Detection threshold reached |
| `<time>` | Alert generated |
| `<time>` | SOC triage started |
| `<time>` | Source IP blocked with UFW |
| `<time>` | Containment verified |
| `<time>` | Temporary firewall rule removed |
| `<time>` | Recovery verified and incident closed |

## 5. Investigation Findings

- The source was identified as the authorized Kali Linux VM.
- The destination was the monitored Ubuntu Server.
- Authentication failures were visible in both the source logs and Kibana.
- `<Document whether any successful authentication followed the failures.>`
- No activity occurred outside the isolated laboratory.

## 6. Containment

A temporary UFW rule blocked SSH connections from `192.168.56.122`. The rule was validated from Kali Linux and removed after the exercise.

## 7. Recovery

SSH connectivity was re-tested after removing the temporary deny rule. Filebeat and the Elastic Stack remained operational.

## 8. MITRE ATT&CK Mapping

- T1110 - Brute Force
- T1110.001 - Password Guessing
- T1078 - Valid Accounts, reviewed as a possible follow-on condition

## 9. Evidence

- `<Link to failed-authentication screenshot>`
- `<Link to Kibana Discover screenshot>`
- `<Link to alert screenshot>`
- `<Link to UFW containment screenshot>`
- `<Link to recovery screenshot>`

## 10. Lessons Learned

- Validate structured fields before enabling a production-style rule.
- Review successes after failures to identify possible account compromise.
- Preserve a management path before applying host firewall containment.
- Keep evidence timestamps consistent, preferably in UTC.
