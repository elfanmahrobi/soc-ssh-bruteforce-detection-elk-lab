# SOC SSH Brute-Force Detection Lab with Elastic Stack

## Overview

This project demonstrates an end-to-end SOC workflow for detecting, investigating, and responding to repeated SSH authentication failures in a controlled VirtualBox home lab.

A Kali Linux virtual machine is used to generate authorized failed SSH authentication attempts against an Ubuntu Server. Filebeat collects the Ubuntu authentication logs and sends them to Logstash on the ELK Server. Logstash processes the events, Elasticsearch stores and indexes them, and Kibana is used for analysis, visualization, detection, and incident investigation.

The project covers:

- Linux authentication-log collection
- SSH failed-login analysis
- Centralized log ingestion with Filebeat and Logstash
- Elasticsearch indexing
- Kibana investigation and dashboarding
- Threshold-based SSH brute-force detection
- Source-IP and target-user identification
- SOC alert triage
- Incident timeline reconstruction
- Containment using UFW
- Containment and recovery verification
- Incident documentation
- MITRE ATT&CK mapping

> [!IMPORTANT]
> All activity in this repository is intended only for systems owned by the lab operator and operated inside an isolated VirtualBox Host-Only network.

---

## Lab Architecture

| Component | Role | IP Address |
|---|---|---:|
| Ubuntu ELK Server | Elasticsearch, Logstash, Kibana | `192.168.56.121` |
| Kali Linux | Authorized attacker and analyst workstation | `192.168.56.122` |
| Ubuntu Server | Monitored endpoint with SSH and Filebeat | `192.168.56.123` |
| VirtualBox Host-Only Network | Isolated lab network | `192.168.56.0/24` |

### Data and Investigation Flow

```text
Kali Linux
192.168.56.122
Authorized failed SSH attempts
        |
        | SSH / TCP 22
        v
Ubuntu Monitored Server
192.168.56.123
OpenSSH + authentication logs + Filebeat
        |
        | Beats / TCP 5044
        v
Ubuntu ELK Server
192.168.56.121
Logstash -> Elasticsearch -> Kibana
        |
        v
Detection -> SOC Triage -> Investigation
        |
        v
Containment -> Verification -> Recovery
```

For the detailed topology, see [`documentation/01-network-topology.md`](documentation/01-network-topology.md).

---

## Tools and Technologies

- Oracle VirtualBox
- Ubuntu Server
- Kali Linux
- OpenSSH
- Filebeat
- Logstash
- Elasticsearch
- Kibana
- Linux authentication logs
- UFW
- Bash and standard Linux utilities
- MITRE ATT&CK

---

## Repository Structure

```text
soc-ssh-bruteforce-detection-elk-lab/
├── README.md
├── detection/
│   ├── kibana-query.md
│   └── ssh-bruteforce-rule.md
├── documentation/
│   ├── 01-network-topology.md
│   ├── 02-log-onboarding.md
│   ├── 03-attack-simulation.md
│   ├── 04-investigation-workflow.md
│   └── 05-containment-recovery.md
├── evidence/
│   └── README.md
├── filebeat/
│   └── filebeat.yml.example
├── incident-report/
│   └── ssh-bruteforce-incident-report.md
├── logstash/
│   └── linux-auth.conf.example
└── screenshots/
    └── README.md
```

---

## SOC Investigation Workflow

```text
Validate normal SSH access
        |
Generate controlled failed SSH logins
        |
Collect authentication logs with Filebeat
        |
Parse and index events in Elasticsearch
        |
Investigate events in Kibana
        |
Trigger threshold-based detection
        |
Identify source IP, user, time, and outcome
        |
Contain the source using UFW
        |
Verify blocked access and continued log flow
        |
Remove the temporary block after validation
        |
Document evidence, findings, and lessons learned
```

---

## Implementation Guide

### 1. Validate Connectivity

From Kali Linux:

```bash
ping -c 4 192.168.56.123
nc -zv 192.168.56.123 22
```

From the Ubuntu monitored server:

```bash
ping -c 4 192.168.56.121
nc -zv 192.168.56.121 5044
```

### 2. Confirm SSH and Filebeat

On the monitored Ubuntu Server:

```bash
sudo systemctl is-active ssh
sudo systemctl is-active filebeat
sudo filebeat test config -e
sudo filebeat test output -e
```

### 3. Confirm the Elastic Stack

On the ELK Server:

```bash
systemctl is-active elasticsearch logstash kibana
sudo ss -lntp | grep -E ':5044|:5601|:9200'
```

### 4. Establish a Normal Baseline

From Kali Linux, perform one authorized successful SSH login:

```bash
ssh <lab-user>@192.168.56.123
```

On the monitored server, verify the accepted event:

```bash
sudo journalctl -u ssh --since "10 minutes ago"
```

### 5. Generate Controlled Failed Authentication Events

Follow [`documentation/03-attack-simulation.md`](documentation/03-attack-simulation.md). Use only the isolated lab system.

### 6. Investigate in Kibana

Open Kibana and use the queries in [`detection/kibana-query.md`](detection/kibana-query.md). Confirm:

- Source IP: `192.168.56.122`
- Destination host: Ubuntu monitored server
- SSH service or process
- Failed authentication outcome
- Targeted username
- Event count and time range

### 7. Create the Detection Rule

Use the implementation guidance in [`detection/ssh-bruteforce-rule.md`](detection/ssh-bruteforce-rule.md).

### 8. Contain and Verify

Follow [`documentation/05-containment-recovery.md`](documentation/05-containment-recovery.md).

### 9. Complete the Incident Report

Record the validated results in [`incident-report/ssh-bruteforce-incident-report.md`](incident-report/ssh-bruteforce-incident-report.md).

---

## Detection Logic

### Objective

Detect repeated failed SSH authentication attempts from the same source IP against the monitored Ubuntu Server.

### Recommended Lab Threshold

- Event: failed SSH authentication
- Group by: `source.ip`
- Threshold: 5 or more events
- Time window: 1 minute
- Severity: Medium
- Risk score: 47

> Adjust field names after checking the actual Filebeat data in Kibana Discover. Field availability can vary depending on the ingest pipeline and Filebeat integration.

### Generic KQL

```text
host.ip:"192.168.56.123" and event.category:authentication and event.outcome:failure
```

Fallback search:

```text
message:"Failed password" and source.ip:"192.168.56.122"
```

---

## MITRE ATT&CK Mapping

| Tactic | Technique | Relevance |
|---|---|---|
| Credential Access | T1110 - Brute Force | Repeated attempts to authenticate to SSH accounts |
| Credential Access | T1110.001 - Password Guessing | Repeated password guesses against one or more usernames |
| Initial Access / Persistence | T1078 - Valid Accounts | Review successful authentication after failures to identify possible account compromise |

---

## Suggested Evidence

Add sanitized screenshots to `screenshots/`:

1. VirtualBox lab environment
2. ELK services and listening ports
3. Filebeat configuration and output validation
4. Normal successful SSH baseline
5. Controlled failed SSH events on Ubuntu
6. Events visible in Kibana Discover
7. Detection rule configuration
8. Generated security alert
9. UFW containment rule
10. Failed connection after containment
11. Recovery verification

See [`screenshots/README.md`](screenshots/README.md) for filenames and guidance.

---

## Expected Outcome

At the end of this lab, the analyst should be able to:

- Explain the end-to-end Linux authentication log pipeline
- Identify repeated SSH failures in Kibana
- Build and validate a threshold-based detection rule
- Perform basic SOC alert triage
- Reconstruct a simple incident timeline
- Contain a suspicious source with a host firewall
- Validate containment and recovery
- Document evidence and investigation findings

---

## Project Status

- [x] Lab topology documented
- [x] Filebeat and Logstash examples added
- [x] Detection logic documented
- [x] Investigation workflow documented
- [x] Containment and recovery procedure documented
- [x] Incident report template added
- [ ] Replace placeholders with actual screenshots
- [ ] Record the actual Kibana data view and field names
- [ ] Export and add the validated Kibana detection rule
- [ ] Complete the incident report with real lab timestamps

---

## Disclaimer

This project is for defensive security education in an isolated environment. Do not perform authentication testing, scanning, or access attempts against systems without explicit authorization.
